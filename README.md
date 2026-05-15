# QuantGPT Signal Attestation

Public attestation record for QuantGPT quantitative trading signals.

## How it works

Before each trading session, we commit a JSON file containing the **SHA-256 hash** of every active strategy signal. Git commits are timestamped and immutable once pushed — this serves as tamper-proof proof that signals were generated **before** market open, not retrofitted after the fact.

**All commits are GPG-signed** by `QuantGPT Attestation <attestation@quantgpt.dev>` (key fingerprint `196CC5B1E2DE02EE2BE05C5D3F649E566F7F506C`). The public key is included in this repo as `GPG_PUBLIC_KEY.asc`.

## Verify GPG signatures

```bash
# Import the public key
gpg --import GPG_PUBLIC_KEY.asc

# Verify all commits
git log --show-signature

# Verify a specific commit
git verify-commit <commit-hash>
```

A valid signature proves the commit was made by the QuantGPT platform — not by someone who cloned or forked this repo.

## Verify signals

1. Pick any file in `attestations/`, e.g. `2026-05-13.json`
2. Each entry contains a `signal_hash` (SHA-256 of the raw signal JSON)
3. Cross-reference the Git commit timestamp — it must predate market open (09:30 CST / UTC+8)
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
  "attested_at": "2026-05-13T01:05:00+00:00",
  "strategies": [
    {
      "strategy_slug": "alpha-strategy-01",
      "signal_hash": "abcdef1234...",
      "universe": "hs300",
      "execution_price": "open",
      "attested_at": "2026-05-13T01:05:00+00:00"
    }
  ]
}
```

## Links

- [QuantGPT Track Record](https://quant-gpt.com/track-record)
- [QuantGPT Platform](https://quant-gpt.com)
