# AgenticRail

**A deterministic gate in front of your AI agent. Every decision is sealed —
before the action runs — into a signed receipt you can verify offline.**

It doesn't make the AI right. It makes the record honest, and impossible to
quietly change.

🌐 [agenticrail.nz](https://agenticrail.nz) · 📄 [Docs](https://agenticrail.nz/docs/) · ✅ [Verify a receipt](https://report.agenticrail.nz/report)

---

## The gap

Almost everywhere AI now makes or drafts a decision, a human is meant to check
it — and almost nowhere is there proof the check happened. The safeguard is
real; the record of it is missing. That silence, not the AI's mistakes, is the
dangerous part: when something goes wrong, there is nothing to point to.

## What it does

Two powers, at the one boundary you can't avoid — the moment before an AI acts:

1. **Refuse.** A step that is out of order, replayed, or not permitted is denied
   — deterministically, before it runs. A computed decision, not an AI guessing
   whether to allow itself.
2. **Witness.** Every decision — ALLOW or DENY — is written to a signed,
   chain-linked receipt *before the action executes*. Nothing is recorded after
   the fact, and no receipt can be altered without leaving a detectable break in
   the chain. Sealed sequences are additionally copied to a separately
   credentialed, write-once archive, so a rewrite by the operator is detectable
   against the witness copy rather than trusted not to happen.

```
agent → gate → ALLOW / DENY → sealed receipt
```

## The proof — verify it yourself

Every receipt is signed with **Ed25519** and verifiable **offline** against a
public key we publish — no callback to AgenticRail. Re-hash the record and check
the signature in your own code, or paste a sequence ID into the
[report tool](https://report.agenticrail.nz/report).

- Public keys: [/spec/receipt-public-keys.json](https://agenticrail.nz/spec/receipt-public-keys.json)
- Enforcement spec: [/spec/](https://agenticrail.nz/spec/) (current version, with links to every prior fingerprinted amendment)
- Machine-readable API: [/openapi.json](https://agenticrail.nz/openapi.json) (OpenAPI 3.1)

## Frameworks

The gate is a plain HTTPS JSON call made before each step, so it works with any
agent framework and does not care which model or vendor produced the request.

- **Python SDK** (`agenticrail`) — purpose-built integrations for **LangGraph**
  and **CrewAI**, with runnable examples for both
- **JavaScript SDK** (`@agenticrail/core`) — **LangGraph.js**, Mastra, Genkit,
  custom loops
- **MCP** — agents speaking Model Context Protocol call the gate as a tool at
  [mcp.agenticrail.nz](https://mcp.agenticrail.nz/) (`evaluate_step`,
  `verify_receipt`)

## Stated plainly

Because these are the questions that get asked, and a straight answer costs less
than a discovered one:

- **Hosted, not self-hosted.** AgenticRail holds the receipt signing keys. That
  residual is disclosed, and narrowed — not closed — by the independent archive.
- **Not SOC 2 or ISO 27001 certified.** No certification is claimed anywhere.
- **No public price list, no self-serve sign-up.** Evaluation is free and needs
  no account; pricing is set per deployment.

Longer answers to all three: [agenticrail.nz/faq/](https://agenticrail.nz/faq/)

## Try it

Public demo key (burnable): `DEMO-AGENTICRAIL-PUBLIC-2026`

```bash
curl -X POST https://api.agenticrail.nz/v1/evaluate \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer DEMO-AGENTICRAIL-PUBLIC-2026" \
  -d '{
    "sequence_id": "my-test-001",
    "step": "intake",
    "function": "intake",
    "action_type": "CHECK_STATE",
    "action": "check state",
    "inputs": {},
    "nonce": "replace-with-a-uuid",
    "ts_ms": 0,
    "step_order": ["intake","disruption","instability","state_read","internal_driver","execution","boundary","settle"]
  }'
```

Set `ts_ms` to the current Unix time in milliseconds (within 300s of server
time) and give each request a unique `nonce`. Full field reference at
[agenticrail.nz/docs](https://agenticrail.nz/docs/).

## What it is — and isn't

| It is | It is not |
|---|---|
| Deterministic enforcement — the verdict is computed | A lie-detector for the AI. It proves *what* was done, not that the AI was *right* |
| A tamper-evident, offline-verifiable record | A stop on hallucination, or a force on human attention |
| Metadata and hashes only — no content inside a receipt | A data-sovereignty claim. AgenticRail holds the receipt signing keys — which is precisely why every receipt ships with the exact preimage it was signed over, so you verify offline without trusting our verifier |

## Where it comes from

AgenticRail was derived from **whakairo** — the carver's discipline, where order
is law and the finished form is sealed. That lineage is the reason for the seal,
not decoration on it. [The whakapapa →](https://agenticrail.nz/whakapapa/)

The enforcement core is private. The receipts are public and independently
verifiable — that's the evidence that matters.

---

*Operated by **TUARA KURI LIMITED** — trading as AgenticRail. Hokianga, Aotearoa New Zealand.*
**hello@agenticrail.nz**
