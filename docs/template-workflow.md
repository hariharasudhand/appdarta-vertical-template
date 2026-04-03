# Template Workflow

Use this template by creating a new repository from it or downloading it as an archive.

This template is vertical-owned business scope. The framework continues to own the CLI, UI shell, gateway, orchestration, schemas, and AI routing/accounting surfaces.

Recommended first step:

```bash
darta run-wizard
```

After bootstrap:

```bash
darta project inspect --file .
darta analyze inspect --project .
darta project signoff --project . --stage usecase --name <approver>
darta project signoff --project . --stage clarification --name <approver>
darta design inspect --project .
darta design compare --project .
darta project signoff --project . --stage design --name <approver>
```

Then evolve the vertical through:

- code generation
- build
- runtime
- test
- deploy
