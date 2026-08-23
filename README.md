# Swamp Campaign Outcome

`@mgreten/campaign-outcome` validates aggregate campaign outcome evidence and
persists its canonical form. It has no network, filesystem, credential, or
execution side effects. Recommendations always require a human decision and
cannot represent an action already taken.

## Package layout

The Swamp manifest is at `extensions/manifest.yaml`; the model and its public
contracts are in `extensions/models/`.

## Contract

The `record` method accepts a strict object containing a safe `recordId` and
unknown `evidence`. Evidence is validated against schema version `1.0` and is
written as `evidence-<recordId>` only when valid. Invalid input raises a
`TypeError` containing deterministic diagnostics.

Accepted evidence is aggregate-only, references immutable SHA-256 artifacts,
requires five unique funnel stages, verifies source and measurement chronology,
and canonicalizes stage and identifier ordering. The public TypeScript exports
are `model`, `recordArgumentsSchema`, `CAMPAIGN_OUTCOME_MODEL_VERSION`,
`campaignOutcomeEvidenceSchema`, `CampaignOutcomeEvidence`,
`CAMPAIGN_OUTCOME_SCHEMA_VERSION`, `CAMPAIGN_OUTCOME_STAGES`, and
`evaluateCampaignOutcomeEvidence`.

```ts
const result = evaluateCampaignOutcomeEvidence(evidence);
if (!result.success) throw new TypeError(JSON.stringify(result.diagnostics));
const canonicalEvidence = result.value;
```

## Development

```sh
deno fmt --check
deno lint
deno check extensions/models/campaign_outcome.ts extensions/models/_lib/campaign_outcome_contracts.ts
deno test --allow-read extensions/models
swamp extension fmt extensions/manifest.yaml --check --json
swamp extension quality extensions/manifest.yaml --json
swamp extension push extensions/manifest.yaml --dry-run --json
```

## Provenance and license

This is a standalone handoff extracted from local implementation and test
artifacts on 2026-08-23. It is licensed under the MIT License; see
[LICENSE](LICENSE).
