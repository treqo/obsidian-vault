Time-money relationships; economic analysis of alternatives including the effects of interest rates, inflation, depreciation, taxation and uncertainty; cost estimation and budgeting; financial analysis of engineering operations.
## L2 – Time Value of Money

#### Computing Cash Flows

- benefits and costs as **receipts** (cash flow in) and **disbursements** (cash flow out)

Interest rate relates to the **time value** of money. Can help us distinguish between different project options given future cash benefits and costs.

Money
1. It has value over time
2. It can be rented; the charge is called interest instead of rent.

---

$$
PV(\$_n)= \frac{\$_n}{(1+r)^n}
$$
*Present Value* #equation 

***n***: The year number where year 0 is the present year (and therefore current value of money is itself).
***PV***: The present value worth of the money.
***$_n***: The amount of money at the nth year.

---

**Interest** is compensation for giving up the use of money
- The difference between the amount loaned and the amount repaid.

An amount of money today **P** can be related to a future amount **F**, by the interest amount **I** or the interest rate (i).

$$
F=P+I=P(1+i)
$$
Suppose you receive a loan of amount $P$ now, in exchange for a future payment amount $F$ at some future time, where $F=P(1+i)$.
* $i$ : the interest rate
* $P$: The present worth of $F$
* $F$: The future worth of $P$

The dimensions of **interest rate** is $s^{-1}$. 

> if $\$1$ is lent at a $9\%$ interest rate then $\$0.09/year$ would be paid in interest per time period.

**Interest Period**: Period over which interest is calculated.
- The longer the interest period, the higher the interest rate must be to provide the same return.
- When given an interest rate without a specified period, assume it is annual.

![[Screenshot 2024-06-25 at 11.18.06 AM.png]]

#### Compound Interest

Use the formula $F=P(1+i)$, but if paying for more than one period, $F$ becomes the next $P$ after the interest period. Hence compounding interest.

$$
F = P(1+i)^N
$$
*Compound Interest* #equation 

$F$: The amount that must be repaid after $N$ periods.
$P$: The amount borrowed.
$i$: The interest rate.
$N$: The number of periods. In this case, the compounding period.

$$
I_{C} = F-P=P(1+i)^N-P
$$
*Total Interest* #equation 

$I_{C}$: The total interest accumulated from compounding. Also known as **compounding interest**.

#### Simple Interest

Interest without compounding – Interest is not added to principal at the end of period.

$$
I_{S}=P \times i \times N
$$

#### Interest Rates

**Nominal Interest Rate**: Stated for a full period
**Effective Interest Rate**: results from the compounding based on the sub-periods.

	Unless otherwise stated, rates are nominal annual rates.

Suppose $r$ is a nominal annual rate for a period (1 year) consisting of $m$ equal compounding periods (sub-periods). The interest rate for each sub-period is 

$$ 
i_s=\frac{r}{m}
$$
An effective interest rate is the actual, but not usually stated interest rate found by

> Converting a given interest rate with an arbitrary compounding period (usually less than a year), to an equivalent interest rate with a one-year compounding period.

$$
i_{e}=(1+i_{s})^m-1
$$
*Effective Interest Rate* #equation 

$i_{e}$: Effective interest rate over a longer period.
$i_{s}$: Interest rate over a compounding sub period.
$m$: number of sub-periods in the longer period.

The effective interest rate under ***continuous compounding*** is $i_{e}=e^r-1$.

![[Screenshot 2024-06-25 at 11.52.47 AM.png]]

## Cash Flow Diagram

**Cash flow diagram** is a graphical summary of the timing and magnitude of a set of cash flows.

![[Screenshot 2024-06-25 at 11.54.54 AM.png]]

- The end of one period is the exact start time of the next period
- "Now" is time 0 which is the end of period -1 and also the beginning of period 1.
- N years from now is the end of year N and beginning of year N+1

![[Screenshot 2024-06-25 at 12.37.21 PM.png]]

Cash flow occurs at the ends of periods.

#### Equivalence

**Mathematical Equivalence**: A consequence of the mathematical relationship between time and money

**Decisional Equivalence**: Happens when two decisions are equally good.

Ex: individual is indifferent about receiving $P$ money now versus $F$ Money at some time $t$. Because of decisional equivalence, we can calculate the interest rate $i$ relating the two.

**Market Equivalence**: Arises due to availability of a market to exchange one cash flow for another at zero cost.

#### Electricity rates Problem