# Sample Design Handoff

## Inputs To Codegen And Build

- Solution design: sample-solution-design.yaml
- Technical sequence diagram: sample-technical-sequence.swim
- AI config: sample-ai-config.yaml
- Model role bindings: sample-model-role-bindings.yaml

## Build Expectations

- prefer reuse of shared common/util assets before creating vertical-specific ones
- generate code only for agreed vertical assets
- keep technical sequence aligned with the primary runtime path
