# Report Lab. 1: Local search

notebook [optimtech_lab1](optimtech_lab1.ipynb)

**Table of Contents**

- [Grid Search](#grid-search)
- [Random Search](#random-search)
- [Grid vs Random Search](#grid-vs-random-search)
- [Nelder Mead Optimization](#nelder-mead-optimization)
	- [Using different functions](#using-different-functions)
- [Powell Optimization](#powell-optimization)
	- [Using different functions](#using-different-functions)
- [Conclusion](#conclusion)


## Grid Search
`step_size`: in grid search increase the step size lead to a reduction of the amount of points visited by the search, with small step size in fact we are increasing the "grid movement" needed.
With small step size the accuracy of the best point obtained is more efficient but the search cost is higher since the number of iterations needed grown exponentially. 

In general we can say that **smaller step size means more exploration of the space, that hopefully means also best point obtained**. However the **computational cost increase**.

Here a visual example on the visited space and iterations changing the step size and using `Ackley` as benchmark function:

| step size = 1                        | step size = 3                        | step size = 5                        |
| ------------------------------------ | ------------------------------------ | ------------------------------------ |
| ![Pasted image 20240530211106](img/Pasted%20image%2020240530211106.png) | ![Pasted image 20240530211134](img/Pasted%20image%2020240530211134.png) | ![Pasted image 20240530211200](img/Pasted%20image%2020240530211200.png) |
| ![Pasted image 20240530211112](img/Pasted%20image%2020240530211112.png) | ![Pasted image 20240530211139](img/Pasted%20image%2020240530211139.png) | ![Pasted image 20240530211205](img/Pasted%20image%2020240530211205.png) |

---
## Random Search
`n_samples_drawn`: since it refer to the number of parameter randomly sampled from the search space, **increasing this parameter the algorithm explores more different parts of the search space and it will reach the best point with more probability**. As in grid search with a small `step_size`, larger `n_samples_drawn` lead to an **higher computational cost**.

So here increase this parameters means try new spots and so visiting more points and improve the efficiency of the search, the drawback is the search cost.  

Here an example using the `Hyperellipsoid` function and changing the `n_samples_drawn` paramter:

| n_samples_drawn = 10                 | n_samples_drawn = 50                 | n_samples_drawn = 250                |
| ------------------------------------ | ------------------------------------ | ------------------------------------ |
| ![Pasted image 20240530211634](img/Pasted%20image%2020240530211634.png) | ![Pasted image 20240530211656](img/Pasted%20image%2020240530211656.png) | ![Pasted image 20240530211742](img/Pasted%20image%2020240530211742.png) |
| ![Pasted image 20240530211642](img/Pasted%20image%2020240530211642.png) | ![Pasted image 20240530211700](img/Pasted%20image%2020240530211700.png) | ![Pasted image 20240530211746](img/Pasted%20image%2020240530211746.png) |

---

## Grid vs Random Search
Grid method search the entire parameter space, performing an exhaustive search, trying so every possible values combination for the parameters, instead the random selects a sub-set of values randomly. This means that theoretically the grid search is more precise but, obviously, it's also more computationally and time demanding. Random search is in fact a lot cheaper than grid search.

Despite this observation is possible to note that in many instances random search performs about as well as grid search, as reported in [this blog post at Dato](https://web.archive.org/web/20160701182750/http://blog.dato.com/how-to-evaluate-machine-learning-models-part-4-hyperparameter-tuning) by Alice Zheng. Without reporting the details, the conclusion is:
> "if the close-to-optimal region of hyperparameters occupies at least 5% of the grid surface, then random search with 60 trials will find that region with high probability."

In general, adapting the step size and and the number of samples drawn in such a way that the comparison between the two methods make sense, I noted that the **performances are quite similar**, in every case we have to remind that both techniques are "uniformed".


---
## Nelder Mead Optimization 
`x_0`: this methods seems to be **sensitive to the initial point**, manually providing a good one in fact it's possibile to use a few iterations to convergence and, on the other hand, with bad a starting point, the number of iterations needed increase.   

`max_iter`: with more iterations possible the **search improve the chance to find a better solution**, in fact is possible to see that with too low value for this parameter the algorithm **stops too early**, so before to find a minimum. But it's also quite easy to note that is some cases, with an higher `max_iter`, the search wastes a lot of iterations (increasing so the cost) due to the fact that the optimum is yet reached and so there is an **unless overhead**.   

### Using different functions
Using different functions the adjustment of the parameters has different effects, here same examples:
- Ackley: 
   -  `x_0 = [2.0, 2.0]` and `max_iter=100` => good solution close to 0 (global minimum)
   -  `x_0 = [5.0, 5.0]` => similar solution
   -  `max_iter=200` => => similar solution
- Rastrigin
   -  `x_0 = [2.0, 2.0]` and `max_iter=100` => bad solution (local minimum?)
   -  `x_0 = [5.0, 5.0]` => similar solution
   -  `max_iter=200` => => improved solution
- Rosenbrock
   -  `x_0 = [2.0, 2.0]` and `max_iter=100` => bad solution (local minimum?)
   -  `x_0 = [5.0, 5.0]` => similar solution
   -  `max_iter=200` => => improved solution
- Sphere
   -  `x_0 = [2.0, 2.0]` and `max_iter=100` => good solution close to 0 (global minimum)
   -  `x_0 = [5.0, 5.0]` => similar solution
   -  `max_iter=200` => => similar solution

So for Rastrigin and Rosenbrock increasing `max_iter` may lead to slightly better solutions, while for Sphere nothing change. Adjust `x_0` has a limited impact.

Here an example of the algorithm using a `Rosenbrock` function and increasing the number of iterations (fixed starting point):

| max_iter = 10                        | max_iter = 50                        | max_iter = 100                       |
| ------------------------------------ | ------------------------------------ | ------------------------------------ |
| ![Pasted image 20240530213523](img/Pasted%20image%2020240530213523.png) | ![Pasted image 20240530213633](img/Pasted%20image%2020240530213633.png) | ![Pasted image 20240530213700](img/Pasted%20image%2020240530213700.png) |
| ![Pasted image 20240530213529](img/Pasted%20image%2020240530213529.png) | ![Pasted image 20240530213641](img/Pasted%20image%2020240530213641.png) | ![Pasted image 20240530213708](img/Pasted%20image%2020240530213708.png) |


---

## Powell Optimization
The adjustment of `x_0` and `max_iter` has **effects very similar to the Nelder-Mead's ones**: good starting point means few iteration and more iteration lead to petter solution but sometimes too high value of this will lead to search that are not useful since the optimum is already found.   

### Using different functions
Using the same functions and adjustments of the previous example, the **results appear similar**.
With `x_0 = [2.0, 2.0]` and `max_iter=100` Ackley and Sphere are slightly better using Powell instead Ackley is equal. The improvements changing the starting point are again similar and slightly more evident using Nelder Mead, same for `max_iter=200`. In general the performance are almost the same. 

---
## Conclusion
In summary, grid and random search are simpler but more computationally expensive and in general inefficient for high-dimensional spaces. Powell and Nelder-Mead methods are more efficient and typically converge faster.
The choice between Nelder-Mead and Powell methods depends on the specific characteristics of the optimization problem and, in particular, by the function. Same for random / grid choice. 

So there is not an algorithm always better than the other: **the kind of objective function influence the entire process**.

> **Benchmark functions play a crucial role in evaluating the performance of optimization algorithms**

We have to **understand their characteristics**, like complexity, multimodality, non-convexity, dimensionality ... then adapt choice of parameters and perform the most suitable optimization algorithms.

So the **choice of parameters is influenced by the characteristics of the function being optimized and different optimization have parameters that control their behavior**. 