# Klar metrics reference

This file explains each metric the Klar tools return: what it means in plain
language, how to read it, and what to tell the user. Thresholds marked
**(Klar)** are the cutoffs the Klar scoring code uses. Use them so your words
match the tool's labels. The other ranges are rules of thumb. Present them as
"typically", not as rules.

The figures and thresholds here are for your reading of the results. In your
reply, follow the skill's storytelling rules: say what the numbers mean in
plain words instead of reading them back, since the dashboard already shows
them.

## Units first

- **`analyze_portfolio` values are already percentages.** `annualVolatility:
  14.0` is 14%, `maxDrawdown: -28.0` is a 28% decline, `cagr5Y: 11.0` is 11%
  a year, and holding `weight: 12.5` is 12.5%. Never multiply them by 100.
- **Ratios have no units.** Sharpe, Sortino, beta, correlation, factor
  loadings, t-stats and R² are plain numbers (R² can also be shown as a
  percentage).
- **Other tools:** follow the units the tool text states. In the raw factor
  output, alpha is a decimal (`0.024` is 2.4% a year).

## Return metrics

### CAGR (compound annual growth rate)

**What it is:** the steady yearly growth rate that would turn the starting
value into the ending value. It smooths out the ups and downs.

**How to read it:** compare it to SPY over the same window. A 2-3 point gap per
year compounds into a large difference over a decade. Over 10 years, 10% vs 7%
a year is about 2.6× vs 2.0× growth. CAGR hides the path, so pair it with
volatility and drawdown.

**Where it is:** `performanceMetrics5Y.cagr5Y`. It isn't in `riskMetrics`.

### Period and cumulative returns (1M, 3M, YTD, 1Y, 3Y, 5Y)

**What they are:** total return over each window. Windows of 1 year or less
aren't annualized. For 3Y and 5Y, check whether the tool labels them as
cumulative or annualized before comparing.

**How to read them:** look for consistency. A portfolio that beats SPY in every
window behaves differently from one with a single great year. Short windows
(1M, 3M) are mostly noise, so don't draw conclusions from them alone.

### Contributions

**What they are:** each holding's share of the portfolio's return, roughly its
weight times its return. They add up to the portfolio return.

**How to read them:** say whether returns were broad or came from one or two
names. A large positive contribution from one holding is a concentration
signal even when the result was good.

## Risk metrics

### Volatility (annualized standard deviation)

**What it is:** how much returns swing around their average, scaled to a year.
In a normal year, returns typically land within about one volatility of the
average. Roughly 1 year in 20 lands more than two volatilities away.

**Typical ranges:**

| Portfolio | Volatility |
|---|---|
| Bonds or cash-heavy | under 6% |
| 60/40 balanced | about 9-11% |
| S&P 500 | about 15-18% long-run |
| Concentrated or tech-heavy portfolios, single stocks | 25%+ |

### Sharpe ratio

**What it is:** return above the risk-free rate per unit of total volatility.
It measures how much return you got for the swings you lived through. Klar
uses the 3-month T-bill return actually earned over the analysis window as the
risk-free rate (the summary prints it). If that data is unavailable, it uses
today's 3-month T-bill yield, or 2%.

**How to read it:** below 0 means the portfolio did worse than cash (T-bills),
0-0.5 is weak, 0.5-1 is decent, and above 1 is strong over a multi-year
period. Always compare to SPY's Sharpe over the same window. Sharpe depends a
lot on the period, because bull markets inflate it.

### Sortino ratio

**What it is:** like Sharpe, but it counts only downside volatility. Upside
swings aren't penalized.

**How to read it:** it's usually higher than Sharpe. If Sortino is much higher
than Sharpe, most of the volatility was upside. If the two are close, the
downside was as bumpy as the upside.

### Beta

**What it is:** sensitivity to the market (SPY).

- β = 1 moves with the market.
- β = 1.3 tends to move about 30% more in both directions.
- β = 0.6 moves about 40% less.

**How to read it:** beta above 1 amplifies drawdowns. Low beta with high
volatility means the risk comes from somewhere other than the broad market,
such as a sector or single stocks.

### Max drawdown

**What it is:** the largest peak-to-trough decline in the period. It's the
worst loss someone who bought at the top would have experienced.

**How to read it:** make it concrete. A -34% drawdown means a third of the
money was gone at the worst point. Compare it to SPY's drawdown in the same window. Also note the
recovery: a -50% loss needs a +100% gain to break even. The question to ask is
whether the user could have held through it without selling.

**Use the "Worst drawdown" section, not guesses.** It gives the peak and low
dates, whether and when the portfolio recovered, SPY's move over the same
stretch, and each holding's move. Build the drawdown story from those facts.

**Judging whether a holding helped (be precise):**
- If the portfolio fell less than SPY over the stretch, the mix **cushioned**
  the drop. Say so, with the gap in points.
- A holding that fell, but **less than stocks**, still helped: "BND fell 17%
  while stocks fell 25%, so it softened the loss." Never say it "didn't
  protect" or "failed when it mattered" in that case.
- Only a holding that rose or held flat was a true **hedge** (offset losses).
  Bonds falling alongside stocks, as in 2022 when rates rose, is worth noting
  as "it fell too, but less," not as a failure.
- Say a holding didn't help only if it fell as much as or more than stocks over
  that stretch.

**Total returns:** all of Klar's returns and drawdowns include dividends, bond
interest and fund distributions, reinvested (Yahoo Finance adjusted prices). They
match brokerage statements and fund fact sheets, not Yahoo's price chart. BND's
2021-2022 decline, for example, was about -18% including interest (about -23% in
price alone). If a user compares with a price chart, explain the difference.

### Sector exposure (look-through)

**What it is:** what industries the portfolio actually owns. Funds are split into
their holdings' sectors and scaled by the fund's weight: VOO at 20% of the
portfolio with 40% in technology adds 8 points of technology, not 40. Individual
stocks count fully in their sector, bond and cash shares show as Bonds and Cash,
and holdings without data show as Unclassified.

**How to read it:** compare the **share of stocks** column with SPY's, not the
whole-portfolio column, or bonds make every sector look underweight. Lead with
the biggest tilt the summary names, and connect it to concentration: several
funds and stocks can all feed the same sector.

**Caveats:** sector mixes are funds' current holdings (Yahoo/Morningstar), not
their history over the analysis window. Yahoo files some big tech names outside
Technology (Alphabet in Communication Services, Amazon in Consumer Cyclical), so
"tech exposure" in everyday terms can be larger than the Technology row.

### Correlation

**What it is:** how closely two holdings move together, from -1 (opposite) to
+1 (in lockstep).

**How to read it:**

| Correlation | Meaning |
|---|---|
| Above 0.85 | Near-duplicates. Two S&P 500 funds are about 0.99. |
| 0.7-0.85 | Highly correlated, with limited diversification benefit |
| 0.3-0.7 | Moderate |
| Below 0.3 | Real diversification, such as stocks vs Treasuries |

Correlations measured over a whole period can change in sell-offs; some pairs
move together more in a crash. Don't assert that from the correlation number
alone: check the summary's "Worst drawdown" section, which shows what each
holding actually did in the portfolio's worst stretch. Name the specific
highly correlated pairs.

### Risk contributions

**What they are:** each holding's share of total portfolio volatility. They
depend on weight, the holding's own volatility, and its correlation with the
rest of the portfolio.

**How to read them:** a holding at 10% of capital and 25% of risk is a hidden
concentration. Point out holdings whose risk share is much larger than their
weight.

### Efficient frontier and the risk/return map

**What it is:** the curve of the best average yearly return each level of
volatility could have had over the analysis period, mixing only the user's
current holdings. The map plots the portfolio, each holding and benchmarks
(SPY, QQQ, AGG, ACWI, a 60/40 mix) on the same axes. Returns here are
*average* yearly returns, a bit higher than CAGR for volatile assets, so don't
mix them with the CAGR figures elsewhere in the summary.

**How to read it:**
- The vertical gap between the portfolio and the curve at the same risk shows
  how much return the same holdings could have added in hindsight. A small gap
  means the mix was already close to efficient for its risk.
- Points to the right are riskier, points higher earned more. A benchmark above
  and to the left of the portfolio did better for less risk over this period.
- It's backward-looking and favors whatever did best. Describe it; never use
  it to propose weights.

## Downside risk (`downside_risk`)

All values in this tool's output are decimals shown as percentages, and loss
amounts are on $10,000 unless the user gave a portfolio value.

### Value at Risk (VaR)

**What it is:** historical, from this portfolio's own daily returns over the
analysis window (rebalanced daily to today's weights). The 1-day 95% VaR is
the return that the worst 5% of days fell to or below. The 1-month figure uses
every overlapping 21-trading-day stretch, not a scaled-up daily number.

**How to read it:** "on about one day in twenty, the portfolio lost 1.6% or
more". It is a threshold, not a worst case. Typical 1-day 95% VaR: about
-1.7% for the S&P 500 over the last 10 years; diversified 60/40 mixes are
lower, concentrated or tech-heavy portfolios higher.

**Breaches:** the share of days worse than the 95% VaR should be about 5% by
construction; it confirms the figure, nothing more.

**Bell-curve comparison:** `normalVar` is what a normal distribution with the
same mean and volatility would give. When the real 99% VaR is noticeably
worse, say the tail is fatter than a bell curve predicts.

### Expected shortfall (CVaR)

The average loss on the days (or months) beyond the VaR line. It answers
"when it's bad, how bad?", which VaR doesn't, so it's the better headline.
Neither is a worst case: the worst day and the past crises show what lies
beyond them.

### Past crises (stress scenarios)

Fixed windows from the S&P 500's peak to its trough (9/11 uses the week of
the attacks): dot-com bust, 9/11, 2008 financial crisis, 2011 debt ceiling and
downgrade, Q4 2018, COVID crash, 2022 inflation and rate shock, 2025 tariff
shock. Results are real total returns, daily-rebalanced to today's weights.

- **Return:** the portfolio's change from the start to the end of the window,
  next to the benchmark's.
- **Worst point:** the lowest value inside the window.
- **Recovered:** the first day after the low when the portfolio was back to
  its starting value, or "not yet".
- **Incomplete:** at least one holding wasn't trading for the whole window.
  There is no portfolio result; the holdings that did trade show their own
  returns. Klar never estimates the missing ones. A stand-in the user chooses
  replaces a holding one-for-one with its own real returns, and only in the
  crises the holding missed.

**Telling it:** for each complete crisis, say how the portfolio did against
the benchmark, how deep it went, how long it took to get back, and what in the
mix explains the difference. An incomplete crisis has no portfolio result;
don't infer one.

These show how today's mix would have fared then, not what will happen next.

## Valuation (`valuation`)

Today's ratios from Yahoo Finance, for the stock part of the portfolio: each
fund counts for its stock share; bond funds, cash and holdings without data are
left out and listed. Every metric reports its **coverage**, the share of the
portfolio it rests on.

**How the portfolio figure is built:**
- **P/E, forward P/E, P/B, P/S:** the harmonic weighted mean, as fund providers
  report it: average the yields (E/P, B/P, S/P) by weight, then invert. Two
  holdings at equal weight with P/Es of 20 and 30 give 24, not 25. A
  loss-making company keeps its negative earnings yield, so it raises the
  portfolio P/E instead of being dropped; if the stocks lose money overall
  the P/E is not meaningful.
- **Dividend yield, ROE, net margin, net income growth, debt/equity:** the
  weighted average of the holdings that report them.

**Reading the numbers:**
- **P/E** (price to the last 12 months' earnings). The S&P 500 has been around
  20-25 recently. Lower can mean cheaper, slower-growing or riskier; higher
  usually means growth is expected. Forward P/E uses analysts' estimates.
- **P/B** (price to book value) matters most for banks and asset-heavy firms;
  **P/S** (price to sales) is useful when earnings are thin or negative.
- **ROE** (profit on shareholders' equity) and **net margin** show quality;
  **net income growth** (last year, and the 3-year CAGR) shows momentum. Growth
  from a loss is "n/m".
- **Debt/equity**: 0.5x is half a dollar of debt per dollar of equity. Banks
  don't report it ("n/a").
- **Weight vs share of earnings:** a holding whose weight is well above its
  share of the portfolio's earnings is what you pay most for.

**Telling it:** compare each multiple with the benchmark in words ("about
half the benchmark's P/E"), then say why: which holdings are cheap or
expensive, whether the quality figures (ROE, margin, growth) justify the
price, and how leveraged the companies are. Mention coverage when it's under
100% and what was left out. These are today's ratios, not a forecast; a low
P/E isn't automatically a bargain.

**Data notes:** funds report only P/E, P/B, P/S and yield. For foreign listings
(e.g. ITUB), Yahoo's sales figure stays in the home currency, so their P/S is
left out.

## Factor analysis (Fama-French)

The regression explains the portfolio's excess return (return minus the T-bill
rate) as a combination of factor returns. Each **loading** says how much the
portfolio behaves like that factor.

| Factor | Name in output | Positive loading means | Negative loading means |
|---|---|---|---|
| Mkt-RF | market | more market sensitivity (≈ beta) | less market sensitivity |
| SMB | size | small-cap tilt | large-cap tilt |
| HML | value | value tilt (cheap, low price-to-book) | growth tilt |
| RMW | profitability (5-factor) | robust, profitable companies | weak profitability |
| CMA | investment (5-factor) | conservative investors (low asset growth) | aggressive investors |

**Models:**

- **3-factor:** market, size and value.
- **5-factor:** adds profitability and investment. HML often weakens in this
  model because RMW and CMA absorb part of it.

**Size of loadings:** about ±0.1 is negligible. About ±0.2-0.4 is a meaningful
tilt. Above ±0.5 is strong. A US large-cap blend fund is typically around
market 1.0, size -0.1 to -0.2, and value near 0.

### Significance (t-stat and p-value)

- **Threshold (Klar):** a loading is `significant` when p < 0.05, which is roughly
  |t| > 2.
- **Insignificant loadings:** don't describe them as a tilt, whatever the
  number. Say "no reliable size tilt".
- **Short samples:** fewer than about 36 monthly observations give wide error
  bars, so few loadings will be significant. Say that the sample is short.

### R² (and adjusted R²)

**What it is:** the share of return variation the factors explain.

| R² | Meaning |
|---|---|
| Above 0.95 | Behaves like a factor or index portfolio. SPY is about 0.995. |
| 0.8-0.95 | Mostly explained, with some stock-specific behavior |
| Below 0.8 | A lot is unexplained (concentrated picks, sectors, non-US, alternatives), so the loadings are less reliable |

Adjusted R² penalizes the extra factors. Use it when you compare models.

### Alpha (annualized)

**What it is:** the return left over after accounting for factor exposure.
Positive alpha means the portfolio did better than its factor exposure alone
would predict.

**How to read it:**

- Always report the sign and the significance together.
- Insignificant alpha, which is common, is statistically indistinguishable
  from zero.
- Significant positive alpha is rare and historical. It can reflect skill,
  luck, or a factor the model doesn't include. Berkshire (BRK-B) shows about
  +2.4%.
- **Index-like funds:** high R² plus insignificant or negative alpha means
  the fund has behaved like factor exposure that index funds also provide.
  This is an Auditor finding. Klar returns no fee data: say the fund's fee is
  worth checking, and don't quote an expense ratio from memory. Stay factual
  and don't push a switch.

### Return attribution

**What it is:** the return split into each factor's contribution (loading ×
average factor premium) plus alpha.

**How to read it:** say how much came from simply being in the market versus
tilts versus the unexplained residual. Factor premiums are averages over the
sample and vary a lot from decade to decade.
