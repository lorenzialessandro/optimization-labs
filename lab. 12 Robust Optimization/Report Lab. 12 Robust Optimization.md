# Report Lab. 12 Robust Optimization

notebook : [optimtech_lab12](optimtech_lab12.ipynb)

**Table of Contents**

- [Modified Knapsack 0/1 Problem](#modified-knapsack-01-problem)
	- [Ellipsoidal Uncertainty Set](#ellipsoidal-uncertainty-set)
		- [Conclusion](#conclusion)
	- [Finite Uncertainty Set](#finite-uncertainty-set)
		- [Conclusion](#conclusion)
	- [Visual comparison:](#visual-comparison)
- [Robust Portfolio Optimization](#robust-portfolio-optimization)
	- [Conclusion](#conclusion)


## Modified Knapsack 0/1 Problem

In order to understand the different impact of the uncertainty sets, here an analysis on both the ellipsoidal and the finite uncertainty sets with different sizes and intervals. 

### Ellipsoidal Uncertainty Set
In this case the values chosen for the for the ellipsoidal radii to test in robust optimization are `[0.5, 1, 2, 3]`, in order to cover different levels of conservatism.

Here the effect on the objective value and on the probability of violation. 

|                          | (r): 0.5 | (r): 1 | (r): 2 | (r): 3 |
| ------------------------ | -------- | ------ | ------ | ------ |
| Objective Value          | 15.0     | 15.0   | 13.0   | 12.0   |
| Probability of Violation | 0.0203   | 0.0203 | 0.0    | 0.0    |


![Pasted image 20240531104028](img/Pasted%20image%2020240531104028.png)
\*Note that in the second plot I've plot the result on different scales in order to have a better view of the probability of violation. 

#### Conclusion 

The results show that:
- with **smaller radius** we have and **higher objective values** but also a **higher chance of constraint violation**, the model is so **less conservative**.
- with **moderate radius** we pick the right **balance between being conservative and maximizing the objective value**, since both the objective value and the violation probability are in fact "moderated".
- with **larger radius** the model is **more conservative**, so we obtain **lower objective values** but also a **lower chance of constraint violation**.

### Finite Uncertainty Set
For the finite uncertainty set the analysis was done using small, moderate and large interval, in order, again, to cover a range of conservatism levels.

In particular:
- small interval: `[-0.1, 0, 0.1]`
- moderate interval: `[-0.5, 0, 0.5]`
- big interval: `[-1, 0, 1]`
- bigger interval: `[-2, -1, 0, 1, 2]`

Here the results:

| interval:                | small   | moderate | big  | bigger |
| ------------------------ | ------- | -------- | ---- | ------ |
| Objective Value          | 16.0    | 17.0     | 18.0 | 21.0   |
| Probability of Violation | 0.49745 | 0.9648   | 1.0  | 1.0    |

![Pasted image 20240531110209](img/Pasted%20image%2020240531110209.png)
 \*Note that in the second plot I've plot the result on different scales in order to have a better view of the probability of violation. 

#### Conclusion 
The results show that:
- with **smaller intervals** we have and **moderate objective values** and also **lower chance of constraint violation**, the model is so **more conservative**.
- with **moderate intervals** we pick the right **balance between being conservative and maximizing the objective value**, since both the objective value and the violation probability are in fact "moderated".
- with **larger intervals** the model is **less conservative**, so we obtain **larger objective values** but also a **bigger chance of constraint violation**.

### Visual comparison:

![Pasted image 20240531111005](img/Pasted%20image%2020240531111005.png)

![Pasted image 20240531111017](University/5%20Optimization%20techniques/optimization-labs/lab.%2012%20Robust%20Optimization/img/Pasted%20image%2020240531111017.png)


---

## Robust Portfolio Optimization

To understand the uncertainty effects on the objective value and on the number of different stocks chosen, I've analyze different values of the `l` lower bound and on the `u` upper bound of the box.

These are the results using respectively uncertainty_bounds = `[(0.1, 0.1),(-0.5, 0.5), (-1, 1), (-3, 2)]`:

|                               | (l=0.1, u=0.1) | (l=-0.5, u=0.5) | (l=-1, u=1) | (l=-3, u=2) |
| ----------------------------- | -------------- | --------------- | ----------- | ----------- |
| Objective value               | 1.0560         | 1.0196          | 0.9963      | 0.9602      |
| N. of different stocks chosen | 1              | 4               | 5           | 7           |

![Pasted image 20240531142232](img/Pasted%20image%2020240531142232.png)

![Pasted image 20240531142515](img/Pasted%20image%2020240531142515.png)

### Conclusion 

1. **Objective Value:** as the **uncertainty range** became **larger**, as the **objective function decrease**, since the portfolio is more sensitive to uncertainty. 

2. **Number of Different Stocks Chosen:** 
	- using **smaller uncertainty** set the model choose a more **located portfolio** so the number of different stocks is lower.
	- with **larger uncertainty** sets the model tends to **diversify more**, selecting so a higher number of stocks to spread the risk. 

3. **Investment Distribution**: higher uncertainty encourages **spreading investments** across more stocks to balance potential risks and returns.


In summary: the portfolio optimization model **adapts** its strategy to deal with increasing uncertainty **diversifying** more and accepting lower worst-case returns. 

This **balance** between risk and return is a key principle in robust portfolio optimization.