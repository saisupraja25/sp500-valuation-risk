# 150 Years of S&P 500 Valuation and Risk

How much has the US stock market really returned, how bad do the losses get, and does starting valuation tell you anything about the next decade? This project answers those questions with 1,829 months of S&P 500 data (Jan 1871 – Jun 2023), using real (inflation-adjusted) total returns with dividends reinvested.

![Overview](outputs/01_overview.png)

## Key findings

| Measure | Result |
|---|---|
| Real total return per year | **6.9%** (nominal 9.2%, inflation 2.1%) |
| Annual volatility (real) | **14.2%** |
| Growth of $1 (real, by Jun 2023) | **$26,210** |
| Worst real drawdown | **-77%** (Oct 1929 to Jun 1932, recovered Nov 1936) |
| Best / worst decade (real per year) | 1950s 16.7% / 2000s -3.2% |
| Monthly VaR 99% / expected shortfall 99% | **-11.6%** / **-15.0%** |
| VaR 99% if returns were normal | -8.9% (understates the real tail) |
| Excess kurtosis | 18.05 (fat tails; 0 would be normal) |
| CAPE vs next-10-year real return | slope -0.41 pp per CAPE point, R² 0.271 |

### Valuation and future returns

![Valuation](outputs/02_valuation.png)

| CAPE quintile | CAPE range | Average next-10-year real return per year |
|---|---|---|
| Q1 cheapest | 4.8–11.1 | 10.9% |
| Q2 | 11.1–14.4 | 7.2% |
| Q3 | 14.4–17.3 | 6.6% |
| Q4 | 17.3–21.0 | 5.6% |
| Q5 richest | 21.0–44.2 | 3.0% |

Starting from the cheapest fifth of history, the next decade returned 10.9% a year after inflation; from the richest fifth, 3.0%. CAPE was 29.9 in Jun 2023, in the richest quintile. Valuation explains about 27% of the variation in 10-year returns: useful for setting long-run expectations, not for timing the market.

### Tail risk

![Tail risk](outputs/03_tail_risk.png)

A normal distribution with the same mean and volatility puts the 1-in-100 monthly loss at -8.9%; history says -11.6%, and when it is breached the average loss is -15.0%. Risk models that assume normal returns understate how bad bad months get.

### Worst real drawdowns

| Peak | Trough | Depth | Recovered | Months to recover |
|---|---|---|---|---|
| Oct 1929 | Jun 1932 | -77% | Nov 1936 | 85 |
| Sep 2000 | Mar 2009 | -52% | May 2013 | 152 |
| Feb 1973 | Dec 1974 | -50% | Jan 1985 | 143 |
| Mar 1937 | Apr 1942 | -48% | Apr 1945 | 97 |
| Dec 1916 | Dec 1920 | -47% | Aug 1924 | 92 |

## Method

- **Total return:** monthly price change plus one-twelfth of the annual dividend; real returns deflate by CPI.
- **Drawdowns:** real total-return index against its running peak; an episode ends when the previous peak is regained.
- **VaR / expected shortfall:** historical (non-parametric) quantiles of real monthly returns, compared with a parametric normal VaR.
- **CAPE test:** each month's CAPE against the annualised real total return over the following 120 months (overlapping windows), with a linear fit and quintile averages.

## Limitations

- Shiller's prices are monthly averages of daily closes, which smooths intra-month moves and slightly understates volatility and drawdowns.
- Overlapping 10-year windows are not independent, so the R² overstates the statistical certainty of the CAPE relationship.
- No taxes, fees or trading costs; the data ends in Jun 2023 because later months are not yet filled in the source.

## Run it

```bash
pip install -r requirements.txt
python analysis.py
```

Charts are written to `outputs/` and every number above to `outputs/results.json`.

## Data

Robert Shiller's monthly S&P 500 data, as packaged by [datasets/s-and-p-500](https://github.com/datasets/s-and-p-500) (Open Data Commons Public Domain Dedication and License, ODC-PDDL-1.0). A copy is in `data/sp500_shiller_monthly.csv`.

**Tools:** Python (pandas, NumPy, Matplotlib).
# sp500-valuation-risk
