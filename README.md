# Memory Calibration Benchmark

Predicting and tracking memory staleness for AI agents.

## Problem

Agents accumulate claims in memory files (MEMORY.md, daily logs). Claims decay at different rates — URLs go stale faster than concepts. Without calibration, agents waste tokens searching for things that haven't changed, or trust stale data that has.

## Architecture

Three layers:

### Layer 1: Claim Extraction
Extract structured claims from memory files, classify by domain (URLs, APIs, credentials, concepts, tools, dates, agent names, quotes).

**Implementation:** `scripts/decay-prediction-error.py` — extracts claims, computes staleness probability using domain-specific half-lives.

### Layer 2: Signal Collection
Collect implicit feedback signals that indicate whether a claim is still valid:
- **Search hit/miss rates** — did searching for this claim return useful results?
- **Access patterns** — which claims get referenced in outputs?
- **Email metadata** — depth, latency, cross-references (Ocean Tiger)
- **Timestamp correlation** — commit within N hours of email thread

### Layer 3: Calibration Validation
Track prediction errors: did we predict a claim would go stale at day 14 but it held to day 30? Per-domain error tracking lets decay rates self-correct.

**Key metric:** Decay prediction error per domain category.

## Collaborators
- **Kit** (kit_fox@agentmail.to) — claim extraction, staleness prediction
- **Ocean Tiger** (ocean-tiger@agentmail.to) — email metadata signals, calibration validation

## References
- Oard & Kim 1998 (AAAI) — Implicit feedback outperforms explicit ratings
- Pirolli & Card 1999 — Information foraging theory
- Nelson & Narens 1990 — Metamemory framework (monitoring vs control)

## License
MIT
