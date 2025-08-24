# Data_Science_Group6_Project

# PROJECT DESCRIPTION
TITLE: Monetary Tightening and Corporate Credit Risk: Analyzing Bond Yield Dynamics With Focus On Major World Disruptions & Across The COVID Divide (1996–2024)
DESCRIPTION: 
Against the backdrop of COVID disruptions, inflation, and the most rapid tightening cycle in decades, we study how U.S. Fed rate moves shape corporate bond yields and credit spreads over 1996–2024. We pay special attention to 2014–2019 (pre-COVID normalization), 2020–2021 (COVID shock and interventions), and 2022–2024 (post-pandemic tightening). Using transparent, data-driven models, our goal is to deliver clear, practical insight into how policy shifts transmit through the curve and risk sentiment to affect the risk premium investors require.

# DATA SOURCE
Federal Reserve Economic Data (FRED) – https://fred.stlouisfed.org
  ICE BofA US High Yield Index Effective Yield
    URL: https://fred.stlouisfed.org/series/BAMLH0A0HYM2EY 
  ICE BofA US Corporate Index Effective Yield 
    URL: https://fred.stlouisfed.org/series/BAMLC0A0CM 
  Effective Federal Funds Rate
    URL: https://fred.stlouisfed.org/series/FEDFUNDS 
We downloaded all series from FRED and made every series (variable) to the month-end for uniformity purposes. Daily market series such as HY, IG, VIX (VIXCLS) and NASDAQ (NASDAQCOM) were resampled to monthly. For those series that were reported monthly  such as FEDFUNDS, GS10/GS2/TB3MS, CPI, INDPRO, UNRATE etc

# SUMMARY OF METHOLOGY AND FINDINGS:
METHODOLOGY
Model 1: Credit-premium equation (ΔStatSpread). We estimate a linear OLS model with HAC (Newey–West) standard errors to handle heteroskedasticity and mild autocorrelation:
∆〖StatSpread〗_t=α+β_1 ∆〖FedFunds〗_t+β_2 ∆〖VIX〗_(t-1)+β_3 ∆〖Slope〗_(t-1)+ρ∆〖StatSpread〗_(t-1)+ε_t
We then run two robustness variants: (A) replace ΔFedFunds with ΔTargetMid; (B) include Fed×USREC to allow different policy effects in recessions; and a winsorized (1%) re-fit on the training data to check sensitivity to crisis outliers.

Model 2: HY yield equation (ΔHY, ECM). Because HY and IG are non-stationary but move together, we first confirm cointegration on the train sample: HY − (a + β·IG) residuals are stationary. We then estimate an Error-Correction Model:

∆〖HY〗_t=α+γ∆〖ECT〗_(t-1)+∅∆〖IG〗_t+β_FF ∆〖FedFunds〗_t+β_VIX ∆〖VIX〗_(t-1)+β_Slope ∆〖Slope〗_(t-1)+ρ∆〖HY〗_(t-1)+ε_t
where (+ ΔFedFunds_t as a small control; ECT_{t−1} = HY_{t−1} − (a + β·IG_{t−1}) enforces mean reversion toward the long-run HY–IG relationship. We use HAC errors here as well.

FINDINGS
RQ2 — Credit spreads (ΔStatSpread)

Answer to RQ2 (Do Fed moves change credit spreads month-to-month?)
•	When market fear rises, the extra yield on risky bonds (the “spread”) widens the next month. Fed rate changes, by themselves, do not move spreads much; their impact is small and depends on whether we’re in a recession or not.

RQ1 — HY yields (ΔHY, ECM)

Answer to RQ1 (Do Fed moves change HY yields month-to-month?)
•	High-yield bonds mostly move with safer corporate bonds and with market fear. After big moves, HY tends to drift back toward its usual gap with IG. The Fed’s direct month-to-month effect is modest once those forces are in play.

Credit markets behave a lot like people’s mood: when fear rises, investors demand extra compensation to hold risky bonds. We measure that fear with the VIX (a “nervousness index” drawn from stock-option prices). In our data, a jump in the VIX last month is followed by wider credit premia and higher HY yields this month. At the same time, HY does not move in isolation; it tends to follow investment-grade (IG) bonds because IG reacts more directly to the interest-rate backdrop. That “IG pass-through” means when IG yields go up, HY usually moves even more in the same direction right away, and then gradually drifts back toward its long-run relationship with IG.

Our findings suggest that interest rate policies interact with credit risk in complex, state-dependent ways. While Fed hikes generally compress spreads in stable periods, they exacerbate credit stress during recessions. For policymakers, this means communication strategies and timing of rate hikes are crucial to mitigating unintended consequences. For investors and risk managers, monitoring volatility (VIX) alongside Fed policy signals provides an early warning system for shifts in credit risk premiums. Future predictive modeling should integrate regime-switching frameworks to better anticipate turning points.

# TEAM MEMBER ROLES
# Vusimani Kubhayi (vukubhayi@osiriuniversity.org)
Project manager and GitHub lead
Create the GitHub repository. Add folders for notebooks, report assets, slides, and a README. Write a short progress note that summarizes data, methods, and first results. Share the repo link with the instructors. Set deadlines and confirm who owns each task.
# Oluchi Amaefule (oluchi@osiriuniversity.org)
Data ingestion and wrangling Lead
Load the raw series into one DataFrame with a proper date index. Fix data types. Remove duplicate dates. Handle missing values with forward fill where appropriate and drop rows only when features cannot be created. Save a clean CSV and keep the code that reproduces it.
# Christian Ngozi Ahiauzu (ngozi@osiriuniversity.org)
Feature engineering and Github Lead
Create changes for HY, IG, and other drivers. Create lags for VIX, CPI, and rates that are known at the time of forecasting. Build the credit spread and the yield curve slope. Build the cointegration weighted change spread using the one point two weight. Keep a clear list of feature names for modeling. Repo structure, README, code style, PR reviews.
# Adenike Ayoola (nikeayoola@osiriuniversity.org)
EDA and visualization Lead
Produce descriptive statistics for the main variables. Make histograms, boxplots, scatter plots, and a correlation heatmap. Make time series plots for HY, IG, spread, VIX, CPI, and the curve slope. Create a pairplot. Save all figures to the report assets folder and add short captions.
# Tadesse Alemu (tadakelatie@osiriuniversity.org)
Modeling and scenarios Lead
Split the data into training up to December two thousand sixteen and testing from two thousand seventeen. Fit the OLS model with robust errors and the SARIMAX model with exogenous variables. Report RMSE, MAE, and directional accuracy. Build the one step scenario tool for context and isolated cases. Save model outputs and charts.
# Adanna Henri-Ukoha (ahenriukoha@osiriuniversity.org)
Robustness and diagnostics Lead
Run stationarity tests on levels and changes using ADF and KPSS. Test cointegration between HY and IG using Engle Granger and Johansen. Build the cointegration weighted spread and refit the spread models. Check residual autocorrelation and note results. Prepare a small comparison table that shows the Fed effect, confidence intervals, and errors for plain spread and weighted spread. Write a short summary of findings.
# Ifeoma Nwaeze (ifeoma@osiriuniversity.org)
Storytelling, slides, and report
Update the slide deck with the headline results, the HY and spread scenario charts, the stationarity slide, and the cointegration slide. Keep text short and charts large. Write a two page summary that covers question, data, method, key results with confidence intervals, limits, and takeaways. Add team roles to Slide one and to the README.



