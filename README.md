# Klar Portfolio Analytics for Claude

Klar turns Claude into a portfolio analyst. Attach a positions export from your brokerage (Fidelity, Schwab, Vanguard, Robinhood, Interactive Brokers and many international brokers) take a screenshot of your positions page, or just type your holdings, and ask Claude to analyze your portfolio. Klar runs the numbers on real market data and shows an interactive dashboard right in the conversation, and Claude explains what it means and answers your follow-up questions.

![Klar performance dashboard in Claude](assets/screenshots/performance.png)

## What you can ask

- **"Analyze my portfolio."** Returns versus the S&P 500 (1 month to 10 years), annual growth rate, volatility, worst drawdown, Sharpe and Sortino ratios, beta, which holdings drove returns and risk, correlations, calendar-year and monthly returns, and a risk/return map against the efficient frontier and major benchmarks.
- **"What's really driving my returns?"** Fama-French factor analysis (3-factor and 5-factor models): how much of your return comes from the market, company size, value, profitability and investment style, and whether any "alpha" is statistically meaningful.

The dashboard has controls for the period, the benchmark and the factor model, and Claude sees whatever you change.

## Install

**Claude (web, desktop and mobile):** open **Customize → Plugins**, search for **Klar**, and click **Add**. If Claude then asks you to connect Klar, click **Connect** once. There's no account or sign-in.

**Claude Code:**

```
/plugin marketplace add mirandag1099/klar-claude-plugin
/plugin install klar@klar-analytics
```

Then start a new conversation, attach your positions CSV, and say "analyze my portfolio." No account or API key is needed.

## What's in this plugin

- **Klar connector**: the tools Claude calls to load a portfolio, analyze performance, run factor regressions and look up tickers, plus the inline dashboard. They run on Klar's server at `https://mcp.klaranalytics.com/mcp`. The server also has one tool that only the dashboard calls, `get_view_data`, which fetches chart data for the view on screen. Claude doesn't call it, so the large chart series stay out of the conversation.
- **Klar skill**: the analyst playbook Claude follows: which analysis to run when, how to read each metric (`skills/klar/references/metrics.md`), and three modes (Analyst, Auditor, Coach).

## Good to know

- Results are based on historical prices from Yahoo Finance and factor data from the Kenneth French Data Library. Returns are total returns: dividends, bond interest and fund distributions are reinvested.
- Klar provides information and analysis, **not financial advice**. Past performance doesn't predict future results, and every decision stays yours.
- Klar keeps no accounts and no database. See the [privacy policy](https://klaranalytics.com/privacy) for exactly what data the server handles and how long it's kept.

## Support

Questions, bugs or broker formats that don't load: [open an issue](https://github.com/mirandag1099/klar-claude-plugin/issues).

## License

[MIT](LICENSE)
