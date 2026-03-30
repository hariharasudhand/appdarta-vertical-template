# AppDarta Vertical Template

> **Beta**  
> AppDarta is in active development. Early trials and feedback are welcome, but do not expect this starter template or every lifecycle command to work completely yet.

This repository is the public starter template for an AppDarta vertical project.

The framework product lives in [`appdarta-framework`](https://github.com/hariharasudhand/appdarta-framework). Start there if you need install docs, architecture, lifecycle guidance, or releases.

## What This Repository Is

It contains:

- `appdarta.project.yaml`
- `appdarta.framework.yaml`
- root lifecycle starter specs
- starter modules and commons layout

It does not contain framework source or framework release artifacts.

This template is meant to become your business-scoped vertical repository.

## Getting Started

1. Install an AppDarta Engine release.
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

Then continue with:

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
darta build project
darta run project
```

## Notes

- Keep framework source separate from this repo.
- Vertical repos should stay business-scoped.
- Keep `appdarta.framework.yaml` committed so the project stays pinned to the intended AppDarta Engine release.
