# AppDarta Vertical Template

This repository is the public starter template for an AppDarta vertical project.

The framework product lives in [`appdarta-framework`](https://github.com/hariharasudhand/appdarta-framework). Start there if you need framework install docs, releases, or the product overview.

## What This Repository Is

It contains:

- `appdarta.project.yaml`
- `appdarta.framework.yaml`
- root lifecycle starter specs
- starter modules and commons layout

It does not contain framework source or framework binaries.

This template is meant to become your business-scoped vertical repository.

## What You Do With It

Use this template to create a new vertical project, then move through the AppDarta lifecycle:

- `analyze`
- `design`
- `codegen`
- `build`
- `run`

## Getting Started

1. Install an AppDarta framework release.
2. Add `APPDARTA_HOME/bin` to `PATH`.
3. Run:

```bash
./bin/bootstrap-framework
darta doctor --skip-stack
darta project inspect
darta analyze inspect
darta design inspect
darta design compare
```

Then continue with the lifecycle:

- `darta codegen plan --project .`
- `darta codegen prepare --project . --task <task-id>`
- `darta build project`
- `darta run project`

## Typical CLI Flow

```bash
darta project bootstrap
darta doctor --skip-stack
darta project inspect
darta analyze inspect
darta design inspect
darta design compare
darta codegen plan --project .
```

## Notes

- Keep framework source separate from this repo.
- Vertical repos should stay business-scoped.
- The framework binary owns schemas, gateway, centralized UI, and shared control-plane logic.
- Keep `appdarta.framework.yaml` committed so the project stays pinned to the intended framework release.
