# Formulas

Every calculated figure on the hub, written as it will appear in the production data dictionary.

Every calculated figure on the hub is one of the formulas below. They are written here in the same form they will take in the production data dictionary.

## Bill Sentinel anomaly score (ACR-04)

```
for each bill, against the account’s other bills (robust statistics):
 z_rate     = |rate − median(rates)| / MAD(rates)
 z_units    = |deseasonalised units − median| / MAD
 z_demand   = |kW − median(kW)| / MAD(kW)
 z_variance = |(billed − tariff re-rating) / re-rating| / 0.04
squash(z)   = 1 − exp(−max(0, z − 2.2) / 2.5)      → each signal in 0..1
score       = 0.75·max(signals) + 0.25·mean(signals)
flagged when score ≥ 0.35; the strongest signal names the hypothesis
```

Deseasonalising divides each month by that calendar month’s median ratio to the account median, or by the NOAA weather model when it fits (R² ≥ 0.5).

## Weather normalisation (ACR-03 / 04)

```
fit units = a + b·HDD + c·CDD   over the account’s history
normalised units = units − b·(HDD − normal HDD) − c·(CDD − normal CDD)
```

## Benchmark gap (ACR-12)

```
effective rate = trailing-12-month cost ÷ trailing-12-month units
benchmark      = mean of last 12 EIA state commercial prices
gap            = effective ÷ benchmark − 1
gap in dollars = (effective − benchmark) × units
```

## Rate-pressure index, 0–100 (ACR-12)

```
pending  = Σ estimated % of open and future-effective rate cases   → clamp(pending / 10%)     weight 35
yoy      = TTM effective rate ÷ prior-12 effective rate − 1          → clamp((yoy + 2%) / 10%)  weight 25
volatility = stdev of monthly log rate changes × √12                → clamp(vol / 28%)        weight 20
market   = EIA state price, last month ÷ 13 months ago − 1           → scaled                  weight 20
index    = 100 × Σ(weight × scaled input)
```

This is the methodology stakeholders asked to see written down. It is modeled today and open to amendment before it is trusted.

## Rate forecast (ACR-03)

```
1. fit log(rate) = intercept + slope·t by least squares
2. drop points whose residual exceeds 3 × MAD, refit    billing anomalies do not steer the trend
3. add month-of-year seasonality
4. for N paths: apply fuel drift, inflation, rate-case steps, volatility shock
5. report P10 / P50 / P90 of rate and cost per month
```

## Tariff re-rating (ACR-02)

```
expected bill = customer charge
              + Σ energy blocks (units in block × block rate)
              + max(billed kW, ratchet % × 12-month peak) × demand rate
              + Σ TOU periods (share × units × period rate)
              + Σ riders
best fit = eligible tariff with the lowest 12-month re-rated total
```

