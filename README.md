# TerminalDesk

A bus and cargo operations system for transport companies, built as a multi-company service. Counter and phone ticket sales, trips and crew, cargo, fleet maintenance, promotions, shifts and payroll, in English and Urdu.

> The source code is private. This page describes what it does and how it is built.
> **Live demo:** https://karwan.gearhost.io (demo data only)

## What it does

- **Selling:** seat map with live holds (SignalR), per-category fares, spot discounts, promotions with codes, limits and approval, refunds, printed tickets with a layout designer, barcode and QR codes, boarding check.
- **Operations:** trips list and board, crew and shifts, cashier closing, terminal statements, route and fare rules with a two-person approval.
- **Cargo:** rate card, waybills, loading and arrivals, proof of delivery, account customers, text-message outbox, reports.
- **Fleet:** buses, inspections, repair jobs, parts and maintenance steps.
- **Companies:** one program, many companies. Sign-up with a 14-day trial, a guided set-up checklist, an operator console, real sign-in with email and password.
- **Languages:** the whole program in English and Urdu.

## How it is built

- C#, .NET 10, ASP.NET Core MVC (Razor), a separate API host, SignalR.
- Layers: Domain, Application (services), Infrastructure (EF Core), Presentation, Web.
- EF Core on SQLite, SQL Server and PostgreSQL, with a migration set for each.
- **Multi-tenancy:** a company column on every table, a global query filter, composite keys that start with the company, and a guard that refuses a write to another company's row.
- **Sign-in:** salted PBKDF2-SHA256 password hashes, HMAC-signed session cookie, lockout after repeated failures, signed single-use links for email confirmation and password reset.
- A print agent for tickets and waybills, and a mail and text-message outbox.

## How it is tested

- More than **3,100 automated tests** (2,111 unit and 1,008 browser-driven), run on every change.
- **Break proofs:** each new rule is checked by breaking the code on purpose and confirming a test fails.
- **Isolation tests** that every table keeps one company's rows away from another's.
- **Performance tests** on a company of 100 routes, 300 terminals, 50 staff and 120,000 tickets. One run found a query filter that slowed a trips search from 0.13 s to 12 s under load. It was fixed and the test now guards it.
- Plain-words and Urdu checks that fail when a screen has an English-only message.

## Author

Zeeshan Saeed, senior .NET engineer. [Portfolio](https://zeeshansaeed83.github.io)
