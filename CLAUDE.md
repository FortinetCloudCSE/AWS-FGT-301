# CLAUDE.md — AWS-FGT-301

> Global preferences (planning workflow, code quality, operations): `~/.claude/CLAUDE.md`

## Project in One Line

A FortinetCloudCSE hands-on workshop — "AWS 301 SDWAN with AWS Cloud WAN" — published as a Hugo static site to GitHub Pages. Content-only: there is no lab automation in this repo; students work against a pre-provisioned AWS environment.

## Stack Quick Reference

| Layer | Tech | Port |
|-------|------|------|
| Site generator | Hugo via `public.ecr.aws/k4n6m5h8/fortinet-hugo:latest` | 1313 (local dev) |
| Site theme/config | [CentralRepo](https://github.com/FortinetCloudCSE/CentralRepo) — mounted at build time, **not** in this repo | — |
| Local dev driver | [fortihugorunner](https://github.com/FortinetCloudCSE/fortihugorunner) CLI | — |
| Hosting | GitHub Pages (`https://fortinetcloudcse.github.io/AWS-FGT-301/`) | — |

## Key File Map

```
content/                    — workshop pages (Hugo page bundles, ordered by `weight`)
  _index.md                 — `title: SD-WAN Workshop`, `archetype: home`, hand-written contents list
  allpages.md               — front matter only: `layout: 'allpages'`, `tec_title: Single Page View`
  XpertsCSV.csv             — bulk-import CSV, published as a sidebar shortcut
  0_LabPrep/                — _index.md, 02_logistics.md, 03_awsnetworkingconcepts.md,
                              04_awstipstricks.md, 05_awsec2serialconsole.md
  1_SD-WAN/                 — _index.md + 1_SD-WAN_Key_Components, 2_SD-WAN_Demo_PreparationSteps,
                              3_SD-WAN_Configuration_Overview, 4_SD-WAN_Monitoring,
                              5_SD-WAN_Link_Impairment, 6_ADVPN, 7_ADVPN_Link_Impairment,
                              8_Application_Performance_Monitoring, 9_Provisioning
  2_CloudWAN/               — _index.md, 35_task.md, 36_task.md, 37_task.md
layouts/shortcodes/         — repo-local shortcodes: ContainerFlow.html, success.html, fail.html,
                              FTNThugoFlow.html, fortihugorunner.html
scripts/repoConfig.json     — per-repo site config (title, author, banner, shortcuts, quizUrl)
plans/                      — plan/log/spec files for this repo (see gotchas); plans/README.md explains why
Jenkinsfile                 — GitHub commit-status pipeline; its content-check stage is disabled
fdevsec.yaml                — FortiDevSec scan config
.github/workflows/
  static.yml                     — build + deploy to Pages; `push` to `main` + `workflow_dispatch`
                                   (inputs: `runner_type`, `image_variant` prod/dev)
  lacework-code-security-pr.yml  — `on: pull_request`
  codex-advisory-review.yml      — `on: pull_request_target` (opened, synchronize, reopened, ready_for_review)
repo_upgrade_spec.json / .repo_upgrade_version  — both say `Hugo-v2.1`
migration_log.csv, migration_log_dry_run.csv    — historical image-migration artifacts
```

No `Dockerfile`, `hugo.toml`, `config.toml`, `docs/`, or `static/` in this repo.

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

## Critical Patterns & Gotchas

- **No `hugo.toml` or `config.toml` here — on purpose.** Hugo config, theme, and layouts come from CentralRepo, which the container mounts alongside this repo. To change the site title, banner text, author, or sidebar shortcuts, edit **`scripts/repoConfig.json`**. Live values: `workshopTitle` `"AWS 301 SDWAN with AWS Cloud WAN"`, `author` `"Hiruy Aberra, Regis Martin"`, `errorLevel` `"warning"`, `marketingCode` `"FortiHugo202"`, `themeVariant` `"CloudCSEMovie"`, plus a `quizUrl` pointing at a Cloud Run app.
- **`docs/` is doubly machine-owned — never put anything there.** `.gitignore` excludes `docs/`, and `CentralRepo/scripts/batch_repo_update.py` hardcodes `FOLDERS_TO_DELETE = ["docs"]` with `BRANCH = "main"`, deleting every blob under `docs/` via the GitHub tree API and pushing that deletion straight to `main`. Nuance: that script does **not** read `repo_upgrade_spec.json` — the spec file documents the same lists but is not what executes, so the two can silently drift.
- **Plan/log/spec files go in root-level `plans/`, not `docs/plans/`.** `plans/` is inert to Hugo (Hugo only reads `content/`, `layouts/`, `static/`, `assets/`, `data/`, `i18n/`, `archetypes/`, `themes/`) and is outside `FOLDERS_TO_DELETE`. `plans/README.md` in this repo states the convention.
- **`.github/workflows/static.yml` is template-managed.** `batch_repo_update.py` overwrites it from the operator's CentralRepo checkout (`FILES_TO_COPY`) — hand-edits get lost. It also copies in a `Dockerfile` (this repo currently has none) and deletes `layouts/shortcodes/FTNThugoFlow.html`, `docker-compose.yml`, `hugo.toml`, `config.toml`, and the `scripts/docker_*.sh` set. `FTNThugoFlow.html` is present here and unused by content — expect it to disappear on the next batch run.
- **`content/allpages.md` is front matter only** (`layout: 'allpages'`) — the whole-workshop single page is rendered by the CentralRepo layout, not by anything in this file. Adding or reordering modules changes that page implicitly; check it renders after structural edits.
- **`content/XpertsCSV.csv` is data, not prose, and no content page references it.** It reaches students only through the `"Bulk Import CSV"` sidebar shortcut in `scripts/repoConfig.json` (`URL: XpertsCSV.csv`). Keep the column shape intact; renaming it means editing `repoConfig.json`.
- **A sidebar shortcut points at a file that is not in this repo:** `"Xperts Preso"` → `Xperts-2025-AWS-201.pdf`. There is no `static/` directory and no PDF tracked here, so that link resolves only if the asset is supplied elsewhere. Don't assume it works.
- **Container mount layout:** the workshop repo mounts at `/home/UserRepo`; CentralRepo lives at `/home/CentralRepo`; Hugo output lands in `/home/CentralRepo/public` (CI copies that to `docs/` and uploads it as the Pages artifact).
- **Shortcodes:** content uses `{{% notice %}}` (37x), `{{% expand %}}` (17x), and the repo-local `{{<success>}}` / `{{<fail>}}` (6x each, defined in `layouts/shortcodes/`). Everything else comes from the CentralRepo theme. Grep existing content before inventing a new shortcode.
- **Page ordering is `weight` in front matter,** not filename. The numeric prefixes (`0_LabPrep`, `1_SD-WAN`) and the gaps in file numbering (`0_LabPrep` starts at `02_`, `2_CloudWAN` at `35_`) are cosmetic.
- **Deploy triggers on push to `main`** (plus manual `workflow_dispatch`). Branch pushes do not deploy.
- **`fdevsec.yaml` is half-configured:** `id.org` is a real UUID (`2e3b7756-…`), but `id.app` is still the literal placeholder `<insert app id here>`. Scanners enabled: `sast`, `secret`, `sca`, `iac`, `container`; `resource.serial_scan: false`; `fail_pipeline.risk_rating: 7`. Leave it unless asked.
- **`codex-advisory-review.yml` uses `pull_request_target` deliberately** so `OPENAI_API_KEY` comes from the trusted base branch and the PR head is never checked out. Do not convert it to `pull_request`.
- **`Jenkinsfile` does not lint content.** Its "Checking for question/discussion section" stage is gated by `when { expression { false } }`; what actually runs is `deleteDir()` plus a GitHub commit-status update.
- **`migration_log*.csv` are stale artifacts and not even about this repo** — their paths point at `/home/ubuntu/pythonProjects/UserRepo/content/…`. Never treat them as inputs.
- **`package.json` / `package-lock.json` are listed in `.gitignore` but tracked** (they predate the ignore entries). Editing them is a real, committed change.

## Environment Variables

None required for authoring. CI-only secrets: `LW_ACCOUNT_NAME`, `LW_API_KEY`, `LW_API_SECRET` (Lacework), `OPENAI_API_KEY` (advisory review), `GITHUB_TOKEN` (Pages).

Optional locally: `DOCKER_CONTEXT` / `DOCKER_HOST` — fortihugorunner honors the active Docker context.

## Common Tasks

**Add a workshop section**: create the page bundle under the right `content/N_*/` parent with `title`, `linkTitle`, `weight` front matter; preview with `launch-server`; confirm the `allpages` single-page view still renders.

**Change site chrome** (title, banner, sidebar links, quiz URL): edit `scripts/repoConfig.json`.

**Plan/log/spec files**: write them to root-level `plans/` as `YYYY-MM-DD_<git-username>_<slug>.md` (+ `.log.md`, optional `.spec.md`). Never `docs/plans/`.

**Debug a broken published page**: run the CI build command locally — the dev server is more forgiving than the static build. `errorLevel` in `scripts/repoConfig.json` is `warning`, so Hugo warnings do not fail the build.
