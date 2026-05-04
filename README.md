<div align="center">
  <p>
    <a href="https://www.dhruvialabs.com/">
      <img src="https://raw.githubusercontent.com/hariharasudhand/appdarta-vertical-template/master/logo_dhruvialabs.png" alt="Dhruvia Labs" width="260">
    </a>
  </p>
  <p><strong>A framework from <a href="https://www.dhruvialabs.com/">Dhruvia Labs</a></strong></p>
</div>

# Darta Vertical Template

> **Beta** — Active development. Early trials and feedback are welcome.

This is the public starter template for a Darta vertical project.

A vertical is your domain app — it carries your agents, your knowledge, your flows, and your rules. The Darta Framework handles everything else: execution, routing, context, policy evaluation, and lifecycle management.

The framework product lives in [`appdarta-framework`](https://github.com/hariharasudhand/appdarta-framework). **Install the framework first before working in this template.**

---

## What This Repository Contains

- `appdarta.project.yaml` — project manifest
- `appdarta.framework.yaml` — framework release pin
- root lifecycle starter specs
- starter modules, commons, runtime, and UI layout

It does not contain framework source or framework binaries. Those live in `APPDARTA_HOME` after install.

---

## Before You Start — Read These

If you are new to Darta, read these in order before touching any code. Each one is short.

| # | Document | Why |
|---|---|---|
| 1 | [appdarta-framework README](https://github.com/hariharasudhand/appdarta-framework#readme) | What Darta is, the three products, and the install path |
| 2 | [Component model](https://github.com/hariharasudhand/appdarta-framework/blob/master/docs/component-model.md) | What the platform owns vs what your vertical owns — the most important mental model |
| 3 | [Architecture](https://github.com/hariharasudhand/appdarta-framework/blob/master/docs/architecture.md) | How CLI → Gateway → Runtime → Tanks fit together |
| 4 | [Data tanks](https://github.com/hariharasudhand/appdarta-framework/blob/master/docs/data-tanks.md) | How verticals manage and attach domain knowledge |
| 5 | [Darta Dhil](https://github.com/hariharasudhand/appdarta-framework/blob/master/docs/dhil.md) | How AI routing works — needed before wiring any AI actions |
| 6 | [Vertical lifecycle walkthrough](https://github.com/hariharasudhand/appdarta-framework/blob/master/docs/quickstart.md) | End-to-end path from setup to deploy |

---

## Prerequisites

Install the Darta Framework once before cloning this template.

**Supported platforms:** macOS (Apple Silicon and Intel), Linux x86_64 and arm64, and Windows via WSL2. Native Windows is not supported.

### macOS / Linux

Add to your shell profile (`~/.zshrc` or `~/.bashrc`), then restart your terminal:

```bash
export APPDARTA_HOME="${APPDARTA_HOME:-$HOME/.appdarta}"
export PATH="$APPDARTA_HOME/bin:$PATH"
```

Download and run the installer:

```bash
curl -fsSL https://raw.githubusercontent.com/hariharasudhand/appdarta-framework/master/scripts/install_framework.sh -o install_darta.sh
bash install_darta.sh
```

The installer detects your OS and CPU automatically and downloads the right binary. When prompted, select **vDR.0.4**. Discard `install_darta.sh` after.

### Windows

Install [WSL2 with Ubuntu](https://learn.microsoft.com/en-us/windows/wsl/install), open a WSL2 terminal, and follow the macOS / Linux steps above exactly.

### Verify

```bash
darta version
darta framework current
```

---

## Getting Started

Clone this template, then run the wizard:

```bash
git clone https://github.com/hariharasudhand/appdarta-vertical-template.git my-vertical
cd my-vertical
darta run-wizard
darta doctor --skip-stack
```

`darta run-wizard` personalises the template in place — it asks for your project name, domain, and summary, pins the framework release, and sets up lifecycle state. Rerun it to resume from where you left off.

Once the wizard is done, move through the lifecycle:

```bash
darta project inspect --file .
darta analyze inspect --project .
darta design inspect --project .
darta validate --project .
darta build project --project .
darta stack up
darta ui serve
darta project run --project .
```

`darta stack up` starts the local framework services (runtime host, context service, gateway).  
`darta ui serve` opens the browser-based wizard — it covers the full lifecycle from Setup through to Deploy.

---

## Typical Full Lifecycle

```bash
darta run-wizard
darta doctor --skip-stack
darta project inspect --file .
darta analyze inspect --project .
darta design inspect --project .
darta validate --project .
darta codegen plan --project .
darta codegen prepare --project . --task <task-id>
darta build project --project .
darta stack up
darta ui serve
darta project run --project .
darta deploy plan --project .
```

---

## What You Configure In A Vertical

| What you declare | File | Schema |
|---|---|---|
| An agent and its runtime | `specs/agents/*.yaml` | `AgentSpec` |
| How agents coordinate | `specs/flows/*.yaml` | `FlowSpec` |
| Decision rules | `specs/policies/*.yaml` | `PolicySpec` |
| Domain knowledge | `specs/tanks/*.yaml` | `DataTankSpec` |
| Gateway execution plan | `specs/orchestration/*.yaml` | `OrchestrationSpec` |

You do not modify the platform. The framework owns the technical substrate — runtime dispatch, gateway, policy evaluation, tank integration, and AI/provider routing.

---

## Validation Runbooks

For a concrete operator-style walkthrough:

- [Healthcare Validation Runbook](docs/healthcare-validation-runbook.md)

---

## Notes

- Keep `appdarta.framework.yaml` committed — it pins the required framework release.
- `darta run-wizard` is the first command to run in a fresh template checkout.
- `darta project bootstrap` is only needed if you want to explicitly switch or reinstall the pinned framework release.
- Keep framework source and binaries out of this repo — they live in `APPDARTA_HOME`.

---

<div align="center">
  <p><strong>Currently available for trial use.</strong></p>
  <p>For production adoption, rollout support, or commercial discussions, contact <a href="https://www.dhruvialabs.com/">Dhruvia Labs</a>.</p>
</div>
