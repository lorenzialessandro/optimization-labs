# Report Lab. 2: Stochastic global optimization

**Table of Contents**

- [DIRECT](#direct)
	- [`eps`](#eps)
	- [`max_iter`](#max_iter)
- [BASIN-HOPPING](#basin-hopping)
- [Conclusion](#conclusion)


## DIRECT

### `eps`

The `eps` parameter, following the documentation, determines how "strong" the convergence criterion is.

With **smaller value** of `eps` in fact we can notice that the performance **tends** to improve due to the high degree of convergence set. So, on the other side, putting `eps` too high will result in a earlier termination of the algorithm, maybe with a not fully optimum converged value. 

The lab studies show that this **parameter influence is really related to the benchmark function selected**, for some of these functions the improvements of the results are evident.

For example using `EggHolder()` as benchmark function with 300 iterations and moving `eps` from 1 to 0.1 we can see a good improvement in the best value founded by the algorithm:

```python
Using eps = 1 # same with 100
Best value: -724.8978437065213
Best location: [-341.33333333 -455.11111111]
--------------------------------------------
Using eps = 0.1 # same with 0.00001
Best value: -931.5964679665101
Best location: [467.75308642 455.11111111]
```

Visually:

| eps = 0.1                                                               | eps = 1                                                                 |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| ![Pasted image 20240601111202](img/Pasted%20image%2020240601111202.png) | ![Pasted image 20240601111142](img/Pasted%20image%2020240601111142.png) |
| ![Pasted image 20240601111224](img/Pasted%20image%2020240601111224.png) | ![Pasted image 20240601111148](img/Pasted%20image%2020240601111148.png) |

As sad before **the choice of the benchmark function is relevant**, for example with *similar* function like the `Ackley` and the `GoldsteinAndPrice`, that are both non-convex and multimodal with global minimum near local optima, even increasing `eps` there are not changes. Furthermore with other functions, like the `Himmelblau`, the improvements (and so the parameter effects) are really small:

```python
Ackley: eps = [0.1, 1] => [0.0309, 0.0309]
# no improvement, same with GoldsteinAndPrice
--------------------------------------------
eps = [0.1, 1] => [0.0785, 0.0012] 
# small improvement
```

It has been noticed that even setting increasingly smaller values (e.g. from 0.1 to 0.00000000000001) there are not changes or the changes are really small, the same thing happens with "from-large-to-very-large" values. That means that, probably, there is a sort of normalization / bound range.

### `max_iter`
An high value of the `max_iter` parameter, as seen in other methods, will probably lead to better result since allow to **explore different regions of the search space** and so can **potentially converge closer to the optimal solution**. On the other side, **too many iterations can be useless** since the optimum can be already discovered. 

We have to keep in mind that this parameter has an **impact on the computational cost** and, a part for the benchmark functions selected, this is the parameter that most influences the cost.

Here we can say that, for example, using a `Ackley` function, increasing the number of iterations is useless, instead with the `EggHolder` (difficult function to optimize because of the large number of local minima) with more iterations the result is better if we increase it from 10 to 100, but not from 100 to 1000.

```python
Ackley: max_iter = [10, 100, 1000] => [0.0309, 0.0309, 0.0309]
# no improvement, same with GoldsteinAndPrice
--------------------------------------------
EggHolder: max_iter = [10, 100, 1000]: [-573.125, -931.596, -931.596] 
# 10-100: improvement, 100-1000: no improvement
```

Visually:

| max_iter = 10                                                           | max_iter = 100                                                          |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| ![Pasted image 20240601111707](img/Pasted%20image%2020240601111707.png) | ![Pasted image 20240601111647](img/Pasted%20image%2020240601111647.png) |
| ![Pasted image 20240601111711](img/Pasted%20image%2020240601111711.png) | ![Pasted image 20240601111652](img/Pasted%20image%2020240601111652.png) |

---

## BASIN-HOPPING
Temperature `T` controls the exploration-exploitation trade-off: higher T encourages exploration, while lower values favor exploitation.

Here all is about the function points locations, in same case we need to escape from local minima ( = large value of T), in other configurations instead we want to search around the local minima to refine the solution ( = smaller value of T).
Find the balance is the key.

Report shows that **functions with some peaks and valley, like the `Rastrigin` or the `Ackley` are more effected by this parameter then the more "smooth" ones, like the `Rosenbrock`.**

```python
Rastrigin: T = [0.1, 1.0, 10.0] => [2.131e-14, 4.974, 1.999]
# from e-14 to 4.9
--------------------------------------------
Rosenbrock: T = [0.1, 1.0, 10.0] => [1.981e-12, 2.011e-12, 4,112e-12]
# all near e-12
```

Here a visual example:

| T = 0.1                                                                 | T = 1                                                                   | T = 10                                                                  |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| ![Pasted image 20240601142807](img/Pasted%20image%2020240601142807.png) | ![Pasted image 20240601142845](img/Pasted%20image%2020240601142845.png) | ![Pasted image 20240601142858](img/Pasted%20image%2020240601142858.png) |

Also for the `stepsize` parameter, larger step sizes allow for more extensive exploration, so again non-smooth functions with multiple local minima are more influenced by changing this configuration.

```python
Rastrigin: s = [10, 100] => [2.131e-14, 0.995]
# from e-14 to 0.99
```

Visually, using `EggHolder`

| stepsize = 10                                                           | stepsize = 100                                                          | stepsize = 1000                                                         |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| ![Pasted image 20240601143158](img/Pasted%20image%2020240601143158.png) | ![Pasted image 20240601143143](img/Pasted%20image%2020240601143143.png) | ![Pasted image 20240601143124](img/Pasted%20image%2020240601143124.png) |
| ![Pasted image 20240601143201](img/Pasted%20image%2020240601143201.png) | ![Pasted image 20240601143147](img/Pasted%20image%2020240601143147.png) | ![Pasted image 20240601143131](img/Pasted%20image%2020240601143131.png) |


The number of iterations influences the trade-off between exploration and exploitation. Similar to the temperature higher value allows the algorithm to explore more and find the balance, like for the temperature again, is fundamental. 

Report shows that **no smooth functions are more sensitive**, exactly like with the temperature but the improvements seems to be more significative, but it's important to consider the **increasing cost** due to more iterations. 


The starting point `x_0` significantly affects the algorithm. If the initial point is close to a local minimum, the algorithm may converge prematurely but too far really affect the computational cost of the exploration. Setting the `x_0` in an appropriate way thank to the prior visual knowledge is important. That holds for all the benchmark functions and **it seems the most crucial parameter**.

Here a simple example with `EggHolder`

```python
x_0 = [-300, 200] => -558.5207320973292 # local
x_0 = [-420, 400] => -894.5789003905979 # near the global
```


Futhermore I've tried to lunch different benchmark functions with randomly starting points and the best values founded changes a lot due to the initialization of `x_0`.  

## Conclusion

As Powell and Nelder-Mead methods, also for the DIRECT and Basin-Hopping the **fundamental thing is the setting of the parameters and the objective function played**, with its peculiarities. It's quite difficult to compare directly these different algorithms, even trying to adapt the parameters to be more "similar" as possible in order to make performance evaluations. 