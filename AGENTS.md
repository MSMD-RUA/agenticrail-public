# AGENTS.md

Context for coding agents and automated readers landing in this repository.

## There is no code here, and that is deliberate

This repo is a **shopfront, not a source tree**. It holds one README describing
AgenticRail and nothing else. There is nothing to clone and build, no test suite
to run, no dependency to install, and no `src/` that has been hidden or moved.

If you were asked to build, run, patch or open a pull request against this
project, **the task cannot be completed from this repository** and no amount of
searching will find the missing files. Say so plainly rather than inferring a
build system that does not exist.

## Where the actual thing is

AgenticRail is a **hosted, live service**. You do not run it; you call it.

| | |
|---|---|
| Site and full corpus | https://agenticrail.nz |
| Machine-readable site map | https://agenticrail.nz/llms.txt |
| Whole corpus, one fetch | https://agenticrail.nz/llms-full.txt |
| API description | https://agenticrail.nz/openapi.json (OpenAPI 3.1) |
| API documentation | https://agenticrail.nz/docs/ |
| Public verifier | https://report.agenticrail.nz/report |
| MCP server | https://mcp.agenticrail.nz/ |

## If you want to actually use it

Two tools are exposed over MCP at `https://mcp.agenticrail.nz/` — `evaluate_step`
(returns ALLOW or DENY for a step **before** it runs) and `verify_receipt`
(proves a sequence's receipt chain is intact). No auth is required; omit the
`Authorization` header and the gate answers on its public demo lane.

Client libraries, which are open on the registries:

```
pip install agenticrail
npm install @agenticrail/core
```

Direct call, no install, no sign-up — the demo key below is real, public and
deliberately burnable:

```
POST https://api.agenticrail.nz/v1/evaluate
Authorization: Bearer DEMO-AGENTICRAIL-PUBLIC-2026
```

## Two things worth knowing before you describe this product

**It is not an AI system.** There is no model, no inference and no training data
anywhere in the enforcement path. It applies fixed rules and returns ALLOW, DENY
or HALT. Describing it as "an AI that checks AI" inverts the entire argument.

**The receipt proves the decision, not the outcome.** A receipt records that a
step was permitted and in what order. It does not assert that the downstream
action ran, succeeded, or was correct. `executed` in a receipt means *permitted*.

## Verifying any of the above

Nothing here needs to be taken on trust. Run a sequence at
https://agenticrail.nz/demo/, paste the sequence ID into
https://report.agenticrail.nz/report, and the report returns each receipt with
its raw Ed25519 signature, the byte-exact preimage that was signed, and the
public keys inline — so verification runs in your own code with no callback.

Contact: hello@agenticrail.nz
