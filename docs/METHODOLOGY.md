# Methodology — JEEVANARTH v1.0.0

## 1. Purpose

JEEVANARTH is designed as a **Life Independence Decision-Support System**, not merely a retirement calculator. It integrates financial and selected non-financial dimensions to help users explore how current choices may affect long-term independence.

## 2. Conceptual model

The v1.0.0 research model can be represented as:

`Personal Profile → Income → Expenditure → Assets → Debt → Protection → Education → Retirement → Second Career → Life Readiness → Stress Testing → Decision Support`

The system considers:
- household income and growth;
- purpose-based expenditure;
- savings and investments;
- debt commitments;
- emergency and protection preparedness;
- education goals;
- retirement accumulation and decumulation;
- second-career/human-capital potential;
- self-assessed health preparedness;
- ageing-at-home readiness;
- family/social support;
- scenario and sensitivity comparisons.

## 3. Decision-support philosophy

JEEVANARTH uses **scenario exploration**, not deterministic prediction. A user supplies present conditions and assumptions. The system projects possible trajectories and exposes pressure points, trade-offs, and potentially feasible changes.

The intended reasoning chain is:

`Input → Calculation → Interpretation → Scenario comparison → Explainable action cue`

## 4. Expenditure intelligence

Expenses are classified by purpose rather than by moral judgement. Suggested classes include Essential, Protection, Commitment, Goal-related, Flexible, Aspirational, Contextual, and Review.

This classification is intended to help users distinguish:
1. expenditure that is difficult or risky to reduce;
2. expenditure tied to contractual/family goals;
3. expenditure with greater adjustment flexibility.

The model must not label a user's spending as morally “good”, “bad”, or “wasteful”.

## 5. Life-course modelling

The model treats financial independence as a life-course problem. Income, expenses, debt, education responsibilities, retirement, health preparedness, and second-career potential can peak at different ages. Therefore, a single corpus number is insufficient to describe the complete decision context.

## 6. Stress testing

The public release includes base assumptions plus adverse scenarios such as lower investment return and higher inflation. These are sensitivity scenarios, not forecasts.

## 7. Explainability

Rule-based guidance should follow:

`Observed condition → Why it matters → Action to explore → Expected directional effect`

The wording should avoid guaranteeing outcomes or presenting regulated personalised advice.

## 8. Experimental research constructs

Two constructs are explicitly experimental:
- **JLI — JEEVANARTH Life Independence Index**
- **MEC — Minimum Effective Change**

Neither should be represented as externally validated until empirical validation is completed.

## 9. Limitations

The model does not fully capture taxation, product-specific rules, behavioural responses, stochastic market sequences, medical events, policy changes, mortality distributions, or all family dynamics. User-entered values may also contain estimation error.

## 10. Reproducibility principle

Every public version should preserve:
- version number;
- formula definitions;
- default assumptions;
- scoring logic;
- scenario definitions;
- validation tests;
- change log.

This allows later research to distinguish algorithmic changes from user-study effects.
