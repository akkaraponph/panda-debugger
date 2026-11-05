# Bug Investigation Report

## 1. Problem Summary

State the defect, affected path, and conditions. Mark any unverified user report as reported.

## 2. Expected vs Actual Behavior

| Expected | Actual | Basis |
|---|---|---|
| … | … | Specification, test, log, or direct observation |

## 3. Evidence

List the relevant log lines, stack frames, code locations, runtime observations, and test outputs. Separate facts from inferences.

## 4. Reproduction

Give steps or commands, environment, and observed result. If not reproduced, state **Not reproduced** and explain the limitation.

## 5. Execution Flow

`Input → entry point → processing → dependency or state change → failure location`

Identify where the defect originates and where the symptom appears.

## 6. Root Cause Analysis

For each material finding:

- **Status:** CONFIRMED / LIKELY / UNKNOWN
- **Cause or hypothesis:** …
- **Supporting evidence:** …
- **Contradicting evidence or missing check:** …
- **Causal explanation:** …
- **Contributing factors:** …

## 7. Proposed Fix

Explain the smallest change, why it addresses the cause, affected components, relevant alternatives, and regression risk. State whether it was implemented.

## 8. Test Cases

| Test Case | Scenario | Expected Result | Status |
|---|---|---|---|
| TC-01 | Original failure | … | Not Run |
| TC-02 | Normal behavior | … | Not Run |
| TC-03 | Important edge case | … | Not Run |
| TC-04 | Regression scenario | … | Not Run |

Use only relevant rows; do not imply that proposed tests ran.

## 9. Verification Results

List executed commands or steps and actual results. If nothing was run, say **Not run** and why. Do not claim the fix is verified without passing relevant checks.

## 10. Remaining Risks

List unresolved questions, evidence gaps, and plausible regression risks. If none remain, state the scope of the verification that supports that conclusion.
