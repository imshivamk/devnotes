Complete ADB, validation, experiment, and plotting commands

```bash

ADB Setup &amp; Verification
cd C:\cellular_experiment
adb version
adb devicesc
adb shell getprop gsm.network.type
adb shell getprop gsm.operator.alpha
adb shell dumpsys telephony.registry | findstr /i CellSignalStrengthLte

Python &amp; Libraries
python --version
python -m pip install pandas matplotlib
python -c "import pandas, matplotlib"
	

python -c "import pandas, matplotlib; print('Pandas:', pandas.__version__); print('Matplotlib:', matplotlib.__version__)"

```

Validation

Experiment 1 — Indoor vs Outdoor
```bash
python cellular_logger.py --mode adb --interval 2 --duration 120 --label indoor --output indoor_final.csv
	
python cellular_logger.py --mode adb --interval 2 --duration 120 --label outdoor --output outdoor_final.csv
```


Experiment 2 — 2G, 3G, 4G and 5G
```bash

python cellular_logger.py --mode adb --interval 2 --duration 60 --label 2G --output 2G_final.csv
python cellular_logger.py --mode adb --interval 2 --duration 60 --label 3G --output 3G_final.csv
python cellular_logger.py --mode adb --interval 2 --duration 60 --label 4G --output 4G_final.csv
python cellular_logger.py --mode adb --interval 2 --duration 60 --label 5G --output 5G_final.csv
```

Experiment 3 — Mobility and Handover

```bash
python cellular_logger.py --mode adb --interval 1 --duration 300 --label walking --output walking_final.csv
```

Experiment 4 — Operator Comparison

```bash
python cellular_logger.py --mode adb --interval 2 --duration 120 --label airtel --output airtel_final.csv
python cellular_logger.py --mode adb --interval 2 --duration 120 --label jio --output jio_final.csv0
```

Generate Plots

```bash
python cellular_logger.py --plot indoor_final.csv
python cellular_logger.py --plot outdoor_final.csv
python cellular_logger.py --plot 2G_final.csv
python cellular_logger.py --plot 3G_final.csv
python cellular_logger.py --plot 4G_final.csv
python cellular_logger.py --plot 5G_final.csv
python cellular_logger.py --plot walking_final.csv
python cellular_logger.py --plot airtel_final.csv
python cellular_logger.py --plot jio_final.csv

Check Output Files
dir *.csv
dir *.png
```


## logger

```python
cellular_logger.py

#!/usr/bin/env python3
"""
Study of Cellular Network Parameters using a Smartphone
-------------------------------------------------------
Logs signal strength and network generation (2G/3G/4G/5G) at fixed intervals,
saves to CSV, prints a summary, and plots the results.

MODES
  termux : run ON the Android phone inside Termux (needs Termux:API app + pkg)
  adb    : run on a PC, phone connected via USB with USB debugging enabled

USAGE
  python cellular_logger.py --mode termux --interval 2 --duration 120 --label indoor
  python cellular_logger.py --mode adb    --interval 2 --duration 120 --label outdoor
  python cellular_logger.py --plot cellular_log.csv
"""

import argparse
import csv
import json
import os
import re
import statistics
import subprocess
import time
from datetime import datetime

CSV_FIELDS = [
    "timestamp", "label", "network_type", "generation", "operator",
    "rat_cell", "signal_dbm", "rsrp", "rsrq", "rssnr", "level_0_4",
    "cell_id", "pci", "tac_lac", "channel", "data_state", "roaming",
]

# ----------------------------------------------------------------------------
# Helpers
# ----------------------------------------------------------------------------
def run(cmd, timeout=15):
    """Run a shell command, return stdout text ('' on failure)."""
    try:
        out = subprocess.run(cmd, capture_output=True, text=True,
                             timeout=timeout, shell=isinstance(cmd, str))
        return out.stdout.strip()
    except Exception as e:
        print(f"[warn] command failed: {e}")
        return ""


def to_generation(net_type: str) -> str:
    """Map Android network type string to 2G/3G/4G/5G."""
    t = (net_type or "").lower()
    if t in ("gsm", "gprs", "edge", "cdma", "1xrtt", "iden"):
        return "2G"
    if t in ("umts", "wcdma", "hsdpa", "hsupa", "hspa", "hspap", "tdscdma",
             "evdo_0", "evdo_a", "evdo_b", "ehrpd"):
        return "3G"
    if t in ("lte", "lte_ca"):
        return "4G"
    if t in ("nr", "5g"):
        return "5G"
    return "Unknown"


def first(d, *keys):
    """Return first non-None value among dict keys."""
    for k in keys:
        if d.get(k) is not None:
            return d[k]
    return None


# ----------------------------------------------------------------------------
# Backend 1: Termux (runs on the phone)
#   pkg install python termux-api ; install "Termux:API" app from F-Droid
# ----------------------------------------------------------------------------
def read_termux():
    info, cells = {}, []
    try:
        info = json.loads(run("termux-telephony-deviceinfo") or "{}")
    except json.JSONDecodeError:
        pass
    try:
        cells = json.loads(run("termux-telephony-cellinfo") or "[]")
    except json.JSONDecodeError:
        pass

    # Prefer the registered (serving) cell
    serving = next((c for c in cells if c.get("registered")), cells[0] if cells else {})

    net_type = info.get("data_network_type") or serving.get("type", "")
    rat = serving.get("type", "")

    return {
        "network_type": net_type,
        "generation": to_generation(net_type if net_type != "unknown" else rat),
        "operator": info.get("network_operator_name", ""),
        "rat_cell": rat,
        "signal_dbm": first(serving, "dbm", "rsrp", "ss_rsrp", "rssi"),
        "rsrp": first(serving, "rsrp", "ss_rsrp"),
        "rsrq": first(serving, "rsrq", "ss_rsrq"),
        "rssnr": first(serving, "rssnr", "ss_sinr"),
        "level_0_4": serving.get("level"),
        "cell_id": first(serving, "cid", "ci", "nci"),
        "pci": first(serving, "pci", "psc"),
        "tac_lac": first(serving, "tac", "lac"),
        "channel": first(serving, "earfcn", "uarfcn", "arfcn", "nrarfcn"),
        "data_state": info.get("data_state", ""),
        "roaming": info.get("network_roaming", ""),
    }


# ----------------------------------------------------------------------------
# Backend 2: ADB (runs on PC)   -> install platform-tools, `adb devices`
#   Output of dumpsys differs between Android versions, so this is best-effort.
# ----------------------------------------------------------------------------
def _grab(pattern, text):
    m = re.search(pattern, text)
    if not m:
        return None

    for group in m.groups():
        if group is not None:
            try:
                return int(group)
            except ValueError:
                return None

    return None


def read_adb():
    net_type = run("adb shell getprop gsm.network.type").split(",")[0]
    operator = run("adb shell getprop gsm.operator.alpha").split(",")[0]
    roaming = run("adb shell getprop gsm.operator.isroaming").split(",")[0]

    dump = run("adb shell dumpsys telephony.registry", timeout=30)

    rat = ""
    rsrp = None
    rsrq = None
    rssnr = None
    dbm = None
    level = None

    cell_id = None
    pci = None
    tac_lac = None
    channel = None

    # ------------------------------------------------------------
    # LTE: find the REGISTERED / SERVING LTE cell
    # ------------------------------------------------------------
    serving_lte = re.search(
        r"CellInfoLte:\{"
        r"mRegistered=YES.*?"
        r"CellIdentityLte:\s*\{(.*?)\}"
        r".*?"
        r"CellSignalStrengthLte:\s*(.*?)(?=CellInfoLte:|$)",
        dump,
        re.DOTALL
    )

    if serving_lte:
        identity = serving_lte.group(1)
        signal = serving_lte.group(2)

        # Cell ID may be masked by Samsung, so keep it as text
        m = re.search(r"\bmCi=(\S+)", identity)
        if m:
            cell_id = m.group(1)

        # PCI
        m = re.search(r"\bmPci=(\d+)", identity)
        if m:
            pci = int(m.group(1))

        # TAC
        m = re.search(r"\bmTac=(\S+)", identity)
        if m:
            tac_lac = m.group(1)

        # EARFCN
        m = re.search(r"\bmEarfcn=(\d+)", identity)
        if m:
            channel = int(m.group(1))

        # LTE signal measurements
        m = re.search(r"\brsrp=(-?\d+)", signal)
        if m:
            rsrp = int(m.group(1))

        m = re.search(r"\brsrq=(-?\d+)", signal)
        if m:
            rsrq = int(m.group(1))

        m = re.search(r"\brssnr=(-?\d+)", signal)
        if m:
            rssnr = int(m.group(1))

        m = re.search(r"\blevel=(\d+)", signal)
        if m:
            level = int(m.group(1))

        if rsrp is not None and abs(rsrp) < 1000:
            rat = "lte"
            dbm = rsrp

    # ------------------------------------------------------------
    # Fallback LTE signal parser
    # ------------------------------------------------------------
    if dbm is None:
        lte_blocks = re.findall(
            r"CellSignalStrengthLte:\s*(.*?)(?=CellSignalStrength[A-Za-z]+:|$)",
            dump,
            re.DOTALL
        )

        for block in lte_blocks:
            m_rsrp = re.search(r"\brsrp=(-?\d+)", block)
            m_rsrq = re.search(r"\brsrq=(-?\d+)", block)
            m_rssnr = re.search(r"\brssnr=(-?\d+)", block)
            m_level = re.search(r"\blevel=(\d+)", block)

            if m_rsrp:
                candidate_rsrp = int(m_rsrp.group(1))

                if abs(candidate_rsrp) < 1000:
                    rat = "lte"
                    dbm = candidate_rsrp
                    rsrp = candidate_rsrp

                    if m_rsrq:
                        rsrq = int(m_rsrq.group(1))

                    if m_rssnr:
                        rssnr = int(m_rssnr.group(1))

                    if m_level:
                        level = int(m_level.group(1))

                    break

    # ------------------------------------------------------------
    # 5G NR fallback
    # ------------------------------------------------------------
    if dbm is None:
        nr_blocks = re.findall(
            r"CellSignalStrengthNr:\s*(.*?)(?=CellSignalStrength[A-Za-z]+:|$)",
            dump,
            re.DOTALL
        )

        for block in nr_blocks:
            m_rsrp = re.search(r"\bssRsrp=(-?\d+)", block)
            m_rsrq = re.search(r"\bssRsrq=(-?\d+)", block)
            m_sinr = re.search(r"\bssSinr=(-?\d+)", block)
            m_level = re.search(r"\blevel=(\d+)", block)

            if m_rsrp:
                candidate = int(m_rsrp.group(1))

                if abs(candidate) < 1000:
                    rat = "nr"
                    dbm = candidate
                    rsrp = candidate

                    if m_rsrq:
                        rsrq = int(m_rsrq.group(1))

                    if m_sinr:
                        rssnr = int(m_sinr.group(1))

                    if m_level:
                        level = int(m_level.group(1))

                    break

    # ------------------------------------------------------------
    # GSM / 2G fallback
    # ------------------------------------------------------------
    if dbm is None:
        gsm_blocks = re.findall(
            r"CellSignalStrengthGsm:\s*(.*?)(?=CellSignalStrength[A-Za-z]+:|$)",
            dump,
            re.DOTALL
        )

        for block in gsm_blocks:
            m_rssi = re.search(r"\brssi=(-?\d+)", block)
            m_level = re.search(r"\blevel=(\d+)", block)

            if m_rssi:
                candidate = int(m_rssi.group(1))

                if abs(candidate) < 1000:
                    rat = "gsm"
                    dbm = candidate

                    if m_level:
                        level = int(m_level.group(1))

                    break

    return {
        "network_type": net_type,
        "generation": to_generation(net_type or rat),
        "operator": operator,
        "rat_cell": rat,
        "signal_dbm": dbm,
        "rsrp": rsrp,
        "rsrq": rsrq,
        "rssnr": rssnr,
        "level_0_4": level,
        "cell_id": cell_id,
        "pci": pci,
        "tac_lac": tac_lac,
        "channel": channel,
        "data_state": "",
        "roaming": roaming,
    }
# ----------------------------------------------------------------------------
# Logging, summary, plotting
# ----------------------------------------------------------------------------
def quality(dbm):
    """Rough LTE-style RSRP quality bucket."""
    if dbm is None:
        return "n/a"
    if dbm >= -80:  return "Excellent"
    if dbm >= -90:  return "Good"
    if dbm >= -100: return "Fair"
    return "Poor"


def log_data(args):
    reader = read_termux if args.mode == "termux" else read_adb
    new_file = not os.path.exists(args.output)
    rows = []

    print(f"Logging every {args.interval}s for {args.duration}s "
          f"-> {args.output}  (Ctrl+C to stop early)\n")
    print(f"{'Time':8} {'Gen':5} {'Type':8} {'dBm':>5} {'RSRQ':>5} "
          f"{'SINR':>5}  Quality   Operator")

    with open(args.output, "a", newline="") as f:
        w = csv.DictWriter(f, fieldnames=CSV_FIELDS)
        if new_file:
            w.writeheader()
        start = time.time()
        try:
            while time.time() - start < args.duration:
                d = reader()
                d["timestamp"] = datetime.now().isoformat(timespec="seconds")
                d["label"] = args.label
                w.writerow({k: d.get(k) for k in CSV_FIELDS})
                f.flush()
                rows.append(d)
                print(f"{d['timestamp'][11:]:8} {d['generation']:5} "
                      f"{str(d['network_type']):8} {str(d['signal_dbm']):>5} "
                      f"{str(d['rsrq']):>5} {str(d['rssnr']):>5}  "
                      f"{quality(d['signal_dbm']):9} {d['operator']}")
                time.sleep(args.interval)
        except KeyboardInterrupt:
            print("\nStopped by user.")
    summarize(rows)


def summarize(rows):
    vals = [r["signal_dbm"] for r in rows if isinstance(r["signal_dbm"], (int, float))]
    if not vals:
        print("\nNo valid signal readings captured. Check permissions / mode.")
        return
    print("\n----- SUMMARY -----")
    print(f"Samples        : {len(rows)}")
    print(f"Signal min/avg/max (dBm): {min(vals)} / "
          f"{statistics.mean(vals):.1f} / {max(vals)}")
    if len(vals) > 1:
        print(f"Std deviation  : {statistics.stdev(vals):.2f} dB")
    gens = {}
    for r in rows:
        gens[r["generation"]] = gens.get(r["generation"], 0) + 1
    for g, n in gens.items():
        print(f"Time on {g:7}: {100*n/len(rows):.0f}%")


def plot_csv(path):
    import pandas as pd
    import matplotlib.pyplot as plt

    df = pd.read_csv(path, parse_dates=["timestamp"])
    df["signal_dbm"] = pd.to_numeric(df["signal_dbm"], errors="coerce")

    fig, ax = plt.subplots(1, 2, figsize=(13, 4.5))

    # Signal vs time, coloured by generation
    colors = {"2G": "tab:red", "3G": "tab:orange", "4G": "tab:blue",
              "5G": "tab:green", "Unknown": "gray"}
    for gen, g in df.groupby("generation"):
	ax[0].plot(g["timestamp"], g["signal_dbm"],
               marker="o", markersize=4,
               label=gen, color=colors.get(gen, "gray"))
    ax[0].set_title("Signal strength over time")
    ax[0].set_ylabel("
    ax[0].legend()
    ax[0].grid(alpha=.3)
    fig.autofmt_xdate()

    # Mean signal by label (e.g., indoor vs outdoor) / by generation
    key = "label" if df["label"].nunique() > 1 else "generation"
    df.groupby(key)["signal_dbm"].mean().plot.bar(ax=ax[1], color="tab:purple")
    ax[1].set_title(f"Average signal by {key}")
    ax[1].set_ylabel("dBm")
    ax[1].grid(alpha=.3)

    plt.tight_layout()
    out = os.path.splitext(path)[0] + "_plot.png"
    plt.savefig(out, dpi=150)
    print(f"Saved plot -> {out}")
    plt.show()


def main():
    p = argparse.ArgumentParser(description="Cellular network parameter logger")
    p.add_argument("--mode", choices=["termux", "adb"], default="termux")
    p.add_argument("--interval", type=float, default=2, help="seconds between samples")
    p.add_argument("--duration", type=float, default=60, help="total seconds")
    p.add_argument("--label", default="run1", help="location tag, e.g. indoor/outdoor")
    p.add_argument("--output", default="cellular_log.csv")
    p.add_argument("--plot", metavar="CSV", help="plot an existing CSV and exit")
    args = p.parse_args()

    if args.plot:
        plot_csv(args.plot)
    else:
        log_data(args)


if __name__ == "__main__":
    main()

Displaying cellular_logger.py.
```