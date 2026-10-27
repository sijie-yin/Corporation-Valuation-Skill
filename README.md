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