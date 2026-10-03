# Validation Protocol — JEEVANARTH

## 1. Objective

Validation should establish whether JEEVANARTH:
1. computes its stated mathematics correctly;
2. behaves sensibly at boundaries;
3. communicates uncertainty without misleading users;
4. is understandable and usable;
5. measures what its experimental constructs claim to measure.

These are separate questions and should not be conflated.

## 2. Level A — Code/formula verification

For every computational module create a test table:

| Test ID | Module | Inputs | Independent expected result | Browser result | Tolerance | Pass/Fail |
|---|---|---|---:|---:|---:|---|
| V-001 | Education FV | controlled case | TBD | TBD | ₹1 or defined rounding | TBD |
| V-002 | Income growth | controlled case | TBD | TBD | defined | TBD |
| V-003 | Loan balance | controlled case | TBD | TBD | defined | TBD |
| V-004 | Retirement accumulation | controlled case | TBD | TBD | defined | TBD |
| V-005 | Decumulation | controlled case | TBD | TBD | defined | TBD |

Expected results should be independently calculated, not copied from the application.

## 3. Level B — Boundary testing

Test:
- age at minimum/maximum allowed values;
- retirement age equal to or below current age;
- zero income;
- zero expenses;
- zero/negative surplus;
- zero interest;
- zero inflation;
- zero return;
- very high inflation;
- loan EMI insufficient to amortise interest;
- education goal starting immediately;
- no dependants;
- no retirement corpus;
- planning horizon before retirement;
- missing self-assessment inputs.

The application should fail safely and explain invalid combinations.

## 4. Level C — Sensitivity testing

Vary one parameter at a time and confirm expected directionality. Examples:
- higher inflation should not improve real purchasing-power outcomes, all else equal;
- higher saving should not reduce accumulated retirement assets, all else equal;
- later retirement should generally increase accumulation time and shorten retirement duration, subject to implementation;
- higher debt burden should not mechanically improve financial readiness.

## 5. Level D — Scenario consistency

Base, lower-return, and higher-inflation scenarios must use the same user profile and differ only in documented scenario parameters.

## 6. Level E — JLI construct validation

Before calling JLI “validated”, research should address:
- content validity: are the domains appropriate?
- face validity: do experts/users understand the construct?
- internal structure: are weights and normalisation defensible?
- convergent/discriminant validity where suitable comparators exist;
- sensitivity to missing values;
- robustness across age/income/family groups;
- test-retest reliability where appropriate;
- fairness and subgroup behaviour.

## 7. Level F — MEC validation

Assess whether suggested small changes are:
- mathematically computed correctly;
- feasible and clearly described;
- not double counted;
- directionally stable;
- not framed as guaranteed or personalised regulated advice.

## 8. Pilot usability study

After documentation freeze, conduct a pilot with diverse users. Suggested outcomes:
- completion rate;
- time to completion;
- input clarity;
- Marathi/English clarity;
- perceived usefulness;
- confusing concepts;
- trust/calibration;
- missing parameters;
- calculation concerns;
- willingness to revisit the tool.

If pilot responses are intended for formal research/publication, obtain applicable institutional ethics/consent clearance before collecting research data.

## 9. Validation status labels

Use:
- `Unverified` — not independently checked
- `Verified` — formula/code output independently checked
- `Pilot-tested` — tested with users
- `Research-validated` — supported by a documented empirical validation study

Do not use “validated” as a generic synonym for “works on my browser”.
