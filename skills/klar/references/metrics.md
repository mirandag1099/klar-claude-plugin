# Klar metrics reference

This file explains each metric the Klar tools return: what it means in plain
language, how to read it, and what to tell the user. Thresholds marked
**(Klar)** are the cutoffs the Klar scoring code uses. Use them so your words
match the tool's labels. The other ranges are rules of thumb. Present them as
"typically", not as rules.

## Units first

- **`analyze_portfolio` values are already percentages.** `annualVolatility:
  14.0` is 14%, `maxDrawdown: -28.0` is a 28% decline, `cagr5Y: 11.0` is 11%
  a year, and holding `weight: 12.5` is 12.5%. Never multiply them by 100.
- **Ratios have no units.** Sharpe, Sortino, beta, correlation, factor
  loadings, t-stats and R² are plain numbers (R² can also be shown as a
  percentage).
- **Other tools:** follow the units the tool text states. In the raw factor
  output, alpha is a decimal (`0.024` is 2.4% a year). In Monte Carlo inputs,
  rates such as withdrawal and inflation are decimals (`0.04` is 4%), unless
  the tool's schema says otherwise.
- **Money amounts are nominal dollars** unless the tool labels them
  inflation-adjusted.

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

**Klar risk tolerance bands (Klar):** `check_goal` compares volatility to
these bands.

| Tolerance | Label | Volatility range |
|---|---|---|
| 1 | Very Conservative | 0-8% |
| 2 | Conservative | 5-12% |
| 3 | Moderate | 10-18% |
| 4 | Aggressive | 15-25% |
| 5 | Very Aggressive | 20-35% |

### Sharpe ratio

**What it is:** return above the risk-free rate per unit of total volatility.
It measures how much return you got for the swings you lived through. Klar
uses the 10-year Treasury yield as the risk-free rate, or 2% if that isn't
available.

**How to read it:** below 0 means the portfolio did worse than Treasuries,
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
such as a sector or single stocks. Rolling beta shows whether the sensitivity
is stable or drifting.

### Max drawdown

**What it is:** the largest peak-to-trough decline in the period. It's the
worst loss someone who bought at the top would have experienced.

**How to read it:** turn it into dollars. A -34% drawdown means $100,000 fell
to $66,000. Compare it to SPY's drawdown in the same window. Also note the
recovery: a -50% loss needs a +100% gain to break even. The question to ask is
whether the user could have held through it without selling.

**Use the "Worst drawdown" section, not guesses.** It gives the peak and low
dates, whether and when the portfolio recovered, SPY's move over the same
stretch, and each holding's move. Quote those facts.

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

**Price-only caveat:** figures exclude dividends and bond interest, so bond and
dividend funds' drawdowns look deeper than investors experienced (BND's 2022
decline was about -23% in price, about -18% including interest). Mention it
when bond or income funds matter to the conclusion.

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
| MOM | momentum (4-factor) | recent winners | recent losers |

**Models:**

- **CAPM:** market only.
- **3-factor:** market, size and value.
- **4-factor (Carhart):** adds momentum.
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
- **Fee check:** high R² plus insignificant or negative alpha in an expensive
  active fund means the user may be paying active fees for factor exposure
  they could get more cheaply. This is an Auditor finding. Stay factual and
  don't push a switch.

### Return attribution

**What it is:** the return split into each factor's contribution (loading ×
average factor premium) plus alpha.

**How to read it:** say how much came from simply being in the market versus
tilts versus the unexplained residual. Factor premiums are averages over the
sample and vary a lot from decade to decade.

## Goal score (`check_goal`)

**Overall score:** 0-100, a weighted average of the dimensions below **(Klar)**.
If Monte Carlo is unavailable, `goal_probability` is dropped and the other
weights are rescaled.

| Dimension | Weight | What it measures |
|---|---|---|
| `return_adequacy` | 25 | Historical CAGR vs the CAGR needed to reach the target from `initial_value` over the horizon (the required rate is lowered when contributions are counted). 100 if the historical rate is at least 1.2× the required rate. 80-100 if it is at least 1×. 30-80 if it is at least 0.5×. Below 30 otherwise. |
| `goal_probability` | 25 | The Monte Carlo probability of reaching the target, mapped directly (68% = 68). |
| `risk_alignment` | 20 | Portfolio volatility vs the risk-tolerance band in the volatility table above. Highest inside the band, lower the further outside it. |
| `diversification` | 20 | The average of three sub-scores (see below). |
| `drawdown_resilience` | 10 | Max drawdown vs a tolerance that grows with the horizon: `min(50%, 5% + 1.5% × years)`. A 20-year horizon tolerates 35%. |

**Diversification sub-scores:**

| Sub-score | Scale |
|---|---|
| Holding count | 1 holding = 10, 2-3 = 30, 4-6 = 50, 7-10 = 70, 11-15 = 85, 16+ = 100 |
| Top-3 concentration | under 30% = 100, under 50% = 80, under 70% = 55, under 90% = 30, otherwise 10 |
| Average pairwise correlation | under 0.3 = 100, under 0.5 = 75, under 0.7 = 50, under 0.85 = 25, otherwise 10 |

**Grades (Klar):**

| Grade | Score | Label |
|---|---|---|
| A | 90+ | Excellent |
| B | 75-89 | Good |
| C | 60-74 | Adequate |
| D | 45-59 | Below Average |
| F | below 45 | Poor |

**Dimension labels (Klar):** Strong (80+), Adequate (60-79), Challenging
(40-59), Unlikely (below 40). The goal probability has its own labels: Highly
Likely (above 80), Likely (above 60), Uncertain (above 40), Unlikely (above
20), Very Unlikely (otherwise).

**How to present it:**

1. Give the overall score and grade.
2. Name the weakest dimension and explain why, using its `detail` text.
3. Present the tool's recommendations as rule-based suggestions.

The score depends entirely on the inputs. Changing the horizon or contribution
can move it a lot, so offer to rerun it with different inputs.

## Monte Carlo (`simulate_future`)

**Method:** Klar simulates thousands of possible futures. Each one draws random
daily returns with the portfolio's historical average return and volatility,
applied month by month. Contributions are added monthly and grow with
inflation each year. Withdrawals are taken as a percentage of the balance.

**Limits to mention when they matter:**

- The draws assume normal, independent returns, so real crashes, fat tails and
  long bear markets are under-represented. That makes the weak-case
  percentiles somewhat optimistic.
- The results depend heavily on the historical window. A strong decade makes
  every percentile look better.
- Terminal values are in nominal dollars unless the tool says otherwise.
  Deflate them before saying "in today's money".

**How to read the outputs:**

- **Percentiles (p5, p10, p25, p50, p75, p90, p95):**
  - p50 (the median) is the middle outcome: half the paths ended above it and
    half below.
  - p10 is a weak-but-plausible case: 1 in 10 paths did worse.
  - p90 is a strong case.
  - Present the range (the p10-p90 band), never just the median, and never the
    mean, which large winners pull upward.
- **Fan chart:** shows the percentile paths spreading out over time. The
  widening band shows uncertainty growing with the horizon.
- **Probability of reaching the target (`probability_above_target`, or
  "success"):** the share of paths that ended at or above the target. Say "In
  X% of simulated paths…". Rough meaning: above 80% is comfortable, 60-80% is
  likely with real risk, 40-60% is a coin flip, and below 40% means the plan
  as specified probably falls short.
- **Probability of gain:** the share of paths that ended above the starting
  value.
- **Probability of ruin:** the share of paths that ran out of money. This
  matters only with withdrawals. Anything above about 5-10% at a given
  withdrawal rate is worth pointing out.
- **Sensitivity:** to show what changes the odds (contribution, horizon,
  withdrawal rate or allocation), offer to rerun the tool with the changed
  input, and rerun it when the user asks. Don't extrapolate.
