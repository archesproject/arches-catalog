# arches-packages

The community registry for [Arches](https://www.archesproject.org/) extensions and applications. Browse the catalog at **https://archesproject.github.io/arches-catalog** (or your deployed URL).

---

## What belongs here

| Kind | Description | Examples |
|------|-------------|---------|
| `extension` | Adds functionality to any Arches installation | arches-modular-reports, arches-querysets, arches-component-lab |
| `application` | A domain-specific composition of core + extensions | arches-lingo, arches-her, arches-addressing |

The registry is **discovery-only** — code stays in the author's own repository and on PyPI. This repo holds only the metadata manifests.

---

## Submitting a package

### 1. Fork this repository

Click **Fork** in the top right of this page.

### 2. Create a manifest file

Create a new file at `packages/<your-package-name>.yaml`. Use the schema below and the existing manifests in `packages/` as examples.

```yaml
name: your-package-name         # PyPI name or unique slug (lowercase, hyphens)
kind: application               # extension | application
summary: One sentence that explains what your package does.
repository: https://github.com/you/your-package
pypi: your-package-name         # omit if not on PyPI
docs: https://your-docs-url     # omit if no docs site
license: AGPL-3.0-or-later      # SPDX identifier
maintainers:
  - your-github-username
arches_versions: ">=8.1,<9.0"  # PEP 440 version specifier
domains:
  - cultural-heritage           # high-level subject area(s)
tags:
  - your-tag                    # lowercase, hyphen-separated
status: experimental            # experimental | beta | stable | maintenance
```

**Full schema reference:** [`schema.json`](schema.json)

### 3. Open a pull request

Open a PR from your fork targeting `main`. The automated CI will:

- Validate your YAML against the manifest schema
- Check that `repository` and `docs` URLs resolve (HTTP 200)
- Verify the `license` value is a recognized SPDX identifier
- Confirm `pypi` (if provided) resolves on PyPI

PRs that pass CI will be reviewed by a maintainer within a few days. See [MAINTAINING.md](MAINTAINING.md) for the review checklist.

---

## Updating an existing manifest

Open a PR that modifies the relevant `packages/<name>.yaml` file. The same CI validation applies.

---

## Removing a package

Open a PR that deletes the manifest file. Include a brief reason in the PR description (e.g. "package is abandoned and no longer compatible with current Arches versions").

---

## Manifest schema

See [`schema.json`](schema.json) for the full JSON Schema definition. Key fields:

| Field | Required | Description |
|-------|----------|-------------|
| `name` | Yes | Package slug (matches PyPI name if published) |
| `kind` | Yes | `extension` or `application` |
| `summary` | Yes | One-sentence description, 140 chars max |
| `repository` | Yes | Source repository URL |
| `license` | Yes | SPDX license identifier |
| `maintainers` | Yes | GitHub usernames/orgs |
| `arches_versions` | Yes | PEP 440 specifier for compatible Arches versions |
| `status` | Yes | `experimental`, `beta`, `stable`, or `maintenance` |
| `pypi` | No | PyPI package name |
| `docs` | No | Documentation URL |
| `domains` | No | High-level subject domains |
| `tags` | No | Freeform search tags (lowercase, hyphen-separated) |

---

## Local validation

You can validate your manifest locally before opening a PR:

```bash
pip install check-jsonschema
check-jsonschema --schemafile schema.json packages/your-package-name.yaml
```

---

## License

Manifest data in this repository is released under [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) — no rights reserved. Each listed package carries its own license.
