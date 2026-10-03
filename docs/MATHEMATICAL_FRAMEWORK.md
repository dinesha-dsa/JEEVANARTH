# Mathematical Framework — JEEVANARTH v1.0.0

## 1. Notation

Let:
- \(a_0\) = current age
- \(a_r\) = intended retirement age
- \(a_h\) = planning-horizon age
- \(n=a_r-a_0\) = years to retirement
- \(Y_0\) = current annual household income
- \(E_0\) = current annual expenditure
- \(g\) = annual income growth rate
- \(i\) = general inflation rate
- \(r_{pre}\) = pre-retirement annual investment return
- \(r_{post}\) = post-retirement annual return
- \(C_0\) = current retirement corpus
- \(S_0\) = first-year retirement contribution
- \(s\) = annual contribution step-up rate

All rates must be expressed consistently as decimals in calculations.

## 2. Income projection

A simple annual compound-growth trajectory is:

\[
Y_t = Y_0(1+g)^t
\]

This is a scenario assumption, not an income forecast.

## 3. Expense projection

For a general inflation assumption:

\[
E_t = E_0(1+i)^t
\]

Where expense categories have distinct inflation assumptions, category-level projection is preferable:

\[
E_t = \sum_{k=1}^{m} E_{k,0}(1+i_k)^t
\]

## 4. Present surplus

\[
Surplus_0 = Y_0 - E_0 - DebtService_0
\]

Implementation must define clearly whether debt EMIs are already included in expenditure to prevent double counting.

## 5. Emergency-fund coverage

If \(L\) is liquid emergency money and \(M_e\) is essential monthly expenditure:

\[
EmergencyMonths = \frac{L}{M_e}
\]

A target threshold is a planning convention and must not be presented as universally correct.

## 6. Education-goal future cost

If \(Edu_0\) is the full equivalent course cost at today's prices, \(i_e\) education inflation, and \(n_e\) years until the goal begins:

\[
EduFV = Edu_0(1+i_e)^{n_e}
\]

If \(EduSaved\) is already accumulated, its future value should be modelled separately if a return assumption is applied.

## 7. Loan amortisation

For principal \(P\), monthly interest \(j\), and monthly payment \(A\), remaining balance after \(m\) payments is:

\[
B_m=P(1+j)^m-A\frac{(1+j)^m-1}{j}
\]

For \(j=0\):

\[
B_m=P-Am
\]

A projected debt-free age is:

\[
Age_{debtfree}=a_0+\frac{m^*}{12}
\]

where \(m^*\) is the first month for which balance is zero or below.

## 8. Retirement corpus accumulation

Existing corpus:

\[
C_{existing}=C_0(1+r_{pre})^n
\]

For a constant annual contribution \(S\) paid at year-end:

\[
C_{contrib}=S\frac{(1+r_{pre})^n-1}{r_{pre}}
\]

For annually increasing contributions \(S_t=S_0(1+s)^t\), a direct finite-sum representation is safer:

\[
C_{contrib}=\sum_{t=0}^{n-1} S_0(1+s)^t(1+r_{pre})^{n-1-t}
\]

Then:

\[
C_{ret}=C_{existing}+C_{contrib}+C_{other}
\]

where \(C_{other}\) must include only explicitly modelled retirement assets.

## 9. Retirement lifestyle at retirement

If \(R_0\) is desired annual retirement lifestyle expenditure in today's money:

\[
R_{ret}=R_0(1+i)^n
\]

Medical reserves or exceptional goals should be modelled separately where possible to avoid hiding them inside lifestyle expenditure.

## 10. Retirement decumulation

A transparent year-by-year recursion is:

\[
C_{t+1}=(C_t-W_t)(1+r_{post})
\]

with inflation-linked withdrawal:

\[
W_{t+1}=W_t(1+i)
\]

The corpus sustainability age is the earliest age at which \(C_t \leq 0\), subject to the chosen timing convention for return and withdrawal.

## 11. Real return

For interpretation:

\[
r_{real}=\frac{1+r_{nominal}}{1+i}-1
\]

This is preferable to the approximation \(r_{nominal}-i\) when precision matters.

## 12. Second-career income

If second-career annual income starts at \(H_0\), grows at \(g_h\), and continues for \(q\) years:

\[
H_t=H_0(1+g_h)^t,\quad 0\leq t<q
\]

Its treatment must be explicit: it may offset retirement withdrawals, increase savings, or both, but should not be double counted.

## 13. Retirement funding ratio

A general definition is:

\[
FundingRatio=\frac{ProjectedAvailableRetirementResources}{ModelledRetirementNeed}
\]

Because “retirement need” depends on the decumulation method, the implementation must document the exact denominator before this ratio is used in research.

## 14. Scenario stress testing

For scenario \(q\), define a parameter vector:

\[
\theta_q = (g_q,i_q,r_{pre,q},r_{post,q},i_{e,q},...)
\]

and compute:

\[
Outcome_q=f(X,\theta_q)
\]

where \(X\) is the user's input profile. Comparisons should report directional sensitivity rather than probability unless a probabilistic model is introduced.

## 15. JLI

A generic research form is:

\[
JLI=\sum_{d=1}^{D} w_d z_d
\]

where:
- \(z_d\) = normalized domain score;
- \(w_d\geq0\);
- \(\sum w_d=1\).

The v1.0.0 JLI is **experimental**. Exact implementation weights, normalization rules, caps, missing-data treatment, and domain definitions must be extracted from and reconciled with the released source code before claiming this equation reproduces the software exactly.

## 16. MEC

For intervention \(k\), define baseline outcome vector \(O_0\) and intervention outcome \(O_k\):

\[
\Delta O_k=O_k-O_0
\]

MEC compares small feasible parameter changes—such as higher monthly saving, later retirement, or second-career income—and reports their effects on selected outcomes.

A future validated MEC formulation may optimize:

\[
k^*=\arg\min_k Cost(k)
\]

subject to:

\[
Improvement(O_k)\geq \tau
\]

but this optimization interpretation is a **research extension**, not a validated v1.0.0 claim.

## 17. Numerical verification requirement

Before publication, every formula used in the application should have:
- a hand-calculated reference case;
- an independent spreadsheet/Python result;
- a browser result;
- an accepted tolerance;
- boundary tests.

This document distinguishes standard financial mathematics from JEEVANARTH-specific experimental constructs.
