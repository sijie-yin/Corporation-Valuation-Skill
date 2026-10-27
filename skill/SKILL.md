---
name: apply-v4v5-valuation
description: Apply V4/V5 two-stage BSM valuation (估值测算/测算报告) to a named company from its financials — input assembly, half-year adaptation, non-listed σ+DLOM, honest-boundary report shape. Use when asked to 测算/估值 a company with V4/V5.
---

# Apply V4/V5 valuation to a company

## Canonical home (parameters live there, not here)

- Model math + current parameter set (λ=0.85, r=2.5%, T=8, M floor 0.3, five-band cutoffs 1.2/1.0/0.8/0.5 — as of 2026-09, re-extract on any version bump): `workspace/reports/valuation-calculator/V5/index.html` + V4 sibling; backtest validity in `Test-Data/Step4-报告.md` (630 obs: V4≈60%, V5≈52% hit).
- Report both models; **V5 (毛利额基座) is the default headline, V4 (营收基座) the cross-check** (operator-settled positioning).
- Statements/quotes/bars/NEEQ access: cn-stock-financials. Final QC: prose-deliverable-gates.

## Input assembly

- **Growth series needs input reach B−3…B** (two-stage formula: first 3 years realized growth, then industry mean + λ×deviation). Annual statements via cn-stock-financials. A missing year becomes a disclosed assumption (mean of available points) — never a fabricated number.
- σ: listed → own bars (daily ×√250 or weekly ×√52). Non-listed/thin (新三板) → comparable listed portfolio σ median × ~1.15 uplift (verified: 网安 comparables 0.52 → 0.60), **plus a DLOM column (~25%) shown both ways** — never silently applied.
- M (quality score): smooth-normalize the indicator set, butterfly-mean compose, floor 0.3 — reuse the composition from the V5 script verbatim; education-mix indicators may take industry-standard assumptions (disclosed).
- Strike X = period-end total liabilities; S₀ = two-stage PV of the revenue (V4) / gross-profit (V5) base; market anchor = live quote cap, or dated news price × share count for thin names.

## Half-year-report adaptation (verified on a NEEX-listed software company (anonymized))

- **Two annualization scenarios, both reported**: pessimistic = H1×2; seasonal = H1-share (~32%) when the report itself states H2-heavy revenue.
- Growth points = H1-vs-H1 YoY only (comparable basis); missing third point = disclosed mean of the two.
- Balance-sheet items: period-end values as-is.

## Report shape (user-accepted)

1. Company/data overview table — every figure with period, YoY, source+date.
2. Adaptation & assumptions section — every model-unobservable input listed with its value.
3. Results table: scenarios × models → S₀, Vp, C, C/市值, band verdict, DLOM column.
4. Interpretation: the mechanism driving the verdict; what the model cannot see (listing-path options, licenses in review, cash-flow turns, buyback/回售 floors); confidence boundaries; tracking checklist with the **first signal named**.
5. Honest-boundary language (standing requirement): V4/V5 are ~52–60% screening signals, not pricing; names outside the A+H validation domain (e.g. NEEQ) get explicit downgrade language; a single-day thin-liquidity price anchor carries its caveat plus C/每股净资产 as the alternative anchor.

## Completion check

Every scenario×model row computed from the canonical formulas (no ad-hoc math); every adaptation assumption disclosed with its value; V5-headline/V4-cross-check ordering holds; report passes prose-deliverable-gates (source+date per fact, 麻小 signature, ai-trace <25 no flags); honest boundaries + first-signal tracking item present.
