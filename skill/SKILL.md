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

---

## 开源公司估值模块（2026-11-10 固化）

适用：开源/开源核心（open core）公司。四算法按标的状态选择，可叠加。

### 判定入口
```
有前瞻 ARR？ → 是：算法A（倍数法）；否：算法B（漏斗法）＋算法C（期权法）
任何情况叠加：算法D（折扣表）
```

### 算法 A：EV/ARR 倍数法（有收入）
```
EV = ARR前瞻 × M
M = M基准 × (1 − d开源) × k增速 × kNRR
M基准 = 5-8x（上市SaaS中位，测算日核验 saasvaluation.io）
d开源 = 20-40%（转化率0.5-5% + 云厂商分食 + 许可证风险）
k增速：Rule of 40 ≥0 → ×1.0-1.2；<0 → ×0.8-0.9
kNRR：NRR≥120% → ×1.1-1.2；<100% → ×0.8
```

### 算法 B：漏斗指标法（无收入早期）
```
估值区间 = 轮次锚定区间 × w
w = 0.2×log10(star) + 0.3×(fork/star比) + 0.3×生产部署信号 + 0.2×维护活跃度
轮次锚（Redpoint 2023-03）：约1k star ≈ seed 区间 $2-10M（海外）；中国 ×0.5-0.7
fork/star 比 >10% = 真实使用强信号；14+月持续 commit = 维护满分
```

### 算法 C：两阶段实物期权（V4/V5 的开源适配）
```
V = PV(当前商业现金流) + BSM_call(S, K, σ, T, r)
S = 社区转化潜在ARR × 倍数（漏斗：star×10%线索×5%POC×30%成交×客单价）
K = 兑现投入现值（按标的实际预算核定——WHartTest 案例从 2.5M 下修至 1.5M 后估值 +11%）
σ = 50%（基准情形，2026-11-10 固化；偏离须给理由，允许带 40-80%）
T = 商业化窗口（典型 2 年）；r = 当期无风险利率
```

### 算法 D：折扣表（乘法叠加）
```
非 OSI 许可（自定义/BSL/SSPL）：−15%
云厂商竞争暴露：−10%
单一团队维护：−20%
（基准档，按严重度上下浮 5%）
```

### 校验案例
WHartTest（2026-11-10）：1080 star / 181 fork（fork比16.8%）→ 算法B＋BSM(S=8M, K=1.5M, σ=50%, T=2y, r=4%) → Call 6.6M × 折扣0.612 ≈ 4.1M美元，区间 1.6-6.5M。

### 信源
Linux基金会×Serena《State of Commercial Open Source》2025-08；a16z 2019-10；Redpoint 2023-03；saasvaluation.io。倍数随市况波动，测算日须当日核验。
