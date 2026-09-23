# CLAUDE.md — AWS-FGT-301

> Global preferences (planning workflow, code quality, operations): `~/.claude/CLAUDE.md`
>
> On-demand docs (this repo is a FortinetCloudCSE Hugo workshop — `docs/` is machine-owned and
> destroyed by automation, so detail lives in root-level `plans/claude/`, not `docs/claude/`):
>
> | Doc | Read when |
> |-----|-----------|
> | [plans/claude/reference.md](plans/claude/reference.md) | You need the full `content/`/`.github/workflows/` file map before adding a page or tracing a workflow |
> | [plans/claude/gotchas.md](plans/claude/gotchas.md) | Editing `scripts/repoConfig.json`, anything `docs/`-adjacent, `static.yml`, `codex-advisory-review.yml`, `content/allpages.md`, `content/XpertsCSV.csv`, the "Xperts Preso" shortcut, `fdevsec.yaml`, `Jenkinsfile`, `migration_log*.csv`, or `package.json` |

## Project in One Line

A FortinetCloudCSE hands-on workshop — "AWS 301 SDWAN with AWS Cloud WAN" — published as a Hugo static site to GitHub Pages. Content-only: there is no lab automation in this repo; students work against a pre-provisioned AWS environment.

## Stack Quick Reference

| Layer | Tech | Port |
|-------|------|------|
| Site generator | Hugo via `public.ecr.aws/k4n6m5h8/fortinet-hugo:latest` | 1313 (local dev) |
| Site theme/config | [CentralRepo](https://github.com/FortinetCloudCSE/CentralRepo) — mounted at build time, **not** in this repo | — |
| Local dev driver | [fortihugorunner](https://github.com/FortinetCloudCSE/fortihugorunner) CLI | — |
| Hosting | GitHub Pages (`https://fortinetcloudcse.github.io/AWS-FGT-301/`) | — |

No `Dockerfile`, `hugo.toml`, `config.toml`, `docs/`, or `static/` in this repo — see reference doc.

## Build & Run Commands

```bash
# Preview the site locally (requires Docker + fortihugorunner on PATH)
fortihugorunner pull-image --env author-dev
fortihugorunner launch-server \
  --docker-image fortinet-hugo:latest \
  --host-port 1313 --container-port 1313 --watch-dir .
# open http://localhost:1313

# Reproduce the CI static build exactly
docker run --rm -v "$PWD:/home/UserRepo" fortinet-hugo:latest build
```

There is no test suite. Content changes are validated by rendering locally.

## Critical Patterns & Gotchas (one-liners — full detail in `plans/claude/gotchas.md`)

- Site chrome (title, banner, author, sidebar shortcuts, quiz URL) lives in `scripts/repoConfig.json`, not a Hugo config file — there isn't one here on purpose (CentralRepo supplies it).
- Never put anything in `docs/` — it's gitignored and gets hard-deleted from `main` by the template upgrade tool on every run.
- Plan/log/spec files go in root-level `plans/` (`NNNN_YYYY-MM-DD_<user>_<slug>.md`), never `docs/plans/`.
- `.github/workflows/static.yml` is template-managed — `batch_repo_update.py` overwrites it and deletes several files (including `FTNThugoFlow.html`) on the next batch run; don't hand-edit expecting it to stick.
- `content/allpages.md` is front-matter only; the single-page view renders via a CentralRepo layout — reconfirm it renders after structural content edits.
- `content/XpertsCSV.csv` is referenced only via a `repoConfig.json` sidebar shortcut — keep its column shape intact.
- The "Xperts Preso" sidebar shortcut points at a PDF not tracked in this repo — don't assume it resolves.
- Shortcodes: `{{% notice %}}`, `{{% expand %}}`, and repo-local `{{<success>}}`/`{{<fail>}}` — grep existing content before inventing a new one.
- Page order is `weight` in front matter, not filename numbering (numeric prefixes/gaps are cosmetic).
- Deploy triggers only on push to `main` (or manual `workflow_dispatch`); branch pushes don't deploy.
- `fdevsec.yaml`'s `id.app` is still a placeholder — leave it unless asked to fix.
- `codex-advisory-review.yml` deliberately uses `pull_request_target` for secret access — don't convert to `pull_request`.
- `Jenkinsfile` doesn't actually lint content; its check stage is disabled.
- `migration_log*.csv` are stale, not-about-this-repo artifacts — never treat as inputs.
- `package.json`/`package-lock.json` are gitignored-but-tracked — editing them is a real committed change.

## Environment Variables

None required for authoring. CI-only secrets: `LW_ACCOUNT_NAME`, `LW_API_KEY`, `LW_API_SECRET` (Lacework), `OPENAI_API_KEY` (advisory review), `GITHUB_TOKEN` (Pages).

Optional locally: `DOCKER_CONTEXT` / `DOCKER_HOST` — fortihugorunner honors the active Docker context.

## Common Tasks

**Add a workshop section**: create the page bundle under the right `content/N_*/` parent with `title`, `linkTitle`, `weight` front matter; preview with `launch-server`; confirm the `allpages` single-page view still renders.

**Change site chrome** (title, banner, sidebar links, quiz URL): edit `scripts/repoConfig.json`.

**Plan/log/spec files**: write them to root-level `plans/` as `NNNN_YYYY-MM-DD_<git-username>_<slug>.md` (+ optional `.log.md`, `.spec.md`). Never `docs/plans/`. `NNNN` is a per-repo sequence; the log is optional; on completion, durable facts get promoted into this file and the plan is left to decay. See `plans/README.md`.

**Debug a broken published page**: run the CI build command locally — the dev server is more forgiving than the static build. `errorLevel` in `scripts/repoConfig.json` is `warning`, so Hugo warnings do not fail the build.
