# Random-Walk Forecasting: A Cheat Sheet

**The random-walk (RW) null concerns the underlying daily, point-sampled price series** (e.g., stocks or commodities)—**not** averages constructed from it. Test forecasts of averages against the RW implied by the **daily** data.

## 1. Daily prices and point-sampled returns

Let $`P_d`$ be the price at the end of day $`d`$. For a **driftless RW in price levels**:

```math
P_{d+1}=P_d+\varepsilon_{d+1}, \qquad E[\varepsilon_{d+1}\mid\mathcal F_d]=0.
```

**Forecast the price level:**

```math
\boxed{\widehat P_{d+1\mid d}^{RW}=P_d}
```

**Forecast the exact return**, $`g_{d+1}=P_{d+1}/P_d-1`$:

```math
\boxed{\widehat g_{d+1\mid d}^{RW}=0}
```

**Forecast the log return**, $`r_{d+1}=\ln P_{d+1}-\ln P_d`$: 

```math
\boxed{\widehat r_{d+1\mid d}^{RW}=0}
```
**Note:** A zero expected log return requires a driftless random walk in log prices, not price levels.

| Daily target | RW forecast | RW forecast error |
|---|---|---|
| Level $`P_{d+1}`$ | $`P_d`$ | $`P_{d+1}-P_d`$ |
| Exact return $`g_{d+1}`$ | $`0`$ | $`g_{d+1}`$ |
| Log return $`r_{d+1}`$ (log-price RW) | $`0`$ | $`r_{d+1}`$ |

*Example:* If today's price is 100, the price-level RW forecasts **100 tomorrow** and a **0% exact return**. 

**Note:** The same rules apply to point-sampled **end-of-month (EOM)** prices and returns.

## 2. What should I forecast and evaluate?

| Application | Forecast target | Evaluate |
|---|---|---|
| **Finance / trading** | **Point-sampled returns** (daily or end-of-period) | Return forecast errors |
| **Economics** | **Period-average price levels** | Level forecast errors |
| **Averages modeled using a growth transformation** | Reconstruct **period-average levels** | Level forecast errors |

- For financial returns:
  - **Use the economically relevant price or payoff series** (e.g., account for dividends or futures rolls where appropriate).
  - **Better to evaluate profitability rather than error** using a trading strategy net of transaction costs and report directional accuracy, including positive/negative and extreme/non-extreme movements ([Arain & Snudden, 2026](https://doi.org/10.1002/for.70077)).

> [!WARNING]
> **Average-to-average returns are rarely the object of interest in finance.**  Moreover, on the RHS they are inefficient and can create apparent predictability.

## 3. Monthly averages: forecast levels

Let $`\bar P_t`$ be the **average daily price** during month $`t`$ and $`P_t^{EOM}`$ its **last daily price**. Forecasting month $`t+1`$ from the end of month $`t`$, the **daily RW** implies ([Ellwanger & Snudden, 2023](https://doi.org/10.1016/j.jbankfin.2023.106962)):

```math
\boxed{\widehat{\bar P}_{t+1\mid t}^{RW}=P_t^{EOM}}
```

> [!WARNING]
> **Not the previous monthly average,** $`\bar P_t`$! The RW forecast uses the **last daily price**. Using $`\bar P_t`$ creates a **straw-man benchmark** and is predictable by construction when daily prices follow a RW.

| Monthly-average target | RW forecast | RW forecast error |
|---|---|---|
| Level $`\bar P_{t+1}`$ | $`P_t^{EOM}`$ | $`\bar P_{t+1}-P_t^{EOM}`$ |

*Example:* If $`\bar P_t=95`$ and $`P_t^{EOM}=100`$, the RW forecasts **next month's average at 100**, not 95.

## 4. Monthly averages: growth-rate transformations

For **average-to-average growth**, the daily RW benchmark is **not zero**. Use the matching forecast and error ([Ellwanger & Snudden, 2023](https://doi.org/10.1016/j.jbankfin.2023.106962)):

| Growth target | RW forecast | RW forecast error |
|---|---|---|
| Exact, $`g_{t+1}^{AA}=\bar P_{t+1}/\bar P_t-1`$ | $`P_t^{EOM}/\bar P_t-1`$ | $`(\bar P_{t+1}-P_t^{EOM})/\bar P_t`$ |
| Log, $`r_{t+1}^{AA}=\ln\bar P_{t+1}-\ln\bar P_t`$ | $`\ln(P_t^{EOM}/\bar P_t)`$ | $`\ln(\bar P_{t+1}/P_t^{EOM})`$ |
| **Preferred exact transformation**, $`z_{t+1}=\bar P_{t+1}/P_t^{EOM}-1`$ | **$`0`$** | $`z_{t+1}`$ |

**Note:** The exact rows assume a driftless RW in price levels. Log rows are exact for a driftless RW in log prices when $`\bar P_t`$ is the geometric average ($`\ln\bar P_t`$ equals the average daily log price); with arithmetic averages they hold to first order.

If a regression needs a growth-rate transformation, **use growth relative to EOM** ([Benmoussa, Ellwanger & Snudden, 2026](https://doi.org/10.1016/j.ijforecast.2025.02.009)):

```math
\boxed{z_{t+1}=\frac{\bar P_{t+1}}{P_t^{EOM}}-1,\qquad \widehat z_{t+1\mid t}^{RW}=0}
```

Convert the model forecast **back to a level** before evaluation:

```math
\boxed{\widehat{\bar P}_{t+1\mid t}=P_t^{EOM}(1+\widehat z_{t+1\mid t})}
```

In the example above, the daily RW implies **5.26% average-to-average growth** (100/95 − 1), **not 0%**—without any predictability in daily price changes.

### Multi-step horizons

Forecasting month $`t+h`$ from the end of month $`t`$, the daily RW forecast of **every** future monthly average is $`P_t^{EOM}`$:

| Target at horizon $`h`$ | RW forecast |
|---|---|
| Level $`\bar P_{t+h}`$ | $`P_t^{EOM}`$ |
| Cumulative growth $`\bar P_{t+h}/\bar P_t-1`$ | $`P_t^{EOM}/\bar P_t-1`$ |
| $`z_{t+h}=\bar P_{t+h}/P_t^{EOM}-1`$ | $`0`$ |
| Month-over-month log growth $`r_{t+h}^{AA}`$ | $`\ln(P_t^{EOM}/\bar P_t)`$ at $`h=1`$; $`0`$ at $`h\ge2`$ |

The EOM gap enters month-over-month growth at $`h=1`$ only; carrying it into later months is an error. For direction at horizon $`h`$, use $`\mathrm{sgn}(\bar P_{t+h}-P_t^{EOM})`$.

## 5. Directional accuracy: compare against the same last daily price

For a candidate model, compare the sign of its forecasted change with that of the **realized change from the last point-sampled observation** ([McCarthy & Snudden, 2025](https://doi.org/10.1080/00036846.2025.2564464)).

| Forecast target | Predicted direction | Realized direction |
|---|---|---|
| **Daily / EOM level** | $`\mathrm{sgn}(\widehat P_{d+1\mid d}-P_d)`$ | $`\mathrm{sgn}(P_{d+1}-P_d)`$ |
| **Next monthly-average level** | $`\mathrm{sgn}(\widehat{\bar P}_{t+1\mid t}-P_t^{EOM})`$ | $`\mathrm{sgn}(\bar P_{t+1}-P_t^{EOM})`$ |
| **Average-to-EOM exact growth** $`z`$ | $`\mathrm{sgn}(\widehat z_{t+1\mid t})`$ | $`\mathrm{sgn}(z_{t+1})`$ |

Under the daily RW null, a non-informative *up/down* prediction has an **expected success ratio of 50%** under standard conditions. Measuring direction relative to $`\bar P_t`$ instead can yield **spuriously high success ratios** and false rejections, under a daily RW ([McCarthy & Snudden, 2025](https://doi.org/10.1080/00036846.2025.2564464)).

## 6. Estimating forecasts of averages

- **Avoid defaulting to lagged average-to-average or EOM returns on the right-hand side** of averaged-price regressions: averaging creates artificial persistence ([Working, 1960](https://doi.org/10.2307/1907574)), while EOM-only data discard within-month information and will be inefficient when the daily process has AR order above one, such as a daily VAR(p) with p > 1.

- Where possible, **use daily data** via **BUMIDAS** ([Lee & Snudden, 2026](http://dx.doi.org/10.2139/ssrn.5312038)) or **bottom-up forecasting** ([Benmoussa, Ellwanger & Snudden, 2026](https://doi.org/10.1016/j.ijforecast.2025.02.009)).


## 7. Common mistakes: what's wrong with these equations?

Figures below assume a driftless daily RW with $`n=21`$ trading days per month; quarterly values ($`n\approx63`$) are slightly larger.

**1. A no-change forecast of monthly averages**

```math
\widehat{\bar P}_{t+1\mid t}=\bar P_t
```

**Actually assumes:** a RW in **monthly averages**, not daily prices. **Problem:** *straw-man benchmark*; spurious forecast gains when a daily RW. The EOM benchmark has about **46% lower MSPE**: $`\mathrm{MSPE}_{EOM}/\mathrm{MSPE}_{\bar P_t}=(n+1)(2n+1)/(2(2n^2+1))\approx0.54`$.

**2. Zero growth between monthly averages**

```math
\widehat g_{t+1\mid t}^{AA}=0
```

**Actually assumes:** no change in successive averages. **Problem:** *straw-man benchmark* with spurious forecast gains when a daily RW; the correct daily RW is $`P_t^{EOM}/\bar P_t-1`$. Same MSPE penalty as Mistake 1: both forecast errors are the Mistake 1 errors divided by $`\bar P_t`$.

**3. No change in returns**

```math
\widehat r_{t+1\mid t}=r_t
```

**Actually assumes:** returns themselves follow a RW. **Problem:** *straw-man benchmark* for a RW in prices as this is **VERY** easy to beat: it doubles the MSPE of the zero-return forecast for point-sampled returns, and for average-to-average returns it has about **2.8 times** the MSPE of the EOM benchmark.

**4. Measuring the direction of an average relative to the previous average**

```math
\mathrm{sgn}(\bar P_{t+1}-\bar P_t)
```

**Actually measures:** average-to-average direction. **Problem:** *spurious directional accuracy* under a daily RW: the RW's own call, $`\mathrm{sgn}(P_t^{EOM}-\bar P_t)`$, is right about **74%** of the time. Instead use $`\mathrm{sgn}(\bar P_{t+1}-P_t^{EOM})`$ ([McCarthy & Snudden, 2025](https://doi.org/10.1080/00036846.2025.2564464)).

**Timing:** Only use observations available at the forecast origin. $`P_t^{EOM}`$ must be the **last available daily price** and consistent with the averaged series. If it is unavailable, monthly averages alone cannot construct this daily-RW benchmark: the optimal forecast from averages alone improves on the AA no-change by only about 7% in MSPE, versus 46% for EOM, which makes the case for daily data.

**5. Rejecting the daily RW because average changes are predictable**

```math
g_{t+1}^{AA}=\alpha+\beta g_t^{AA}+u_{t+1},\qquad \beta\ne0
```

**Problem 1:** Averaging creates serial correlation even under a daily RW, so under the null $`\beta=(n^2-1)/(2(2n^2+1))\approx0.25`$ ([Working, 1960](https://doi.org/10.2307/1907574)). A significant $`\beta`$ is *spurious predictability*, not evidence against the daily RW. 

**Problem 2:** Other predictors of average returns can inherit the bias; for oil, the predictability vanishes with EOM prices ([Conlon, Cotter & Eyiah-Donkor, 2022](https://doi.org/10.1016/j.jbankfin.2021.106331)).

**Traditional fix:** EOM sampling, which avoids both problems but stops forecasting the average.

**Key insight 1 ([Ellwanger & Snudden, 2023](https://doi.org/10.1016/j.jbankfin.2023.106962)):** Keep forecasting the average (levels or growth rates), but test against the **last daily price**. The test above omits the EOM gap, which the daily RW says predicts average growth one-for-one; once it is included, lagged growth adds nothing:

```math
g_{t+1}^{AA}=\alpha+\beta_1\left(\frac{P_t^{EOM}}{\bar P_t}-1\right)+\beta_2 g_t^{AA}+u_{t+1},\qquad H_0:\ \alpha=0,\ \beta_1=1,\ \beta_2=0
```

**Key insight 2 ([Benmoussa, Ellwanger & Snudden, 2026](https://doi.org/10.1016/j.ijforecast.2025.02.009)):** Model growth relative to EOM ($`z_{t+1}`$), which imposes $`\beta_1=1`$. Any predictor known at the end of month $`t`$ can then replace $`z_t`$ without spurious predictability:

```math
z_{t+1}=\alpha+\beta z_t+u_{t+1},\qquad H_0:\ \alpha=\beta=0
```

### References

- [Working (1960)](https://doi.org/10.2307/1907574), *Econometrica*, 28(4), 916–918.
- [Ellwanger & Snudden (2023)](https://doi.org/10.1016/j.jbankfin.2023.106962), *Journal of Banking & Finance*, 154, 106962.
- [McCarthy & Snudden (2025)](https://doi.org/10.1080/00036846.2025.2564464), *Applied Economics*, published online October 16, 2025.
- [Benmoussa, Ellwanger & Snudden (2026)](https://doi.org/10.1016/j.ijforecast.2025.02.009), *International Journal of Forecasting*, 42(1), 281–295.
- [Lee & Snudden (2026)](http://dx.doi.org/10.2139/ssrn.5312038), *Available at SSRN:* [http://dx.doi.org/10.2139/ssrn.5312038](http://dx.doi.org/10.2139/ssrn.5312038).
- [Arain & Snudden (2026)](https://doi.org/10.1002/for.70077), *Journal of Forecasting*, 45(3), 1245–1260.
- [Conlon, Cotter & Eyiah-Donkor (2022)](https://doi.org/10.1016/j.jbankfin.2021.106331), *Journal of Banking & Finance*, 134, 106331.
