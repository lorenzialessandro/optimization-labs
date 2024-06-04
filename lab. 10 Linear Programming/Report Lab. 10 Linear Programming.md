# Report Lab. 10 Linear Programming

notebook : [optimtech_lab10](University/5%20Optimization%20techniques/lab/Lab.%2010%20Linear%20Programming/optimtech_lab10.ipynb)

**Table of Contents**

- [Exercise 1](#exercise-1)
	- [Text](#text)
	- [Solution](#solution)
	- [*Canonical Form*](#canonical-form)
	- [*Slack Form*](#slack-form)
	- [Results](#results)
- [Exercise 2](#exercise-2)
	- [Text](#text)
	- [Solution](#solution)
	- [*Canonical Form*](#canonical-form)
	- [*Slack Form*](#slack-form)
	- [Results](#results)
- [Exercise 3](#exercise-3)
	- [Text](#text)
	- [Solution](#solution)
	- [*Problem Form*](#problem-form)
	- [Result](#result)

### Exercise 1

#### Text

*A company makes two products (X and Y) using two machines (A and B). Each unit of X that is produced requires 50 minutes processing time on machine A and 30 minutes processing time on machine B. Each unit of Y that is produced requires 24 minutes processing time on machine A and 33 minutes processing time on machine B.*

*At the start of the current week there are 30 units of X and 90 units of Y in stock. Available processing time on machine A is forecast to be 40 hours and on machine B is forecast to be 35 hours.*

*The demand for X in the current week is forecast to be 75 units and for Y is forecast to be 95 units. Company policy is to maximize the combined sum of the units of X and the units of Y in stock at the end of the week.*

#### Solution 
let $x_1$ products of $X$ and $x_2$ products of $Y$ to produce

**constraints**:
- $50 x_1 + 24 x_2 \le 40 \space h$ (machine A)
- $30 x_1 + 33 x_2 \le 35 \space h$ (machine B)
- $x_1 \ge 0$
- $x_2 \ge 0$

**objective function**:
- we start from $30 X$ and $90 Y$
- the final demand is $75 X$ and $95 Y$
- and the goal is to maximize the combination

so:

- $maximize: (75 + x_1 - 30) + (95 + x_2 - 90)$ 
	- *(yet produced + to produce - final demand)*

#### *Canonical Form*
- maximize $x_1 + x_2 + 50$
- subject to:
	- $50 x_1 + 24 x_2 \le 2400$
	- $30 x_1 + 33 x_2 \le 2100$
	- $x_1 \ge 0$
	- $x_2 \ge 0$

#### *Slack Form*
- maximize $x_1 + x_2 + 50$
- subject to:
	- $50 x_1 + 24 x_2 + s_1 = 2400$
	- $30 x_1 + 33 x_2 + s_2 = 2100$
	- $x_1 , x_2, s_1, s_2 \ge 0$


#### Results 
```
Optimal value: 116.4516129032258
Units of X to produce: 31
Units of Y to produce: 35
```


![Pasted image 20240517103416](University/5%20Optimization%20techniques/lab/Lab.%2010%20Linear%20Programming/img/Pasted%20image%2020240517103416.png)

---


### Exercise 2

#### Text
*A factory manufactures chairs and tables, each requiring the use of three operations: Cutting, Assembly, and Finishing. The first operation can be used at most 40 hours; the second at most 42 hours; and the third at most 25 hours. A chair requires 1 hour of cutting, 2 hours of assembly, and 1 hour of finishing; a table needs 2 hours of cutting, 1 hour of assembly, and 1 hour of finishing. If the profit is 20 per unit for a chair and 30 for a table, how many units of each should be manufactured to maximize profit?*

#### Solution 
let $x_1$ Chairs and $x_2$ Tables to produce

**constraints**:
- $1 x_1 + 2 x_2 \le 40 \space h$ (Cutting)
- $2 x_1 + 1 x_2 \le 42 \space h$ (Assembly)
- $1 x_1 + 1 x_2 \le 25 \space h$ (Finishing)
- $x_1 \ge 0$
- $x_2 \ge 0$

**objective function**:
- maximize the Profit : $20 x_1 + 30 x_2$

#### *Canonical Form*
- maximize $20 x_1 + 30 x_2$
- subject to:
	- $1 x_1 + 2 x_2 \le 40$
	- $2 x_1 + 1 x_2 \le 42$ 
	- $1 x_1 + 1 x_2 \le 25$
	- $x_1, x_2 \ge 0$

#### *Slack Form*
- maximize $20 x_1 + 30 x_2$
- subject to:
	- $1 x_1 + 2 x_2 + s_1 = 40$
	- $2 x_1 + 1 x_2 + s_2 = 42$ 
	- $1 x_1 + 1 x_2 + s_3 = 25$
	- $x_1, x_2, s_1, s_2, s_3f \ge 0$
	- 
#### Results 

```
Optimal value: 650.0
Chairs to produce: 10
Tables to produce: 15
```

![Pasted image 20240517112011](University/5%20Optimization%20techniques/lab/Lab.%2010%20Linear%20Programming/img/Pasted%20image%2020240517112011.png)


---

## Exercise 3
#### Text
A mutual fund has 100,000 $ to be invested over a three year horizon.
Three investment options are available:
1. **Annuity:**  the fund can  pay a same amount of new capital at the beginning of each of three years and receive a payoff of 130% of **total capital** invested  at the end of the third year. Once the mutual fund decides to invest in this annuity, it has to keep investing in all subsequent  years in the three year horizon.  
2. **Bank account:** the fund can deposit any amount  into a bank at the beginning of each year and receive its capital plus 6% interest at the end of that year. In addition, the mutual fund is permitted to borrow no more than \$20,000 at the beginning of each year and is asked to pay back the amount borrowed plus 6% interest at the end of the year. The mutual fund can choose whether to deposit or borrow at the beginning of each year.  
3. **Corporate bond:** At the beginning of the second year, a  corporate bond becomes available.
  The fund can buy an amount
  that is no more than $50,000 of this bond at the beginning of the second year and  at the end of the third year receive a payout of 130% of the amount invested in the bond.  

The mutual fund’s objective is to maximize total payout that it owns at the end of the third year.

The table below shows the mutual fund’s decision variables together with the timing protocol described above:

|                 | Year 1 | Year 2 | Year 3 |
| --------------- | ------ | ------ | ------ |
| Annuity         | x1     | x1     | x2     |
| Bank Account    | x2     | x3     | x4     |
| Corporate bound | 0      | x5     | 0      |

#### Solution 

Year 1: 
- $x_2 \ge -20000$ (Bank Account)
- $x_1 + x_2 = 100000$ (definition)

Year 2:
- from Bank Account we have $x_2 + 6/100$
- from Annuity we must have again $x_1$ 
- from Bank Account we can decide to borrow
- from Corporate bound we have $x_5$
- so : $x_1  + x_5 = (x_2 + 0.06x_2) - x_3$
- other 
	- $x_3 \ge -20000$ (Bank Account)
	- $x_5 \le 50000$ (Corporate Bound)
Year 3:
- from Bank Account we have $x_3 + 6/100$
- from Annuity we must have again $x_1$
- from Bank Account we can decide to borrow
- so : $x_1 = (x_3 + 0.06x_3) - x_4$
- other
	- $x_4 \ge -20000$ (Bank Account)

Final profit:
- 130% of $3 x_1$ +
- 6% of $x_4$ +
- 130% of $x_5$

#### *Problem Form*
- maximize $1.3 (3x_1) + 1.06 (x_4) + 1.3 (x_5)$
- subject to:
	- $x_1 + x_2 = 100000$
	- $x_1  + x_5 = (x_2 + 1.06x_2) - x_3$
	- $x_1 = (x_3 + 1.06x_3) - x_4$
	- $x_2, x_3, x_4 \ge -20000$
	- $x_5 \le 50000$
	- $x_1,x_2,x_3,x_4,x_5 \ge 0$

#### Result 
```
Annuity: $ 24927.75
Bank Deposit: $ 75072.25
Bank Borrowing: $ 4648.83
Corporate Bond: $ -20000.0

Optimal total payout: $ 141018.24
```

```
        message: Optimization terminated successfully. (HiGHS Status 7: Optimal)
        success: True
         status: 0
            fun: -141018.24349792697
              x: [ 2.493e+04  7.507e+04  4.649e+03 -2.000e+04  5.000e+04]
            nit: 0
          lower:  residual: [ 2.493e+04  9.507e+04  2.465e+04  0.000e+00
                              5.000e+04]
                 marginals: [ 0.000e+00  0.000e+00  0.000e+00  1.650e-01
                              0.000e+00]
          upper:  residual: [       inf        inf        inf        inf
                              0.000e+00]
                 marginals: [ 0.000e+00  0.000e+00  0.000e+00  0.000e+00
                             -1.470e-03]
          eqlin:  residual: []
                 marginals: []
        ineqlin:  residual: [ 0.000e+00  0.000e+00  0.000e+00]
                 marginals: [-1.376e+00 -1.299e+00 -1.225e+00]
 mip_node_count: 0
 mip_dual_bound: 0.0
        mip_gap: 0.0
````