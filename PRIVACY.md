# Klar Privacy Policy

_Last updated: October 17, 2026_

Klar is a portfolio-analysis connector for Claude, operated by Klar Analytics. This policy explains what data the Klar server handles when Claude calls Klar's tools. It covers the Klar server only; your conversation with Claude is handled by Anthropic under its own terms and privacy policy.

## What Klar receives

When you ask Claude for an analysis, Claude sends Klar's server only what that analysis needs:

- **Your holdings**: ticker symbols with their weights, or the contents of the positions file you attached (which can include quantities, market values and account totals).
- **Analysis settings**: date ranges, the factor model, and for goal and projection questions the numbers you provide, such as a target amount, time horizon, current portfolio value, contributions, withdrawal rate, risk tolerance and, if you share them, your current and retirement ages.

Klar does not ask for your name, email, brokerage login or account numbers, and has no user accounts.

## How it's used

Klar uses this data only to compute the analysis you asked for and return it to Claude and the dashboard. To do that, the server requests public market data for your ticker symbols:

- **Yahoo Finance**, for prices, dividends and ticker search. Only ticker symbols, search terms and date ranges are sent.
- **Kenneth French Data Library and FRED**, for factor and risk-free-rate data. No portfolio data is sent.

Klar does not use AI services to process your data, does not sell or share it, and does not use it for advertising or profiling.

## What Klar keeps

- **No database.** Klar stores no portfolios or results permanently.
- **Short-lived cache.** Results are held in the server's memory for up to one hour so the dashboard can load and redraw charts. They are lost when the server restarts.
- **Server logs.** Operational logs can include ticker symbols, a portfolio's total value when your file contains one, analysis settings, timing and errors. They do not include names or account numbers. Logs are kept by our hosting provider, Railway (United States), under its standard retention, and are used only to run and debug the service.

## Children

Klar is not directed to anyone under 18.

## Not financial advice

Klar provides information and historical analysis, not investment, tax or legal advice.

## Changes and contact

We'll update this page if Klar's data handling changes. Questions: [open an issue](https://github.com/mirandag1099/klar-claude-plugin/issues).
