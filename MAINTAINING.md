# Maintaining arches-packages

This document is for repository maintainers reviewing community-submitted pull requests.

---

## PR review checklist

Before merging any manifest PR, verify the following:

### Automated checks (must all pass)

CI runs automatically on every PR. Do not merge until all checks are green:

- [ ] **Schema validation** — YAML is valid and all required fields are present and typed correctly
- [ ] **URL resolution** — `repository` and `docs` (if present) return HTTP 200
- [ ] **SPDX license** — `license` field is a recognized SPDX identifier
- [ ] **PyPI presence** — `pypi` field (if present) resolves to a real package on pypi.org

### Manual review

- [ ] **`summary` is accurate** — one sentence, describes what the package actually does, ≤140 chars
- [ ] **`kind` is correct** — `extension` (standalone feature), `application` (domain composition), or `tool` (developer tooling)
- [ ] **`arches_versions` is plausible** — matches what the package's own `pyproject.toml` declares; not so broad it misleads users
- [ ] **`status` is honest** — `experimental` for anything pre-1.0 or untested at scale; `stable` only for production-ready packages
- [ ] **`tags` are useful** — lowercase, hyphen-separated, relevant to search; no spam tags
- [ ] **No duplicate** — check `packages/` for an existing manifest with the same name or PyPI package
- [ ] **Package exists** — the `repository` URL points to a real, accessible repository with recognizable Arches integration code

### Red flags (request changes or close)

- Repository is private or returns 404
- Package has no code (empty repo, placeholder)
- `summary` is promotional copy rather than a factual description
- `status: stable` for a package on PyPI with a pre-release version (0.x.y, alpha, beta)
- No Arches dependency visible in the repository

---

## Merge process

1. Ensure all automated CI checks pass
2. Complete the manual review checklist above
3. Approve the PR and merge using **Squash and merge**
4. The site build workflow will trigger automatically and deploy within a few minutes

---

## Adding a maintainer

Edit `.github/CODEOWNERS` to add a GitHub username to the `packages/` line. Maintainers receive review requests on every new manifest PR.

---

## Deprecating or removing packages

If a package appears abandoned or incompatible with all current Arches versions:

1. Change `status` to `maintenance` in the manifest (prefer this for packages that still work but are no longer developed)
2. Remove the manifest file entirely only if the repository is deleted or the package causes harm to users who install it

Open a PR with a brief explanation in the description. Any maintainer can merge deprecation/removal PRs without a second review.
