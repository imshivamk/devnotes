```sh
#!/bin/bash

# ================================
# Linux Commands Practical - WSL
# ================================

cd ~
mkdir -p demo
cd demo

# ---------- Setup ----------
mkdir -p files directories backup text logs links
touch files/file1.txt files/file2.txt files/file3.txt
echo "Hello Linux" > files/file1.txt
echo "Operating Systems Assignment" > files/file2.txt
echo "Linux commands are useful" > files/file3.txt

# ---------- Basic Commands ----------
pwd
ls
ls -l
ls -a
ls -la
ls -lh
ls -lt
ls -lah

mkdir testdir
mkdir -p testdir/subdir1/subdir2
ls -R testdir
mkdir emptydir
rmdir emptydir

touch files/newfile.txt
touch files/a.txt files/b.txt files/c.txt

cp files/file1.txt backup/file1_backup.txt
cp -r files backup/files_copy
mv files/newfile.txt files/renamed.txt
mv files/renamed.txt backup/

touch files/delete_me.txt
rm files/delete_me.txt

mkdir -p directories/remove_me/subdir
touch directories/remove_me/test.txt
rm -r directories/remove_me

cd files
pwd
cd ..
cd ~
cd ~/demo

# ---------- cat / less / head / tail ----------
cat files/file1.txt
cat files/file1.txt files/file2.txt

cat > files/cat_demo.txt <<'EOF'
Line 1
Line 2
Line 3
Line 4
Line 5
EOF

cat files/cat_demo.txt
cat >> files/cat_demo.txt <<'EOF'
Line 6
Line 7
EOF

cat files/cat_demo.txt
head files/cat_demo.txt
head -n 3 files/cat_demo.txt
tail files/cat_demo.txt
tail -n 3 files/cat_demo.txt

# ---------- echo / date / sleep / time ----------
echo "Hello Linux"
echo $USER
echo $HOME
echo $PATH

echo "First line" > files/echo.txt
echo "Second line" >> files/echo.txt
cat files/echo.txt

date
date +"%Y-%m-%d"
date +"%H:%M:%S"

sleep 1
time sleep 1
time find . -type f

# ---------- clear / history / alias ----------
clear
history
history | grep mkdir

alias ll='ls -lah'
ll

alias demo-pwd='pwd'
demo-pwd
alias
unalias demo-pwd

# ---------- find ----------
find . -type f
find . -type d
find . -type f -name "*.txt"
find . -type f -mmin -10
find files -type f -name "*.txt" -exec wc -l {} \;

# ---------- grep ----------
cat > text/students.txt <<'EOF'
Alice 85
Bob 72
Charlie 91
David 68
Eve 95
Frank 76
EOF

grep "Alice" text/students.txt
grep -i "alice" text/students.txt
grep -n "Alice" text/students.txt
grep -v "Alice" text/students.txt
grep -R "Linux" .

# ---------- chmod ----------
cat > files/hello.sh <<'EOF'
#!/bin/bash
echo "Hello from shell script"
EOF

ls -l files/hello.sh
chmod +x files/hello.sh
./files/hello.sh

chmod 644 files/file1.txt
chmod 755 files/hello.sh
chmod u+x files/file1.txt
chmod u-x files/file1.txt
chmod g+r files/file1.txt
chmod o-r files/file1.txt
ls -l files

# ---------- chown ----------
ls -l files/file1.txt
sudo chown $USER:$USER files/file1.txt
sudo chown -R $USER:$USER backup
ls -l files/file1.txt

# ---------- hard link / symbolic link ----------
ln files/file1.txt links/file1_hardlink
ls -li files/file1.txt links/file1_hardlink

ln -s ../files/file1.txt links/file1_symlink
ls -l links
cat links/file1_symlink

# ---------- diff / cmp / comm ----------
cat > text/a.txt <<'EOF'
apple
banana
cherry
EOF

cat > text/b.txt <<'EOF'
apple
banana
orange
EOF

diff text/a.txt text/b.txt
cmp text/a.txt text/b.txt

sort text/a.txt > text/a_sorted.txt
sort text/b.txt > text/b_sorted.txt
comm text/a_sorted.txt text/b_sorted.txt

# ---------- cut / sort / wc / paste / awk / sed ----------
cut -d' ' -f1 text/students.txt
cut -d' ' -f2 text/students.txt

sort text/students.txt
sort -k2 -n text/students.txt
sort -k2 -nr text/students.txt

wc text/students.txt
wc -l text/students.txt
wc -w text/students.txt
wc -c text/students.txt

cat > text/names.txt <<'EOF'
Alice
Bob
Charlie
EOF

cat > text/marks.txt <<'EOF'
85
72
91
EOF

paste text/names.txt text/marks.txt

awk '{print $1}' text/students.txt
awk '{print $2}' text/students.txt
awk '{print "Student:", $1, "Marks:", $2}' text/students.txt
awk '$2 > 80 {print $1, $2}' text/students.txt
awk '{sum += $2; count++} END {print "Average:", sum/count}' text/students.txt

sed 's/Linux/UNIX/' files/file3.txt
sed '2d' files/cat_demo.txt
sed -i 's/Line/ROW/g' files/cat_demo.txt
cat files/cat_demo.txt

# ---------- tar ----------
tar -cvf backup.tar files/
tar -tvf backup.tar

mkdir extracted
tar -xvf backup.tar -C extracted

tar -czvf backup.tar.gz files/
mkdir extracted_gz
tar -xzvf backup.tar.gz -C extracted_gz

# ---------- zip / unzip ----------
sudo apt update
sudo apt install -y zip unzip

zip -r files.zip files/
unzip -l files.zip
mkdir unzip_demo
unzip files.zip -d unzip_demo

# ---------- disk commands ----------
df
df -h
du .
du -h .
du -sh .
du -h --max-depth=1 . | sort -h

# ---------- system information ----------
uname
uname -r
uname -a
whoami
id
id $USER
groups
uptime
free
free -h

# ---------- environment variables ----------
env
echo $HOME
echo $USER
echo $PATH

export COLLEGE="Operating Systems"
echo $COLLEGE
env | grep COLLEGE
unset COLLEGE

# ---------- processes ----------
ps
ps aux
ps aux | grep bash
ps -ef --forest

sleep 300 &
pgrep -a sleep
kill $!

sleep 300 &
sleep 300 &
pgrep -a sleep
killall sleep

sleep 30 &
jobs
kill $!

echo $!

# ---------- watch ----------
watch -n 1 free -h &
WATCH_PID=$!
sleep 3
kill $WATCH_PID

# ---------- strace ----------
sudo apt install -y strace
strace ls files
strace echo "Hello"

# ---------- lsof ----------
sudo apt install -y lsof
lsof files/file1.txt
sudo lsof -i

# ---------- dmesg / vmstat / iostat ----------
sudo dmesg | head
sudo dmesg | grep -i usb

vmstat
vmstat 2 3

sudo apt install -y sysstat
iostat
iostat -xz 1 3

# ---------- networking ----------
ip a
ip addr
ip link
ip route
ip addr show eth0

sudo apt install -y net-tools
ifconfig

ping -c 4 127.0.0.1

ss -l
ss -t
ss -tln
ss -tuln

netstat -tulnc,

curl https://example.com
curl -I https://example.com
curl -o example.html https://example.com

wget https://example.com
wget -O example2.html https://example.com

sudo apt install -y dnsutils
nslookup example.com

sudo apt install -y traceroute
traceroute example.com

# ---------- nc ----------
# Requires two terminals:
# Terminal 1: nc -l 5000
# Terminal 2: nc localhost 5000

# ---------- tcpdump ----------
# Example:
# sudo tcpdump -i lo
# In another terminal:
# ping -c 3 127.0.0.1

# ---------- firewall ----------
sudo iptables -L -n -v
sudo nft list ruleset
sudo ufw status
sudo ufw status verbose

# ---------- SSH ----------
ssh -V

# ---------- rsync ----------
mkdir -p sync_demo
rsync -av files/ sync_demo/

# ---------- mount ----------
mount
findmnt
ls /mnt
ls /mnt/c

# ---------- systemctl / service ----------
systemctl
systemctl status
service --status-all

# ---------- journalctl ----------
journalctl -n 20

# ---------- whereis / whatis ----------
whereis ls
whereis bash
whereis python

whatis ls
whatis grep
whatis chmod
whatis ps

# ---------- package management ----------
apt search htop
apt list --installed | grep htop

# ---------- RPM ----------
rpm -qa
rpm -q bash

# ---------- safe dd demonstration ----------
clear
# ---------- Useful combinations ----------
ps aux | grep bash
du -h --max-depth=1 . | sort -h
find . -type f -name "*.txt" -exec grep -l "Linux" {} \;
find . -type f | wc -l
find . -name "*.txt" -exec wc -l {} +
find . -type f -name "*.txt" | sort
history | grep chmod
ss -tuln
free -h
df -h

# ---------- Final verification ----------
pwd
ls -lah
find . -type f | sort
echo "===== Linux practical commands completed ====="
```