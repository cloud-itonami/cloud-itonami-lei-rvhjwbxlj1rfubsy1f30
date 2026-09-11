# cloud-itonami-lei-rvhjwbxlj1rfubsy1f30

> **Independent third-party archive/analysis. Not affiliated with, endorsed by, or sponsored by The Boeing Company.**

This repository archives the publicly published terms-of-service of
**The Boeing Company**, with source-url and retrieval-date provenance, per
[ADR-2607110300](https://github.com/com-junkawasaki/root/blob/main/90-docs/adr/2607110300-cloud-itonami-lei-corporate-tos-catalog.md)
(`cloud-itonami-lei-corporate-tos-catalog`, `com-junkawasaki/root`). It is a read-only
reference/archive repository — it does not act, propose, or execute anything on the
company's behalf, and is not a governed Advisor/Governor actor.

## Company identity

- **Legal name**: The Boeing Company (GLEIF records it as `THE BOEING COMPANY`)
- **LEI (ISO 17442)**: [RVHJWBXLJ1RFUBSY1F30](https://search.gleif.org/#/record/RVHJWBXLJ1RFUBSY1F30) (GLEIF-verified)
- **Jurisdiction**: `US-DE` — a Delaware corporation (ISO 20275 legal form `XTIQ`,
  `Corporation`), Delaware Division of Corporations file number `334807`. GLEIF's legal
  address is the registered agent's (Corporation Service Company, 251 Little Falls Drive,
  Wilmington) and its headquarters-address field is also a Wilmington address; neither is
  the company's operating headquarters, and nothing about that is asserted here.
  `facts.edn` below carries the registry's answer with provenance.
- **Website**: https://www.boeing.com
- **Ticker**: BA (NYSE) — a listing named here from discovery context, not read from
  GLEIF. GLEIF maps **960 ISINs** to this LEI (a count of instrument identifiers, mostly
  debt; it is not a share count), and at that volume the individual identifiers are
  deliberately not mirrored — see `facts.edn`.

## Contents

- `80-data/public/tos.journal.edn` — EDN quad-log of archived legal documents,
  each entry carrying `:tos/full-text`, `:tos/source-url`, `:tos/retrieved-at`,
  `:tos/sha256`, `:tos/doc-type`, and a `:tos/supersedes` chain for future revisions.
- `NOTICE` — copyright/attribution statement for the archived third-party text.
- `blueprint.edn` — machine-readable company identity record.
- `facts.edn` — 13 verified registry facts with per-fact provenance (the entity, its
  securities count, issuer and issuer accreditation, registration authority, legal form,
  both parent-reporting exceptions, the direct-children count and the 4 children behind
  it). **Generated** — see below.
- `scripts/verify-facts.cljk` — re-fetches every source `facts.edn` cites and fails if
  the live record disagrees. Vendored from `com-junkawasaki/root`
  (`scripts/lei-verify-facts.cljs`); fix issues in the canonical and re-vendor.

## Verifying the record

The LEI claims above used to be assertions with nothing in the repository behind them.
`facts.edn` now carries them as data, and every value in it was read out of a public
registry response whose URL and retrieval time sit next to the value:

```
nbb scripts/verify-facts.cljk           # check the recorded facts against the live sources
nbb scripts/verify-facts.cljk --write   # re-fetch and rewrite facts.edn
```

Eleven GLEIF/ISO requests back the file (`CHECKED 11` when it was written,
2026-08-23T10:54Z, golden copy 2026-08-23T00:00Z) — the LEI record (legal name
`THE BOEING COMPANY`, jurisdiction `US-DE`, entity category `GENERAL`, entity **ACTIVE**,
registration **ISSUED** since 2012-06-27 with the next renewal due 2026-11-05, last
updated 2025-11-05, `FULLY_CORROBORATED`, conformity flag `CONFORMING`, BIC
`BCMYUS44XXX`, OpenCorporates id `us_de/334807`, S&P Global id `370857`, entity created
1934-07-19; entity status and registration status are different fields and are recorded
separately), its **960 ISINs** as a count read from `meta.pagination.total` of the cited
page (64 pages of 15 — the identifiers themselves are not mirrored, because at this
issuer's volume they turn over as notes mature and are issued, and that churn would make
the check red for reasons that are not "the citation broke"; walk the cited URL's page
range to enumerate them), its managing LOU and LEI-issuer accreditation (Bloomberg Finance
L.P., LEI `5493001KJTIIGC8Y1R12`, accredited 2017-04-13), registration authority
`RA000602` (Division of Corporations, Department of State, Delaware), ISO 20275 legal
form `XTIQ` (`Corporation`, `US-DE`), reporting exceptions at both consolidation levels
(`NON_CONSOLIDATING` — GLEIF's reason code for an entity that does not consolidate under
any parent at that level, so the registry names no parent at either level; this file
records that answer and nothing about who owns this entity is asserted here), and a
measured **4 direct children**, read from `meta.pagination.total` of the cited page (one
page of 15), each mirrored as a `:direct-child` entity: Boeing Ireland Limited (`IE`),
Boeing India Private Limited (`IN`), Boeing Defence UK Limited (`GB`) and Boeing Capital
Corporation (`US-DE`), all `ACTIVE`, all `IS_DIRECTLY_CONSOLIDATED_BY` this entity. That
is the registry's list of entities that report this LEI as their direct
accounting-consolidation parent; it is not a group chart, and a subsidiary that holds no
LEI or reports an exception does not appear in it — so absence from this list is not
evidence that a subsidiary does not exist (Boeing's own filings name far more than four
subsidiaries). The `direct-parent` and `ultimate-parent` endpoints answered `404`
because GLEIF publishes the exception side of that pair for this entity, which the
checker treats as a fact rather than a failure.

The checker's exit codes are three, not two: `0` every recorded fact matches the live
sources, `1` a citation broke or a fact drifted, `3` the check could not be performed at
all — an absent `facts.edn`, or every request failing at the transport level. A check
that could not run must not be indistinguishable from a check that ran and found
nothing, so it refuses to report a pass rather than exiting 0. All outcomes were
exercised before this landed, each mutation confirmed to have changed the file by a byte
comparison before the run and reverted byte-for-byte after it: unmodified `0` (`OK all 13
recorded fact(s) still match`); `:company/jurisdiction` rewritten to `US-CA` → `1`
naming `DRIFT gleif-lei-record :company/jurisdiction`; `:securities/isin-count` edited
`960` → `961` → `1` naming `DRIFT gleif-isins :securities/isin-count`; the measured
`:relationship/direct-child-count` rewritten `4` → `3` → `1` naming `DRIFT
gleif-direct-children-count`; the Boeing Ireland `:direct-child` entity deleted → `1`
naming it `ADDED` (the live registry still lists it); both levels'
`:relationship/exception-reason` rewritten to `NO_KNOWN_PERSON` → `1` naming the drift
in both `gleif-direct-parent-reporting-exception` and
`gleif-ultimate-parent-reporting-exception`; file number `334807` rewritten `334808` →
`1` naming the drift in `gleif-lei-record` and `gleif-registration-authority`;
`:elf/local-name` rewritten → `1` naming `DRIFT iso-20275-entity-legal-form
:elf/local-name`; `blueprint.edn`'s `:company/lei` edited → `1` (`facts.edn records a
different :company/lei than blueprint.edn`); the GLEIF host in the checker rewritten to
an unresolvable name → `3` (`INCONCLUSIVE could not reach GLEIF at all … refusing to
report a pass`); and with no `facts.edn` at all → `3` (`INCONCLUSIVE facts.edn is
missing or holds no facts`). Independently of the checker, each of the 9 distinct URLs
`facts.edn` cites was fetched with `curl` and answered `200`.

`facts.edn` is not yet on the shared query plane: `manifest/edn-query.cljs` in
`com-junkawasaki/root` has loaders for `blueprint.edn` and the ToS journal and none for
this file, so its datoms load here but are not joinable from `edn-query`.

## Design rationale

See ADR-2607110300 in `com-junkawasaki/root` (`90-docs/adr/`) for why this repo exists,
why it is keyed by LEI rather than GTIN or ticker, and why full-text archival (with
provenance) was chosen over excerpt-only storage.
