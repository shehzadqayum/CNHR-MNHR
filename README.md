# CNHR–MNHR Model

### Cost-Normalised Hourly Rate & Marginal Net Hourly Rate

**An Operational Framework for Rideshare Driver Performance Analytics**

---

## The Problem

Rideshare platforms report driver earnings in terms of *engaged time* — the interval from passenger pickup to dropoff. This metric is structurally misleading for three reasons:

**1. It excludes unpaid working time.** Every trip requires the driver to travel from their current location to the passenger — unpaid. At a measured mean of 15.8 minutes per trip and 66% utilisation, one-third of the driver's committed working time is invisible to the platform's reported rate.

**2. It ignores the cost of being in business.** Vehicle hire, insurance, charging, telecommunications — these costs are incurred weekly regardless of how many trips are completed. A driver who works 15 hours sees a very different net position from one who works 30 hours, even at the same gross hourly rate, because fixed costs are amortised over fewer hours.

**3. It provides no marginal signal.** The platform tells a driver their average rate. It does not tell them whether the *last trip* helped or hurt their overall position, whether their current trajectory will reach profitability, or whether to continue driving or stop.

A driver observing £32/hr on the Uber dashboard may, after accounting for unpaid enroute time and weekly fixed costs, be earning £12.07/hr on all committed time — barely above the UK National Living Wage.

No existing rideshare analytics tool addresses all three deficiencies within a mathematically consistent framework.

---

## The Framework

The CNHR–MNHR model is a first-principles analytical system that measures what the platform does not: **true net profitability per hour of total committed work, with a real-time signal on whether each trip is improving or deteriorating that position.**

It defines two complementary metrics:

**CNHR (Cost-Normalised Hourly Rate)** — the driver's cumulative net earnings per hour after deducting all fixed and variable costs. This is the answer to: *"What am I actually earning?"*

```
rₙ = (Eₙ − Cf − Σcᵢ) / Tₙ
```

**MNHR (Marginal Net Hourly Rate)** — the net economic contribution of each individual trip after its share of costs. This is the answer to: *"Did that trip help or hurt?"*

```
mᵢ = (eᵢ − cᵢ) / tᵢ − λ̂f
```

Both metrics are computed in **dual-track variants**:

- **Paid** — using platform-reported engaged time only, for comparison with platform figures.
- **True** — incorporating unpaid enroute driving time, reflecting operational reality.

The gap between paid and true variants quantifies exactly how much the platform overstates the driver's effective rate.

---

## Core Principles

### 1. Costs Are Real and Must Be Deducted

The framework decomposes total weekly cost into a fixed component (incurred regardless of activity) and a variable component (scaling with each trip):

```
𝒞(n) = Cf + Σᵢ cᵢ
```

Variable costs include energy consumption (modelled per-mile at the actual charging price), distance-proportional wear, and per-session costs such as congestion charges. This structure handles vehicle hire, personal ownership, and regime transitions without modifying any formula — only the parameter values change.

### 2. Unpaid Time Is Working Time

Platform-reported "hourly rates" use only the time the driver has a passenger in the vehicle. The CNHR–MNHR model tracks total committed time: engaged time plus the unpaid drive to pickup. At the empirical mean utilisation of 66%, the platform overstates the driver's true rate by a factor of 1.52.

### 3. The Binding Constraint Is Hours, Not Trip Quality

The framework's variance decomposition reveals a counterintuitive finding: **over 90% of week-to-week variation in net hourly rate comes from how many hours the driver works, not from the quality of individual trips.** The gross hourly rate is relatively stable (CV ≈ 15%); the cost rate is highly volatile (CV ≈ 90%) because it is inversely proportional to hours.

A driver obsessing over individual trip quality addresses under 3% of their earnings variance. Working an additional five hours per week addresses the dominant source. Across 13 weeks, gross ρ̄ ranges only from £28.51 to £45.67 (1.6× spread), while hours range from 4.16h to 39.78h (9.6× spread). The cost rate, not the revenue rate, determines weekly outcomes.

### 4. Every Trip Has a Directional Signal

The **Directional Theorem** — an unconditional algebraic identity, not a statistical estimate — establishes that if the marginal trip's gross rate exceeds the current cumulative net rate, then the cumulative rate rises; if it falls below, the cumulative rate falls. Smoothed with an exponential moving average (α = 0.15), this signal predicts the correct direction of CNHR movement 92.5% of the time.

This gives the driver something no platform provides: a real-time answer to *"is my last trip pulling me up or dragging me down?"*

### 5. The Framework Is Distribution-Free

All core results — the CNHR decomposition, the Directional Theorem, the Aggregation Identity, the utilisation–rate relationship — are algebraic identities that hold for any sequence of trips, however generated. No distributional assumptions, no stationarity requirements, no ergodicity conditions. The framework evaluates any realised trip stream exactly.

---

## Diagnostic System

The framework combines CNHR level (above or below target?) with MNHR momentum (improving or deteriorating?) into a four-state diagnostic matrix:

| | Recent trips improving CNHR | Recent trips deteriorating CNHR |
|---|---|---|
| **CNHR at or above target** | **SUSTAINED** | **DECELERATING** |
| **CNHR below target** | **ACCEL RECOVERY** | **STALLED** |

These states provide session-level guidance governed by a strict operational hierarchy: weekly hours targets take absolute priority over session-level decisions. The variance decomposition proves that volume dominates — so no session-level signal may reduce projected weekly hours below the required threshold.

---

## Context: Where This Sits in Transport Economics

The CNHR–MNHR framework is not without precedent. Every transport sector faces the same fundamental problem: measuring net profitability when revenue is earned per unit of output but costs are incurred over time.

**Aviation** uses CASM (Cost per Available Seat Mile) and RASM (Revenue per Available Seat Mile). The CNHR decomposition `rₙ = ϱ̄ₙ − λ` is algebraically isomorphic to the airline operating margin identity `Margin/ASM = RASM − CASM`. The variance decomposition finding — that volume dominates per-unit revenue quality — has a direct parallel: airlines with high fixed costs achieve profitability through aircraft utilisation (block hours per day), not per-flight yield. A low-cost carrier flying 12 block hours/day at moderate yield outperforms one flying 8 hours/day at premium yield. Same finding, same economics.

**Trucking** owner-operators compute Cost Per Mile (CPM) and evaluate loads against it — structurally identical to the MNHR positivity test. They also explicitly distinguish loaded miles (revenue) from deadhead miles (repositioning empty), paralleling the paid/true distinction.

**Fleet management** tracks vehicle utilisation KPIs with the same block-hours-versus-revenue-hours structure.

What the CNHR–MNHR framework contributes beyond these established systems:

| Contribution | Nearest Analogue | Advancement |
|---|---|---|
| MNHR directional theorem | Trucker's informal break-even check | Mathematical identity with 92.5% predictive accuracy via EMA |
| Dual-track time accounting | Aviation block/airborne hours | Continuous real-time tracking with exact utilisation coefficient |
| Four-state diagnostic matrix | Fleet KPI dashboards | Level + momentum combined with decision mapping |
| Variance decomposition | Industry volume intuition | Formal proof: hours dominate, not trip quality |
| AM–GM volatility measure | No equivalent | Model-free rate consistency metric with economic interpretation |

Within single-agent operational optimisation under partial information, the framework is more rigorous than anything documented in rideshare literature or comparable owner-operator sectors.

---

## What the Framework Does and Does Not Do

| The framework provides | The framework does not provide |
|---|---|
| Exact measurement of realised net profitability | Prediction of future trip offer quality |
| Per-trip marginal contribution with directional signal | Optimal acceptance policy under platform feedback dynamics |
| Dual-track time accounting (paid vs true) | Modelling of platform dispatch algorithms |
| Cost decomposition across vehicle regimes | Risk-sensitive objective functions |
| Four-state diagnostic classification | Causal inference on driver–platform interactions |
| Cross-segment comparison (time-of-day, zone, service type) | Multi-platform aggregation |

The framework is **observationally closed but causally open**: it evaluates any realised trip sequence exactly, but does not model how driver actions shape future offer distributions. This is a deliberate architectural choice — the platform's dispatch algorithm is unobservable, non-stationary, and possibly personalised. Any model of platform response would be speculative. The framework's strength is that it operates purely on observables.

---

## Empirical Basis

All parameters are calibrated from operational data, not theoretical assumptions.

| Metric | Value |
|---|---|
| Total trips | 628 |
| Uber weeks | 13 (12 complete + 1 in progress) |
| Period | December 2025 – February 2026 |
| Platform | Uber (London, UK) |
| Vehicle | Kia e-Niro EV (PHV) |
| Typical shift | 19:00–07:00 (night) |
| Mean gross ρ̄ | £32.77/hr (CV 14.7%) |
| Mean portal hours | 22.15h/week (CV 48.5%) |
| Mean cost rate λ | £28.26/hr (CV 89.8%) |
| Weekly fixed cost | £430 (hire regime) |
| EMA smoothing factor | α = 0.15 |
| Directional accuracy | 92.5% |
| Hours for r*=£15 | 24.2h/week at mean ρ̄ |
| Hours for break-even | 13.1h/week at mean ρ̄ |

Service-type distribution: UberX 51%, Electric 43%, Comfort 4%, UberX Priority 2%. Comfort yields the highest gross rate (£37.96/hr) but lowest volume. UberX and Electric perform near-identically at £32.31 and £32.01/hr respectively.

Weeks reaching break-even (rₙ ≥ 0): 8 of 13. Weeks reaching target (rₙ ≥ r*=£15): 6 of 13. The six profitable weeks all logged 23+ portal hours, confirming the binding constraint theorem. No week below 15 portal hours reached break-even.

---

## Specification Structure

The complete specification (48 pages) is organised into eleven parts plus appendices:

| Part | Title | Content |
|---|---|---|
| I | Foundations | Axioms, primary state variables, conventions |
| II | Core Metrics | CNHR, MNHR, decomposition theorem, aggregation identity, AM/GM means |
| III | Enroute & Deadhead | Unpaid time taxonomy, three-tier metric hierarchy, utilisation quantification |
| IV | Empirical Analysis | Regression models, variance decomposition, binding constraint theorem |
| V | Smoothing & Diagnostics | EMA calibration, directional theorem, four-state matrix, operational hierarchy |
| VI | Time Horizons | Uber week, session, segmented CNHR, multi-week aggregation |
| VII | Operational Applications | Shift planning, trip acceptance, NLW reconciliation |
| VIII | Generalised Cost Structure | Fixed/variable decomposition, charging model, vehicle regime switching |
| IX | Cross-Industry Comparison | Aviation CASM/RASM, trucking CPM, unique contributions |
| X | Data Schema & Implementation | Object schemas, reference implementation, data integrity invariants |
| XI | Visualisation | Chart system, colour palette, metrics panel |
| App | Appendices | Notation reference, glossary, proofs, future extensions |

---

## Files

| File | Description |
|---|---|
| `CNHR_MNHR_Formal_Specification.pdf` | Complete specification (48 pages) |
| `CNHR_MNHR_Formal_Specification.md` | Full Markdown version |
| `CNHR_MNHR_Dashboard_v2.2.html` | Interactive dashboard (13 weeks, 628 trips, self-contained) |

---

## Keywords

`rideshare analytics` · `driver profitability` · `cost-normalised hourly rate` · `marginal net hourly rate` · `Uber driver earnings` · `gig economy analytics` · `PHV driver performance` · `enroute time accounting` · `deadhead time` · `utilisation rate` · `CASM RASM parallel` · `transport economics` · `EV charging optimisation` · `trip acceptance optimisation` · `four-state diagnostic` · `EMA smoothing` · `variance decomposition` · `geometric mean volatility` · `AM-GM inequality` · `fixed variable cost decomposition` · `London private hire` · `rideshare cost accounting` · `gig worker analytics` · `driver decision support` · `operational framework` · `owner-operator economics`

---

## Author

**Shehzad Qayum** — PHV driver and analytical framework developer, London, UK.

---

## Citation

```
Qayum, S. (2026). CNHR–MNHR Model Formal Specification: Cost-Normalised Hourly Rate
& Marginal Net Hourly Rate — Integrated Mathematical, Statistical & Operational
Framework for Rideshare Driver Performance Analytics.
```

---

## Licence

This work is licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/). You may share and adapt with attribution for non-commercial purposes under the same licence.
