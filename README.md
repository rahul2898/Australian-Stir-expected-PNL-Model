# Probabilistic Expected PnL Model for STIR Futures

> A quantitative framework for estimating the expected profit and loss (PnL) of a Short-Term Interest Rate (STIR) futures portfolio by modelling alternative central bank meeting premium repricing scenarios before major macroeconomic events.

---

## Project Overview

Major macroeconomic releases such as CPI, labour market reports, inflation data, GDP releases, and central bank communications frequently trigger significant repricing of interest rate expectations. Rather than modelling the policy decision itself, this project models the **repricing of central bank meeting premiums** implied by STIR futures.

The objective is to estimate the expected profit and loss (PnL) of a STIR futures portfolio by evaluating multiple user-defined meeting premium scenarios and assigning statistically derived probabilities to each scenario based on historical market behaviour.

The framework combines:

- Portfolio positions across STIR futures contracts
- A contract-to-meeting impact matrix
- User-defined meeting premium scenarios
- Historical meeting premium observations
- Multivariate normal probability estimation

The resulting expected PnL provides a probabilistic assessment of portfolio performance before major macroeconomic events, allowing traders to quantify portfolio risk under alternative market repricing scenarios.

---

## Motivation

Prior to major economic releases, traders routinely evaluate how market expectations for future central bank policy may change. The largest source of portfolio risk is often not the realised policy decision itself, but the **repricing of central bank meeting premiums** that occurs as new economic information is incorporated into market expectations.

Traditional scenario analysis typically assigns equal probability to every scenario or relies on subjective judgement. This project replaces those assumptions with a statistical framework that estimates scenario probabilities using the historical joint distribution of meeting premiums.

By combining statistically derived scenario probabilities with portfolio exposures, the model produces an expected PnL that provides a more objective measure of potential portfolio performance and risk before significant macroeconomic events.

---

## Methodology

The model estimates the expected profit and loss (PnL) of a STIR futures portfolio by combining portfolio exposures, user-defined meeting premium scenarios, and statistically inferred scenario probabilities.

### 1. Contract-to-Meeting Impact Matrix

Each STIR futures contract is exposed to one or more future central bank meetings. An impact matrix is constructed to quantify the sensitivity of every contract to changes in each meeting premium based on the contract's expiry date and the timing of future meetings.

This impact matrix transforms portfolio positions into meeting-level exposures.

### 2. Portfolio Exposure

Portfolio exposure is calculated by multiplying the portfolio position vector by the contract-to-meeting impact matrix.

**Portfolio Exposure = Portfolio Positions × Impact Matrix**

This produces the portfolio's effective exposure to every meeting premium.

### 3. Scenario PnL

For every user-defined scenario, the projected portfolio PnL is calculated by multiplying the meeting exposure vector by the scenario's meeting premium changes and scaling the result by the portfolio DV01.

**Scenario PnL = −DV01 × (Portfolio Exposure · Scenario Premium Changes)**

### 4. Statistical Scenario Probabilities

When historical meeting premium data is available, the model estimates the probability of each scenario by fitting a multivariate normal distribution to historical observations.

Each scenario receives a probability based on its likelihood under the historical distribution, after which all probabilities are normalised so that they sum to 100%.

### 5. Expected Portfolio PnL

The expected portfolio PnL is calculated as the probability-weighted average of the PnL generated under each scenario.

**Expected Portfolio PnL = Σ (Scenario Probability × Scenario PnL)**

This framework provides an objective estimate of portfolio risk by combining historical market behaviour with user-defined market scenarios.

---

## Workflow Diagram

![Project Workflow](figures/Workflow.png)

---

## Features

- Automatically maps STIR futures contracts to future central bank meetings using a contract-to-meeting impact matrix.
- Calculates portfolio exposure to each central bank meeting based on user-defined positions.
- Supports multiple market repricing scenarios through user-defined meeting premium changes.
- Computes scenario-specific portfolio PnL using portfolio DV01 and meeting-level exposures.
- Estimates scenario probabilities using a multivariate normal distribution fitted to historical meeting premium observations.
- Supports equal-weight scenario analysis when historical data is unavailable.
- Produces a probability-weighted expected portfolio PnL for risk assessment before major macroeconomic events.

---

## Applications

This model is intended for pre-event portfolio risk analysis in Short-Term Interest Rate (STIR) futures markets. Rather than forecasting the outcome of an economic release or monetary policy decision, it evaluates the portfolio impact of alternative meeting premium repricing scenarios.

Typical applications include:

- Portfolio risk assessment before CPI releases.
- Labour market data announcements.
- Inflation releases.
- GDP announcements.
- Central bank speeches and policy guidance.
- Internal stress testing using custom meeting premium scenarios.
- Comparing expected portfolio outcomes under different market pricing assumptions.

---

## Repository Structure

```
Australian-Stir-expected-PNL-Model/
│
├── STIR-Expected-PNL-Model.ipynb   # Complete project implementation
├── README.md                       # Project documentation
├── LICENSE                         # MIT License
├── src/                            # Source code (under development)
└── data/                           # Sample datasets
```

---

## Installation

Clone the repository

```bash
git clone https://github.com/<your-username>/Australian-Stir-expected-PNL-Model.git
```

Install the required Python libraries

```bash
pip install pandas numpy scipy openpyxl
```

Open the notebook in Jupyter Notebook or Google Colab and execute the cells sequentially.

---

## Workflow

1. Upload the portfolio positions file.
2. Generate the contract-to-meeting impact matrix.
3. Upload meeting premium scenarios.
4. *(Optional)* Upload historical meeting premium data.
5. Estimate scenario probabilities using a multivariate normal distribution.
6. Calculate the PnL for each scenario.
7. Compute the probability-weighted expected portfolio PnL.

---

## Example Usage

### Required Inputs

The model requires the following input files:

| File | Description |
|------|-------------|
| Positions | Portfolio positions across STIR futures contracts |
| Scenario Premiums | User-defined meeting premium scenarios together with current market premiums |
| Historical Meeting Premiums *(Optional)* | Historical meeting premium observations used to estimate scenario probabilities |

If historical meeting premium data is not provided, the model assigns equal probability to every scenario.

---

## Example Output

For each user-defined scenario, the model produces:

- Scenario probability
- Scenario-specific portfolio PnL
- Probability-weighted expected portfolio PnL

Example:

| Scenario | Probability | Portfolio PnL |
|----------|------------:|--------------:|
| Hawkish | 42.3% | $18,450 |
| Neutral | 37.8% | $4,120 |
| Dovish | 19.9% | -$12,760 |

**Expected Portfolio PnL = $6,973**

*Illustrative example only.*

---

## Model Assumptions

The current implementation makes the following assumptions:

- Historical meeting premium changes are representative of future market behaviour.
- Scenario probabilities are estimated using a multivariate normal distribution.
- Portfolio DV01 remains constant across scenarios.
- User-defined meeting premium scenarios sufficiently capture the range of plausible market repricing outcomes.

---

## Future Improvements

Potential extensions include:

- Student's t-distribution and Gaussian mixture models for scenario probability estimation.
- Monte Carlo simulation for scenario generation.
- Historical backtesting of expected PnL estimates.
- Interactive dashboard using Streamlit.
- Automated market data ingestion.
- Sensitivity analysis for portfolio DV01 and meeting exposures.
- Support for additional STIR markets and central bank curves.

---

## Disclaimer

This project was developed as part of my quantitative trading workflow for analysing STIR futures portfolios. It is designed to support scenario analysis by estimating the expected portfolio PnL under alternative central bank meeting premium repricing scenarios prior to major macroeconomic events.

The implementation presented in this repository demonstrates the underlying quantitative methodology. Certain inputs, assumptions, and datasets have been simplified or anonymised for public release.
