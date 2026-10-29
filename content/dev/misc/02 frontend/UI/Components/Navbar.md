## using react


```jsx
// Requirements:
// npm install react-router-dom lucide-react
// Tailwind CSS configured in your React app.

import { useState } from "react";
import { Link } from "react-router-dom";
import { Menu, X, User } from "lucide-react";

const navLinks = [
  { to: "/", label: "Home" },
  { to: "/features", label: "Features" },
  { to: "/pricing", label: "Pricing" },
  { to: "/about", label: "About" },
];

export function Navbar() {
  const [open, setOpen] = useState(false);

  return (
    <header className="w-full border-b border-slate-200 bg-white/80 backdrop-blur">
      <nav className="mx-auto flex h-16 max-w-6xl items-center justify-between px-4 sm:px-6 lg:px-8">
        {/* Logo left */}
        <Link
          to="/"
          className="flex items-center gap-2 text-lg font-semibold tracking-tight text-slate-900"
        >
          <span className="h-8 w-8 rounded-lg bg-slate-900 text-white flex items-center justify-center text-sm font-bold">
            L
          </span>
          <span className="hidden sm:inline">LogoName</span>
        </Link>

        {/* Desktop links right */}
        <div className="hidden md:flex items-center gap-6">
          {navLinks.map((item) => (
            <Link
              key={item.to}
              to={item.to}
              className="text-sm font-medium text-slate-600 hover:text-slate-900 transition-colors"
            >
              {item.label}
            </Link>
          ))}
          <button
            type="button"
            className="inline-flex items-center gap-2 rounded-full border border-slate-200 px-3 py-1.5 text-sm font-medium text-slate-700 hover:border-slate-300 hover:bg-slate-50 transition-colors"
          >
            <User className="h-4 w-4" />
            <span className="hidden sm:inline">Account</span>
          </button>
        </div>

        {/* Mobile hamburger right */}
        <button
          type="button"
          className="inline-flex items-center justify-center rounded-md p-2 text-slate-700 hover:bg-slate-100 md:hidden"
          onClick={() => setOpen((prev) => !prev)}
          aria-label="Toggle navigation"
        >
          {open ? <X className="h-6 w-6" /> : <Menu className="h-6 w-6" />}
        </button>
      </nav>

      {/* Mobile menu (full-width dropdown) */}
      {open && (
        <div className="border-b border-slate-200 bg-white md:hidden">
          <div className="space-y-1 px-4 pb-3 pt-2">
            {navLinks.map((item) => (
              <Link
                key={item.to}
                to={item.to}
                className="block rounded-md px-3 py-2 text-base font-medium text-slate-700 hover:bg-slate-100"
                onClick={() => setOpen(false)}
              >
                {item.label}
              </Link>
            ))}
            <button
              type="button"
              className="mt-2 flex w-full items-center justify-between rounded-md border border-slate-200 px-3 py-2 text-sm font-medium text-slate-700 hover:bg-slate-50"
            >
              <span>Account</span>
              <User className="h-4 w-4" />
            </button>
          </div>
        </div>
      )}
    </header>
  );
}

```

## using shadcn

```jsx
// Requirements:
// - shadcn/ui set up (Button, Sheet, SheetContent, SheetTrigger, etc.)
// - Tailwind CSS configured
// - npm install react-router-dom lucide-react

import { useState } from "react";
import { Link } from "react-router-dom";
import { Menu, X, User } from "lucide-react";

import {
  Sheet,
  SheetContent,
  SheetTrigger,
  SheetClose,
} from "@/components/ui/sheet";
import { Button } from "@/components/ui/button";

const navLinks = [
  { to: "/", label: "Home" },
  { to: "/dashboard", label: "Dashboard" },
  { to: "/projects", label: "Projects" },
  { to: "/settings", label: "Settings" },
];

export function AppNavbar() {
  const [open, setOpen] = useState(false);

  return (
    <header className="sticky top-0 z-40 border-b bg-background/80 backdrop-blur">
      <div className="container flex h-16 items-center justify-between">
        {/* Logo left */}
        <Link to="/" className="flex items-center gap-2">
          <div className="flex h-8 w-8 items-center justify-center rounded-md bg-primary text-primary-foreground text-sm font-bold">
            A
          </div>
          <span className="hidden text-base font-semibold tracking-tight md:inline">
            AppName
          </span>
        </Link>

        {/* Desktop nav right */}
        <nav className="hidden items-center gap-6 md:flex">
          {navLinks.map((item) => (
            <Link
              key={item.to}
              to={item.to}
              className="text-sm font-medium text-muted-foreground transition-colors hover:text-foreground"
            >
              {item.label}
            </Link>
          ))}
          <Button
            variant="outline"
            size="icon"
            className="h-9 w-9 rounded-full"
          >
            <User className="h-4 w-4" />
          </Button>
        </nav>

        {/* Mobile nav: hamburger + sheet */}
        <div className="flex items-center gap-2 md:hidden">
          <Button
            variant="outline"
            size="icon"
            className="h-9 w-9 rounded-full"
          >
            <User className="h-4 w-4" />
          </Button>

          <Sheet open={open} onOpenChange={setOpen}>
            <SheetTrigger asChild>
              <Button
                variant="outline"
                size="icon"
                className="h-9 w-9 rounded-md"
              >
                {open ? (
                  <X className="h-5 w-5" />
                ) : (
                  <Menu className="h-5 w-5" />
                )}
              </Button>
            </SheetTrigger>
            <SheetContent side="right" className="w-64">
              <div className="mt-6 flex flex-col gap-4">
                {navLinks.map((item) => (
                  <SheetClose asChild key={item.to}>
                    <Link
                      to={item.to}
                      className="text-sm font-medium text-foreground"
                    >
                      {item.label}
                    </Link>
                  </SheetClose>
                ))}
              </div>
            </SheetContent>
          </Sheet>
        </div>
      </div>
    </header>
  );
}

```