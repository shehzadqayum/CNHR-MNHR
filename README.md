# CNHR–MNHR Model: Formal Specification

### Cost-Normalised Hourly Rate & Marginal Net Hourly Rate — Operational Framework for Rideshare Driver Performance Analytics

[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/)
[![Version](https://img.shields.io/badge/version-3.1-blue.svg)](#changelog)
[![Dataset](https://img.shields.io/badge/dataset-635%2B%20trips-green.svg)](#dataset)

---

## Overview

A first-principles mathematical framework for measuring **true net profitability** of rideshare and private hire vehicle (PHV) driving, after accounting for fixed costs, variable costs, unpaid working time, and rate volatility.

The framework defines two complementary metrics:

- **CNHR** (Cumulative Net Hourly Rate) — aggregate profitability after all costs, tracking the driver's economic state across an Uber week.
- **MNHR** (Marginal Net Hourly Rate) — the net economic contribution of each individual trip, providing a real-time directional signal on whether performance is improving or deteriorating.

Both metrics are computed in **dual-track variants**: *paid* (platform-reported engaged time) and *true* (incorporating unpaid enroute driving time), quantifying the gap between what the platform reports and operational reality.

**Key finding:** At typical utilisation of 66%, platform-reported earnings overstate the driver's true hourly rate by 52%. After deducting weekly fixed costs of £430, a driver seeing £32/hr on the Uber dashboard is actually earning £12.07/hr on all committed time — barely above the UK National Living Wage.

---

## Why This Exists

Rideshare platforms report earnings exclusively in terms of *engaged time* — the interval from passenger pickup to dropoff. This systematically overstates effective hourly earnings by excluding:

1. **Enroute time** — unpaid driving from the driver's location to the passenger pickup point (mean: 15.8 minutes per trip).
2. **Deadhead time** — repositioning between trips without a fare.
3. **Fixed costs** — vehicle hire, insurance, charging, telecommunications — incurred weekly regardless of trip volume.
4. **Variable costs** — energy consumption, tyre wear, congestion charges — scaling with activity.

No existing rideshare analytics tool provides a mathematically rigorous, cost-normalised, dual-track metric system with formal proofs of internal consistency.

---

## Core Results

| Result | Finding | Section |
|---|---|---|
| **Variance Decomposition** | 90.7% of weekly CNHR variance comes from hours worked, not trip quality | §13 |
| **Binding Constraint** | The primary lever for profitability is *volume* (hours), not *selectivity* (trip quality) | §13.3 |
| **Utilisation Gap** | Paid CNHR overstates true CNHR by factor 1/ū ≈ 1.52 at 66% utilisation | §9.3 |
| **Directional Theorem** | Smoothed MNHR predicts CNHR direction with 92.5% accuracy (EMA α=0.15) | §17 |
| **Break-Even Hours** | 24.0 hours/week required at current gross rate to reach £15/hr net target | §13.4 |
| **Charging Optimisation** | Off-peak vs peak charging saves ~£1,700/year | §42.1 |
| **Cost Decomposition** | Generalised model separates fixed (Cf) and variable (cᵢ) costs per trip | Part VIII |
| **Cross-Industry Parallel** | Structural isomorphism with aviation CASM/RASM methodology | Part IX |

---

## Mathematical Framework

### Algebraic Properties

The framework is **distribution-free** and **pathwise exact**. All core results are algebraic identities, not statistical estimates:

- **CNHR Decomposition:** `rₙ = ϱ̄ₙ − λw` (net rate = gross rate minus cost rate)
- **Directional Theorem:** `sign(rₙ₊₁ − rₙ) = sign(ϱₙ₊₁ − rₙ)` — unconditionally true for any trip sequence
- **Aggregation Identity:** CNHR is the exact time-weighted average of per-trip MNHRs under time-proportional cost allocation (unique basis preserving this identity)
- **Utilisation–Rate Identity:** `ϱᵢ† = ϱᵢ · uᵢ` — true rate equals paid rate discounted by utilisation

### Generalised Cost Structure

The specification decomposes total weekly cost into:

```
𝒞(n) = Cf + Σᵢ cᵢ
```

where `Cf` = fixed weekly cost and `cᵢ = cᵢ^(charge) + cᵢ^(wear) + cᵢ^(session)` captures per-trip variable costs. This preserves all algebraic identities via the substitution `eᵢ → eᵢ* = eᵢ − cᵢ`.

### Diagnostic System

A **four-state diagnostic matrix** combines CNHR level and MNHR momentum:

| | MNHR > CNHR (Improving) | MNHR ≤ CNHR (Deteriorating) |
|---|---|---|
| **CNHR ≥ target** | SUSTAINED | DECELERATING |
| **CNHR < target** | ACCEL RECOVERY | STALLED |

### Operational Hierarchy

All stopping decisions follow strict priority: **Week > Session > Trip**. The weekly hours target (derived from variance decomposition) is non-negotiable. Session-level diagnostic states provide guidance subordinated to the weekly hours imperative.

---

## Specification Structure

| Part | Title | Content |
|---|---|---|
| I | Foundations | 5 axioms, 10 primary state variable definitions, conventions |
| II | Core Metrics | Gross rate, CNHR (paid/true), decomposition theorem, cost rate, MNHR, aggregation identity, AM/GM means |
| III | Enroute & Deadhead | Unpaid time taxonomy, three-tier metric hierarchy (paid → true → full), utilisation quantification |
| IV | Empirical Analysis | 635+ trip dataset, 3 regression models, variance decomposition (90.7% hours), binding constraint theorem |
| V | Smoothing & Diagnostics | EMA calibration (α=0.15, 92.5% DA), directional theorem with proof, four-state matrix, operational stopping hierarchy, endogeneity scope boundary |
| VI | Time Horizons | Uber week, session, segmented CNHR, multi-week aggregation |
| VII | Operational Applications | Shift planning, trip acceptance decision support, NLW reconciliation |
| VIII | Generalised Cost Structure | Fixed/variable decomposition, charging cost model, wear model, vehicle regime switching (hire ↔ ownership), shift variability |
| IX | Cross-Industry Comparison | Aviation CASM/RASM isomorphism, trucking CPM, fleet management KPIs, unique contributions |
| X | Data Schema & Implementation | Week/trip/offer object schemas, ES5 JavaScript implementation, data integrity invariants |
| XI | Visualisation | Chart system, colour palette, AM/GM metrics panel |
| App | Appendices | Notation reference (30+ symbols), glossary (20+ terms), uniqueness proof, AM–GM derivation, 9 future extensions |

---

## Dataset

| Metric | Value |
|---|---|
| Total trips | 635+ |
| Uber weeks | 12 |
| Date range | 7 December 2025 – 16 February 2026 |
| Platform | Uber (London, UK) |
| Vehicle | Kia e-Niro EV |
| Licence type | Private Hire Vehicle (PHV) |
| Typical shift | 19:00–07:00 (night) |
| Enroute data coverage | 77–91% per week |
| Mean utilisation | 66% |
| Mean enroute time | 15.8 min/trip |

---

## Files

| File | Description |
|---|---|
| `CNHR_MNHR_Formal_Specification.pdf` | Complete specification (48 pages) |
| `CNHR_MNHR_Formal_Specification.tex` | LaTeX source (main document) |
| `cost_extension.tex` | LaTeX source (Part VIII, `\input` by main) |
| `CNHR_MNHR_Formal_Specification.md` | Full Markdown version |

### Compiling the LaTeX

```bash
# Requires: texlive-base, texlive-latex-extra, texlive-fonts-recommended
pdflatex CNHR_MNHR_Formal_Specification.tex   # pass 1
pdflatex CNHR_MNHR_Formal_Specification.tex   # pass 2 (TOC/refs)
pdflatex CNHR_MNHR_Formal_Specification.tex   # pass 3 (stable)
```

Both `.tex` files must be in the same directory. The main document uses `\input{cost_extension}`.

---

## Scope Boundaries

The specification explicitly documents what it does and does not do:

| In Scope | Out of Scope |
|---|---|
| Realised performance evaluation | Platform dispatch algorithm modelling |
| Per-trip marginal contribution | Optimal acceptance policy under feedback |
| Dual-track time accounting (paid/true) | Full deadhead tracking (pending Sentinel) |
| Cost decomposition (fixed + variable) | Predictive offer flow modelling |
| Four-state diagnostic classification | Reinforcement learning policy optimisation |
| Cross-industry structural comparison | Multi-platform aggregation |
| Empirical calibration from operational data | Risk-sensitive objective functions |

The framework is **observationally closed but causally open** — it evaluates any realised trip sequence exactly, but does not model how driver actions shape future offer distributions. This boundary is a deliberate architectural choice, not a deficiency (§18.5.4).

---

## Related Concepts

This framework draws structural parallels with established transport economics:

- **Aviation:** CASM (Cost per Available Seat Mile) / RASM (Revenue per Available Seat Mile) — the CNHR decomposition is algebraically isomorphic to airline operating margin per ASM.
- **Trucking:** Cost Per Mile (CPM) for owner-operators — loaded vs deadhead miles parallel paid vs true time.
- **Fleet Management:** Vehicle utilisation KPIs — block hours vs airborne hours parallel true vs paid time.
- **Quantitative Finance:** AM–GM gap as a model-free volatility measure — analogous to geometric Brownian motion variance interpretation.

---

## Future Extensions

| ID | Extension | Status |
|---|---|---|
| E1 | Deadhead integration via GPS logging | Pending Sentinel |
| E2 | Sentinel real-time MNHR computation | In development |
| E3 | Time-of-day MNHR decomposition | Specification ready (§22) |
| E4 | Geospatial zone analysis | Specification ready (§22) |
| E5 | Multi-platform support (Bolt, FreeNow) | Framework-ready |
| E6 | Dynamic (Bayesian) cost rate estimation | Planned |
| E7 | Bootstrap confidence intervals on CNHR | Planned |
| E8 | Surge/demand overlay | Planned |
| E9 | Reinforcement learning policy layer (POMDP) | Scoped (§18.5.4) |

---

## Sentinel App

The **Sentinel** iOS application (in development) is the observation and decision-support layer built on the CNHR–MNHR specification. It uses:

- **ReplayKit** screen capture of the Uber driver app
- **Vision framework** OCR to extract trip offer parameters in real-time
- **Accessibility frameworks** for hands-free voice advisory during active driving

Sentinel extends the specification's measurement capabilities by observing what the specification cannot assume: declined offers, surge state, zone context, acceptance rate, and bonus structures. It provides the data pipeline required for Tier 3 (Full) time accounting and the empirical foundation for future RL extensions.

---

## Keywords

`rideshare analytics` · `driver profitability` · `cost-normalised hourly rate` · `marginal net hourly rate` · `Uber driver earnings` · `gig economy analytics` · `PHV driver performance` · `enroute time accounting` · `deadhead time` · `utilisation rate` · `CASM RASM parallel` · `transport economics` · `EV charging optimisation` · `trip acceptance optimisation` · `four-state diagnostic` · `EMA smoothing` · `variance decomposition` · `geometric mean volatility` · `AM-GM inequality` · `fixed variable cost decomposition` · `London private hire` · `Kia e-Niro` · `night shift optimisation` · `rideshare cost accounting` · `gig worker analytics` · `driver decision support` · `operational framework`

---

## Changelog

| Version | Date | Changes |
|---|---|---|
| 1.0 | 7 Feb 2026 | Initial CNHR framework |
| 2.0 | 16 Feb 2026 | MNHR, dual-track, EMA, four-state matrix, variance decomposition |
| 3.0 | 23 Feb 2026 | Operational hierarchy, cross-industry comparison, Feynman explanation |
| 3.1 | 25 Feb 2026 | Generalised cost structure (Part VIII), §17.2 prospective/retrospective analysis, §18.5.4 endogeneity scope boundary, offer schema, segmented CNHR, AM–GM distributional caveat, Tier 3 bias characterisation |

---

## Author

**Shehzad Qayum** — PHV driver (London), ESOL tutor, and developer of the Sentinel trip advisory system.

---

## Citation

```
Qayum, S. (2026). CNHR–MNHR Model Formal Specification: Cost-Normalised Hourly Rate
& Marginal Net Hourly Rate — Integrated Mathematical, Statistical & Operational
Framework for Rideshare Driver Performance Analytics. v3.1, 25 February 2026.
```

---

## Licence

This work is licensed under [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/). You may share and adapt with attribution for non-commercial purposes under the same licence.
