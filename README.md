Random-Walk Forecasting: A Cheat Sheet
The random-walk (RW) null concerns the underlying daily, point-sampled price series (e.g., stocks or commodities)—not monthly averages constructed from it. Test forecasts of averages against the RW implied by the daily data.
1. Daily prices and point-sampled returns
Let $P_d$ be the price at the end of day $d$. For a driftless RW in price levels:
$$
P_{d+1}=P_d+\varepsilon_{d+1}, \qquad E[\varepsilon_{d+1}\mid\mathcal F_d]=0.
$$
Forecast the price level:
$$
\boxed{\widehat P_{d+1\mid d}^{RW}=P_d}
$$
Forecast the exact return, $g_{d+1}=P_{d+1}/P_d-1$:
$$
\boxed{\widehat g_{d+1\mid d}^{RW}=0}
$$
Forecast the log return, $r_{d+1}=\ln P_{d+1}-\ln P_d$: zero is the benchmark for a driftless RW in log prices (a different null).
$$
\boxed{\widehat r_{d+1\mid d}^{RW}=0}
$$
Daily target	RW forecast	RW forecast error
Level $P_{d+1}$	$P_d$	$P_{d+1}-P_d$
Exact return $g_{d+1}$	$0$	$g_{d+1}$
Log return $r_{d+1}$ (log-price RW)	$0$	$r_{d+1}$
Example: If today's price is $100, the price-level RW forecasts $100 tomorrow and a 0% exact return. The same rules apply to point-sampled end-of-month (EOM) prices and returns.
2. What should I forecast and evaluate?
Application	Forecast target	Evaluate
Finance / trading	Point-sampled returns (daily or EOM)	Return forecast errors
Economics / commodity prices	Monthly-average price levels	Level forecast errors
Averages estimated using a growth transformation	Reconstruct monthly-average levels	Level forecast errors
For financial returns, use the economically relevant price or payoff series (e.g., account for dividends or futures rolls where appropriate). Average-to-average returns are usually a transformation, not the economic object of interest.
3. Monthly averages: forecast levels
Let $\bar P_t$ be the average daily price during month $t$ and $P_t^{EOM}$ its last daily price. Forecasting month $t+1$ from the end of month $t$, the daily RW implies (Ellwanger & Snudden, 2023):
$$
\boxed{\widehat{\bar P}_{t+1\mid t}^{RW}=P_t^{EOM}}
$$
> [!WARNING]
> **Not $\bar P_t$!** The RW forecast uses the **last daily price**, not the previous month's average. Using $\bar P_t$ creates a **straw-man benchmark** and apparent predictability even when daily prices follow a RW.
Monthly-average target	RW forecast	RW forecast error
Level $\bar P_{t+1}$	$P_t^{EOM}$	$\bar P_{t+1}-P_t^{EOM}$
Example: If $\bar P_t=95$ and $P_t^{EOM}=100$, the RW forecasts next month's average at 100, not 95.
4. Monthly averages: growth-rate transformations
For an average-to-average growth outcome, the daily RW benchmark is not zero. Use the matching forecast and error (Ellwanger & Snudden, 2023):
Growth target	RW forecast	RW forecast error
Exact, $g_{t+1}^{AA}=\bar P_{t+1}/\bar P_t-1$	$P_t^{EOM}/\bar P_t-1$	$(\bar P_{t+1}-P_t^{EOM})/\bar P_t$
Log, $r_{t+1}^{AA}=\ln\bar P_{t+1}-\ln\bar P_t$	$\ln(P_t^{EOM}/\bar P_t)$	$\ln(\bar P_{t+1}/P_t^{EOM})$
Preferred exact transformation, $z_{t+1}=\bar P_{t+1}/P_t^{EOM}-1$	$0$	$\bar P_{t+1}/P_t^{EOM}-1$
If a regression needs a stationary growth-rate outcome, prefer exact growth relative to EOM:
$$
\boxed{z_{t+1}=\frac{\bar P_{t+1}}{P_t^{EOM}}-1,\qquad \widehat z_{t+1\mid t}^{RW}=0}
$$
Convert the model forecast back to a level before evaluation:
$$
\boxed{\widehat{\bar P}{t+1\mid t}=P_t^{EOM}(1+\widehat z{t+1\mid t})}
$$
In the example above, the daily RW implies 5.26% average-to-average growth ($100/95-1$), not 0%—without any predictability in daily price changes.
5. Directional accuracy: compare against the same last daily price
For a candidate model, compare the sign of its forecasted change with the sign of the realized change from the last point-sampled observation (McCarthy & Snudden, Applied Economics, 2025).
Forecast target	Predicted direction	Realized direction
Daily / EOM level	$\operatorname{sgn}(\widehat P_{d+1\mid d}-P_d)$	$\operatorname{sgn}(P_{d+1}-P_d)$
Next monthly-average level	$\operatorname{sgn}(\widehat{\bar P}_{t+1\mid t}-P_t^{EOM})$	$\operatorname{sgn}(\bar P_{t+1}-P_t^{EOM})$
EOM-to-average exact growth $z$	$\operatorname{sgn}(\widehat z_{t+1\mid t})$	$\operatorname{sgn}(z_{t+1})$
Under the daily RW null, a candidate model has an expected success ratio of 50% under the standard conditions. Measuring direction relative to $\bar P_t$ instead can yield spuriously high success ratios and false rejections even under a daily RW (McCarthy & Snudden, 2025).
6. Estimating forecasts of averages
Avoid defaulting to lagged average-to-average or EOM returns on the right-hand side of averaged-price regressions: averaging creates artificial persistence, while EOM-only data discard within-month information. Where possible, use daily data via BUMIDAS or bottom-up forecasting (Benmoussa, Ellwanger & Snudden, 2026).
7. Common mistakes: what's wrong with these equations?
① A no-change forecast of monthly averages:
$$
\widehat{\bar P}_{t+1\mid t}=\bar P_t
$$
Actually assumes: a RW in monthly averages, not daily prices. Problem: straw-man benchmark; creates spurious forecast gains against the daily RW.
② Zero growth between monthly averages:
$$
\widehat g_{t+1\mid t}^{AA}=0
$$
Actually assumes: no change in successive averages. Problem: straw-man benchmark; the daily RW predicts $P_t^{EOM}/\bar P_t-1$, generally nonzero.
③ No change in returns:
$$
\widehat r_{t+1\mid t}=r_t
$$
Actually assumes: returns themselves follow a RW. Problem: straw-man benchmark for a RW in prices; a price RW does not imply $\widehat r=r_t$.
④ Rejecting the daily RW because average changes are autocorrelated:
$$
g_{t+1}^{AA}=\alpha+\beta g_t^{AA}+u_{t+1},\qquad \beta\ne 0\ \Rightarrow\ \text{reject daily RW}
$$
Actually tests: dependence in averaged changes. Problem: spurious predictability—temporal averaging induces serial dependence even under a daily RW (Working, 1960).
⑤ Measuring the direction of an average relative to the previous average:
$$
\operatorname{sgn}(\bar P_{t+1}-\bar P_t)
$$
Actually measures: average-to-average direction. Problem: spurious directional accuracy under a daily RW; instead use $\operatorname{sgn}(\bar P_{t+1}-P_t^{EOM})$ (McCarthy & Snudden, 2025).
Timing: Only use observations available at the forecast origin; $P_t^{EOM}$ must be the last available daily price and consistent with the averaged series. If it is unavailable, monthly averages alone cannot construct this daily-RW benchmark.
References
Working (1960), Econometrica, 28(4), 916–918.
Ellwanger & Snudden (2023), Journal of Banking & Finance, 154, 106962.
McCarthy & Snudden (2025), Applied Economics, published online October 16, 2025.
Benmoussa, Ellwanger & Snudden (2026), International Journal of Forecasting, 42(1), 281–295.
