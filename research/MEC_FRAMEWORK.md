# MEC Framework — Experimental

## Minimum Effective Change (MEC)

### Purpose

MEC is intended to shift the user from a potentially overwhelming long-term gap to a comparison of **small, understandable changes** and their modelled effects.

Typical v1.0.0-style comparisons include:
- increasing monthly saving by a modest amount;
- retiring later;
- adding second-career income for a defined period.

## Formal representation

Let baseline inputs be \(X\) and model output \(O=f(X)\).

For candidate change \(k\):

\[
X_k=X+\Delta X_k
\]

\[
O_k=f(X_k)
\]

\[
Impact_k=O_k-O
\]

The application should present the changed assumption and resulting outcome transparently.

## What MEC is not

MEC is not automatically:
- the mathematically optimal intervention;
- the easiest behavioural change;
- the least costly change;
- personalised financial advice.

Those claims would require additional objective functions, constraints, user preferences, and validation.

## Future research extension

A formal optimization version could define:
- intervention burden/cost;
- minimum desired improvement;
- feasibility constraints;
- multi-objective outcomes.

Then:

\[
k^*=\arg\min_k Burden(k)
\]

subject to a defined improvement threshold.

This is a future research direction, not a v1.0.0 validated feature.

## Evaluation questions

Pilot research should test:
- Do users understand the comparison?
- Does MEC reduce decision paralysis?
- Are the suggested changes perceived as feasible?
- Do users understand that outcomes are scenarios?
- Does MEC encourage constructive exploration without implying certainty?
