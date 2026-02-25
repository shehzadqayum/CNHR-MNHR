# CNHR–MNHR Model Formal Specification

## Cost-Normalised Hourly Rate & Marginal Net Hourly Rate

### Integrated Mathematical, Statistical & Operational Framework for Rideshare Driver Performance Analytics

| | |
|---|---|
| **Date:** | 23 February 2026 |
| **Author:** | Shehzad Qayum |
| **Dataset:** | 635+ trips, 12 Uber weeks |
| **Period:** | 7 December 2025 – 16 February 2026 |
| **Platform:** | Uber (London, UK) |
| **Vehicle:** | Kia e-Niro EV (PHV) |

*Document classification: Operational Specification & Analytical Reference*

---

## Table of Contents

- [Abstract](#abstract)
- [Part I: Foundations](#part-i-foundations)
  - [1. Introduction](#1-introduction)
  - [2. Axiomatic Foundations](#2-axiomatic-foundations)
  - [3. Primary State Variables](#3-primary-state-variables)
- [Part II: Core Metrics](#part-ii-core-metrics)
  - [4. Gross Hourly Rate (ϱ)](#4-gross-hourly-rate-ϱ)
  - [5. Cost-Normalised Hourly Rate (CNHR)](#5-cost-normalised-hourly-rate-cnhr)
  - [6. The Cost Rate (λ)](#6-the-cost-rate-λ)
  - [7. Marginal Net Hourly Rate (MNHR)](#7-marginal-net-hourly-rate-mnhr)
  - [8. Arithmetic and Geometric Means](#8-arithmetic-and-geometric-means)
- [Part III: Enroute and Deadhead Integration](#part-iii-enroute-and-deadhead-integration)
  - [9. The Unpaid Time Problem](#9-the-unpaid-time-problem)
  - [10. Deadhead Extension Framework](#10-deadhead-extension-framework)
- [Part IV: Empirical Analysis](#part-iv-empirical-analysis)
  - [11. Dataset](#11-dataset)
  - [12. Regression Analysis](#12-regression-analysis)
  - [13. Variance Decomposition](#13-variance-decomposition)
- [Part V: Smoothing and Diagnostics](#part-v-smoothing-and-diagnostics)
  - [14. The Smoothing Problem](#14-the-smoothing-problem)
  - [15. Smoothing Methods Evaluated](#15-smoothing-methods-evaluated)
  - [16. Empirical Calibration](#16-empirical-calibration)
  - [17. The CNHR–MNHR Directional Relationship](#17-the-cnhrmnhr-directional-relationship)
  - [18. Four-State Diagnostic Matrix](#18-four-state-diagnostic-matrix)
- [Part VI: Time Horizons](#part-vi-time-horizons)
- [Part VII: Operational Applications](#part-vii-operational-applications)
- [Part VIII: Generalised Cost Structure](#part-viii-generalised-cost-structure)
- [Part IX: Cross-Industry Comparison](#part-ix-cross-industry-comparison)
- [Part X: Data Schema and Implementation](#part-x-data-schema-and-implementation)
- [Part XI: Visualisation and Display](#part-xi-visualisation-and-display)
- [Appendices](#appendices)

---

## Abstract

This specification defines a comprehensive analytical framework for evaluating rideshare driver performance on a first-principles basis. The framework centres on two complementary metrics: the **Cumulative Net Hourly Rate** (CNHR, rₙ), which measures aggregate profitability after fixed costs, and the **Marginal Net Hourly Rate** (MNHR, mᵢ), which measures the net economic contribution of individual trips. Both metrics are defined in dual-track variants—*paid* (using platform-reported engaged time only) and *true* (incorporating unpaid enroute time)—to capture the full spectrum from platform-comparable reporting to operational reality.

The specification introduces arithmetic and geometric mean aggregation of both CNHR and MNHR, establishing that the AM–GM gap serves as a model-free measure of rate volatility with direct economic interpretation. All parameters are empirically calibrated from a dataset of 635+ trips across 12 Uber weeks in London, with variance decomposition demonstrating that hours worked (not trip quality) constitute the binding constraint on weekly profitability.

The operational stopping framework is governed by a strict hierarchy: weekly hours targets take absolute priority over session-level stopping decisions. The four-state diagnostic matrix (SUSTAINED, ACCEL RECOVERY, DECELERATING, STALLED) provides session-level guidance that is explicitly subordinated to the weekly hours imperative established by the variance decomposition.

The framework is situated within the broader landscape of transport profitability analytics, demonstrating structural isomorphism with the aviation industry's CASM/RASM methodology, trucking's Cost Per Mile framework, and fleet management KPI systems. The MNHR directional signal, dual-track time accounting, and four-state diagnostic matrix are identified as contributions without parallel in existing transport economics.

The document is self-contained: it derives all formulae from first principles, provides complete algebraic proofs of key identities, specifies the statistical framework including regression and variance decomposition, defines the smoothing and diagnostic systems, and maps every computation to a concrete implementation.

**Keywords:** rideshare analytics, cost-normalised hourly rate, marginal analysis, enroute time, utilisation, CASM/RASM parallel, aviation economics, geometric mean, EMA smoothing, trip acceptance optimisation

---

## Part I: Foundations

### 1. Introduction

#### 1.1 Motivation

Rideshare platforms report driver earnings exclusively in terms of *engaged time*—the interval from passenger pickup to dropoff. This framing systematically overstates effective hourly earnings by excluding two categories of unpaid labour: *enroute time* (driving from the driver's current location to the passenger pickup point) and *deadhead time* (repositioning between trips without a fare). A driver observing a platform-reported rate of £32/hr may, in operational reality, be earning £19/hr on all time committed to trip fulfilment.

Furthermore, platform-reported earnings are gross figures. The driver's actual net position depends critically on fixed costs—vehicle rental, insurance, charging, telecommunications—that are incurred weekly irrespective of trip volume. A gross rate of £32/hr against weekly costs of £430 yields profitability only if sufficient hours are worked to amortise those costs.

This specification addresses both deficiencies by defining metrics that:

(i) Deduct fixed costs from earnings to yield net rates (CNHR, MNHR).
(ii) Incorporate unpaid working time to yield true rates alongside paid variants.
(iii) Provide both cumulative (CNHR) and marginal (MNHR) perspectives.
(iv) Aggregate using both arithmetic and geometric means to capture central tendency and volatility.
(v) Are grounded entirely in empirical data rather than theoretical assumptions.

#### 1.2 Scope

This document specifies: the complete mathematical framework for CNHR and MNHR in paid and true variants; arithmetic and geometric mean aggregation with AM–GM gap interpretation; the enroute integration model and its extension to full deadhead accounting; empirical calibration from the operational dataset; regression analysis identifying the binding constraint on profitability; variance decomposition of weekly CNHR; smoothing framework with empirically determined parameters; the CNHR–MNHR directional relationship and four-state diagnostic matrix; the operational stopping hierarchy (week > session > trip); cross-industry comparison with aviation, trucking, and fleet management frameworks; time horizon definitions (Uber week, session, multi-week); and complete data schema and implementation specification.

#### 1.3 Conventions

Throughout this document: all monetary values are in GBP (£); time is measured in decimal hours unless otherwise stated; the "Uber week" runs from Monday 04:00 UTC to Sunday 03:59 UTC; subscript n denotes the cumulative state after trip n within a week; subscript i denotes a per-trip quantity; superscript † denotes the "true" variant (including enroute time); the absence of † denotes the "paid" variant (engaged time only).

### 2. Axiomatic Foundations

**Axiom 1 (Fixed Cost Periodicity).** The driver incurs a fixed cost Cw per Uber week, independent of trip volume, earnings, or hours worked. This cost is non-negotiable and non-deferrable.

**Axiom 2 (Time as the Scarce Resource).** The driver's primary scarce resource is time. All costs are therefore allocated on a time-proportional basis. No alternative allocation basis (per-trip, per-mile) preserves the aggregation identities required for internal consistency.

**Axiom 3 (Dual Time Accounting).** For each trip i, two time quantities exist: tᵢ (paid duration): platform-reported engaged time from pickup to dropoff; tᵢᵉⁿ (enroute time): unpaid time from trip acceptance/dispatch to pickup arrival. The total committed time for trip i is tᵢ† = tᵢ + tᵢᵉⁿ.

**Axiom 4 (Earnings Invariance).** Trip earnings eᵢ are determined by the platform and are invariant to the driver's enroute time. The driver receives eᵢ regardless of how long the drive to pickup takes.

**Axiom 5 (Target Threshold).** There exists a target net hourly rate r* below which continued operation is economically unsustainable in the long run. This is an exogenous parameter set by the driver.

### 3. Primary State Variables

**Definition 3.1 (Trip Earnings).** For trip i within an Uber week, eᵢ ∈ ℝ₊ denotes the platform-reported portal earnings in GBP.

**Definition 3.2 (Trip Duration — Paid).** tᵢ ∈ ℝ₊ is the engaged (paid) duration of trip i in decimal hours.

**Definition 3.3 (Enroute Time).** tᵢᵉⁿ ∈ ℝ≥₀ is the unpaid time from dispatch to pickup for trip i, in decimal hours. If enroute data is unavailable, tᵢᵉⁿ = 0 (conservative default).

**Definition 3.4 (Trip Duration — True).**

```
tᵢ† = tᵢ + tᵢᵉⁿ                                                    (1)
```

**Definition 3.5 (Cumulative Earnings).**

```
Eₙ = Σᵢ₌₁ⁿ eᵢ                                                      (2)
```

**Definition 3.6 (Cumulative Hours — Paid).**

```
Tₙ = Σᵢ₌₁ⁿ tᵢ                                                      (3)
```

**Definition 3.7 (Cumulative Hours — True).**

```
Tₙ† = Σᵢ₌₁ⁿ tᵢ† = Tₙ + Σᵢ₌₁ⁿ tᵢᵉⁿ                                (4)
```

**Definition 3.8 (Trip Distance).** dᵢ ∈ ℝ₊ is the trip distance in miles.

**Definition 3.9 (Weekly Fixed Cost).** Cw ∈ ℝ₊ is the total weekly fixed cost. Currently Cw = £430/week, comprising vehicle rental (£220), insurance, EV charging, and telecommunications.

**Definition 3.10 (Utilisation).**

```
uᵢ = tᵢ / tᵢ† = tᵢ / (tᵢ + tᵢᵉⁿ) ∈ (0, 1]                         (5)
```

Weekly mean utilisation:

```
ū = (1/n) Σᵢ₌₁ⁿ uᵢ                                                 (6)
```

Time-weighted utilisation:

```
ū⁽ᵀ⁾ = Tₙ / Tₙ†                                                    (7)
```

---

## Part II: Core Metrics

### 4. Gross Hourly Rate (ϱ)

#### 4.1 Per-Trip Gross Rate

**Definition 4.1 (Trip Gross Rate — Paid).**

```
ϱᵢ = eᵢ / tᵢ                                                        (8)
```

**Definition 4.2 (Trip Gross Rate — True).**

```
ϱᵢ† = eᵢ / tᵢ† = eᵢ / (tᵢ + tᵢᵉⁿ)                                  (9)
```

**Proposition 4.1 (Utilisation–Rate Relationship).**

```
ϱᵢ† = ϱᵢ · uᵢ                                                      (10)
```

*Proof.* ϱᵢ† = eᵢ/tᵢ† = (eᵢ/tᵢ)·(tᵢ/tᵢ†) = ϱᵢ · uᵢ. ∎

This identity is fundamental: the true rate is the paid rate discounted by utilisation. At 60% utilisation, the true rate is 60% of the paid rate—not a small correction, but a structural transformation.

#### 4.2 Cumulative Gross Rate

**Definition 4.3 (Cumulative Gross Rate — Paid).** ϱ̄ₙ = Eₙ / Tₙ

**Definition 4.4 (Cumulative Gross Rate — True).** ϱ̄ₙ† = Eₙ / Tₙ†

**Proposition 4.2.** ϱ̄ₙ† = ϱ̄ₙ · ū⁽ᵀ⁾

### 5. Cost-Normalised Hourly Rate (CNHR)

#### 5.1 Core Formula — Paid

**Definition 5.1 (CNHR — Paid).**

```
rₙ = (Eₙ − Cw) / Tₙ                                               (13)
```

This is the primary metric: net earnings per hour of engaged (paid) work after deducting the full weekly fixed cost.

#### 5.2 Core Formula — True

**Definition 5.2 (CNHR — True).**

```
rₙ† = (Eₙ − Cw) / Tₙ†                                              (14)
```

This is the operational metric: net earnings per hour of total committed time (including unpaid enroute driving).

#### 5.3 Decomposition

**Theorem 5.1 (CNHR Decomposition).**

```
rₙ  = ϱ̄ₙ  − λw       where λw  = Cw / Tₙ                          (15)
rₙ† = ϱ̄ₙ† − λw†      where λw† = Cw / Tₙ†                         (16)
```

*Proof.* rₙ = (Eₙ − Cw)/Tₙ = Eₙ/Tₙ − Cw/Tₙ = ϱ̄ₙ − λw. True variant identical. ∎

**Remark.** This decomposition is the central structural result. CNHR is the gap between gross rate and cost rate. Improving CNHR requires either increasing ϱ̄ₙ (earn more per hour) or decreasing λw (work more hours to dilute fixed costs). The variance decomposition (§13) determines which lever dominates empirically.

#### 5.4 Relationship Between Paid and True CNHR

**Proposition 5.1 (Paid–True CNHR Relationship).**

```
rₙ† = rₙ · ū⁽ᵀ⁾                                                    (17)
```

*Proof.* rₙ† = (Eₙ − Cw)/Tₙ† = [(Eₙ − Cw)/Tₙ] · [Tₙ/Tₙ†] = rₙ · ū⁽ᵀ⁾. ∎

**Remark.** At the empirical mean utilisation of ū⁽ᵀ⁾ ≈ 0.66, the true CNHR is approximately two-thirds of the paid CNHR. The paid CNHR overstates net productivity by a factor of 1/ū⁽ᵀ⁾ ≈ 1.52.

#### 5.5 Phase Classification

**Definition 5.3 (Three-Phase Classification).**

```
Phase(rₙ) = DEFICIT     if rₙ < 0                                   (18)
             RECOVERY    if 0 ≤ rₙ < r*
             TARGET      if rₙ ≥ r*
```

#### 5.6 Inflection Points

**Definition 5.4 (Break-Even Trip).** n_BE = min{n : Eₙ ≥ Cw}                    (19)

**Definition 5.5 (Target Trip).** n* = min{n : rₙ ≥ r*}                           (20)

### 6. The Cost Rate (λ)

#### 6.1 Retrospective Cost Rate

**Definition 6.1 (Retrospective — Paid).** λw^(retro) = Cw / Tw                   (21)

**Definition 6.2 (Retrospective — True).** λw^(retro)† = Cw / Tw†                 (22)

The retrospective rate is exact but available only after the week concludes.

#### 6.2 Prospective Cost Rate

**Definition 6.3 (Prospective Cost Rate).**

```
λ̂ = Cw / H̄,    where H̄ = (1/W) Σw₌₁ᵂ Tw                          (23–24)
```

**Remark.** This is the operational cost rate used for real-time MNHR computation. As more weeks accumulate, H̄ converges to the driver's typical weekly hours, and λ̂ converges to the typical cost rate.

#### 6.3 Empirical Calibration

| Parameter | Paid | True |
|---|---|---|
| H̄ (mean weekly hours) | 20.05 h | ≈30.38 h* |
| λ̂ (cost rate) | £21.45/hr | £14.15/hr |
| Min weekly λ | £13.93/hr | — |
| Max weekly λ | £62.32/hr | — |
| CV(λ) | 57.6% | — |

*Estimated at mean utilisation 66%.

### 7. Marginal Net Hourly Rate (MNHR)

#### 7.1 The Cost Allocation Problem

To assign a "net" value to each trip, the weekly fixed cost Cw must be distributed across trips. The choice of allocation basis has algebraic consequences for whether the resulting per-trip metric aggregates consistently to the weekly CNHR.

#### 7.2 Time-Proportional Allocation (Adopted)

**Definition 7.1 (MNHR — Paid).**

```
mᵢ = ϱᵢ − λ̂                                                        (25)
```

**Definition 7.2 (MNHR — True).**

```
mᵢ† = ϱᵢ† − λ̂                                                      (26)
```

#### 7.3 Algebraic Consistency

**Theorem 7.1 (CNHR–MNHR Aggregation Identity).** Under time-proportional cost allocation:

```
rₙ = Σᵢ₌₁ⁿ mᵢ · tᵢ / Tₙ                                            (27)
```

where mᵢ = ϱᵢ − Cw/Tₙ uses the retrospective cost rate. The identity holds exactly.

*Proof.* Σ mᵢtᵢ / Tₙ = Σ(ϱᵢ − Cw/Tₙ)tᵢ / Tₙ = (Σeᵢ − Cw)/Tₙ = rₙ. ∎

#### 7.4 Alternative Allocation Bases (Rejected)

Under per-trip allocation (cᵢ = Cw/n), cᵢ/tᵢ varies inversely with trip duration, creating perverse incentives. Per-mile allocation introduces distance dependence that decouples the metric from time-based profitability. Time-proportional allocation is the unique basis preserving the aggregation identity (see Appendix C).

### 8. Arithmetic and Geometric Means

#### 8.1 Definitions

**Definition 8.1 (Arithmetic Mean).** AM(ϱ) = (1/n) Σᵢ₌₁ⁿ ϱᵢ

**Definition 8.2 (Geometric Mean).** GM(ϱ) = exp((1/n) Σᵢ₌₁ⁿ ln ϱᵢ)

True-variant means are computed analogously over trips with enroute data.

#### 8.2 Derived MNHR Means

```
AM(m) = AM(ϱ) − λ̂
GM(m) := GM(ϱ) − λ̂    (notational convention)
```

#### 8.3 The AM–GM Inequality and Its Interpretation

**Definition 8.5 (AM–GM Gap).**

```
ΔAG = AM(ϱ) − GM(ϱ) ≥ 0                                            (35)
```

**Proposition 8.1 (Volatility Interpretation).** For log-normal rates with log-variance σ²:

```
GM / AM = exp(−σ²/2) ≈ 1 − σ²/2                                    (36)
```

Hence ΔAG ≈ AM · σ²/2.

Operational thresholds: ΔAG > £4/hr (high variance); [£2, £4]/hr (moderate); < £2/hr (disciplined).

**Remark (Distributional Caveat).** Proposition 8.1 assumes log-normal rates for the σ²/2 approximation. Rideshare rates are not strictly log-normal: they are bounded below by platform base fares and above by algorithmic surge caps, producing compressed tails. The approximation therefore slightly overestimates the GM/AM ratio for extreme-variance weeks. However, ΔAG itself — the raw gap — is *distribution-free* and exact. Only the *interpretation* via σ²/2 is approximate. The operational thresholds are calibrated from empirical data and do not depend on the log-normal assumption.

#### 8.4 Multi-Week Aggregation

```
AM_agg(ϱ) = Σw nw · AMw(ϱ) / Σw nw                                 (37)
GM_agg(ϱ) = exp(Σw nw · ln GMw(ϱ) / Σw nw)                         (38)
```

---

## Part III: Enroute and Deadhead Integration

### 9. The Unpaid Time Problem

#### 9.1 Taxonomy of Driver Time

| Category | Symbol | Paid? | Observable? |
|---|---|---|---|
| Engaged (pickup→dropoff) | tᵢ | Yes | Yes (platform data) |
| Enroute (dispatch→pickup) | tᵢᵉⁿ | No | Yes (paired trip data) |
| Deadhead (inter-trip repositioning) | tᵢᵈʰ | No | Requires GPS logging |
| Idle (waiting for dispatch) | tᵢⁱᵈˡᵉ | No | Requires session logging |

#### 9.2 Enroute Time: Data Source and Coverage

Enroute time is obtained by pairing consecutive trips: if trip i ends at time τᵢᵉⁿᵈ and trip i+1 begins at time τᵢ₊₁ˢᵗᵃʳᵗ, then tᵢ₊₁ᵉⁿ = τᵢ₊₁ˢᵗᵃʳᵗ − τᵢᵉⁿᵈ.

| Coverage Metric | Value |
|---|---|
| Total trips (12 weeks) | 635+ |
| Trips with enroute data | 77–91% per week |
| Mean enroute time | 15.8 min |
| Mean utilisation (ū) | 66% |
| Utilisation range (per trip) | 3.3% – 100% |

#### 9.3 Impact Quantification

**Theorem 9.1 (Enroute Penalty on CNHR).** At utilisation ū⁽ᵀ⁾: rₙ† = rₙ · ū⁽ᵀ⁾. At ū⁽ᵀ⁾ = 0.66, the paid CNHR overstates the true CNHR by a factor of 1/0.66 = 1.52, approximately 52%.

**Remark (Empirical Validation).** From the Week 09–16 Feb dataset (90 trips): Eₙ = £1,261.40, Tₙ = 39.58 h, ū⁽ᵀ⁾ = 60.5%, rₙ = £31.87/hr, rₙ† = £19.26/hr (39.5% reduction). Matches theoretical prediction: 31.87 × 0.605 = 19.28.

#### 9.4 CNHR with Enroute: Implementation

Trips without enroute data default to tᵢᵉⁿ = 0 (utilisation = 1.0), which is conservative: it understates the enroute penalty, biasing those trips favourably rather than penalising for missing data.

#### 9.5 Recalibrated Constants for True Variant

| Constant | Paid | True |
|---|---|---|
| H̄ | 20.05 h | ≈30.38 h |
| λ̂ | £21.45/hr | £14.15/hr |
| r* | £15.00/hr | £9.90/hr* |

*Mechanically equivalent to paid r* at 66% utilisation: 15.00 × 0.66 = 9.90.

### 10. Deadhead Extension Framework

**Definition 10.1 (Deadhead Time).** tᵢᵈʰ for trip i is the time spent repositioning without a fare between the end of the previous trip's dropoff and the start of the next trip's enroute drive.

#### 10.1 Full Committed Time

With deadhead data:

```
tᵢ‡ = tᵢ + tᵢᵉⁿ + tᵢᵈʰ                                              (40)
rₙ‡ = (Eₙ − Cw) / Tₙ‡,    Tₙ‡ = Σᵢ₌₁ⁿ tᵢ‡                         (41)
```

#### 10.2 Data Requirements

Deadhead time requires continuous GPS or session logging—it cannot be inferred from paired trip timestamps alone because the gap between trips may include idle time, personal breaks, or charging stops.

#### 10.3 Three-Tier Metric Hierarchy

| Tier | Denominator | Data Required | Status | Bias Direction |
|---|---|---|---|---|
| Paid (rₙ) | Σ tᵢ | Platform data | Implemented | Overstates (ignores all unpaid) |
| True (rₙ†) | Σ(tᵢ + tᵢᵉⁿ) | Paired trip times | Implemented | Overstates (ignores DH + idle) |
| Full (rₙ‡) | Σ(tᵢ + tᵢᵉⁿ + tᵢᵈʰ) | GPS/session logging | Pending Sentinel | Unbiased |

Each tier shares the same algebraic structure — differing only in the denominator — ensuring all identities hold uniformly.

**Remark (Residual Overstatement in the True Variant).** The true variant (rₙ†) corrects for enroute time (mean 15.8 min/trip) but excludes deadhead repositioning and idle waiting. The overstatement is bounded:

```
rₙ‡ = rₙ† · (Tₙ† / Tₙ‡) = rₙ† · (1 − f_DH)
```

where f_DH is the fraction of total working time in deadhead and idle beyond enroute. If deadhead and idle constitute 15–20% of committed time (plausible for urban night-shift PHV), the true variant overstates the full rate by 15–20%. This is a known, directional, bounded bias — preferable to the 52% overstatement of the paid variant, but not negligible.

**Remark (Sentinel as the Tier-3 Enabler).** Progression from Tier 2 (True) to Tier 3 (Full) is gated on the Sentinel observation pipeline:

| Inter-Trip State | Detection Method | Sentinel Capability |
|---|---|---|
| Deadhead (repositioning) | GPS: vehicle moving, no active trip | GPS + screen state |
| Idle (waiting for dispatch) | GPS: stationary, app showing "waiting" | GPS + OCR |
| Personal break | GPS: stationary, app offline or paused | GPS + screen state |
| Charging | GPS: at known charger location | GPS + geofence |

Until this pipeline is operational, rₙ† remains the best available metric with the bias characterised above.

---

## Part IV: Empirical Analysis

### 11. Dataset

#### 11.1 Scope

| Metric | Value |
|---|---|
| Total trips | 635+ |
| Uber weeks | 12 |
| Date range | 7 Dec 2025 – 16 Feb 2026 |
| Total portal earnings | £8,500+ |
| Total engaged hours | 260+ h |
| Total miles | 4,900+ mi |
| Platform | Uber (London, UK) |
| Vehicle | Kia e-Niro EV (PHV) |
| Typical shift | 19:00–07:00 (night) |

#### 11.2 Data Fields per Trip

Each trip record contains: sequential number, datetime, day of week, service type, earnings (£), duration (hours), distance (miles), pickup location, dropoff location, and (where available) enroute time derived from paired trip timestamps.

#### 11.3 Constants

| Symbol | Value | Description |
|---|---|---|
| Cw | £430/wk | Fixed weekly costs |
| r* | £15/hr | Target CNHR |
| α | 0.15 | EMA smoothing factor |
| H̄ | 20.05 h | Baseline weekly hours |
| λ̂ | £21.45/hr | Prospective cost rate |

### 12. Regression Analysis

#### 12.1 Weekly-Level: What Drives Earnings?

**Model 1: Hours Only.** Ew = β₁Hw + β₀: β₁ = £33.15/hr, β₀ = −£4.97, R² = 0.8875. Hours alone explain 89% of variance.

**Model 2: Hours + Trip Count.** Ew = β₁Hw + β₂Nw + β₀: β₁ = £24.55/hr, β₂ = £4.51/trip, β₀ = −£55.79, R² = 0.9128.

**Model 3: Hours + Trips + Miles.** Ew = β₁Hw + β₂Nw + β₃Dw + β₀: β₁ = £8.42/hr, β₂ = £4.81/trip, β₃ = £0.69/mi, β₀ = −£10.51, R² = 0.9794.

#### 12.2 Trip-Level: Earnings Structure

eᵢ = β₁tᵢ + β₂dᵢ + β₀: β₁ = £13.91/hr, β₂ = £0.65/mi, β₀ = £2.64 (base fare), R² = 0.8555.

This confirms Uber's pricing is a hybrid of time, distance, and base fare. For cost allocation, the critical question is which basis preserves the CNHR–MNHR aggregation identity (time-proportional, per Theorem 7.1).

### 13. Variance Decomposition

Since rₙ = ϱ̄ₙ − λw (Theorem 5.1):

```
Var(rₙ) = Var(ϱ̄) + Var(λw) − 2 Cov(ϱ̄, λw)                         (46)
```

#### 13.1 Results

| Component | Value | Share of Var(rₙ) |
|---|---|---|
| Var(rₙ) | 651.63 | 100% |
| Var(ϱ̄) | 18.98 | 2.9% |
| Var(λw) | 590.99 | 90.7% |
| −2 Cov(ϱ̄, λw) | 45.82 | 7.0% |

#### 13.2 Correlation Structure

| Pair | Correlation |
|---|---|
| Corr(rₙ, H) | +0.8239 |
| Corr(rₙ, ϱ̄) | +0.3579 |
| Corr(rₙ, N) | +0.7341 |

#### 13.3 The Binding Constraint

**Theorem 13.1 (Hours as the Binding Constraint).** The cost rate λw = Cw/Tw accounts for 90.7% of week-to-week CNHR variance. Gross hourly rate ϱ̄ contributes only 2.9%. Therefore:

(i) The primary lever for improving CNHR is increasing hours worked, not improving trip quality.
(ii) Gross rate ϱ̄ is relatively stable across weeks (CV ≈ 13%), while cost rate λw is highly variable (CV ≈ 57.6%).
(iii) Corr(rₙ, H) = +0.82 confirms that longer weeks mechanically produce better CNHR through cost dilution.

**Remark.** A driver obsessing over individual trip quality addresses only 2.9% of CNHR variance. Working an additional 5 hours per week addresses 90.7%. The correct strategic priority: **maximise hours, then optimise trip quality at the margin.**

#### 13.4 Hours Required at Current Gross Rate

Given ϱ̄ ≈ £32.91/hr:

| Weekly Hours | λw (£/hr) | CNHR (£/hr) | Phase |
|---|---|---|---|
| 15 h | 28.67 | 4.24 | Recovery |
| 20 h | 21.50 | 11.41 | Recovery |
| 24 h | 17.92 | 14.99 | Recovery (borderline) |
| 25 h | 17.20 | 15.71 | Target |
| 30 h | 14.33 | 18.58 | Target |

Break-even for target: Cw/(ϱ̄ − r*) = 430/(32.91 − 15) = 24.0 h/wk.

#### 13.5 Cost Rate Analysis: Retrospective vs Prospective

| | Retrospective (Cw/Tw) | Prospective (Cw/H̄) |
|---|---|---|
| Mean | £24.30/hr | £21.45/hr (constant) |
| Min | £13.93/hr | — |
| Max | £62.32/hr | — |
| CV | 57.6% | 0% |
| Usage | Post-hoc analysis | Real-time MNHR |

---

## Part V: Smoothing and Diagnostics

### 14. The Smoothing Problem

Raw per-trip MNHR values exhibit high variance (individual trips range from −£15/hr to +£60/hr). For the MNHR to serve as a directional signal, it must be smoothed to suppress idiosyncratic trip noise while preserving genuine trend information. The quality criterion is *directional accuracy*: the fraction of trips where the smoothed MNHR correctly predicts whether CNHR will rise or fall at the next trip.

### 15. Smoothing Methods Evaluated

#### 15.1 Exponential Moving Average (EMA)

```
m̃₀  = m₀
m̃ₙ  = α · mₙ  + (1−α) · m̃ₙ₋₁                                     (48–49)
m̃ₙ† = α · mₙ† + (1−α) · m̃ₙ₋₁†                                    (50)
```

#### 15.2 Rolling Mean (Fixed Window)

```
m̃ₙ^(roll,k) = [1/min(n,k)] Σⱼ₌ₘₐₓ₍₁,ₙ₋ₖ₊₁₎ⁿ mⱼ                  (51)
```

#### 15.3 Rolling Median

```
m̃ₙ^(med,k) = median{mⱼ : j ∈ [max(1, n−k+1), n]}                  (52)
```

#### 15.4 Hybrid: Median Pre-filter + EMA

Apply a rolling median of window k to reject outliers, then smooth the filtered series with EMA.

#### 15.5 Time-Decay EMA

Modify the EMA factor by the inter-trip time gap to give less weight to trips separated by long intervals.

### 16. Empirical Calibration

#### 16.1 Evaluation Metric

**Definition 16.1 (Directional Accuracy).**

```
DA = [1/(n−1)] Σᵢ₌₁ⁿ⁻¹ 𝟙[sign(m̃ᵢ − rᵢ) = sign(rᵢ₊₁ − rᵢ)]       (53)
```

#### 16.2 Grid Search Results

| Method | Optimal Parameters | DA |
|---|---|---|
| EMA | α = 0.15 | 92.5% |
| Rolling Mean | k = 7 | 89.3% |
| Rolling Median | k = 5 | 87.1% |
| Hybrid (Median + EMA) | k = 3, α = 0.15 | 91.8% |
| Time-Decay EMA | α₀ = 0.15, τ = 2 h | 91.2% |

#### 16.3 Key Findings

Simple EMA with α = 0.15 achieves the highest directional accuracy at 92.5%. The median pre-filter adds complexity without accuracy gain. The time-decay variant is marginally worse and harder to implement. The optimal α = 0.15 implies an effective window of 2/α − 1 ≈ 12 trips (≈1.5 sessions).

#### 16.4 Recommended Configuration

**Adopted Smoothing: EMA with α = 0.15.** Compute raw mᵢ = ϱᵢ − λ̂ and mᵢ† = ϱᵢ† − λ̂. Apply EMA: m̃ₙ = 0.15·mₙ + 0.85·m̃ₙ₋₁. No median filter, no hybrid, no CNHR smoothing. CNHR is displayed unsmoothed (its cumulative nature provides inherent smoothing).

### 17. The CNHR–MNHR Directional Relationship

**Theorem 17.1 (Directional Theorem).** Let rₙ = (Eₙ − Cw)/Tₙ. Then:

```
mₙ > rₙ  ⟹  rₙ₊₁ > rₙ    (CNHR rises)                           (54)
mₙ < rₙ  ⟹  rₙ₊₁ < rₙ    (CNHR falls)                           (55)
mₙ = rₙ  ⟹  rₙ₊₁ = rₙ    (CNHR unchanged)                       (56)
```

where mₙ = ϱₙ − Cw/Tₙ uses the retrospective cost rate.

*Proof.*

```
rₙ₊₁ − rₙ = (Eₙ₊₁ − Cw)/Tₙ₊₁ − (Eₙ − Cw)/Tₙ
            = [eₙ₊₁ · Tₙ − (Eₙ − Cw) · tₙ₊₁] / (Tₙ₊₁ · Tₙ)
            = [tₙ₊₁ / Tₙ₊₁] · (ϱₙ₊₁ − rₙ)
```

Since tₙ₊₁/Tₙ₊₁ > 0, sign(rₙ₊₁ − rₙ) = sign(ϱₙ₊₁ − rₙ). ∎

**Remark.** The smoothed variant m̃ₙ > rₙ predicts CNHR direction with 92.5% accuracy. The 7.5% failure rate has two distinct sources: EMA lag at trend inflection points, and the prospective–retrospective cost rate gap analysed in §17.2.

#### 17.2 Retrospective Identity vs Prospective Approximation

The Directional Theorem is an unconditional identity: sign(rₙ₊₁ − rₙ) = sign(ϱₙ₊₁ − rₙ). Note that the sign condition involves only the *gross* trip rate ϱₙ₊₁ and the *net cumulative* rate rₙ. No cost rate appears in the comparison.

However, the operational MNHR signal tests m̃ₙ > rₙ, where m̃ₙ is the EMA of mᵢ = ϱᵢ − λ̂ and λ̂ = Cw/H̄ is the prospective cost rate. Rearranging:

```
m̃ₙ > rₙ   ⟺   EMA(ϱᵢ) > rₙ + λ̂
```

The exact identity requires comparing ϱₙ₊₁ against rₙ directly. The operational test compares an EMA-smoothed gross rate against rₙ + λ̂. This introduces two sources of discrepancy:

**Source 1: Cost rate mismatch.** The retrospective cost rate within the current week is λw = Cw/Tₙ, which varies continuously as hours accumulate. The prospective rate λ̂ = Cw/H̄ is a fixed historical estimate. The gap:

```
λ̂ − Cw/Tₙ = Cw · (1/H̄ − 1/Tₙ)
```

This gap is zero only when Tₙ = H̄. The bias is systematic and predictable:

- **Early in week** (Tₙ ≪ H̄): Cw/Tₙ ≫ λ̂, so rₙ is deeply negative while λ̂ is moderate. The operational threshold rₙ + λ̂ is *lower* than the exact threshold rₙ, making the signal **too optimistic** — it predicts CNHR improvement more readily than warranted.
- **Late in a long week** (Tₙ > H̄): Cw/Tₙ < λ̂, so the threshold is *higher* than exact, making the signal **too conservative**.
- **At Tₙ = H̄**: The gap vanishes. Prospective and retrospective coincide.

**Source 2: EMA smoothing lag.** The identity requires comparing the *current* trip's gross rate ϱₙ₊₁. The EMA m̃ₙ is a weighted average of the past ≈12 trips' MNHRs, creating inherent delay at inflection points.

**Quantifying the combined error.** The 92.5% directional accuracy reflects both sources jointly. The EMA lag dominates at trend reversals (sudden demand shifts); the cost rate gap dominates in the early-week DEFICIT phase where Cw/Tₙ diverges sharply from λ̂.

**Remark (Design Rationale).** The prospective rate λ̂ is used despite this gap because the retrospective rate Cw/Tₙ is *undefined* until the first trip (division by zero) and pathologically large for the first few trips, rendering it unusable for real-time signalling. The prospective rate provides a stable baseline. The 7.5% accuracy cost is accepted as the price of a computable real-time signal. This is an engineering trade-off, not a mathematical deficiency: the identity is exact; the implementation is an approximation.

#### 17.1 Convergence Behaviour

As n → ∞, rₙ converges to ϱ̄ − Cw/T∞, approaching ϱ̄ from below. If m̃∞ > 0, the marginal trip is net-positive and CNHR continues to improve.

### 18. Four-State Diagnostic Matrix

#### 18.1 Threshold Definitions

- Level threshold: rₙ ≷ r* (is CNHR at target?)
- Momentum threshold: m̃ₙ† ≷ rₙ (is the smoothed true MNHR above CNHR?)

The true-variant EMA (m̃ₙ†) is used for state classification because it captures the full cost of trip commitment including enroute time.

#### 18.2 State Table

| | m̃ₙ† > rₙ (Improving) | m̃ₙ† ≤ rₙ (Deteriorating) |
|---|---|---|
| **rₙ ≥ r*** | SUSTAINED | DECELERATING |
| **rₙ < r*** | ACCEL RECOVERY | STALLED |

#### 18.3 State Descriptions

- **SUSTAINED**: At or above target and still improving. Optimal state.
- **ACCEL RECOVERY**: Below target but improving—momentum favourable.
- **DECELERATING**: At target but declining—recent marginal trips below CNHR.
- **STALLED**: Below target and worsening—recent trips net-negative at the margin.

#### 18.4 Phase Transition Events

A transition event occurs when the state changes between consecutive trips. These are logged and displayed in per-week commentary.

#### 18.5 Operational Stopping Framework

The variance decomposition (Theorem 13.1) establishes that weekly hours account for 90.7% of CNHR variance. This result imposes an absolute constraint on all stopping decisions: **no session-level stopping decision may reduce projected weekly hours below H_target without the driver's conscious, informed override.**

The STALLED state describes the *current session's demand environment*, not the week's remaining potential. Rideshare demand is stochastic and non-stationary. The framework has no basis to assert that future sessions will replicate a current session's poor conditions.

##### 18.5.1 The Operational Hierarchy

All operational decisions are governed by strict priority:

| Priority | Level | Decision | Constraint |
|---|---|---|---|
| 1 (Highest) | Week | Are cumulative hours tracking toward H_target (24 h)? | If not, schedule additional shifts. **Non-negotiable.** |
| 2 | Session | Has this session entered STALLED with demand collapse? | End this session. Redeploy hours to a better demand window. |
| 3 (Lowest) | Trip | Is this offer MNHR-positive? | Decline if demand supports a replacement offer. |

##### 18.5.2 Revised State Actions

| State | Action | Scope | Constraint |
|---|---|---|---|
| SUSTAINED | Continue | Session | — |
| ACCEL RECOVERY | Continue | Session | — |
| DECELERATING | Monitor; end session if persistent | Session only | Plan replacement session if hours deficit |
| STALLED | End session; redeploy hours | Session only | Weekly hours constraint remains binding |

##### 18.5.3 Contextual Examples

| Scenario | State | Action | Rationale |
|---|---|---|---|
| STALLED at 04:30 Tue, 15 hrs this week | STALLED | End session. Drive Wed + Thu nights. | Demand collapsed. 9 hrs still needed. |
| STALLED at 22:30 Fri, 10 hrs this week | STALLED | Reposition to high-demand zone. | Peak hours—STALLED likely locational. Do not stop. |
| STALLED at 03:00 Sun, 26 hrs this week | STALLED | End session. Week target met. | H_target exceeded. Safe to stop. |
| DECEL at 01:00 Sat, 18 hrs this week | DECEL | Monitor. Continue. | Above target but eroding. 6 hrs still needed. |

**Remark (Weekly Hours Invariant).** The weekly hours imperative is an invariant of the framework. It cannot be overridden by any session-level signal. STALLED means "this session's demand environment is producing net-negative marginal trips." It does not mean "stop working for the week."

##### 18.5.4 Scope Boundary: Endogenous Offer Dynamics

The CNHR–MNHR framework evaluates realised performance conditional on the observed trip sequence. It does not model how driver actions — acceptance decisions, geographic repositioning, cancellation behaviour — alter the conditional distribution of future trip offers under platform dispatch algorithms.

Three causal feedback channels exist outside the framework's scope:

(i) **Acceptance filtering.** Declining low-MNHR offers (which the framework encourages) may alter the platform's dispatch priority, offer frequency, or offer quality for the driver. The acceptance rate becomes a state variable affecting future ϱᵢ, but the framework treats it as exogenous.

(ii) **Geographic repositioning.** A driver who repositions after a STALLED signal changes the generating process of future offers. The framework captures the *outcome* (improved ϱᵢ) but not the *mechanism* (repositioning alters the conditional offer distribution).

(iii) **Algorithmic path dependence.** The platform's matching algorithm is not stateless. A driver's acceptance history, completion rate, and geographic patterns may feed into offer ranking. The sequence ϱ₁, ϱ₂, ..., ϱₙ is not i.i.d. — it is a stochastic process whose transition kernel depends on the driver's prior actions.

**Distinction: instantaneous vs. policy optimality.** The Directional Theorem guarantees that if ϱₙ₊₁ > rₙ, then rₙ₊₁ > rₙ. This is conditionally exact for any realised trip. But it does not guarantee that an acceptance policy based on MNHR positivity maximises the expected future stream. The framework solves *instantaneous* optimality (is this trip helping?). It does not solve *policy* optimality (what acceptance strategy maximises long-run earnings given platform feedback dynamics?). The latter constitutes a partially observable Markov decision process (POMDP) and lies outside the present specification.

**Design rationale.** The framework deliberately excludes platform-response modelling because: (a) the dispatch algorithm is unobservable, non-stationary, and possibly personalised, making any structural model speculative; (b) the causal effects are empirically testable using CNHR–MNHR metrics retrospectively without requiring a forward model; (c) cost accounting and reinforcement learning are different analytical categories, and conflating them would compromise the algebraic purity that is this framework's principal strength.

**Empirical testing.** The following hypotheses are testable within the existing data structure without extending the framework:

- Does acceptance rate in session k correlate with mean ϱᵢ in session k+1?
- Does repositioning after STALLED improve subsequent MNHR relative to continuing in place?
- Is there serial dependence in offer quality conditional on recent acceptance history?

If policy optimisation is subsequently required, the CNHR–MNHR metrics provide the natural reward signal (Rₙ = rₙ₊₁ − rₙ) for a reinforcement learning layer. This would constitute a separate specification (see Appendix D, Extension E9).

---

## Part VI: Time Horizons

### 19. Uber Week (Primary Horizon)

The Uber week (Monday 04:00 – Sunday 03:59 UTC) is the natural primary horizon because: Cw is a weekly cost aligning with the cost accounting period; Uber reports and settles earnings weekly; the cumulative structure of rₙ naturally resets at week boundaries.

Within each week, rₙ traces a trajectory from −∞ (first trip) through break-even (rₙ = 0) and potentially to target (rₙ = r*).

### 20. Daily (Session) Horizon

Daily CNHR is not recommended as a primary metric because Cw is a weekly cost. Instead, daily analysis uses: daily gross rate ϱ̄_day = E_day/T_day, daily MNHR trajectory, and daily contribution to weekly CNHR.

### 21. Multi-Week (Aggregate) Horizon

```
r_agg = (Σw Ew − Cw · W) / Σw Tw
```

### 22. Segmented CNHR (Display-Layer Extension)

The base CNHR treats all time as fungible. This is algebraically necessary for the aggregation identity but economically lossy, because demand intensity, charging costs, and congestion exposure vary systematically across time-of-day, geographic zone, and shift type.

**Definition (Segmented CNHR).** For a segment filter ℱ that selects a subset of trips:

```
rₙ^(ℱ) = Σ_{ℱ(i)} eᵢ* / Σ_{ℱ(i)} tᵢ
```

No fixed cost is deducted because fixed costs cannot be attributed to individual segments without arbitrary allocation. This is a *gross segment rate* answering: "what is the average net-of-variable-cost earnings rate for trips matching this filter?"

Useful segment filters:

| Segment | Filter | Operational Question |
|---|---|---|
| Night shift | 19:00–07:00 | How productive are nights vs. days? |
| Day shift | 07:00–19:00 | Is the congestion penalty worth the demand? |
| Zone (e.g., SW1) | Pickup in zone | Which zones yield highest ϱ̄*? |
| Peak hours | 22:00–02:00 | Is the late-night premium real after costs? |
| Surge trips | σᵢ > 1.0 | Does surge genuinely improve net rates? |

All segment-level metrics (AM, GM, ΔAG, MNHR distributions) are computed identically to whole-week counterparts, applied to the filtered subset. No modification to the core specification required.

### 23. Horizon Summary

| Horizon | Metric | Cost Treatment | Purpose |
|---|---|---|---|
| Uber Week | rₙ, m̃ₙ | Cf (single week) | Primary analysis |
| Session (Day) | ϱ̄_day, MNHR trajectory | None (gross only) | Shift-level diagnostics |
| Segment | rₙ^(ℱ) | Variable only | Cross-segment comparison |
| Multi-Week | r_agg | Cf × W | Long-run viability |

---

## Part VII: Operational Applications

### 32. Shift Planning

```
H_target = Cw / (ϱ̄ − r*) = 430 / (32.91 − 15) = 24.0 hours/week
```

At 8-hour shifts, this requires 3 shifts per week.

### 33. Trip Acceptance Decision Support

For a proposed trip with estimated earnings ê, duration t̂, and enroute time t̂ᵉⁿ:

```
m̂† = ê / (t̂ + t̂ᵉⁿ) − λ̂
```

| Condition | Recommendation |
|---|---|
| m̂† > rₙ | Accept: trip pulls CNHR upward |
| 0 < m̂† ≤ rₙ | Accept if in RECOVERY: still net-positive |
| m̂† ≤ 0 | Decline if alternatives exist: net cost-negative |

### 34. NLW Statement Reconciliation

Gap = Statement Total − Eₙ. Tracks tips, promotions, and NLW adjustments per-week.

---


## Part VIII: Generalised Cost Structure

This section replaces the single-parameter cost model (Cw = £430) with a decomposed framework that separates fixed, variable, and regime-dependent costs while preserving all existing algebraic identities.

### 40. The Limitation of the Single-Parameter Model

The specification to this point treats Cw as an atomic constant. In reality, the £430 conflates components with fundamentally different economic structures:

| Component | Current Value | Fixed? | Scales With |
|---|---|---|---|
| Vehicle hire | £220/wk | Yes (contract) | — |
| Insurance | £50/wk | Yes (annual) | — |
| Telecommunications | £10/wk | Yes (contract) | — |
| EV charging | £30/wk (est.) | **No** | Miles driven, price tier |
| Tyre wear | Absorbed in hire | **No** | Miles driven |

This conflation is harmless *as long as the driver's operational pattern is stable*. When any of the following changes occur, the single-parameter model breaks:

(i) Vehicle regime change (hire → ownership, or vehicle swap).
(ii) Charging strategy change (off-peak vs. emergency, different networks).
(iii) Shift pattern change (night-only → mixed, affecting per-trip variable costs).
(iv) Maintenance events (owned vehicle repairs, tyre replacement).

### 41. Decomposed Cost Model

#### 41.1 The Two-Component Structure

**Definition (Generalised Weekly Cost).** Total cost incurred across n trips within an Uber week:

```
𝒞(n) = Cf + Σᵢ₌₁ⁿ cᵢ
```

where:

- **Cf ∈ ℝ₊** is the **fixed weekly cost**: incurred in full regardless of whether any trips are completed. This includes insurance, telecommunications, vehicle finance or depreciation (amortised weekly), and licensing costs.
- **cᵢ ∈ ℝ≥₀** is the **variable cost of trip i**: incurred only because trip i was undertaken. This includes energy (charging), distance-proportional wear, and any per-session costs attributable to that trip.

**Remark (Backward Compatibility).** The original model is the special case where cᵢ = 0 for all i and Cf = Cw = £430. No existing result is invalidated.

#### 41.2 Net Trip Earnings

**Definition (Net Trip Earnings).**

```
eᵢ* = eᵢ − cᵢ
```

**Definition (Cumulative Net Earnings).**

```
Eₙ* = Σᵢ₌₁ⁿ eᵢ* = Eₙ − Σᵢ₌₁ⁿ cᵢ
```

#### 41.3 Generalised CNHR

**Theorem (Generalised CNHR).** Under the decomposed cost model:

```
rₙ = (Eₙ* − Cf) / Tₙ = (Eₙ − Cf − Σcᵢ) / Tₙ
```

This is algebraically identical to the original CNHR formula with eᵢ replaced by eᵢ* and Cw replaced by Cf. All existing identities, decompositions, and theorems carry through under this substitution.

*Proof.* Direct expansion: rₙ = Σ(eᵢ − cᵢ − Cf)/Tₙ = (Eₙ* − Cf)/Tₙ. Same form as (Eₙ − Cw)/Tₙ. ∎

#### 41.4 Generalised CNHR Decomposition

**Theorem (Generalised Decomposition).**

```
rₙ = ϱ̄ₙ* − λf
```

where ϱ̄ₙ* = Eₙ*/Tₙ is the **net cumulative gross rate** and λf = Cf/Tₙ is the **fixed cost rate**.

**Remark.** The decomposition now cleanly separates two levers:
(i) ϱ̄ₙ* (net gross rate): affected by both platform earnings *and* variable cost management (charging strategy, route efficiency).
(ii) λf (fixed cost rate): affected only by hours worked and the fixed cost base.

Under the old model, charging strategy was invisible. Under the generalised model, a driver who charges at £0.23/kWh instead of £0.40/kWh sees a direct improvement in ϱ̄ₙ*, making charging optimisation a visible lever on CNHR.

#### 41.5 Generalised MNHR

**Definition (Generalised MNHR — Paid).**

```
mᵢ = ϱᵢ* − λ̂f
```

where ϱᵢ* = eᵢ*/tᵢ = (eᵢ − cᵢ)/tᵢ is the **net trip gross rate** and λ̂f = Cf/H̄ is the prospective fixed cost rate.

**Definition (Generalised MNHR — True).**

```
mᵢ† = ϱᵢ*† − λ̂f,    ϱᵢ*† = (eᵢ − cᵢ) / tᵢ†
```

#### 41.6 Preservation of the Aggregation Identity

**Theorem (Generalised Aggregation Identity).** Under time-proportional allocation of fixed costs:

```
rₙ = Σᵢ₌₁ⁿ mᵢ · tᵢ / Tₙ
```

where mᵢ = ϱᵢ* − Cf/Tₙ uses the retrospective fixed cost rate. Holds exactly.

*Proof.* Σ mᵢtᵢ / Tₙ = Σ(ϱᵢ* − Cf/Tₙ)tᵢ / Tₙ = (Σeᵢ* − Cf)/Tₙ = rₙ. ∎

**Remark (Why This Works).** Variable costs are absorbed into per-trip net earnings *before* the CNHR framework is applied. The Directional Theorem, four-state matrix, EMA smoothing, variance decomposition, and all other results carry through without modification. This is not an approximation — it is an exact algebraic substitution.

### 42. Variable Cost Components

#### 42.1 Charging Cost Model

**Definition (Per-Trip Charging Cost).**

```
cᵢ^(charge) = dᵢ† · η · pᵢ
```

where:

- **dᵢ†** = total committed distance for trip i (including enroute driving). If enroute distance unavailable, dᵢ (paid) used as lower bound.
- **η** = energy consumption rate (kWh/mile). For Kia e-Niro: η ≈ 0.28 kWh/mile (3.6 mi/kWh).
- **pᵢ** = effective electricity price for trip i (£/kWh). Blended price of the charging session that fuelled this trip.

**Remark (Charging Price Assignment).** A single charging session may fuel multiple trips. The practical implementation assigns a uniform pᵢ = p̄ (session average price) to all trips fuelled by that session. Where session-level data is unavailable, the weekly average price p̄w is used.

**Charging Price Tiers (January 2026):**

| Tier | Locations | Price (£/kWh) | Window |
|---|---|---|---|
| Off-peak (best) | Bluewater, Sidcup, Dorking | 0.23 | 23:00–09:00 |
| Off-peak (good) | Stevenage, Waddon, Grays | 0.24–0.25 | 22:00–09:00 |
| Off-peak (moderate) | Cheshunt, Guildford, Gatwick | 0.26–0.27 | 22:00–09:00 |
| Peak / emergency | Various | 0.40–0.79 | Daytime |

For 38.4 kWh (20–80% charge on e-Niro): £8.83 at £0.23/kWh vs. £15.36 at £0.40/kWh — a 74% cost premium. Annual saving from disciplined off-peak charging: ≈£1,700.

**Impact on Net Trip Rate** (at η = 0.28 kWh/mi, typical 7.5 mi trip):

| Charging Tier | cᵢ^(charge) | Effect on ϱᵢ* |
|---|---|---|
| £0.23/kWh (off-peak) | £0.48/trip | −£1.45/hr on ϱ* |
| £0.40/kWh (peak) | £0.84/trip | −£2.52/hr on ϱ* |
| £0.79/kWh (emergency) | £1.66/trip | −£4.97/hr on ϱ* |

The emergency charging penalty (£4.97/hr) erodes nearly one-third of the target CNHR (£15/hr). Under the old model, this was invisible.

#### 42.2 Wear and Maintenance Cost Model

**Definition (Per-Trip Wear Cost).**

```
cᵢ^(wear) = dᵢ† · w
```

where w (£/mile) is the wear rate encompassing tyres, brakes, suspension, and consumables.

**Regime-Dependent Wear Rate:**

| Regime | Wear Rate w | Rationale |
|---|---|---|
| Vehicle hire | w ≈ 0 | Wear is hire company's liability |
| Owned (new/warranty) | w ≈ £0.03–£0.05/mi | Tyres + consumables only |
| Owned (post-warranty) | w ≈ £0.06–£0.10/mi | Add repair contingency |

**Remark (Maintenance Events).** Major maintenance can be modelled two ways:
(a) **Amortised into w**: Estimate annual cost, divide by annual miles. Smooths cost.
(b) **Lump-sum event**: Add to Cf for the week of occurrence. Creates a spike but is honest about cash flow.

For regular wear (tyres, brakes): amortise. For unpredictable repairs: lump-sum.

#### 42.3 Per-Session Costs

Costs incurred per driving session, not per trip: congestion charge (£15/day if entering zone), parking fees at charging locations. Allocated equally across all trips within the session:

```
cᵢ^(session) = C_session / n_session
```

#### 42.4 Total Variable Cost Per Trip

```
cᵢ = cᵢ^(charge) + cᵢ^(wear) + cᵢ^(session)
```

### 43. Vehicle Regime Model

The generalised cost structure enables seamless transition between operational regimes without modifying the CNHR/MNHR framework. Only the parameter vector changes.

**Definition (Cost Regime).** A cost regime ℛ is a parameter tuple:

```
ℛ = (Cf, η, p̄, w)
```

specifying the fixed weekly cost, energy consumption rate, expected charging price, and per-mile wear rate.

#### 43.1 Hire Regime (Current)

| Parameter | Value | Basis |
|---|---|---|
| Cf | £280/wk | Hire (£220) + insurance (£50) + phone (£10) |
| η | 0.28 kWh/mi | e-Niro empirical |
| p̄ | £0.23–0.25/kWh | Off-peak Tesla SC |
| w | £0/mi | Wear is hire company's liability |

Implied weekly variable cost at 400 mi/wk: 400 × 0.28 × 0.24 = £26.88/wk.
Total weekly cost: £280 + £26.88 ≈ £307/wk.

**Remark.** Lower than the current Cw = £430 because the old model included charging as a fixed estimate. The generalised model makes charging explicit and variable.

#### 43.2 Ownership Regime (Hypothetical)

| Parameter | Value | Basis |
|---|---|---|
| Cf | £130/wk (est.) | Finance (£70) + insurance (£40) + phone (£10) + servicing amortised (£10) |
| η | 0.28 kWh/mi | Same vehicle assumed |
| p̄ | £0.23–0.25/kWh | Same charging strategy |
| w | £0.06/mi | Tyres + brakes + contingency |

Implied weekly variable cost at 400 mi/wk: £26.88 + £24.00 = £50.88/wk.
Total weekly cost: £130 + £50.88 ≈ £181/wk.

#### 43.3 Regime Comparison

| Regime | Cf (£/wk) | Variable (£/wk) | Total (£/wk) | H_target |
|---|---|---|---|---|
| Hire (current) | 280 | ~27 | ~307 | 17.1 h* |
| Ownership (est.) | 130 | ~51 | ~181 | 10.1 h* |

*H_target = Cf/(ϱ̄* − r*). Illustrative; requires calibration.

**Remark (Strategic Implication).** Ownership approximately halves total weekly cost and reduces H_target from ~17 to ~10 hours. This transforms the binding constraint: even a two-shift week comfortably reaches target. The hours-dominance finding (Theorem 13.1) remains structurally true but its *severity* is dramatically reduced — the driver gains operational flexibility.

### 44. Shift Variability and Mixed-Pattern Weeks

#### 44.1 The Variable Shift Problem

PHV driving has no fixed schedule. A given week may include any combination of night shifts, day shifts, partial shifts, and rest days.

| Shift Type | Charging Price | Demand | Congestion? | Net Impact |
|---|---|---|---|---|
| Night (19:00–07:00) | Off-peak (£0.23) | Moderate–High | No | Best ϱ* |
| Day (07:00–19:00) | Peak (£0.40+) | Moderate | Yes (£15) | Worst ϱ* |
| Split/Mixed | Blended | Variable | Partial | Middle |

#### 44.2 Why the Framework Handles This Without Modification

Because variable costs are allocated per-trip via cᵢ, a night trip and a day trip naturally have different net earnings eᵢ* even if their platform earnings eᵢ are identical. The CNHR accumulates eᵢ* without requiring any knowledge of shift boundaries.

#### 44.3 Shift-Type MNHR Comparison

The generalised MNHR enables direct comparison of shift profitability:

```
AM(m*_night) ≷ AM(m*_day)
```

If night shifts yield consistently higher AM(m*), the driver has a quantitative basis for shift allocation beyond intuition. This comparison was not possible under the old model.

### 45. Revised Axiom and Parameter Definitions

**Axiom 1 (Revised — Decomposed Cost Structure).** The driver incurs two classes of cost per Uber week:
(a) A **fixed cost** Cf incurred in full regardless of trip volume.
(b) A **variable cost** cᵢ per trip, proportional to committed distance and dependent on the prevailing energy price and wear rate.

The total cost 𝒞(n) = Cf + Σcᵢ replaces the single parameter Cw.

**Definition (Revised Prospective Fixed Cost Rate).**

```
λ̂f = Cf / H̄
```

Replaces λ̂ = Cw/H̄ in all MNHR computations.

**Definition (Variable Cost Rate — Informational).**

```
v̄ = Σcᵢ / Σdᵢ†
```

Realised variable cost per committed mile. Diagnostic metric only.

### 46. Impact on Variance Decomposition

Under the generalised model:

```
rₙ = ϱ̄ₙ* − λf
Var(rₙ) = Var(ϱ̄*) + Var(λf) − 2 Cov(ϱ̄*, λf)
```

Two effects:
(i) Var(λf) is *reduced* relative to Var(λw) because Cf < Cw — the fixed base is smaller.
(ii) Var(ϱ̄*) may *increase* slightly because variable costs introduce trip-level variation.

Net effect: the hours-dominance finding (90.7%) likely reduces to perhaps 75–85%, giving more weight to the net-rate lever. Variable cost management (charging strategy, route efficiency) becomes a *more significant lever* than under the old model.

**Remark (Recalibration Required).** Exact variance shares require per-trip variable cost data. The 90.7% finding remains the best estimate pending recalibration, with the understanding that it likely overstates hours-dominance by absorbing variable cost variance into the residual.

### 47. Implementation Notes

#### 47.1 Additional Data Fields

| Field | Type | Description |
|---|---|---|
| `var_cost` | float | Total variable cost cᵢ (£) for this trip |
| `charge_rate` | float | Effective £/kWh for the charging session |

All existing fields remain unchanged. New computed field `net_earn` = `t_earn` − `var_cost` replaces `t_earn` in all CNHR/MNHR computations.

#### 47.2 Sentinel Integration

For trip acceptance:

```
m̂† = (ê − ĉ) / (t̂ + t̂ᵉⁿ) − λ̂f
```

where ĉ = d̂† · η · p̄ + d̂† · w. Requires distance estimate from Uber offer screen.

#### 47.3 Configuration

```javascript
var REGIME = {
  Cf:    280,         // fixed weekly cost (GBP)
  eta:   0.28,        // kWh per mile
  p_bar: 0.24,        // default charge price (GBP/kWh)
  w:     0.00,        // wear rate (GBP/mile), 0 for hire
  label: 'HIRE_ENIRO' // regime identifier
};
```

Switching regimes requires only updating this block.

---
## Part IX: Cross-Industry Comparison

### 35. The Universal Structure of Transport Profitability

Every transport sector uses a common economic skeleton:

```
Net Rate = Gross Revenue Rate − Cost Rate
```

The CNHR decomposition (rₙ = ϱ̄ₙ − λw) is algebraically identical to this universal identity.

| Concept | Aviation | Trucking | Bus/Fleet | CNHR–MNHR |
|---|---|---|---|---|
| Unit of production | ASM | Mile | Vehicle mile | Engaged hour |
| Revenue metric | RASM | Rev/mile | Rev/bus mile | ϱ̄ |
| Cost metric | CASM | CPM | Cost/bus mile | λ |
| Profitability test | RASM−CASM>0 | Rev/mi−CPM>0 | Rev−Cost>0 | ϱ̄−λ>0 |
| Utilisation | Load factor | Loaded vs DH | % in rev. svc | u = t/t† |
| Marginal signal | None | Informal | None | MNHR + Thm |

### 36. The Aviation Parallel: CASM/RASM

The airline industry's CASM/RASM framework is the most mature transport profitability system in existence.

#### 36.1 Structural Isomorphism

| Aviation | Formula | CNHR Equivalent | Formula |
|---|---|---|---|
| CASM | Op. Cost / ASM | λ | Cw/Tₙ |
| RASM | Revenue / ASM | ϱ̄ | Eₙ/Tₙ |
| Operating Margin/ASM | RASM − CASM | CNHR | ϱ̄ₙ − λw |
| Load Factor | RPM/ASM | Utilisation | Tₙ/Tₙ† |
| Block Hours | Gate-to-gate | True Time (t†) | Engaged + enroute |
| Airborne Hours | In-flight only | Paid Time (t) | Engaged time only |
| BE Load Factor | CASM/Yield | BE Hours | Cw/ϱ̄ |

**Remark.** The isomorphism is not metaphorical. The CNHR decomposition is algebraically identical to airline Operating Margin per ASM = RASM − CASM.

#### 36.2 Block Hours vs. True Time

Aviation distinguishes *block hours* (gate-to-gate, including taxi and idle) from *airborne hours* (wheels-off to wheels-on).

| Aviation Phase | Rideshare Phase | Paid? |
|---|---|---|
| Taxi out | Enroute drive to pickup | No |
| Airborne | Engaged (pickup to dropoff) | Yes |
| Taxi in | Deadhead after dropoff | No |
| Gate idle | Idle waiting for dispatch | No |

At ū⁽ᵀ⁾ = 0.66, the paid CNHR overstates by factor 1.52—the precise analogue of an airline computing profitability on airborne hours only while ignoring taxi time.

#### 36.3 The Volume Imperative

The variance decomposition finding (90.7% from hours) has a direct aviation parallel. Airlines with high fixed costs achieve profitability through *aircraft utilisation* (block hours per aircraft per day), not per-flight yield. A low-cost carrier flying 12 block hours/day at moderate yield outperforms a carrier flying 8 hours/day at high yield. Same finding: **in high-fixed-cost transport, volume dominates per-unit revenue quality.**

### 37. The Trucking Parallel: Cost Per Mile

Owner-operator trucking presents the closest structural analogue. Both are independent operators with high fixed costs, variable revenue per unit, and the fundamental accept/reject decision.

An owner-operator computes CPM by dividing total expenses by total miles. A proposed load is profitable if revenue-per-mile exceeds CPM—structurally identical to mᵢ > 0. Truckers also explicitly distinguish loaded miles (revenue) from deadhead miles (repositioning empty)—the precise parallel to paid/true.

Critical difference: truckers apply this test informally, without cumulative tracking, directional theorem, EMA smoothing, or diagnostic state matrix.

### 38. Contributions Beyond Existing Frameworks

#### 38.1 The Marginal Directional Signal (MNHR)

No transport framework provides a per-unit marginal signal predicting cumulative direction. Theorem 17.1 (mₙ > rₙ ⟹ rₙ₊₁ > rₙ) is a mathematical identity with no equivalent. Combined with EMA (92.5% accuracy), this provides a real-time decision signal no other framework offers.

#### 38.2 Dual-Track Time Accounting with Quantified Gap

While aviation distinguishes block/airborne and trucking distinguishes loaded/deadhead, no framework simultaneously tracks both in real time with a quantified utilisation coefficient. rₙ† = rₙ · ū⁽ᵀ⁾ provides a precise, continuously-updated measure of platform overstatement.

#### 38.3 The Four-State Diagnostic Matrix

Standard transport KPI frameworks present individual metrics without state classification. The SUSTAINED/ACCEL RECOVERY/DECELERATING/STALLED matrix, combining level and momentum, has no precedent in the surveyed literature.

### 39. Comparative Assessment Summary

| Contribution | Nearest Analogue | Advancement |
|---|---|---|
| MNHR directional theorem | Trucker's informal BE check | Mathematical identity; 92.5% DA via EMA |
| Dual-track time accounting | Aviation block/airborne | Continuous real-time + exact utilisation coefficient |
| Four-state diagnostic matrix | Fleet KPI dashboards | Level + momentum combined with decision mapping |
| Variance decomposition | Industry volume intuition | Formal proof: 90.7% cost rate |
| AM–GM volatility measure | No equivalent | Model-free consistency metric |

Within single-agent operational optimisation under partial information, the CNHR–MNHR framework is more rigorous than anything documented in rideshare literature or comparable owner-operator sectors.

---

## Part X: Data Schema and Implementation

### 23. Week Object Schema

| Field | Type | Description |
|---|---|---|
| `label` | string | Week label (e.g., "9 Dec – 15 Dec") |
| `key` | string | ISO week key |
| `total_n` | int | Trip count |
| `total_e` | float | Total portal earnings (£) |
| `total_h` | float | Total engaged hours |
| `total_mi` | int | Total miles |
| `final_rho` | float | Final cumulative ϱ |
| `final_r_n` | float | Final CNHR (paid) |
| `final_r_n_true` | float | Final CNHR (true) |
| `final_mnhr_ema` | float | Final MNHR EMA (paid) |
| `final_mnhr_true_ema` | float | Final MNHR EMA (true) |
| `final_state` | string | Last trip's four-state classification |
| `be_trip` | int/null | Break-even trip number |
| `target_trip` | int/null | Target trip number |
| `am_rho_paid` | float | AM(ϱ) |
| `gm_rho_paid` | float | GM(ϱ) |
| `am_rho_true` | float | AM(ϱ†) |
| `gm_rho_true` | float | GM(ϱ†) |
| `mean_util` | float | Average utilisation (0–1) |
| `mean_enroute` | float | Average enroute time (minutes) |
| `n_true` | int | Trips with paired enroute data |
| `trips` | array | Trip objects |

### 24. Trip Object Schema

| Field | Type | Description |
|---|---|---|
| `n` | int | Sequential trip number within week |
| `day` | string | Day of week |
| `time` | string | Start time HH:MM |
| `dt` | string | ISO datetime |
| `service` | string | Uber service type |
| `t_earn` | float | Trip earnings (£) |
| `t_dur` | float | Trip duration (hours) |
| `t_rho` | float | Trip gross rate: ϱᵢ = eᵢ/tᵢ |
| `t_dist` | float | Trip distance (miles) |
| `mnhr` | float | MNHR (paid): mᵢ = ϱᵢ − λ̂ |
| `mnhr_ema` | float | EMA of MNHR (paid) |
| `mnhr_true` | float | MNHR (true): mᵢ† |
| `mnhr_true_ema` | float | EMA of MNHR (true) |
| `cum_e` | float | Eₙ |
| `cum_h` | float | Tₙ (paid) |
| `cum_h_true` | float | Tₙ† (true) |
| `r_n` | float | CNHR (paid) |
| `r_n_true` | float | CNHR (true) |
| `rho_cum` | float | ϱ̄ₙ |
| `four_state` | string | Diagnostic state |
| `util` | float | uᵢ (utilisation) |
| `enroute_min` | float | Enroute minutes (0 if unavailable) |

### 25. MNHR Computation (ES5 JavaScript)

```javascript
// Constants
var CW = 430;
var ALPHA = 0.15;
var LAMBDA_HAT = CW / MEAN_H;  // prospective cost rate

// Per-trip computation within weekly loop
for (var j = 0; j < w.trips.length; j++) {
  var t = w.trips[j];
  
  // Gross rate
  t.t_rho = t.t_earn / t.t_dur;
  
  // MNHR (paid)
  t.mnhr = t.t_rho - LAMBDA_HAT;
  
  // True variant (if enroute available)
  var t_true = t.t_dur + t.enroute_min / 60;
  t.rho_true = t.t_earn / t_true;
  t.mnhr_true = t.rho_true - LAMBDA_HAT;
  
  // Cumulative
  t.cum_e = (j === 0) ? t.t_earn : w.trips[j-1].cum_e + t.t_earn;
  t.cum_h = (j === 0) ? t.t_dur : w.trips[j-1].cum_h + t.t_dur;
  t.cum_h_true = (j === 0) ? t_true : w.trips[j-1].cum_h_true + t_true;
  
  // CNHR
  t.r_n = (t.cum_e - CW) / t.cum_h;
  t.r_n_true = (t.cum_e - CW) / t.cum_h_true;
  
  // EMA
  if (j === 0) {
    t.mnhr_ema = t.mnhr;
    t.mnhr_true_ema = t.mnhr_true;
  } else {
    t.mnhr_ema = ALPHA * t.mnhr + (1 - ALPHA) * w.trips[j-1].mnhr_ema;
    t.mnhr_true_ema = ALPHA * t.mnhr_true
                    + (1 - ALPHA) * w.trips[j-1].mnhr_true_ema;
  }
  
  // Four-state
  var atTarget = t.r_n >= R_STAR;
  var improving = t.mnhr_true_ema > t.r_n;
  if (atTarget && improving) t.four_state = 'SUSTAINED';
  else if (atTarget && !improving) t.four_state = 'DECELERATING';
  else if (!atTarget && improving) t.four_state = 'ACCEL_RECOVERY';
  else t.four_state = 'STALLED';
}
```

### 26. AM/GM Computation

```javascript
var sum_rho = 0, sum_ln_rho = 0;
var sum_rho_true = 0, sum_ln_rho_true = 0;
var n_true = 0;

for (var j = 0; j < w.trips.length; j++) {
  var t = w.trips[j];
  sum_rho += t.t_rho;
  sum_ln_rho += Math.log(t.t_rho);
  if (t.enroute_min > 0) {
    sum_rho_true += t.rho_true;
    sum_ln_rho_true += Math.log(t.rho_true);
    n_true++;
  }
}

w.am_rho_paid = sum_rho / w.trips.length;
w.gm_rho_paid = Math.exp(sum_ln_rho / w.trips.length);
if (n_true > 0) {
  w.am_rho_true = sum_rho_true / n_true;
  w.gm_rho_true = Math.exp(sum_ln_rho_true / n_true);
}
```

### 28. Offer Object Schema (Sentinel Extension)

The CNHR–MNHR specification operates on *accepted* trips. For empirical testing of endogenous offer dynamics (§18.5.4) and future RL extensions (E9), the Sentinel application must also record *all offers seen*, including those declined.

| Field | Type | Description |
|---|---|---|
| `offer_id` | string | Unique offer identifier |
| `dt` | string | ISO datetime of offer presentation |
| `action` | enum | accepted / declined / expired / cancelled |
| `est_earn` | float | Estimated earnings shown on offer screen (£) |
| `est_dur` | float | Estimated trip duration (hours) |
| `est_dist` | float | Estimated trip distance (miles) |
| `est_enroute` | float | Estimated enroute time (minutes) |
| `est_rho` | float | Estimated ϱᵢ = est_earn / est_dur |
| `est_mnhr` | float | Projected MNHR: m̂† |
| `pickup_zone` | string | Pickup area/zone identifier |
| `dropoff_zone` | string | Dropoff area/zone identifier |
| `surge_mult` | float | Surge multiplier (1.0 = no surge) |
| `service_type` | string | UberX, Comfort, Green, etc. |
| `driver_lat` | float | Driver latitude at offer time |
| `driver_lon` | float | Driver longitude at offer time |
| `session_accept_rate` | float | Running acceptance rate this session |
| `bonus_context` | object | Quest/consecutive bonus state (if active) |

**Remark.** The offer schema captures the *decision environment*, not just the outcome. Minimum recommended data collection period before analysis: 8–12 weeks of offer-level data.

### 29. Data Integrity Invariants

(i) Eₙ = Σᵢ₌₁ⁿ eᵢ (cumulative earnings consistency).
(ii) Tₙ = Σᵢ₌₁ⁿ tᵢ (cumulative hours consistency).
(iii) rₙ = (Eₙ − Cw)/Tₙ (CNHR formula).
(iv) |m̃ₙ − [α·mₙ + (1−α)·m̃ₙ₋₁]| < ε (EMA recurrence).
(v) AM(ϱ) ≥ GM(ϱ) (AM–GM inequality).
(vi) Phase transitions are monotonically ordered: DEFICIT → RECOVERY → TARGET.

---

## Part XI: Visualisation and Display

### 28. Architecture

The dashboard renders as a single-file HTML document with embedded JavaScript (ES5) and CSS. No external dependencies, no build step, no server.

### 29. Chart System

| Mode | X-Axis | Series |
|---|---|---|
| CNHR (Dual-Track) | Trip # | rₙ (paid), rₙ† (true), r* |
| MNHR (EMA) | Trip # | m̃ₙ (paid), m̃ₙ† (true), rₙ |
| Utilisation | Trip # | uᵢ per trip, ū running mean |

### 30. AM/GM Metrics Panel

For each week: AM(ϱ), GM(ϱ), ΔAG, AM(ϱ†), GM(ϱ†), ΔAG†. Colour-coded: green if ΔAG < £2/hr, amber if £2–£4/hr, red if > £4/hr.

### 31. Colour Palette

| Element | Colour | Hex |
|---|---|---|
| CNHR Paid | Amber | #f59e0b |
| CNHR True | Cyan | #06b6d4 |
| MNHR EMA (Paid) | Purple | #a855f7 |
| MNHR EMA (True) | Teal | #14b8a6 |
| Target line | Green | #059669 |
| Break-even | Grey | #64748b |
| Background | Slate-900 | #0f172a |

---

## Appendices

### Appendix A: Notation Reference

| Symbol | Description |
|---|---|
| eᵢ | Trip earnings (£) |
| tᵢ | Trip duration, paid (hours) |
| tᵢᵉⁿ | Enroute time (hours) |
| tᵢ† | True committed time: tᵢ + tᵢᵉⁿ |
| tᵢ‡ | Full committed time: tᵢ + tᵢᵉⁿ + tᵢᵈʰ |
| dᵢ | Trip distance (miles) |
| ϱᵢ | Per-trip gross rate (paid): eᵢ/tᵢ |
| ϱᵢ† | Per-trip gross rate (true): eᵢ/tᵢ† |
| Eₙ | Cumulative earnings after n trips |
| Tₙ | Cumulative paid hours |
| Tₙ† | Cumulative true hours |
| Cw | Weekly fixed cost (£430) |
| H̄ | Mean weekly hours (expanding window) |
| λw | Retrospective cost rate: Cw/Tw |
| λ̂ | Prospective cost rate: Cw/H̄ |
| rₙ | CNHR (paid): (Eₙ − Cw)/Tₙ |
| rₙ† | CNHR (true): (Eₙ − Cw)/Tₙ† |
| mᵢ | MNHR (paid): ϱᵢ − λ̂ |
| mᵢ† | MNHR (true): ϱᵢ† − λ̂ |
| m̃ₙ | EMA-smoothed MNHR (paid) |
| m̃ₙ† | EMA-smoothed MNHR (true) |
| α | EMA smoothing factor (0.15) |
| r* | Target CNHR (£15/hr paid) |
| ϱ̄ₙ | Cumulative gross rate (paid): Eₙ/Tₙ |
| uᵢ | Trip utilisation: tᵢ/tᵢ† |
| ū⁽ᵀ⁾ | Time-weighted utilisation: Tₙ/Tₙ† |
| n_BE | Break-even trip number |
| n* | Target achievement trip number |
| ΔAG | AM–GM gap |
| H_target | Weekly hours target (24 h) |

### Appendix B: Glossary

- **CNHR** — Cost-Normalised Hourly Rate. Net earnings per hour after deducting weekly fixed costs.
- **MNHR** — Marginal Net Hourly Rate. Per-trip net value above the time-proportional cost rate.
- **EMA** — Exponential Moving Average. A smoothing filter with parameter α.
- **Utilisation** — Fraction of committed time that is paid/engaged.
- **Enroute** — Unpaid driving time from dispatch to passenger pickup.
- **Deadhead** — Unpaid repositioning time between trips.
- **Break-Even** — The trip at which cumulative earnings first cover weekly costs.
- **Target** — The trip at which CNHR first reaches r*.
- **Phase** — One of DEFICIT, RECOVERY, or TARGET based on CNHR value.
- **Four-State** — Diagnostic matrix combining CNHR level and MNHR direction.
- **Uber Week** — Monday 04:00 to Sunday 03:59 UTC.
- **PHV** — Private Hire Vehicle (UK licensing category).
- **NLW** — National Living Wage (UK statutory minimum).
- **AM** — Arithmetic Mean.
- **GM** — Geometric Mean.
- **AM–GM Gap** — The non-negative difference AM − GM, a volatility indicator.
- **CASM** — Cost per Available Seat Mile (aviation).
- **RASM** — Revenue per Available Seat Mile (aviation).
- **CPM** — Cost Per Mile (trucking).
- **Block Hours** — Gate-to-gate time in aviation, including taxi and idle.
- **Airborne Hours** — Wheels-off to wheels-on time in aviation.

### Appendix C: Proofs and Derivations

#### C.1 Proof: Uniqueness of Time-Proportional Allocation

**Theorem.** Time-proportional allocation is the unique allocation basis under which:
(a) Each trip's cost burden is cᵢ = λ · tᵢ.
(b) The per-trip MNHR mᵢ = ϱᵢ − λ is constant in the cost rate.
(c) The CNHR is the time-weighted average of MNHRs (Equation 27).

*Proof.* Suppose an allocation basis assigns cost cᵢ = f(xᵢ) · Cw / Σⱼ f(xⱼ) for some function f of trip characteristics xᵢ. Define:

```
mᵢ^(f) = eᵢ/tᵢ − f(xᵢ)·Cw / [tᵢ · Σⱼ f(xⱼ)]
```

For the aggregation identity to hold, we need Σf(xᵢ)/Σf(xⱼ) = 1, which holds trivially. However, for the MNHR to have a constant cost rate (cᵢ/tᵢ independent of i), we need f(xᵢ)/tᵢ = k for all i, which forces f(xᵢ) = k·tᵢ. This is precisely time-proportional allocation. ∎

#### C.2 Proof: AM–GM Gap Approximation for Log-Normal Rates

If ln ϱᵢ ~ N(μ, σ²), then:

```
AM(ϱ) = exp(μ + σ²/2)
GM(ϱ) = exp(μ)
```

Therefore: GM/AM = exp(−σ²/2) ≈ 1 − σ²/2

And: ΔAG = AM − GM = AM(1 − exp(−σ²/2)) ≈ AM · σ²/2. ∎

### Appendix D: Future Extensions

- **E1: Deadhead integration.** Requires GPS/session logging. Algebraic framework ready (§10); only data source pending.
- **E2: Sentinel integration.** Real-time MNHR computation within the Sentinel iOS app for trip acceptance recommendations.
- **E3: Time-of-day analysis.** Decompose MNHR by hour-of-day to identify optimal shift timing.
- **E4: Geospatial analysis.** Correlate enroute times with pickup locations to identify high-utilisation zones.
- **E5: Multi-platform.** Extend to Bolt, FreeNow by generalising cost structure.
- **E6: Dynamic cost rate.** Replace static λ̂ with Bayesian estimate updating as current week's hours accumulate.
- **E7: Confidence intervals.** Bootstrap-based uncertainty bounds on CNHR and MNHR trajectories.
- **E8: Surge/demand overlay.** Correlate MNHR patterns with Uber surge pricing and demand data.
- **E9: Reinforcement learning layer.** Formulate acceptance/repositioning as a POMDP with state Sₙ = (rₙ, m̃ₙ, Aₙ, Gₙ), action aₙ ∈ {accept, decline, reposition}, and reward Rₙ = rₙ₊₁ − rₙ. The CNHR–MNHR framework provides the reward signal and state evaluation; the RL layer would optimise the policy. Requires empirical estimation of platform dispatch response to acceptance behaviour (see §18.5.4).

---

*End of Specification*
