# NERC (nerc)

The North American Electric Reliability Corporation (NERC) is the not-for-profit international regulatory authority certified by FERC in July 2006 as the Electric Reliability Organization under Section 215 of the Federal Power Act, added by the Energy Policy Act of 2005. From offices in Washington, DC and Atlanta, and working through six Regional Entities (MRO, NPCC, RF, SERC, Texas RE, WECC), NERC develops and enforces the mandatory Reliability Standards — including the CIP cyber security standards — that govern the bulk power system across the contiguous United States, all of Canada, and a portion of Baja California, Mexico. It registers and certifies the entities that operate that system, monitors and enforces compliance, runs the Electricity Information Sharing and Analysis Center (E-ISAC), and collects mandatory industry performance data through Rules of Procedure Section 1600 data requests into GADS, TADS, DADS and MIDAS.

NERC sits at the top of the North American bulk-power value chain — above the ISOs/RTOs, transmission owners and generator owners it registers, and entirely above the retail utility-to-customer relationship. Its API posture is the plainest in this series: NERC publishes **no** developer API, **no** OpenAPI, **no** SDK, **no** developer portal and **no** open data portal. A site search of nerc.com for the term "API" on 2026-07-27 returned zero matching documents.

NERC is a **mandator** of energy data, not a publisher of it — the obligation it imposes is to *submit* data upward to NERC, and the data submitted is explicitly confidential under Rules of Procedure Section 1500. What NERC does publish anonymously is documents: reliability assessments, the State of Reliability report, aggregated GADS/TADS statistics, Power BI dashboards and the public NERC Compliance Registry Matrix spreadsheet of every registered entity. Everything machine-facing — the ERO Portal, Align, the Secure Evidence Locker, CORES, the GADS/TADS data stores and the E-ISAC portal — is behind an approved account.

No Green Button, no ESPI, no consumer data right, and no consumer usage or billing API exists anywhere in NERC's surface, because retail customer data is outside its reach entirely. NERC is a regulator whose peer FERC publishes a documented public API and whose neighbour EIA publishes one of the best government APIs anywhere, while NERC itself publishes none.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/nerc/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/nerc/refs/heads/main/apis.yml)

## Tags

- Energy
- United States
- Canada
- Electricity
- Grid
- Regulator
- Government
- Reliability
- Bulk Power System
- Critical Infrastructure
- Cyber Security
- Energy Markets
- Compliance

## Timestamps

- **Created:** 2026-07-27
- **Modified:** 2026-07-27

## APIs

None. NERC publishes no documented public API. `apis[]` in `apis.yml` is intentionally empty.

Four real but **undocumented** JSON endpoints power nerc.com's own search UI (`/api/search`, `/api/search/events`, `/api/search/news`, `/api/search/terms`). They answer anonymously and were verified live on 2026-07-27, but they are internal Optimizely CMS routes with no documentation, no terms of programmatic use and no support. They are recorded in `review.yml` as probe evidence and deliberately not promoted to APIs.

## Mandate posture

| Field | Value |
| --- | --- |
| Home market | United States (jurisdiction also covers Canada and part of Baja California, Mexico) |
| Mandate regime | `other` — FPA Section 215 mandatory Reliability Standards plus Rules of Procedure Section 1600 mandatory data requests (GADS, TADS, DADS, MIDAS). No Green Button, no CDR, no smart-meter data right. |
| Mandate status | `live-implemented` — live, enforced, with a public register (NERC Compliance Registry Matrix XLSX, HTTP 200). But the obligation is to **submit** data to NERC, and no API implements any part of it. |
| Data standard | No standard reference found. NERC-proprietary GADS/TADS/DADS/MIDAS reporting record layouts, published as PDF instructions. |
| Consumer data API | No — out of jurisdiction entirely. |
| Market data open | No — aggregated PDFs, registry spreadsheets and Power BI embeds only; underlying GADS/TADS/DADS data is confidential under Rules of Procedure Section 1500. |
| Access gate | `none-published` for developers. Industry participants register for an ERO Portal account and then request access per data store or application. |
| Auth model | None published for developers. Interactive account login only (ERO Portal with BotDetect CAPTCHA, E-ISAC on Salesforce Experience Cloud, EasyVista SSO for support). No API keys, no client-credentials grant, no mTLS. `www.nerc.com/.well-known/openid-configuration` returns 404 — but see the E-ISAC exception below. |

## Common Properties

- [Website](https://www.nerc.com/)
- [ERO Portal](https://eroportal.nerc.net/) — account registration and access requests. Not a developer portal.
- [Reliability Standards](https://www.nerc.com/standards/reliability-standards) — web and PDF only
- [Compliance Registry Files](https://www.nerc.com/programs/registration/compliance-registry-files) — the public register of registered entities
- [NERC Compliance Registry Matrix (XLSX)](https://www.nerc.com/globalassets/programs/registration/compliance-registry-files/nerc_compliance_registry_matrix_excel.xlsx)
- [Generating Availability Data System (GADS)](https://www.nerc.com/programs/reliability-assessment--performance-analysis/generating-availability-data-system)
- [Transmission Availability Data System (TADS)](https://www.nerc.com/programs/reliability-assessment--performance-analysis/transmission-availability-data-system)
- [Align and Secure Evidence Locker](https://www.nerc.com/applications/align-and-secure-evidence-locker-sel)
- [Centralized Organization Registration ERO System (CORES)](https://www.nerc.com/applications/centralized-organization-registration-ero-system-cores-technology-project)
- [NERC Data Stores & Extranet Sites](https://www.nerc.com/applications/nerc-data-stores--extranet-sites)
- [Support](https://support.nerc.net/)
- [Newsroom](https://www.nerc.com/newsroom)
- [Privacy Policy](https://www.nerc.com/legal-privacy-policy)
- [Terms of Use](https://www.nerc.com/terms-of-use-policy)
- [LinkedIn](https://www.linkedin.com/company/north-american-electric-reliability-corporation/)
- [Twitter](https://twitter.com/NERC_Official)
- [YouTube](https://www.youtube.com/@NERCOfficial)
- [E-ISAC](https://www.eisac.com/) — membership-gated
- [NERC Alerts](https://www.nercalerts.com/index.php)

## The one machine-readable exception: E-ISAC

Enrichment round 2 (2026-07-27) re-ran contract discovery against **every** NERC-operated host rather than `www.nerc.com` alone, and found the single anonymous, standards-based discovery document in NERC's estate:

```
https://www.eisac.com/.well-known/openid-configuration   HTTP 200, 2,267 bytes
```

The Electricity Information Sharing and Analysis Center — which NERC operates — runs on Salesforce Experience Cloud, and that platform serves a valid **OpenID Connect Discovery 1.0** document: issuer `https://www.eisac.com`, authorization / token / userinfo / introspection / revocation / registration endpoints, RS256 ID tokens, `private_key_jwt` client authentication, and 36 advertised scopes. The Salesforce version list at `/services/data` also answers anonymously.

This does **not** change the finding. It is the platform's stock issuer metadata, not a NERC-authored developer contract: no NERC page references it, there is no self-serve client registration for outside developers, and every resource it protects is membership-gated (`/services/data/v62.0` → HTTP 401 `INVALID_SESSION_ID`). It is captured because it is real and anonymous, and because a future round should not have to rediscover it.

Two other hosts — `eroportal.nerc.net` and `www.nercalerts.com` — return HTTP 200 on *every* probed path but serve HTML **soft-404** error pages, not documents. They are recorded as soft-404s so they are never mistaken for hits.

## Artifacts

| Artifact | File | Method |
| --- | --- | --- |
| Well-Known index | [`well-known/nerc-well-known.yml`](well-known/nerc-well-known.yml) | searched |
| E-ISAC OIDC discovery | [`well-known/nerc-eisac-openid-configuration.json`](well-known/nerc-eisac-openid-configuration.json) | searched (verbatim) |
| Content Signals / robots.txt | [`well-known/nerc-robots.txt`](well-known/nerc-robots.txt) | searched (verbatim) |
| Authentication profile | [`authentication/nerc-authentication.yml`](authentication/nerc-authentication.yml) | searched |
| OAuth scopes | [`scopes/nerc-scopes.yml`](scopes/nerc-scopes.yml) | searched |
| Conformance | [`conformance/nerc-conformance.yml`](conformance/nerc-conformance.yml) | derived |
| Domain security | [`security/nerc-domain-security.yml`](security/nerc-domain-security.yml) | probed |
| llms.txt | [`llms/nerc-llms.txt`](llms/nerc-llms.txt) | generated |

No `openapi/`, `asyncapi/`, `mcp/`, `packages/`, `cli/`, `sandbox/`, `changelog/`, `errors/`, `skills/` or `data-model/` artifacts exist here, and none were manufactured. There is no contract to derive them from.

## AI and agent consent

`nerc.com/robots.txt` carries a Cloudflare-managed **Content Signals** declaration — `search=yes, ai-train=no, use=reference` — asserted as an express reservation of rights under Article 4 of EU Directive 2019/790, with explicit `Disallow` blocks for GPTBot, ClaudeBot, Google-Extended, CCBot, Amazonbot, Applebot-Extended, Bytespider and meta-externalagent. Captured verbatim; honour it.

## Maintainers

- Kin Lane — kin@apievangelist.com
