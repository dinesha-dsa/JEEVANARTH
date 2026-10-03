# Assumptions and Model Boundaries — JEEVANARTH v1.0.0

## Purpose

This file records the assumptions that must be visible when interpreting JEEVANARTH outputs.

## Core assumptions

1. User inputs are assumed to be entered in consistent units and reasonably reflect the household's present situation.
2. Growth, inflation, and return rates are scenario parameters; they are not guaranteed future values.
3. Compound-growth mathematics assumes a defined compounding interval.
4. Retirement modelling requires an explicit convention for contribution and withdrawal timing.
5. Education inflation may differ from general inflation.
6. Second-career income is uncertain and must be treated as a scenario.
7. Health preparedness, ageing readiness, skill readiness, and social/family support are self-assessed and are not clinical or diagnostic measures.
8. Taxation and product-specific regulations are not comprehensively modelled in the public research preview.
9. Unexpected medical, family, employment, market, policy, and longevity shocks may materially alter outcomes.
10. The JLI and MEC constructs are experimental.

## Illustrative defaults used during v1.0.0 development

The development baseline discussed for the simulator included illustrative values such as:
- income growth: 7% p.a.;
- general inflation: 6% p.a.;
- education inflation: 8% p.a.;
- pre-retirement return: 10% p.a.;
- post-retirement return: 7.5% p.a.

These values must be treated as **illustrative assumptions**, not recommendations. Before academic publication, the released `index.html` should be audited so this document matches the exact defaults implemented in code.

## Model boundaries

JEEVANARTH v1.0.0 should not be interpreted as:
- a personalised investment recommendation engine;
- an actuarial longevity model;
- a medical risk score;
- a credit-underwriting model;
- a tax calculator;
- an insurance suitability engine;
- a guarantee of financial independence.

## Assumption governance

Any future version that changes a default parameter, equation, scoring rule, or threshold should record:
`old value → new value → rationale → evidence/source → version introduced`.
