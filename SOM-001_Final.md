# SUSTAINABLE OPERATIONS MODEL

**SOM-001 Final**

PHV Day Shift Operations · Kia e-Niro · Uber London · 2:1 Cycle Pattern

Author: Shehzad Qayum  
Date: March 2026  
Classification: Operational · Derived from CNHR-MNHR Dashboard v2.3  
Based on 735 trips across 13 weeks (Nov 2025 – Feb 2026)

---

## Change Log

| Version | Date | Change |
|---------|------|--------|
| v1.0 | Mar 2026 | Initial release. Night shift model (23:00–09:00). |
| v2.0 | Mar 2026 | Transition to day shift model (04:00–14:00). Revised shift window, rest day alignment, sleep/health optimisation, updated financial projections. Added trial protocol. |
| v2.1 | Mar 2026 | Rest days revised from Wed+Sat to Wed+Sun based on structural demand analysis. Night-shift carry-over artefact identified. Added Section 6 (Structural Demand Analysis). |
| v2.2 | Mar 2026 | Financial model rebuilt from first principles. Cost decomposition, charging sensitivity, £1-earned retention analysis (53.5p net per £1 gross). |
| **Final** | **Mar 2026** | **Consolidated self-contained edition. Added Section 2 (Data Basis): CNHR-MNHR definitions, 735-trip data source, binding constraint analysis. Expanded Section 10 (Mathematical Foundation): full CNHR derivative, attractor/inertia/gap framework, closed-form solution. Expanded Section 7 (Monitoring): linear target rationale. All cross-references resolved.** |

---

## 1. Executive Summary

This document is the consolidated, self-contained Sustainable Operations Model for PHV day shift operations on the Uber platform. It supersedes the v1.0 night model and all interim versions (v2.0–v2.2) by combining operational design, financial analysis, mathematical theory, and monitoring protocol into a single reference.

All quantitative findings derive from 735 trips across 13 weeks of verified data (Section 2). The fundamental insight is that hours worked—not trip quality—is the binding constraint on earnings, which drives the model's emphasis on sustainable structure over trip optimisation.

The core design principle: structure eliminates decision fatigue. The monitoring system reduces to a single number: **£192 per working day.** The shift window moves from 23:00–09:00 to **04:00–14:00**, rest days are Wednesday and Sunday (calibrated to structural demand, not historical rates), and the driver sleeps 20:00–04:00 every night.

The financial model shows a 53.5p net retention rate per £1 earned. Fixed vehicle costs (£953/month) dominate; charging is just 3.8% of gross. The transition from nights costs approximately £336/month (15.9% of night model profit) in exchange for circadian health, reduced accident risk, and a 50% increase in study capacity.

| Metric | Value | Basis |
|--------|-------|-------|
| Daily earnings target | **£192 (floor)** | NLW × engaged ratio × shift hours + fixed cost ÷ 5 |
| Daily earnings (expected) | **£231** | 04:00–14:00 at ρ = £33.69/hr |
| Weekly earnings target | **£958** | 5 × £192 (NLW floor) |
| Weekly earnings (expected) | **£1,157** | 5 × £231 |
| Annual net income | **£21,400** | Expected ρ, 2:1 cycle, 35 working weeks |
| Monthly net income | **£1,783** | Annual ÷ 12 |
| CNHR (engaged hours) | **£16.80/hr** | 37.5% above NLW (£12.21) |
| Study hours to Nov 2026 | **872+** | Afternoons free; exceeds night model |
| Shift window | **04:00–14:00** | Captures 04–08 premium; avoids afternoon trough |
| Sleep window | **20:00–04:00** | 8 hours, circadian-aligned |

---

## 2. Data Basis and Analytical Framework

### 2.1 Data Source

All quantitative findings in this document derive from the **CNHR-MNHR Dashboard v2.3**, which contains 735 trips recorded across 13 consecutive weekly cycles (1 December 2024 – 2 March 2025). The data was collected during predominantly night-shift operations (23:00–09:00) and includes GPS-verified trip durations, fare breakdowns, deadhead time, and charging costs. The dashboard was subjected to a full audit confirming all calculations are correct and internally consistent.

### 2.2 CNHR and MNHR Defined

**Cumulative Net Hourly Rate (CNHR)** is the rolling net earnings per trip hour from the start of a weekly cycle, after deducting the fixed weekly cost (C_W = £430). At trip *n*:

> CNHR(n) = (cumulative earnings − C_W) / cumulative trip hours

**Marginal Net Hourly Rate (MNHR)** is the instantaneous rate of a single trip: fare divided by trip duration. MNHR is volatile (ranging from £10 to £80 across individual trips). CNHR is stable (locked within ±£2 of its final value after 30 shift hours).

The engaged-hours variant CNHR(e) adjusts for unpaid time between trips by applying an engagement multiplier α = 1.26. This yields the net rate per hour of platform availability, which is the correct comparator against the National Living Wage (NLW, £12.21/hr as of April 2025).

### 2.3 The Binding Constraint

The 13-week audit established a fundamental finding: **hours worked, not trip quality, is the binding constraint on earnings.** Trip rate ρ varies by ±10–15% week-to-week, but total shift hours vary by 2–3× (from 20h to 55h per week). A regression of weekly CNHR(e) against shift hours shows that approximately 90% of the variation in net earnings is attributable to hours worked. Trip-by-trip selection contributes the remainder.

This means the model must optimise for sustainable hours above all else. Trip rate improvements (surge-chasing, area selection) offer diminishing returns compared to maintaining consistent 50h weeks. The 2:1 cycle, rest day structure, and sleep protocol all serve this principle.

### 2.4 Key Constants

| Constant | Value |
|----------|-------|
| C_W (fixed weekly cost) | £430 working / £220 off |
| NLW (National Living Wage) | £12.21/hr |
| α (engagement multiplier: engaged hrs / trip hrs) | 1.26 |
| r (trip ratio: trip hrs / shift hrs) | 0.687 |
| k (composite: r × α) | 0.866 |
| Target shift hours per working week | 50h (5 × 10h) |
| NLW-compliant minimum weekly earnings | £958 |
| NLW-compliant minimum daily earnings | £192 |

---

## 3. Design Rationale: Why Day Shifts

The prior night model (23:00–09:00) optimised for peak ρ at £37.08/hr. The day model accepts a lower ρ of £33.69/hr in exchange for structural benefits that the financial model cannot capture.

### 3.1 Health and Safety

Night shift driving between 02:00–05:00 carries the highest accident risk in the TfL PHV data. Chronic circadian disruption is linked to cardiovascular disease, metabolic disorders, and cognitive impairment. The day model eliminates all driving during the 02:00–05:00 fatigue window and restores a natural sleep–wake cycle.

### 3.2 Study Capacity

A 04:00–14:00 shift releases 14:00–20:00 (6 hours) every working day for study. This is high-quality, daylight study time—materially better than the fragmented pre-sleep blocks available under the night model. On rest days, the full day is available. The cybersecurity traineeship deadline (November 2026) makes this the higher-priority consideration.

### 3.3 Financial Cost

| | Night (v1.0) | Day (current) |
|---|---|---|
| ρ (trip rate) | £37.08/hr | £33.69/hr |
| Daily earnings (10h shift) | £255 | £231 |
| Monthly net profit | £2,120 | £1,783 |
| Annual net profit | £25,435 | £21,400 |
| CNHR (engaged) | £19.49/hr | £16.80/hr |
| NLW margin | £7.28/hr | £4.59/hr |
| **Transition cost** | | **£336/month (−15.9%)** |

The £336/month cost is the price of sleeping every night, studying every afternoon, and materially reducing driving risk. The model remains NLW compliant with £4.59/hr headroom—sufficient to absorb seasonal variation.

---

## 4. The 2:1 Cycle

The operating cycle spans three weeks: two working weeks followed by one off-week. This yields 35 working weeks and 17 off-weeks per year (17.3 complete cycles).

### 4.1 Weekly Pattern

Rest days are aligned with the two lowest structural demand days in the 04:00–14:00 window, corrected for night-shift carry-over artefacts (see Section 6), subject to the constraint that no more than 3 consecutive shifts occur. The analysis identifies Wednesday (structural score 11/20) and Sunday (structural score 6/20) as the optimal rest days.

| | Mon | Tue | Wed | Thu | Fri | Sat | Sun |
|---|---|---|---|---|---|---|---|
| **Wk 1** | SHIFT | SHIFT | **REST** | SHIFT | SHIFT | SHIFT | **REST** |
| **Wk 2** | SHIFT | SHIFT | **REST** | SHIFT | SHIFT | SHIFT | **REST** |
| **Wk 3** | OFF | OFF | OFF | OFF | OFF | OFF | OFF |

Maximum consecutive shifts: 3 (Thu–Sat). This satisfies fatigue limits while maintaining 5-day earning capacity.

### 4.2 Daily Schedule

| Time | Activity |
|------|----------|
| 03:45 | Wake, prepare |
| 04:00–14:00 | **Shift (10 hours)** |
| 14:00–15:00 | Transition (lunch, decompress) |
| 15:00–20:00 | **Study block (5 hours)** |
| 20:00–04:00 | **Sleep (8 hours)** |

### 4.3 Fortnightly Totals

| | Standard (10 days) | With sacrifice (11 days) |
|---|---|---|
| Working days | 10 | 11 |
| Rest days | 4 | 3 |
| Shift hours | 100h | 110h |
| Expected earnings | £2,132 | £2,341 |
| Fortnightly cost | £860 | £860 |
| Fortnightly net | £1,272 | £1,481 |

---

## 5. Shift Specification

### 5.1 Fixed Parameters

**Shift length: 10 hours (04:00–14:00). Non-negotiable.** This window captures the early-morning premium (04:00–08:00, ρ ≈ £37/hr) and the mid-morning volume (08:00–14:00, ρ ≈ £28–32/hr), while terminating before the low-yield afternoon trough (14:00–16:00, ρ ≈ £27/hr).

### 5.2 Earning Rate by Time Block

| Time Block | Trips | Trip Hours | ρ (trip) | In Window? |
|------------|-------|------------|----------|------------|
| 04:00–08:00 | 108 | 55.4h | **£36.60/hr** | ✅ YES |
| 08:00–12:00 | 45 | 24.4h | £27.89/hr | ✅ YES |
| 12:00–14:00 | 36 | 16.0h | £27.56/hr | ✅ YES |
| 14:00–16:00 | 71 | 32.6h | £27.45/hr | ❌ NO — cut off |
| **Blended 04:00–14:00** | **189** | **95.9h** | **£32.92/hr** | |

The 04:00–08:00 block produces £36.60/hr—comparable to peak night rates—driven by airport runs, early commuter demand, and reduced driver supply. This premium anchors the day model's viability.

### 5.3 Day-of-Week Variation

| Day | ρ (trip) | Est Daily | Trips | Trip Hrs | Status |
|-----|----------|-----------|-------|----------|--------|
| Sunday | £36.03 | £248 | 39 | 16.5h | **REST** † |
| Friday | £35.91 | £247 | 6 | 3.5h | WORK |
| Tuesday | £35.32 | £243 | 37 | 15.6h | WORK |
| Monday | £34.52 | £237 | 22 | 13.5h | WORK |
| Thursday | £29.26 | £201 | 33 | 18.8h | WORK |
| Saturday | £31.64 | £217 | 27 | 14.3h | WORK * |
| Wednesday | £30.40 | £209 | 25 | 13.6h | **REST** |

\* Saturday's historical ρ (£31.64) is partially inflated by night-shift carry-over. Under a dedicated 04:00-start pattern, the 04:00–08:00 block is structurally weaker. An estimated 30% discount applies. Saturday is retained because it still outperforms Sunday structurally.

† Sunday's historical ρ (£36.03) is the most heavily inflated day (+57% above structural expectation). Sunday 04:00–08:00 is structurally dead—zero organic demand, 60–90 minutes of dead time. See Section 6 for the full structural demand analysis.

---

## 6. Structural Demand Analysis

The historical trip data records weekend early-morning trips (04:00–08:00) at high ρ values: Saturday £36/hr, Sunday £38/hr. However, these trips were captured during night shifts that started at 20:00–22:00 the previous evening. A driver already on the road at 03:00 receives trips from passengers who happen to request at 04:00—this is fundamentally different from a driver cold-starting from home at 04:00 on a weekend morning.

### 6.1 The Carry-Over Artefact

Under a dedicated 04:00-start pattern, there is no warm-up period, no pre-positioning near high-demand zones, and no continuation of night-shift momentum. Weekend 04:00–06:00 has near-zero organic demand: no commuters, no school run, minimal flights. The first real trip on a Saturday or Sunday morning is unlikely before 05:30–06:30, implying 30–90 minutes of dead time that the historical data does not reflect.

### 6.2 Structural Demand Scoring

Each day is scored by 2-hour block on a 0–4 scale based on real-world demand drivers: airport schedules, commuter patterns, school runs, business travel, and leisure demand.

| Day | 04–06 | 06–08 | 08–10 | 10–12 | 12–14 | Total /20 |
|-----|-------|-------|-------|-------|-------|-----------|
| Mon | 3 Strong | **4 Peak** | 3 Strong | 2 Mod | 2 Mod | **14** |
| Tue | 3 Strong | **4 Peak** | 3 Strong | 2 Mod | 2 Mod | **14** |
| Wed | 3 Strong | 3 Strong | 2 Mod | 1 Weak | 2 Mod | 11 |
| Thu | 3 Strong | **4 Peak** | 2 Mod | 1 Weak | 2 Mod | 12 |
| Fri | 3 Strong | 3 Strong | 2 Mod | 1 Weak | 1 Weak | 10 |
| Sat | 1 Weak | 1 Weak | 2 Mod | 2 Mod | 2 Mod | 8 |
| Sun | **0 Dead** | 1 Weak | 1 Weak | 2 Mod | 2 Mod | **6** |

### 6.3 Historical vs Structural Comparison

| Day | Hist ρ | Hist Rank | Struct Score | Struct Rank |
|-----|--------|-----------|--------------|-------------|
| Sunday | £36.03 | 2nd | 6/20 | **7th (LAST)** |
| Saturday | £31.64 | 5th | 8/20 | 6th |
| Friday | £35.91 | 3rd | 10/20 | 5th |

Sunday's inflation is +57%: historical ρ ranks it 2nd, structural demand ranks it last. This is the carry-over artefact in its most extreme form.

### 6.4 Demand Drivers by Day

**Weekday 04:00–08:00:** Airport early flights (Heathrow, City, Stansted), hospital shift changes, pre-commute business travel, school run from 07:30. This is the primary revenue driver. Peak commute demand (06:00–08:00) generates the highest trip density and longest average fares.

**Saturday 04:00–08:00:** Minimal—no commuters, no school run, limited flights. Some airport demand from leisure travellers. Organic demand begins ~08:00. Working Saturday is viable but discounted (~30% lower 04–08 rate vs weekdays).

**Sunday 04:00–08:00:** Structurally dead—the lowest demand slot of the entire week. No commuters, no school, minimal flights. A driver starting at 04:00 faces 60–90 minutes of dead time before any trip materialises. Demand builds only after 10:00.

---

## 7. Monitoring System

### 7.1 The Single Number

The monitoring target is: **£192 per working day.** This is the NLW compliance floor, not the expected outcome. At the day model's expected ρ of £33.69/hr, daily earnings are £231—providing £39/day of headroom above the floor.

### 7.2 Why a Linear Target

Three monitoring approaches were tested against all 13 weeks of historical data and rejected before arriving at the linear daily target:

**Real-time CNHR thresholds fail** because CNHR is always below NLW for the first 3–4 days of every week. This is not a signal of distress—it is the mathematical consequence of recovering £430 in fixed costs from zero. A threshold-based system would trigger false alarms on every week, without exception.

**Slope projection fails** because df/ds is proportional to 1/s (see Section 10.2). The slope at hour 5 is +324. At hour 20 it is +2.4. At hour 50 it is +0.2. Linear extrapolation of a decelerating function overshoots by three orders of magnitude in the early phase and converges only when it is too late to be useful.

**Trip rate (ρ) extrapolation fails** because ρ has a within-week profile shaped by variable shift lengths in the historical data. Under a fixed 10h shift, this variability is eliminated, and daily earnings become approximately linear.

**The linear target survives** because it asks the simplest possible question: "at my current pace, will I reach £958 by the end of the week?" With fixed 10-hour shifts, the answer scales linearly: £192 per day, cumulated. This requires no computation on shift—just a comparison of two numbers. The analytical justification for this simplicity is in Section 10.6.

### 7.3 Operational Protocol

At the end of each shift, compute: total earnings this week ÷ days worked. Compare against £192.

| After | Cumulative Target | If Below | If Above |
|-------|-------------------|----------|----------|
| Day 1 (10h) | **£192** | Note. Early signal only. | On track. |
| Day 2 (20h) | **£384** | Trend forming. No action yet. | On track. |
| Day 3 (30h) | **£576** ⚠️ | **DECISION: flag rest day sacrifice.** | Rest days protected. ✅ |
| Day 4 (40h) | **£768** | Confirm sacrifice if still below. | On track. |
| Day 5 (50h) | **£958** | Record. Assess fortnight position. | NLW compliant. |

### 7.4 Rest Day Sacrifice Rule

1. **Maximum one sacrifice per fortnight.**
2. When triggered, the sacrificed day is always Wednesday (structural score 11/20). Sunday rest is never sacrificed—it recovers the structurally dead morning and preserves the partial weekend.
3. The sacrifice converts Wednesday into a standard 04:00–14:00 shift.
4. The decision is made once, at the Day 3 checkpoint. Not revisited.

---

## 8. Financial Model

### 8.1 Cost Decomposition

Total weekly cost is £430 (working week) or £220 (off week). The £220 baseline runs every week regardless of driving; the £210 increment covers variable costs of operating.

| Cost Item | Per Week | Per Month | % of Gross |
|-----------|----------|-----------|------------|
| Fixed vehicle (insurance, finance, phone, road tax, MOT, PCO) | £220.00 | £953 | 28.3% |
| Variable non-charging (tyres, maintenance, cleaning, etc.) | £165.84 * | £484 | 14.4% |
| Charging (e-Niro 20→80%, 38.4 kWh × £0.23/kWh) | £44.16 * | £129 | 3.8% |
| **TOTAL** | **£430 / £220** | **£1,566** | **46.5%** |

\* Working weeks only. Monthly figures use 2:1 cycle averages: 2.92 working weeks, 1.42 off weeks per month.

**For every £1 earned, £0.54 is retained as net profit.** The fixed vehicle cost (£953/month) accounts for 28% of gross earnings and runs whether the vehicle is driven or not. Charging is the smallest cost component at 3.8%—confirming that off-peak discipline matters but is not the primary lever.

### 8.2 Charging Economics

| Charging Rate | Per Day | Per Month | Net Profit | Impact |
|---------------|---------|-----------|------------|--------|
| £0.23/kWh (Tier 1 off-peak) | £8.83 | £129 | £1,803 ✅ | Model rate |
| £0.25/kWh (Tier 2) | £9.60 | £140 | £1,792 | −£11/mo |
| £0.30/kWh | £11.52 | £168 | £1,764 | −£39/mo |
| £0.40/kWh (peak rate) | £15.36 | £224 | £1,708 ⚠️ | −£95/mo |

The peak-to-off-peak differential is £95/month (£1,142/year). The 04:00–14:00 shift window supports off-peak charging: the vehicle returns home by ~14:30 and can charge overnight at Tier 1 rates (23:00–09:00).

### 8.3 Gross and Net Earnings

| | Minimum (£192/day) | Expected (£231/day) |
|---|---|---|
| Working days per month | 14.6 | 14.6 |
| **GROSS EARNINGS** | **£2,800** | **£3,369** |
| Less vehicle cost | −£1,437 | −£1,437 |
| Less charging cost | −£129 | −£129 |
| **NET PROFIT** | **£1,234** | **£1,803** |
| Net per day worked | £85 | £124 |
| Net per shift hour | £8.46 ⚠️ | £12.36 |

**At the minimum floor, net hourly rate is £8.46/shift hour.** The gap between the £192 floor and the £231 expected level represents £569/month of safety margin.

### 8.4 Weekly Cash Flow Cycle

| Week | Type | Earnings | Cost | Profit |
|------|------|----------|------|--------|
| Week 1 | Working | £1,157 | £430 | **+£727** ✅ |
| Week 2 | Working | £1,157 | £430 | **+£727** ✅ |
| Week 3 | Off | £0 | £220 | **−£220** ❌ |
| **Cycle total** | | **£2,314** | **£1,080** | **+£1,234** |

Minimum cash reserve: £660 (three off-week costs).

### 8.5 Annual Projection

| Metric | Value |
|--------|-------|
| Annual gross earnings (35 × £1,157) | £40,495 |
| Annual vehicle cost | £17,244 |
| Annual charging cost | £1,546 |
| Annual total cost | £18,790 |
| **Annual net profit** | **£21,705** |
| Monthly net | £1,809 |
| CNHR (engaged hours) | £16.80/hr |
| NLW headroom | £4.59/hr (37.5%) |
| Retention rate | 53.5p per £1 earned |

---

## 9. Time Budget

| | Shift Day | Rest / Off Day |
|---|---|---|
| Sleep (20:00–04:00) | 8h | 8h |
| Shift (04:00–14:00) | 10h | — |
| Transition (14:00–15:00) | 1h | — |
| **Study block (15:00–20:00)** | **5h** | — |
| **Full free time (04:00–20:00)** | — | **16h** |

### 9.1 Study Allocation

| Source | Hours | Notes |
|--------|-------|-------|
| Shift day afternoons (5/wk × 5h × 2 wks) | 50h | 15:00–20:00, daylight hours |
| Rest days (2/wk × 8h × 2 wks) | 32h | Full day study blocks |
| Off week | 31.5h | Per life plan allocation |
| **Total per 3-week cycle** | **113.5h** | 37.8h/week average |
| **Annual study hours** | **1,967h** | 164h/month |
| **To November 2026 (~8 months)** | **1,311h** | 33 full-time study weeks |

Study capacity increases by 50% versus the night model (1,967h vs 1,309h annually). The 5-hour daily afternoon block is the key structural gain—it replaces the 2h fragmented windows that the night model offered on shift days.

---

## 10. Mathematical Foundation

This section provides the complete mathematical derivation of CNHR dynamics. It is self-contained and does not require reference to prior versions.

### 10.1 CNHR as a Function of Shift Hours

Define f(s) as CNHR(engaged) at shift-hour s. Using the constants from Section 2.4:

> f(s) = (E(s) − C_W) / (k · s)

where E(s) is cumulative earnings at shift-hour s, C_W = £430, and k = r × α = 0.687 × 1.26 = 0.866.

This is a ratio of net earnings to engaged time. Because the numerator starts negative (−C_W = −£430) and grows linearly with earnings, while the denominator grows linearly with hours, f(s) follows a hyperbolic trajectory: deeply negative at the start of a week, climbing rapidly, then decelerating toward an asymptotic ceiling.

### 10.2 The CNHR Derivative

Differentiating f(s) with respect to shift hours using the quotient rule and simplifying, the rate of change of CNHR per shift hour is:

> **df/ds = (ρ_t / α − f(s)) / s**

where ρ_t is the instantaneous trip earning rate (£ per trip hour). This compact equation governs all CNHR behaviour and reveals three components.

### 10.3 Three Components

**The Attractor (ρ_t / α).** CNHR is always being pulled toward the trip earning rate divided by the engagement multiplier. At the reference ρ_t = £32.28/hr, the attractor is £25.62/hr. If f(s) is below the attractor, df/ds is positive (CNHR rises). If above, it falls. At equilibrium, f(s) = ρ_t / α and CNHR is stationary. The fixed cost C_W means the attractor is never reached in a single week.

**The Inertia (1/s).** The rate of change is inversely proportional to shift hours accumulated. At hour 1, one trip changes everything. At hour 10, trends become visible. At hour 30, the trajectory is locked within ±£2 of its final value. At hour 40, a single bad trip barely registers. This is the mathematical proof that late-week panic driving—extending shifts or chasing surge—is structurally futile. By Day 3 of a 5-day week, CNHR is already committed.

**The Gap (ρ_t / α − f(s)).** The signed distance from current CNHR to the attractor. Always positive in practice (you are always recovering fixed cost). As CNHR climbs toward the attractor, the gap shrinks and the rate of climb decelerates—explaining the 1,000× slope collapse observed in the empirical data (df/ds falls from +324 at hour 5 to +0.2 at hour 50).

### 10.4 Closed-Form Solution

If ρ_t is constant (steady earning rate), the ODE has the exact solution:

> **f(s) = ρ_t / α − C_W / (k · s)**

CNHR is a hyperbola in s, approaching ρ_t / α from below. The gap between current CNHR and the attractor closes as C_W / (k · s), which at 50 shift hours still depresses CNHR by £430 / (0.866 × 50) = £9.93/hr. This is the structural drag that never fully closes in a single week.

### 10.5 Day Model Parameters

| Parameter | Night (v1.0) | Day (current) |
|-----------|-------------|---------------|
| ρ (trip rate) | £37.08/hr | £33.69/hr |
| Attractor (ρ/α) | £29.43/hr | £26.74/hr |
| f(50) at steady ρ | £19.49/hr | £16.80/hr |
| Fixed cost depression at 50h | £9.94/hr | £9.94/hr |
| Minimum ρ for NLW at 50h | £27.90/hr | £27.90/hr |
| Headroom (ρ − minimum ρ) | £9.18/hr | £5.79/hr |

The £5.79/hr headroom above the NLW-critical ρ floor means the day model has less tolerance for demand drops. If ρ falls below £27.90/hr on a sustained basis, the model fails. This is the primary risk addressed in the trial protocol.

### 10.6 Operational Implications

The derivative reveals why real-time CNHR monitoring works differently than expected:

**First, CNHR cannot be steered after ~30 shift hours.** The 1/s inertia makes it unresponsive. By Day 3, the CNHR is locked to within ±£2 of its final value. This means decisions must be made early or not at all—the Day 3 monitoring checkpoint (Section 7.3) is calibrated to this window.

**Second, the only variable that matters is ρ_t.** ρ_t / α is where CNHR converges. If ρ_t drops, the attractor drops, and no amount of extra hours fixes it. This is why "driving more" is the wrong response to low CNHR—the correct response is to assess whether demand conditions support the model.

**Third, final weekly CNHR is predetermined by average ρ_t.** Individual trip variance washes out. The monitoring question is not "is my CNHR below NLW" (it always is early in the week) but "are my cumulative earnings on pace for £192/day?"

These three properties—inertia, attractor dominance, and average convergence—together explain why the linear daily target (£192/day, checked at Day 3) outperforms all other monitoring approaches tested (real-time thresholds, slope projection, ρ extrapolation). The target is simple precisely because the underlying dynamics are well-understood.

---

## 11. Risk Register

| Risk | Impact | Threshold | Mitigation |
|------|--------|-----------|------------|
| 04–08 premium erosion | **CRITICAL** | ρ (04–08) drops below £30 | Historical ρ of £37/hr may include night surge carry-over. Trial will quantify. Fallback: revert to night model (23:00–09:00). |
| NLW margin erosion | **HIGH** | Avg daily < £192 | 6th day trigger. If persistent (>3 fortnights), assess window viability. |
| Early wake fatigue | MED | Inability to sustain 04:00 starts | 20:00 bedtime non-negotiable. If adaptation fails, shift to 05:00–15:00. |
| Seasonal demand | LOW | Sustained ρ < £28/hr | Monitor monthly. Off-peak: increase trip selectivity. |
| Vehicle downtime | LOW | Lost shift day | Maintain on Wed/Sun or off-weeks. £660 buffer covers overhead. |

---

## 12. Trial Protocol

The day model is based on 189 trips recorded during predominantly night-shift operations. The 04:00–08:00 earning rate may not persist under a dedicated daytime pattern. A structured trial is required before full commitment.

### 12.1 Conservative Scenario

If the 04:00–08:00 premium evaporates and the true daytime ρ converges to the 08:00–14:00 rate of £27.87/hr, daily earnings fall to £191—£1 below the NLW floor. The model becomes non-viable. This is the hypothesis the trial must test.

### 12.2 Trial Design

- **Duration:** 2–3 complete working weeks (10–15 shift days).
- **Window:** 04:00–14:00, strict. No extensions.
- **Schedule:** Mon, Tue, Thu, Fri, Sat working. Wed, Sun rest.
- **Sleep:** 20:00–04:00, strict. No exceptions.

### 12.3 Success Criteria

| Criterion | Pass | Fail |
|-----------|------|------|
| Average daily earnings | ≥ £210 ✅ | < £192 ❌ |
| 04:00–08:00 ρ | ≥ £33/hr ✅ | < £28/hr ❌ |
| Sleep adaptation | Consistent 20:00–04:00 ✅ | Chronic sleep debt ❌ |
| Consecutive shifts | 3 max without fatigue ✅ | Fatigue before Day 3 ❌ |

### 12.4 Decision Matrix

- **All criteria pass:** Adopt day model as permanent operating pattern.
- **Earnings pass, sleep fails:** Shift window to 05:00–15:00. Re-trial for 2 weeks.
- **Earnings fail, sleep passes:** Day model non-viable at current demand. Revert to night model (23:00–09:00).
- **All criteria fail:** Revert to night model immediately.

---

## 13. Implementation Checklist

**Phase 0: Pre-Trial (3 days)**

- [ ] Begin sleep schedule transition: 20:00–04:00 for 3 nights before first shift
- [ ] Set phone alarm for 03:45
- [ ] Charge vehicle overnight (off-peak 23:00–09:00 at Tier 1 rate)
- [ ] Write £192 on a card and keep in car
- [ ] Establish £660 cash reserve

**Phase 1: Trial (Weeks 1–3)**

- [ ] Run 2–3 weeks on strict 04:00–14:00 schedule
- [ ] Record daily earnings, noting 04–08 vs 08–14 split
- [ ] Track sleep quality and fatigue levels
- [ ] Evaluate against success criteria (Section 12.3)
- [ ] Decision: adopt, adjust, or revert (Section 12.4)

**Phase 2: Steady State (Cycle 2+)**

- [ ] Monthly review: earnings vs £1,783 net target
- [ ] Track study hours against 164h/month target
- [ ] Quarterly: assess cybersecurity traineeship readiness
- [ ] Recalibrate ρ and rest day allocation if demand patterns shift

---

*— End of Model —*
