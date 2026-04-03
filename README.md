<div align="center">
  <p>
    <a href="https://www.dhruvialabs.com/">
      <img src="https://raw.githubusercontent.com/hariharasudhand/appdarta-vertical-template/master/logo_dhruvialabs.png" alt="Dhruvia Labs" width="260">
    </a>
  </p>
  <p><strong>A framework from <a href="https://www.dhruvialabs.com/">Dhruvia Labs</a></strong></p>
</div>

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

The working model is simple:

- your repo owns the domain workflow
- AppDarta owns the framework machinery underneath it

That lets your team focus on the functional piece while the framework handles the technical lifting around runtime, gateway, policy, AI governance, and knowledge integration.

## Start Here

If you just want the fastest path:

```bash
darta run-wizard
darta doctor --skip-stack
darta project inspect --file .
darta validate --project .
darta build project --project .
darta run project --project .
```

That path should tell you:

- the project is personalized correctly
- the pinned framework release is usable
- the starter specs validate
- the vertical can build and run through the framework lifecycle

## Validation Runbooks

For a concrete, operator-style walkthrough instead of generic lifecycle docs, use:

- [Healthcare Validation Runbook](docs/healthcare-validation-runbook.md)

## Getting Started

Start here inside the template repo:

```bash
darta run-wizard
darta doctor --skip-stack
darta project inspect
darta analyze inspect
darta design inspect
darta design compare
```

`darta run-wizard` is the guided project path after the framework is available. It should:

1. ask for the project name, domain, and summary
2. personalize the checked-out template in place
3. pin the project to the intended AppDarta Engine release or `dev` local-source mode
4. create lifecycle state for staged resume
5. stop and ask you to rerun if the repo directory needs to be renamed
6. resume from the first incomplete stage on rerun

Then continue with:

- `darta codegen plan --project .`
- `darta codegen prepare --project . --task <task-id>`
- `darta build project`
- `darta run project`

## What You Configure In A Vertical

Use the template repo to define business-scoped behavior:

- use case and design specs
- domain agents
- domain policies
- domain tanks and sources
- flows and orchestration bindings
- runtime modules and demo fixtures

The framework continues to handle:

- lifecycle commands
- gateway/runtime contracts
- AI/provider control plane
- tank integration surfaces
- operator/runtime visibility

This split is the reason the template matters. You are not starting from a blank repo and rebuilding the technical substrate yourself.

## Where AI Is Configured

In a vertical project, the practical AI story is:

- business specs describe where AI is needed
- framework-managed roles decide which provider/model path is allowed
- the framework records token and cost usage

Look for:

- design-time AI config in `specs/design/`
- framework-visible role and provider bindings through the project’s framework-managed AI configuration

The point is to keep AI usage visible and governed instead of burying provider calls in random project code.

## Typical CLI Flow

```bash
darta run-wizard
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
- `darta run-wizard` is the supported first command inside a fresh template checkout.
- `darta project bootstrap` is only needed when you want to switch or reinstall the pinned framework release explicitly.

---

<div align="center">
  <p><strong>Currently available for trial use.</strong></p>
  <p>For production adoption, rollout support, or commercial discussions, contact Dhruvia Labs.</p>
  <p>We support flexible pricing, including outcome-based engagement models where that fits better than heavy fixed pricing.</p>
</div>
