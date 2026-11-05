# Evaluation guide

These three scenarios are synthetic text fixtures, not runnable applications. They test investigation quality and honesty about evidence. They do not by themselves establish that the Skill improves real-world bug resolution.

## Fair comparison

1. Give the same scenario file and core request to an agent without this Skill, then to an agent with `$panda-debugger` available and explicitly invoked. Use the same model, tools, time budget, and other instructions where possible. Start each run in a fresh context.
2. Do not give either agent this evaluation guide or the answer key below. Do not add hints about the suspected cause.
3. Save the full answers and record whether each agent actually ran any command. The supplied snippets cannot be executed alone, so an honest answer should say **Not reproduced** and mark proposed tests **Not Run**.
4. Score each answer independently against the same rubric. Have a human reviewer check disputed causal claims. Compare the six scores, error types, and investigation time; do not report a performance gain before running the comparison.

## Scoring rubric (100 points)

| Criterion | Points | Award points for |
|---|---:|---|
| Root Cause Accuracy | 30 | Identifies the mechanism causing the reported behavior and separates contributing risks from the primary cause. |
| Evidence & Execution Trace | 20 | Connects input, code path, state or dependency, and symptom with precise supplied evidence. |
| Fix Correctness | 20 | Proposes a minimal change that closes the causal gap without introducing a new failure mode. |
| Test Case Quality | 15 | Covers the original failure, normal path, and important boundary or concurrent path. |
| Hallucination Prevention | 10 | Distinguishes facts from inference; does not claim reproduction, executed tests, or unseen configuration. |
| Report Quality | 5 | Uses short root-cause bullets, clear B2-level English, confidence labels, and a concise account of remaining risks. |
| **Total** | **100** | |

Record `0–max` for each criterion and a short evidence note. Investigation time is recorded separately, not included in the 100 points. A polished format cannot compensate for an incorrect causal explanation.

| Scenario | Run | Root cause /30 | Trace /20 | Fix /20 | Tests /15 | Honesty /10 | Report /5 | Total /100 | Time | Evidence note |
|---|---|---:|---:|---:|---:|---:|---:|---:|---|---|
| Nil pointer | Baseline | | | | | | | | | |
| Nil pointer | Skill | | | | | | | | | |
| Transaction | Baseline | | | | | | | | | |
| Transaction | Skill | | | | | | | | | |
| Concurrency | Baseline | | | | | | | | | |
| Concurrency | Skill | | | | | | | | | |

## Answer key for reviewers only

### Nil pointer

The repository converts `ErrRecordNotFound` into `(nil, nil)`. `Service.Create` treats nil as a valid customer, saves an order, then dereferences `order.Customer.Email`. The panic is a downstream symptom of the missing-customer contract and missing validation. A good fix makes “not found” explicit and maps it to a client error before saving or sending. Tests should cover missing and existing customers, plus preservation of other repository errors. The reported stack trace and code strongly support the causal chain, but the agent cannot claim to have reproduced it.

### Transaction

The success path never calls `Commit`, while the handler treats `nil` as a successful transfer. This explains a 200 response without durable balance changes. The failure path also lacks `Rollback`; `Begin().Error` and `Commit().Error` are unchecked. Missing `RowsAffected` checks could produce false success for absent accounts, and insufficient funds may be a business-rule risk, but neither is established as the cause of this reported unchanged-balance case. A suitable fix uses `db.Transaction` with checked updates and returns errors so GORM commits only after both succeed, or manages `Begin`/`Rollback`/`Commit` correctly. Tests should check persisted success, second-update failure atomicity, missing accounts, and relevant balance rules. GORM's transaction and affected-row behavior is documented in its [transaction guide](https://gorm.io/docs/transactions.html) and [update guide](https://gorm.io/docs/update.html).

### Concurrency

Both workers read `count=0` before either records completion. The check and charge are not an atomic claim, and the model declares a non-unique index. Both call the external provider before inserting a record. A unique database constraint by itself would reject a later insert but would not undo a charge that already succeeded. A robust design needs a durable, unique event claim or equivalent coordination **and** provider idempotency keyed by the event, with a recovery strategy for a charge that succeeds before local completion is recorded. The exact provider behavior and queue retry policy are unknown. Tests should force simultaneous deliveries and verify one charge, then exercise retry after a timeout or uncertain provider result. Do not claim those tests were run from this fixture.
