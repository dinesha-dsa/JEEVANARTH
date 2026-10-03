# JEEVANARTH v1.0.0 — Source-Code ↔ Mathematical Framework Audit

**Audit date:** 03 October 2026  
**Scope:** Public `index.html` calculation engine and `docs/MATHEMATICAL_FRAMEWORK.md`.

## Executive result

The public simulator and the first mathematical-document draft were directionally aligned, but several equations in the document described standard/reference mathematics rather than the exact released implementation. The corrected framework in this patch now distinguishes **IMPLEMENTED** equations from **REFERENCE / FUTURE** formulations.

## Key reconciliation results

| Module | Source implementation | Previous document | Audit result |
|---|---|---|---|
| Income | Compound growth before retirement | Compound growth | MATCH |
| Expense | Aggregate inflation; retirement lifestyle inflated from current age | General inflation | MATCH / clarified |
| Emergency fund | EF ÷ (Essential + Protection) | Generic essential expenditure | CLARIFIED |
| Education FV | Present cost × education inflation | Same | MATCH |
| Home loan | Monthly iterative amortisation | Closed-form reference | METHOD DIFFERENCE |
| Retirement accumulation | Annual return + stepped annual contribution with half-year return approximation | Standard finite-sum/year-end framing | DIFFERENCE |
| Decumulation | Return first, then withdrawal gap | Withdrawal then return | DIFFERENCE |
| Second career | Constant entered income | Growth-linked generic formula | DIFFERENCE |
| Required corpus | Growing-annuity PV | Generic funding-ratio definition | MISSING DETAIL |
| Funding ratio | Retirement corpus / required corpus × 100 | Generic ratio | CLARIFIED |
| JLI | Six-domain average with specific Financial sub-score | Generic weighted index | IMPLEMENTATION NOW DOCUMENTED |
| Pressure map | Heuristic score | Not formally documented | ADDED |
| Stress tests | -2 percentage points returns; +2 points inflation | Generic parameter-vector concept | IMPLEMENTATION NOW DOCUMENTED |
| MEC | Three fixed rerun scenarios | Generic/future optimization framing | IMPLEMENTATION NOW DOCUMENTED |

## Research interpretation

No finding above proves that the implemented design is invalid. It shows where the first documentation draft did not reproduce the source algorithm exactly. For reproducible research, the source behaviour must be documented first; normative improvements can then be proposed and tested as later versions.

## Recommended next gate

Do not call the model “mathematically validated” yet. The next gate is an independent numerical verification suite with controlled test cases and boundary tests.
