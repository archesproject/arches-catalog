# Arches Community Extensions & Applications — Sharing Plan

## Reference Model

Hybrid of **djangopackages.org** (metadata-driven catalog) + **QGIS plugin repository** (manifest standard), implemented as a static catalog generated from a GitHub registry. PyPI remains the distribution channel; the catalog is a discovery layer over it.

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│  archesproject.org/packages  (static site, GitHub Pages)    │
│  - Searchable catalog (client-side index, no backend)       │
│  - Faceted by: type, Arches version, domain, license, tags  │
│  - Each entry: README, install snippet, links, badges       │
└──────────────────────▲──────────────────────────────────────┘
                       │  rebuilt on push by GitHub Actions
┌──────────────────────┴──────────────────────────────────────┐
│  github.com/archesproject/arches-packages  (registry repo)  │
│  - /packages/<name>.yaml  (one manifest per entry)          │
│  - PRs from community = submission                          │
│  - CODEOWNERS + 1-2 maintainers review                      │
└──────────────────────▲──────────────────────────────────────┘
                       │  enriched at build time
┌──────────────────────┴──────────────────────────────────────┐
│  PyPI + upstream GitHub repos (source of truth for code,    │
│  versions, downloads, stars, last-commit, CI status)        │
└─────────────────────────────────────────────────────────────┘
```

## Manifest Spec (`packages/<name>.yaml`)

Minimum viable fields the catalog needs for filtering and display:

```yaml
name: arches-lingo
kind: application          # extension | application
summary: Vocabulary management application built on Arches
repository: https://github.com/archesproject/arches-lingo
pypi: arches-lingo
docs: https://arches-lingo.readthedocs.io
license: AGPL-3.0
maintainers: [archesproject]
arches_versions: [">=8.0,<9.0"]
domains: [vocabularies, thesauri, skos]
tags: [skos, controlled-lists, reference-data]
status: stable             # experimental | beta | stable | maintenance
```

Build-time enrichment (no manual upkeep) fetches: latest PyPI release, monthly downloads, GitHub stars, last commit, default-branch CI status, README excerpt.

## Submission Flow (lightly moderated)

1. Contributor opens PR adding `packages/<name>.yaml`.
2. CI validates: schema, repo/PyPI URLs resolve, license SPDX valid, name not squatting an existing package.
3. One maintainer reviews for: scope fit (actually an Arches extension/app), no obvious harm, manifest accuracy. Target SLA: 1 week.
4. Merge → site rebuilds → entry live within minutes.

Maintainer load is bounded: review is gatekeeping, not curation. Quality signals (downloads, stars, CI status, "stable") are surfaced as data, not editorial judgement.

## Discoverability Mechanics

- **Search**: client-side index (Pagefind or Lunr) over name, summary, tags, domains, README.
- **Browse**: filter chips for kind, Arches version compatibility, domain, status.
- **Sort**: relevance, recent activity, downloads, alphabetical.
- **Cross-linking**: Arches core docs (Read the Docs) link out to catalog by topic; catalog entries link back to the relevant docs section.
- **RSS/Atom**: feed of new and updated packages so the community can follow without polling.

## Governance

- Registry repo lives under `archesproject` org with 2-3 designated maintainers (rotates with core team capacity).
- `MAINTAINING.md` documents review checklist so the bar is transparent and reproducible.
- Removal policy: maintainers can archive entries that go unmaintained >18 months, become broken, or violate the (short) code of conduct. Archival hides but doesn't delete — preserves history.
- Self-service updates: PyPI/GitHub metadata refreshes automatically; manifest changes via PR.

## Rollout Phases

1. **Phase 1 (complete)**: Schema, registry repo, static site scaffold, build pipeline. Seed with ~10 known extensions/apps (lingo, her, controlled-lists, modular-reports, querysets, component-lab, search, addressing, rascolls, ai-agent).
2. **Phase 2**: Polish UX, write contributor guide, integrate links from arches-docs.
3. **Phase 3**: Announce on community forum + GitHub; open for submissions.
4. **Ongoing**: Quarterly review of maintainer load; adjust review SLA or add maintainers if backlog grows.

## What This Plan Deliberately Avoids

- No managed services, no database, no auth — every component is free-tier or zero-cost.
- No alternate distribution channel — PyPI does what it already does well.
- No quality scoring beyond surfacing objective signals — avoids editorial overhead and political friction.
- No federation / multiple registries — one canonical source keeps discoverability strong.

## Success Metrics (revisit at 6 months)

- # of listed packages, # of community submissions vs. maintainer-seeded
- Catalog page views, search queries, click-throughs to repos/PyPI
- Maintainer review backlog size and median time-to-merge
- Community forum mentions of "found via catalog"
