# Report Lab. 3: GD, Newton methods and BFGS

notebook [optimtech_lab3](optimtech_lab3.ipynb)

**Table of Contents**

- [GRADIENT DESCENT](#gradient-descent)
- [NEWTON METHOD](#newton-method)
- [BFGS OPTIMIZATION](#bfgs-optimization)
- [Conclusion](#conclusion)


## GRADIENT DESCENT

 `learning rate`: 
It's evident that with **higher** learning rate in general we have **faster convergence** (larger step), but in same case the algorithm **overshoots** the min. Using a **smaller** learning rate instead sometimes we are more precise but, since the steps are smaller, the number of **iterations grown**. Furthermore, using benchmark function having landscape with a lot of valleys and peaks, a too **low** learning stop the process even if the point is a local minima. 

After different tried the function that seems to be more **sensitive** to the `lr` changes are the ones with **more many local minima or saddle points**. 

For example using the `Ackley` function, we can see the differences using various learning rates. In particular with a too small one the method is not able to reach a good point, and, with a higher `lr` the convergence is faster, finally with a too big parameter the number of iteration will continue not improving the final result.  

| ![img](img/Pasted%20image%2020240315102019.png) | ![img](img/Pasted%20image%2020240315102113.png) | ![img](img/Pasted%20image%2020240315102133.png) |
| ----------------------------------------------- | ----------------------------------------------- | ----------------------------------------------- |
| ![img](img/Pasted%20image%2020240315102052.png) | ![img](img/Pasted%20image%2020240315102122.png) | ![img](img/Pasted%20image%2020240315102142.png) |
| `lr = 0.01`                                     | `lr = 0.1`                                      | `lr = 0.9`                                      |

`tolereance`:
This parameter determines the **convergence criterion** and with smaller tolerance value the optimization algorithm should converge to a more precise solution but leads to more iterations (looking for better refining). 

It influences the **stop** of the algorithm, and so it's another parameters, as the max number of iterations, that has an impact on the termination of the process.

Following the documentation, when the improvement in the objective function between consecutive iterations is smaller than the tolerance value, the optimization process is terminated, assuming that further iterations are unlikely to significantly improve the solution.

In general with the lab experiment this parameter seems to be the one that **influences less the final result**, it's quite difficult to see strongly different result changing the tolerance value. But, for example, it the setting is too high (0.1 in most cases) the algorithm basically stop immediately.

The number of iterations parameter instead has **more effect** that's can be seen easily.

As for the learning rate, the choice of a different tolerance is more evident with benchmark function with "irregular behavior", that present a lot of local mins, ups and downs.    

---

## NEWTON METHOD
Newton Method, using also the curvature information (second derivative), **converge faster** than the Gradient approach, since is using some add information, beyond the first derivate ones.  

Using the simplest example, it's possible to observe that using an `Hypersphere` function the number of evaluation needed using Newton Method is (varying the starting point) about a seventh in respect to the Gradient. 

| Gradient Descent                                                            | Newton  Method                                                              |
| --------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| ![img/Pasted image 20240601155440](img/Pasted%20image%2020240601155440.png) | ![img/Pasted image 20240601155447](img/Pasted%20image%2020240601155447.png) |
| ![img/Pasted image 20240601155456](img/Pasted%20image%2020240601155456.png) | ![img/Pasted image 20240601155450](img/Pasted%20image%2020240601155450.png) |
 
In general, even using other functions the improvement in convergence of this method is not always clear, also tuning the parameter in the more similar way.



The `tolerance` parameter seems to behave exactly like for the Gradient method, using the same benchmark functions in fact we can notice similar result. 

As for other methods seen, the well-behaved benchmark functions are less effected by parameter changes. However, for more complex and non-convex functions like the `Rosenbrock` function, the **impact of parameter** changes in more evident.


---

## BFGS OPTIMIZATION

In the BGSS optimization method the parameter that more influences the algorithm result is the **initial point** `x0`, this method, as the other basically, is really sensitive to the initial guest, and that's true for every benchmark function.

The `esp` tolerance and the `max_iter` are instead **less relevant** than the choice of the starting point and of the function. That's not means that is not important to try different value of both this parameters, since, as sad before, they can strongly influence the stopping criteria of the method. 

Here the results using the `Rosenbrock` as benchmark function and varying the parameters:

```python
x0 = [-2., -2.] eps = 0.0000001 maxiter = 1000
Best value: 9.031763612320655e-10
Best location: [0.99996995 0.99993985]
# ------------------------------------
# change x0:
x0 = [-5., 7.] eps = 0.0000001 maxiter = 1000
Best value: 8.67906030135037e-10
Best location: [0.99997055 0.99994102]
# ------------------------------------
# change eps:
x0 = [-5., 7.] eps = 0.01 maxiter = 1000
Best value: 16.67021868289558
Best location: [-3.08291791  9.5043753 ]
# ------------------------------------
# change maxiter:
x0 = [-2., -2.] eps = 0.0000001 maxiter = 100
Best value: 8.185036722109023e-10
Best location: [0.99997144 0.99994271]
# ------------------------------------
```

| x0 = [-2., -2.]                      | x0 = [-5., 7.]                       |
| ------------------------------------ | ------------------------------------ |
| ![img/Pasted image 20240601155928](img/Pasted%20image%2020240601155928.png) | ![img/Pasted image 20240601160011](img/Pasted%20image%2020240601160011.png) |

| eps = 0.0000001                                                             | eps = 0.01                                                                  |
| --------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| ![img/Pasted image 20240601155928](img/Pasted%20image%2020240601155928.png) | ![img/Pasted image 20240601160320](img/Pasted%20image%2020240601160320.png) |


We know that **computation of the approximate inverse Hessian matrix** is a key element in this optimization process and the main difference between BFGS and L-BFGS lies in how they do that, **L-BFGS is more efficient** since it keeps track of a limited history of previous iterates and gradient differences and not storing the full matrix, as done by BFGS.

Using the benchmark functions in lab is quite different to see different results using BFGS and L-BFGS. For example here using the `Rosenbrock` function we can see the same result: 
```python
# using BFGS:
Best value: 9.031763612320655e-10
Best location: [0.99996995 0.99993985]
--------------------------------------------
# using L-BFGS:
Best value: 9.031763612320655e-10
Best location: [0.99996995 0.99993985]
```

In order to understand better the differences in memory performances I've tried to run a more large optimization problem based on Rosenbrock and compare the memory usages using the `tracemalloc` module.
Here the result:
```python
[BFGS] - Memory usage difference:
>:77: size=307 KiB (+59.3 KiB), count=3047 (+844), average=103 B

[L-BFGS-B] - Memory usage difference:
>:77: size=248 KiB (-59.3 KiB), count=2203 (-844), average=115 B
```


---

## Conclusion 

Despite the differences in the search cost are really influenced by the choice of the benchmark function and the starting point, in general we can note that:

**Gradient Descent**:
- requires **fewer function evaluations** per iteration compared to Newton's Method and BFGS, especially in high-dimensional problems
- may require **more iterations** to converge to the optimal solution

**Newton's Method**: 
- requires **more function evaluations** per iteration compared to Gradient Descent because it involves computing both the function values and the second derivatives (Hessian matrix)
- can **converge faster** than Gradient Descent, especially when the objective function is well-behaved and has low to moderate condition number

**BFGS**:
- it's a sort of **balance between the computational cost of Gradient Descent and the convergence speed of Newton's Method**


Regaring Nelder-Mead and Powell's methods they require more function evaluations per iteration compare to all this methods.

---
