# AppDarta Vertical Template

This repository is the public starter template for an AppDarta vertical project.

It contains:

- `appdarta.project.yaml`
- `appdarta.framework.yaml`
- root lifecycle starter specs
- starter modules and commons layout

It does not contain framework source or framework binaries.

## Getting Started

1. Install an AppDarta framework release.
2. Add `APPDARTA_HOME/bin` to `PATH`.
3. Run:

```bash
./bin/bootstrap-framework
darta doctor --skip-stack
darta analyze inspect
darta design inspect
```

Then continue with the lifecycle:

- `design compare`
- `codegen`
- `build`
- `run`

## Notes

- Keep framework source separate from this repo.
- Vertical repos should stay business-scoped.
- The framework binary owns schemas, gateway, centralized UI, and shared control-plane logic.
