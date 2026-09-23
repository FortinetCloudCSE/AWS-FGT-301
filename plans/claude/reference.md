# AWS-FGT-301 — Key File Map

> On-demand detail for `CLAUDE.md`. Read when you need the full repo layout (adding a page,
> tracing a workflow, or checking what a file is for before editing it).

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
plans/                      — plan/log/spec files, `NNNN_` prefixed (see gotchas); plans/README.md explains why
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
