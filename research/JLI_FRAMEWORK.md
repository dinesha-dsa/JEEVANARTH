# JLI Framework — Experimental

## JEEVANARTH Life Independence Index (JLI)

### Research question

Can a multidimensional indicator provide a useful, explainable summary of a person's preparedness for long-term independent living without reducing the problem to retirement corpus alone?

## Candidate domains

The v1.0.0 concept spans domains such as:
- financial preparedness;
- retirement readiness;
- debt/resilience;
- health preparedness;
- human-capital/second-career readiness;
- housing/ageing-at-home readiness;
- family/social support.

The exact domain set used in research must match the released implementation or be explicitly identified as a later research revision.

## Generic formulation

For normalized domain scores \(z_d \in [0,100]\):

\[
JLI=\sum_{d=1}^{D}w_dz_d,\qquad \sum w_d=1
\]

This is a framework, not evidence that any particular weights are valid.

## Required design decisions

Before empirical use, document:
1. domain definitions;
2. input variables per domain;
3. normalization function;
4. floor/cap rules;
5. missing-data handling;
6. weighting rationale;
7. threshold/band rationale, if any;
8. whether compensation across domains is allowed;
9. subgroup sensitivity.

## Avoid false precision

A single score can hide important weaknesses. Therefore JLI should be shown with a domain-level profile (e.g., wheel/map) and explanatory text, not alone.

## Validation hypotheses

Examples for future testing:
- improved emergency resilience should increase the relevant financial-resilience component, all else equal;
- higher unsustainable debt should not increase readiness;
- stronger second-career preparedness should improve the human-capital component;
- domain scores should remain interpretable across different ages and household structures.

These are hypotheses to test, not validated findings.

## Naming and claims

Until validation is complete, always use:
**“Experimental JEEVANARTH Life Independence Index (JLI)”**.

Do not describe JLI as an actuarial, medical, credit, investment-risk, or clinically validated score.
