# QuantGPT Signal Attestation

Public attestation record for QuantGPT's quantitative trading signals.

## How it works

Before each trading session, we commit a JSON file containing the **SHA-256 hash** of every active strategy's signal. Because Git commits are timestamped and immutable once pushed, this serves as a tamper-proof proof that signals were generated **before** market open — not retrofitted after the fact.

## Verification

1. Pick any file in `attestations/`, e.g. `2026-05-13.json`
2. Each entry contains a `signal_hash` (SHA-256 of the raw signal JSON)
3. Cross-reference the Git commit timestamp — it must predate market open (09:30 CST)
4. The hash can be independently verified against the signal data served by the QuantGPT platform

## Structure

```
attestations/
  2026-05-13.json    # one file per trading day
  2026-05-14.json
  ...
```

Each attestation file:

```json
{
  "date": "2026-05-13",
  "attested_at": "2026-05-13T09:05:00+08:00",
  "strategies": [
    {
      "strategy_id": "hs300_ml_alpha",
      "model_id": "lgb_...",
      "signal_hash": "sha256:abcdef1234..."
    }
  ]
}
```

## Links

- [QuantGPT Track Record](https://quantgpt.cloud/track-record)
- [QuantGPT](https://quantgpt.cloud)
