# Security Center integration contract — clash-AdsBlock

## Audit snapshot
- Existing repository publishes plain-text Mihomo/Clash rule lists.
- README documents OISD small/big/NSFW variants and ABPindo; the tree also contains `notrack.txt` and `dns.txt`.
- `.github/workflows/auto.yml` downloads upstream lists hourly, transforms wildcard syntax, updates README counts and pushes generated files to the repository.
- No package manifest or application runtime is present; this is a static rule-data repository.
- GitHub metadata reports about 1.64 GB, so do not clone or mirror all rule history into the gateway.

## Integration role
Expose curated rule artefacts as static, read-only data. This is distinct from the geoip/geosite catalogue in Rule-Provider.

## Preview API contract
- `GET /v1/rules/adblock/catalog` — available list names, source, update timestamp and content format.
- `GET /v1/rules/adblock/{list}` — return the selected text list with ETag/hash where available.
- Never merge lists silently; keep source and transformation provenance per list.

## Risks and guardrails
- Upstream content is mutable and fetched automatically. Verify source availability and content format before publishing.
- Keep adult/NSFW lists opt-in and clearly labelled; never include them in a default profile.
- Validate domain syntax and reject malformed rows before caching.
- The existing scheduled workflow is a data-refresh mechanism, not the external deployment pipeline. The Security Center Preview must not depend on GitHub Actions.
- No production enforcement, DNS changes or paid services.
