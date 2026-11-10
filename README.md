# Corporation-Valuation-Skill

A two-stage BSM (Black-Scholes-Merton) valuation framework for software/intangible-asset companies,
with progressive validation across V1-V5 iterations.

## What This Is

An AI-assisted valuation methodology that:
- Applies two-stage BSM to value companies whose assets are primarily intangible
- Iterates through V1-V5 versions with diagnostic-driven improvements
- Includes 50-company no-lookahead backtesting
- Produces a research paper documenting the methodology

## Repository Structure

| Directory | Contents |
|---|---|
| skill/ | SKILL.md -- the agent-executable methodology instructions |
| data/V1-V5/ | Version evolution: each iteration with inputs, outputs, and diagnostics |
| data/backtest-progressive* | 50-company progressive backtesting results |
| data/fin-data.json | Financial data inputs |
| data/validation-20.md | 20-company sample validation report |
| data/Test-Data/ | Test datasets |

## V1-V5 Evolution Summary

- **V1**: Initial two-stage BSM application
- **V2**: Diagnostic iteration -- volatility and DLOM refinements
- **V3**: Non-listed company adaptations
- **V4**: Progressive validation across larger sample
- **V5**: Final calibrated framework with backtesting

## Research Paper

See data/paper-valuation-model-diagnosis-iteration.md (Chinese) for the full research paper.

## License

MIT License

## Credits

- Methodology: Jerome Yin
- AI-assisted implementation: OpenClaw + zai/glm-5.3

---

Built with the Corporation Valuation Skill for OpenClaw.
## V6: Open-Source Company Valuation Module (2026-11-10)

Four-algorithm framework for open-source / open-core companies:

- **Algorithm A** — EV/ARR multiple method (for companies with revenue): base 5-8x SaaS median, 20-40% OSS discount, growth/NRR adjustments
- **Algorithm B** — Community funnel method (pre-revenue): weighted score of stars / fork ratio / production signals / maintenance activity, anchored on Redpoint funding-stage data
- **Algorithm C** — Two-stage real-option (V4/V5 adapted): current cash-flow PV + BSM call on community-conversion potential; sigma=50% baseline (hardened 2026-11-10)
- **Algorithm D** — Discount table (multiplicative): non-OSI license -15%, cloud-vendor exposure -10%, single-team maintenance -20%

Validation case: WHartTest (1,080 stars / 181 forks) → funnel + BSM(S=8M, K=1.5M, sigma=50%, T=2y) → ~$4.1M mid, range $1.6-6.5M.

Sources: Linux Foundation x Serena (2025-08), a16z (2019), Redpoint (2023).