# Report Lab. 5: Iterated local Search and Simulated Annealing

notebook [optimtech_lab5](optimtech_lab5.ipynb)

**Table of Contents**

- [Iterated Local Search](#iterated-local-search)
	- [starting point](#starting-point)
	- [`ls_max`](#ls_max)
	- [Perturbation](#perturbation)
	- [Local Search](#local-search)
- [Simulated Annealing](#simulated-annealing)
	- [implementations](#implementations)
	- [starting point](#starting-point)
	- [temperature](#temperature)
	- [neighborhood](#neighborhood)
	- [acceptance policy](#acceptance-policy)
	- [temperature update and alpha](#temperature-update-and-alpha)
- [Conclusion](#conclusion)

## Iterated Local Search

### starting point
The initial solution or initial state can significantly influence the search process and so the quality of the solution found. Exactly like the VNS, as the `x0` is far from the global optimum, as the number iterations grown. 

Again, the **starting solution is also a sort of exploration - exploitation trade-off**, with a lot of influence, particularly at the beginning. 

Since in the lab problem we cannot embed some heuristic-based initialization, I have generated a random binary vector `initial_point` and we can observe that the number of iterations varies, basis on the starting solution picked. 

| `vns_result`                                                            | `vns_result`                                                            |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| [1 1 0 0 0 0 0 1 1 1]                                                   | [0 1 1 0 1 1 0 0 1 1]                                                   |
| ![Pasted image 20240405110739](Pasted%20image%2020240405110739.png) | ![Pasted image 20240405110720](Pasted%20image%2020240405110720.png) |

### `ls_max`
This parameter is the maximum number of iterations allowed for the local search at each iteration of the algorithm. 

A higher value of `ls_max` allows the local search to explore the local neighborhood of a solution more extensively. This can be beneficial when the initial solution is close to a local optimum. However, excessively high values of `ls_max` might lead to over exploitation, causing the algorithm to get stuck in local optima and potentially missing better regions of the search space. Obviously increasing `ls_max` generally increases the computational resources required for each iteration of the algorithm.

| `ls_max = 1`                         | `ls_max = 50`                        |
| ------------------------------------ | ------------------------------------ |
| ![Pasted image 20240405111403](Pasted%20image%2020240405111403.png) | ![Pasted image 20240405111422](Pasted%20image%2020240405111422.png) |

So **setting `ls_max` too low means not analyzing the current neighborhood structure properly**, on the other side if this parameter is **too large we are wasting iterations** in the structure and we will instead prefer to move on. 

The correct set is so in between, in order to be able to explore deep each neighborhood but without spending too much time in each one.

The behavior is really similar to the `kmax` in VNS.


### Perturbation

The strength of perturbation significantly influences both the quality and velocity of the search process.
In particular strong perturbation accelerates the search process that is so similar to random one, meaning that better solutions will only be found with a very low probability. Controversy with weaker perturbations only a few new solutions will be explored. 

Perturbation can accelerate the search process by quickly escaping from local optima but on the other hand, if the perturbation process is computationally expensive, it may slow down the search. 

To better understand the influence of the strength perturbations, I've first increase the Knapsack size to 50 and then I lunch the algorithm with 3 different perturbations:
1. max strength perturbation: `xp = perturbation_k(x, len(x) - 1)`
2. min strength perturbation: `xp = perturbation_k(x, 1)`
3. "middle" strength perturbation: `xp = perturbation_k(x, int(len(x) / 2))`

Here we can see the result: 

| max strength perturbation            | middle strength perturbation         | min strength perturbation            |
| ------------------------------------ | ------------------------------------ | ------------------------------------ |
| ![Pasted image 20240405121041](Pasted%20image%2020240405121041.png) | ![Pasted image 20240405120920](Pasted%20image%2020240405120920.png) | ![Pasted image 20240405120844](Pasted%20image%2020240405120844.png) |
| k = 49                               | k = 25                               | k  = 1                               |

This shows that too strong perturbations result in random restart, in particular at the beginning and instead minimum perturbations seem to perform better even if at the start the process introduces less significant changes.

As for the VNS definition of neighborhood, the solution involve a trade-off balancement. 

### Local Search 

We can notice that the best improvement performs a more complete search, requiring so more iterations. The first improvement is instead less cost demanding but can stuck in local optima if the first improving solution is not globally optimal. 

| First improvement                                                       | Best improvement                                                        |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| ![Pasted image 20240405160637](Pasted%20image%2020240405160637.png) | ![Pasted image 20240405160704](Pasted%20image%2020240405160704.png) |
| 86 iterations                                                           | 289 iterations                                                          |


---

## Simulated Annealing 

### implementations 
In order to have a better analysis and understanding of this algorithm behavior, two different version of the simulated annealing have been implemented:  
1. First version:    
    - Acceptance criterion solely based on whether the evaluation of the new solution is better than the current one.
    - Doesn't include probability calculation for acceptance based on the Metropolis criterion.
2. Second version:
    - Includes the Metropolis criterion for acceptance, which involves generating a random number and comparing it with the probability ratio.
    - Adjusts the temperature inside the acceptance function, considering its use in the probability calculation.

### starting point 
As seen for almost every optimum algorithm so far, the starting point can affect how quickly the algorithm converges to a solution. We will se that for this algorithm other parameters have more effect on the search process than the staring point itself.

### temperature 

We can notice that if `T` is really small, we will basically never accept worst solution, if instead `T` is too high, we will do the opposite, accepting always worst solution (so we are close to a random series of steps).  

Higher initial temperatures allow for more exploration, potentially helping the algorithm escape local optima and lower initial temperatures focus more on exploitation and may converge faster but can get trapped in local optima.

Here we can notice as the running a search process with the same parameters (`iter = 10, k = 7, alpha = 0.8`), a small `T` is faster but worst ending in a local minimum, with higher temperature instead the number of iterations grows a lot and there is a "deeper" exploitation.  

| T = 1                                                                         | T = 10                                                                                 |
| ----------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| ![Pasted image 20240405164915 1](Pasted%20image%2020240405164915%201.png) | ![2e573d0588ab16322fbc0e20a823d715_MD5](2e573d0588ab16322fbc0e20a823d715_MD5.jpeg) |

In general we want a temperature relatively high at the beginning in order to accept worse solution with more probably when it start.

### neighborhood

The effect of the choice of neighborhood is similar to other algorithm yet seen. 

If the neighborhood is too small, the algorithm may get stuck in local optima. If it's too large, it may take longer to explore all possible solutions.
Furthermore a smaller neighborhood typically results in faster searches but it might increase the risk of getting trapped in local optima. Conversely, larger neighborhood lead to slower searches but could potentially find better solutions.

### acceptance policy 

In general a more permissive acceptance policy allows for more exploration, while a stricter policy favors exploitation.

The Metropolis criterion is a key component, it allows for exploration and escaping from local minimum. Without it we maybe have faster convergence but we risk of getting trapped in local optima and in general exploration is reduced.

Here a comparison between Simulated Annealing using the Metropolis criterion and not using it where the acceptance criterion is solely based on whether the evaluation of the new solution is better than the current one.

| without Metropolis criterion                                            | with Metropolis criterion                                               |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| ![Pasted image 20240405170239](Pasted%20image%2020240405170239.png) | ![Pasted image 20240405170235](Pasted%20image%2020240405170235.png) |
Using the Metropolis criterion allows for a more balanced exploration of the solution space, potentially leading to the discovery of better solutions and avoiding premature convergence.

### temperature update and alpha
The `alpha` parameter (Boltzmann-factor) works as cooling scheduling parameter. Slowing down the cooling schedule allows for more exploration early in the search, while a faster cooling schedule focuses on exploitation.
 
| `alpha = 0.2`                        | `alpha = 0.8`                        |
| ------------------------------------ | ------------------------------------ |
| ![Pasted image 20240405170758](Pasted%20image%2020240405170758.png) | ![Pasted image 20240405170805](Pasted%20image%2020240405170805.png) |
In general we want to set it quite large (close to 1) in order to have a slow decreasing of the temperature, and so not stucking near a local minimum.

---

## Conclusion

**Efficiency**:
- Simulated Annealing tends to be more efficient when the objective function is complex and has many local optima. 
- Variable Neighborhood Search is particularly effective for optimization problems where the structure of the neighborhood can be dynamically adapted during the search process.
- Iterated Local Search has an efficient balancing local exploration and exploitation.

**Parameters effect**:
   - For Simulated Annealing the cooling schedule (`alpha)` plays a crucial role. 
   - In Variable Neighborhood Search, the choice of neighborhood structures and their order greatly impacts the algorithm's performance more than on the other. 
   - Iterated Local Search's efficiency is more influenced by the perturbation strength, and the acceptance criteria for solutions.
   
The choice of the other parameters result in similar effects.

**Conclusion**
There isn't a universally "more efficient" algorithm among ILS, VNS, and SA. The choice depends on the problem characteristics, computational resources, and desired trade-offs between exploration and exploitation. Each algorithm has its pros and cons and require tuning of its specific parameters. 

| ILS                                  | VNS                                  | SA                                   |
| ------------------------------------ | ------------------------------------ | ------------------------------------ |
| ![Pasted image 20240405172224](Pasted%20image%2020240405172224.png) | ![Pasted image 20240405172333](Pasted%20image%2020240405172333.png) | ![Pasted image 20240405172425](Pasted%20image%2020240405172425.png) |

---