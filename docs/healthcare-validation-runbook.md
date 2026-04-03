# Healthcare Validation Runbook

This runbook documents the recommended real-world validation path for a healthcare vertical built from `appdarta-vertical-template`.

It is intentionally written as an operator/developer checklist. The goal is to make one healthcare scenario runnable from a clean vertical repo without reverse-engineering internal tests or scattered notes.

This document prepares the steps only. It does not claim that every step below has already been executed in this repo snapshot.

## Outcome

Validate one concrete healthcare use case end to end:

- clinical intake arrives for contrast-imaging medication review
- public clinical reference material is imported into a healthcare Data Tank
- client-specific patient guidance is added as client-scoped evidence
- a three-agent review path produces an operator-readable recommendation

Expected business result:

- flag a moderate-risk metformin/contrast-dye review
- cite public guidance plus client-specific evidence
- produce a final operator-facing recommendation before release

## Recommended Validation Scenario

Use case:

- pre-imaging medication safety review for a patient taking metformin before contrast dye exposure

Why this one:

- it is easy to explain to non-engineers
- it needs both public standards and client-specific evidence
- it is narrow enough to validate without a full EHR integration story

## Recommended Three-Agent Shape

This is the recommended validation target for the healthcare showcase. It is slightly richer than the currently checked-in `hydrate -> review` reference flow.

1. `clinical-intake-normalizer`
   Responsibility: normalize the incoming task into a compact medication-review payload.

2. `medication-guidance-reviewer`
   Responsibility: read tank evidence from public healthcare references and client documents, then determine risk and supporting evidence.

3. `operator-disposition-writer`
   Responsibility: convert the review output into an operator-facing recommendation with next action, rationale, and evidence summary.

Recommended flow:

`clinical-intake-normalizer -> medication-guidance-reviewer -> operator-disposition-writer`

Recommended operator output:

- patient/task summary
- detected medication risk
- public evidence summary
- client-document evidence summary
- recommended next action

## Recommended Tank Layout

Use one tank:

- `healthcare-standards-tank`

Use these partitions:

- `fhir-r4`
- `fda-alerts`
- `client-documents`

Why this tank shape:

- `fhir-r4` gives public clinical structure and terminology context
- `fda-alerts` gives public drug-safety/regulatory signal
- `client-documents` gives case-specific evidence that should remain client-scoped

## Public Source Inputs

Use real public sources that are already compatible with the existing healthcare tank contract:

1. HL7 FHIR R4
   Source: `https://hl7.org/fhir/R4/`
   Use for: public standards and clinical terminology context

2. FDA drug enforcement API
   Source: `https://api.fda.gov/drug/enforcement.json`
   Use for: public safety and recall/regulatory context

Recommended local staging directory inside a vertical repo:

```text
demo/public-sources/
  fhir-r4-medicationrequest.html
  fhir-r4-observation.html
  fda-drug-enforcement.json
```

Recommended example fetch commands:

```bash
mkdir -p ./demo/public-sources

curl -L https://hl7.org/fhir/R4/medicationrequest.html -o ./demo/public-sources/fhir-r4-medicationrequest.html
curl -L https://hl7.org/fhir/R4/observation.html -o ./demo/public-sources/fhir-r4-observation.html
curl -L "https://api.fda.gov/drug/enforcement.json?search=product_description:metformin&limit=10" -o ./demo/public-sources/fda-drug-enforcement.json
```

These exact URLs are recommended because they are public, stable enough for manual validation, and clinically relevant to the chosen scenario.

## Client-Specific Inputs

Use a small client-scoped document set:

- one patient medication note
- one radiology/contrast instruction note

Recommended local directory:

```text
demo/sample-docs/
  patient-medication-guidance.txt
  contrast-dye-alert.txt
```

## Runbook

### 1. Bootstrap The Vertical

Start from a healthcare vertical repo created from the template:

```bash
darta run-wizard
darta doctor --skip-stack
darta project inspect --file .
```

### 2. Start Required Services

Start the local context service.

If you want runtime-host service mode instead of local-process runtime execution, also start `wasmtime-host` in HTTP mode.

Recommended service intent:

- context service on `http://127.0.0.1:18001`
- runtime host on `http://127.0.0.1:18091`

### 3. Build The Tank

Use the healthcare tank spec:

```bash
darta tank build --spec ./specs/tanks/healthcare-standards-tank.yaml
```

### 4. Import Public Clinical References

Ingest the staged public files into public partitions:

```bash
darta tank ingest --tank healthcare-standards-tank --partition fhir-r4 --file ./demo/public-sources/fhir-r4-medicationrequest.html
darta tank ingest --tank healthcare-standards-tank --partition fhir-r4 --file ./demo/public-sources/fhir-r4-observation.html
darta tank ingest --tank healthcare-standards-tank --partition fda-alerts --file ./demo/public-sources/fda-drug-enforcement.json
```

### 5. Import Client Documents

Ingest client-scoped documents:

```bash
darta tank ingest --tank healthcare-standards-tank --partition client-documents --file ./demo/sample-docs/patient-medication-guidance.txt --client-id client-oak
darta tank ingest --tank healthcare-standards-tank --partition client-documents --file ./demo/sample-docs/contrast-dye-alert.txt --client-id client-oak
```

### 6. Verify Tank Readiness

Recommended checks:

```bash
darta tank status --name healthcare-standards-tank
```

What to verify:

- `fhir-r4` is queryable
- `fda-alerts` is queryable
- `client-documents` is queryable
- freshness looks acceptable for the imported data

### 7. Validate The Vertical

Recommended checks:

```bash
darta validate --project .
darta build project --project .
darta test project --project .
```

### 8. Run The Healthcare Scenario

Use a task shaped around contrast-imaging medication review.

Recommended task content:

- patient is taking metformin
- contrast imaging is pending
- renal-risk review is required before proceeding

Recommended run:

```bash
darta run project --project .
```

### 9. Inspect Output Artifacts

Recommended artifact checks:

- prepared bundle exists
- runtime result exists
- runtime handoff exists
- verification brief exists

Recommended files:

```text
demo/artifacts/healthcare-reference-run.json
demo/artifacts/healthcare-reference-result.json
demo/artifacts/healthcare-reference-run-runtime-handoff.md
demo/artifacts/healthcare-reference-run-verification-brief.md
```

### 10. Validate The Result

The final result should be easy for an operator to read.

Minimum expected result characteristics:

- identifies metformin/contrast-dye review as the scenario
- reports a moderate-risk or review-required outcome
- references public healthcare evidence
- references client-document evidence
- recommends a concrete next action such as clinician review or temporary medication hold review

## Success Criteria

Call the run successful when all of the following are true:

1. the tank is built and queryable
2. at least one public source file and one client document are ingested successfully
3. the project validates cleanly
4. the project run emits the expected artifact set
5. the final output contains both evidence-backed reasoning and an operator-facing recommendation

## Current Repo Reality

The current checked-in healthcare reference in the framework repo is still the simpler `hydrate -> review` path centered on `drug-interaction-checker`.

This runbook intentionally documents the next cleaner public validation target:

- same healthcare domain
- same tank contract
- richer three-agent story
- real public-source ingestion

That makes it a better public validation scenario for `appdarta-vertical-template` users.

## Troubleshooting Checklist

If validation fails, check these first:

1. context service is reachable
2. runtime host is reachable if HTTP mode is being used
3. tank partitions were ingested into the expected names
4. client-scoped documents used the same `client-id` as the task/run
5. the final agent/runtime path still points at the expected WASM module and tank refs

## Suggested Follow-On Work

After the first manual validation pass, tighten this runbook with:

1. the exact generated healthcare template paths from `darta init --domain healthcare`
2. the final committed names for the three showcase agents
3. one checked-in sample task JSON dedicated to this three-agent scenario
4. one automated smoke check for artifact presence and result shape
