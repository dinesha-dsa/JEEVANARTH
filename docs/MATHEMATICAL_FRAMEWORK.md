# Mathematical Framework — JEEVANARTH v1.0.0

> **Audit status:** Reconciled against the public `index.html` implementation on 03 October 2026.  
> **Important:** Sections marked **IMPLEMENTED** describe the released code. Sections marked **REFERENCE / FUTURE** are research/reference formulations and are not claims about the current implementation.

## 1. Notation

- $a_0$ = current age
- $a_r$ = intended main-career retirement age
- $a_h$ = planning-horizon age
- $y=a-a_0$ = model year index at age $a$
- $Y_0$ = current annual household income
- $E_m$ = current entered monthly expenditure
- $g$ = annual income-growth assumption
- $i$ = general inflation assumption
- $r_{pre}$ = pre-retirement annual return assumption
- $r_{post}$ = post-retirement annual return assumption
- $C_y$ = retirement corpus at model year $y$
- $S_0$ = current annual retirement contribution
- $s$ = annual contribution step-up rate

Rates are stored as decimals in the calculation engine.

## 2. Income projection — IMPLEMENTED

Before retirement, annual household income is:

$$
Y_y = Y_0(1+g)^y
$$

After main-career retirement, the current implementation sets main-career income to zero and, for the entered second-career duration, uses a **constant** annual second-career income $H_0$:

$$
Y_y = Y_0(1+g)^y \quad \text{if } a<a_r
$$

$$
Y_y = H_0 \quad \text{if } a_r \le a < a_r+q
$$

$$
Y_y = 0 \quad \text{if } a \ge a_r+q
$$

The v1.0.0 code does **not** apply a separate growth rate to second-career income.

## 3. Expense projection — IMPLEMENTED

Before retirement:

$$
E_y = (12E_m)(1+i)^y
$$

At and after retirement, if $R_0$ is desired annual retirement lifestyle expenditure in today's rupees:

$$
E_y = R_0(1+i)^y
$$

**Audit note:** inflation is indexed from current age in both phases. Therefore the retirement lifestyle entered in today's money is inflated to retirement age automatically.

The current implementation uses one general inflation rate for the aggregate living-expense trajectory. Category-specific inflation is **not implemented** in v1.0.0.

## 4. Modelled monthly balance — IMPLEMENTED

The report's present monthly balance is:

$$
B_0=\frac{Y_0}{12}-E_m-EMI_{home}-EMI_{other}-\frac{S_0}{12}
$$

This is a current-period cash-flow indicator.

**Important double-counting rule:** users should not include loan EMIs again inside entered living expenses if those EMIs are separately entered in the debt section.

## 5. Emergency-fund coverage — IMPLEMENTED

The denominator is the sum of expenditure categories classified as **Essential** and **Protection**:

$$
E_{EP}=E_{Essential}+E_{Protection}
$$

Then:

$$
EmergencyMonths=\frac{EmergencyFund}{\max(1,E_{EP})}
$$

The `max(1, …)` term prevents division by zero. The six-month level used elsewhere in the scoring/guidance is a model convention, not a universal rule.

## 6. Education-goal future cost — IMPLEMENTED

If $Edu_0$ is today's equivalent total course cost, $i_e$ is education inflation, and $n_e$ is years until the goal:

$$
EduFV=Edu_0(1+i_e)^{n_e}
$$

**Audit note:** `edusaved` is displayed and included in current net financial assets, but v1.0.0 does **not** compound it or subtract it from `EduFV` to calculate an education funding gap.

## 7. Home-loan amortisation — IMPLEMENTED ITERATIVELY

The annual nominal loan rate is converted to monthly rate:

$$
j=\frac{r_{loan}}{12}
$$

For each modelled month:

$$
Interest_m=P_mj
$$

$$
Principal_m=\max(0,EMI-Interest_m)
$$

$$
P_{m+1}=\max(0,P_m-Principal_m)
$$

The simulator processes up to 12 monthly payments per model year.

### Important boundary finding

If:

$$
EMI \le P_mj
$$

then `Principal_m = 0`; the loan balance does not reduce. The current implementation does not separately warn that the EMI is insufficient to amortise principal. This should be added in a later patch.

The displayed debt-free age is the model age in which the balance first reaches zero, so it is **year-granular**, not exact to the month.

### Closed-form reference — REFERENCE ONLY

For independent verification, the standard closed-form balance after $m$ payments is:

$$
B_m=P(1+j)^m-A\frac{(1+j)^m-1}{j}
$$

This equation is useful for validation but is not the algorithm used by v1.0.0.

## 8. Retirement corpus accumulation — IMPLEMENTED

For each pre-retirement model year $y$:

$$
C_{y+1}=C_y(1+r_{pre})+
S_0(1+s)^y\left(1+\frac{r_{pre}}{2}\right)
$$

The factor:

$$
1+\frac{r_{pre}}{2}
$$

is a simplified approximation used by the released code to give the year's contribution roughly a half-year return.

**Audit finding:** this differs from a strict year-end annuity formulation and from a true monthly SIP calculation. Documentation must describe the implemented approximation rather than silently substituting a standard annuity formula.

The projected retirement corpus shown by the report is the corpus after the final pre-retirement iteration (age $a_r-1$).

## 9. Retirement lifestyle at retirement — IMPLEMENTED

If $R_0$ is desired annual retirement lifestyle expenditure in today's rupees and $n=a_r-a_0$:

$$
R_{ret}=R_0(1+i)^n
$$

This quantity is also used as the first retirement expenditure in the required-corpus calculation.

## 10. Retirement decumulation — IMPLEMENTED

At and after retirement:

$$
C_{y+1}=
\max\left(
0,\;
C_y(1+r_{post})-\max(0,E_y-Y_y)
\right)
$$

Thus the implementation applies the annual post-retirement return first, then subtracts the positive gap between modelled expenditure and second-career income.

**Audit finding:** this is different from the previously documented recursion $(C_t-W_t)(1+r_{post})$. The corrected equation above matches v1.0.0.

Corpus sustainability age is the first model age at which corpus becomes zero. If it does not reach zero by the planning horizon, the report displays the horizon age with `+`.

## 11. Estimated required retirement corpus — IMPLEMENTED

Let:

$$
N=\max(1,a_h-a_r+1)
$$

and:

$$
R_{ret}=R_0(1+i)^{a_r-a_0}
$$

When $r_{post}\ne i$, the implementation uses a finite growing-annuity form:

$$
RequiredCorpus=
\frac{R_{ret}}{r_{post}-i}
\left[
1-
\left(\frac{1+i}{1+r_{post}}\right)^N
\right]
$$

When $r_{post}\approx i$, the implementation uses:

$$
RequiredCorpus=\frac{R_{ret}N}{1+r_{post}}
$$

### Funding ratio — IMPLEMENTED

$$
FundingRatio=
100\times
\frac{ProjectedRetirementCorpus}{RequiredCorpus}
$$

If required corpus evaluates to zero, the code returns 100%.

## 12. Second-career income — IMPLEMENTED

For the entered duration $q$, v1.0.0 uses constant annual second-career income $H_0$:

$$
H_y=H_0
$$

It offsets retirement-period expenditure in the decumulation calculation.

A growth formulation such as $H_t=H_0(1+g_h)^t$ is a **future research extension**, not part of v1.0.0.

## 13. Education, retirement and other assets — separation rule

The current code intentionally gathers dedicated education savings, retirement corpus, cash, and other investments separately.

However:
- education savings are not used to reduce the projected education goal;
- cash/other assets are not automatically added to retirement corpus;
- insurance cover is not treated as wealth.

These distinctions should remain explicit.

## 14. Current net financial assets — IMPLEMENTED

The report computes:

$$
NFA=Cash+OtherAssets+EmergencyFund+RetirementCorpus+EducationSavings-OtherLoan-HomeLoan
$$

Insurance cover and the primary home's property value are excluded.

## 15. JLI — IMPLEMENTED, EXPERIMENTAL

### 15.1 Emergency component

$$
EmergencyScore=
\min\left(100,\frac{EmergencyMonths}{6}\times100\right)
$$

### 15.2 Debt-transition component

$$
DebtScore=
\begin{cases}
20, & DebtFreeAge\le a_r \\
7, & \text{otherwise}
\end{cases}
$$

### 15.3 Financial domain

$$
Financial=
clip_{0}^{100}
\left(
0.6\,FundingRatio+
0.2\,EmergencyScore+
DebtScore
\right)
$$

### 15.4 Retirement domain

$$
Retirement=\min(100,FundingRatio)
$$

### 15.5 Self-assessed domains

The user selects coded values for:
- Health preparedness: 35 / 60 / 80
- Human Capital: 35 / 60 / 80
- Housing readiness: 35 / 60 / 80
- Family & Social support: 35 / 60 / 80

### 15.6 Overall experimental JLI

The released code gives equal weight to six final domains:

$$
JLI=
\frac{
Financial+
Retirement+
Health+
HumanCapital+
Housing+
FamilySocial
}{6}
$$

**Important research observation:** `Financial` already contains 60% of the uncapped funding ratio, while `Retirement` separately contains the funding ratio capped at 100. Retirement preparedness therefore influences two of the six final domains. This is an intentional description of the current code, **not a validation of the weighting design**.

JLI remains experimental and should not be described as an actuarial, clinical, investment, credit, or validated financial-wellness score.

## 16. Life Pressure Map — IMPLEMENTED, HEURISTIC

For each model year:

$$
Pressure=
clip_{5}^{100}
\left(
DebtBurden+
CashFlowPenalty+
CorpusPenalty
\right)
$$

where, approximately:

$$
DebtBurden=
\begin{cases}
100\times\frac{AnnualHomeEMI}{Income}, & HomeLoan>0\\
0, & \text{otherwise}
\end{cases}
$$

$$
CashFlowPenalty=
\begin{cases}
40, & CashFlow<0\\
10, & CashFlow\ge0
\end{cases}
$$

and before retirement:

$$
CorpusPenalty = 20 \quad \text{if } Corpus < AnnualIncome
$$

$$
CorpusPenalty = 5 \quad \text{otherwise}
$$

After retirement the implementation contributes zero for this last term because the condition is pre-retirement only.

This is a heuristic visual planning indicator, not a probability or validated risk measure.

## 17. Scenario laboratory — IMPLEMENTED

### Base
Uses entered assumptions unchanged.

### Lower-return stress

$$
r_{pre}^{stress}=\max(0,r_{pre}-0.02)
$$

$$
r_{post}^{stress}=\max(0,r_{post}-0.02)
$$

### Higher-inflation stress

$$
i^{stress}=i+0.02
$$

These are deterministic sensitivity tests, not probabilistic forecasts.

## 18. MEC — IMPLEMENTED AS THREE FIXED WHAT-IF COMPARISONS

v1.0.0 evaluates:

1. **Save +₹2,000/month**

$$
S_0'=S_0+₹24,000/year
$$

2. **Retire 2 years later**

$$
a_r'=\min(a_h-1,a_r+2)
$$

3. **Second career +₹10,000/month**

$$
H_0'=H_0+₹120,000/year
$$

Each option reruns the same model and compares funding ratio and sustainability age with baseline.

MEC in v1.0.0 is therefore a **fixed scenario-comparison mechanism**, not an optimization algorithm. A future $\arg\min$ formulation is a research extension only.

## 19. Scenario-independent guidance — IMPLEMENTED

The action-guidance layer is rule based. Examples include conditions on:
- emergency coverage below six months;
- negative modelled monthly balance;
- flexible/aspirational/review spending;
- funding ratio below 100%;
- debt remaining close to retirement;
- entered second-career income;
- education-goal separation.

These rules provide educational prompts and should not be interpreted as regulated personalised advice.

## 20. Audit findings requiring documentation or future-code attention

| ID | Finding | v1.0.0 status | Action |
|---|---|---|---|
| A-01 | GitHub equations used plain `[]` / `()` rather than math delimiters | Documentation issue | Fixed in this patch with `$...$` / `$$...$$` |
| A-02 | Corpus accumulation uses half-year-return approximation on annual stepped contributions | Implemented | Document exactly; validate independently |
| A-03 | Decumulation applies return before withdrawal | Implemented | Corrected documentation |
| A-04 | Second-career income is constant, not growth-linked | Implemented | Corrected documentation |
| A-05 | Education savings do not reduce/compound against education goal | Implemented limitation | Consider v1.1.0 |
| A-06 | Home-loan EMI at/below monthly interest produces no principal reduction and no explicit warning | Boundary limitation | Add validation/warning |
| A-07 | Debt-free age is year-granular | Implemented limitation | Consider month-level output |
| A-08 | JLI retirement preparedness influences both Financial and Retirement domains | Experimental design issue | Validate/review weights before research claims |
| A-09 | Life Pressure Map is heuristic | Experimental | Keep clearly labelled |
| A-10 | MEC is fixed what-if comparison, not optimization | Implemented | Keep future optimization separate |

## 21. Numerical verification requirement

Before a research publication or a claim of mathematical validation, each implemented module should have:

- a hand-calculated reference case;
- an independent spreadsheet/Python result;
- the browser result;
- an accepted numerical tolerance;
- boundary/invalid-input tests;
- a documented pass/fail record.

## 22. Version-control note

This document describes **JEEVANARTH v1.0.0 source behaviour** as audited on 03 October 2026. A future change to formulas, thresholds, JLI weights, scenario definitions, or timing conventions should update both source and documentation and receive a new documented version.
