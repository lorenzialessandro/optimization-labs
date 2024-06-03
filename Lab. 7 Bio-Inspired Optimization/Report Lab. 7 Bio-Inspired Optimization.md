
# Report Lab. 7 Bio-Inspired Optimization

notebook : [optimtech_lab7](optimtech_lab7.ipynb)

**Table of Contents**

- [Genetic algorithms](#genetic-algorithms)
	- [Mutation and Crossover](#mutation-and-crossover)
	- [Selective pressure](#selective-pressure)
	- [Benchmark functions](#benchmark-functions)
- [Evolutionary Strategies](#evolutionary-strategies)
	- [Number of offspring](#number-of-offspring)
	- [Mixing number](#mixing-number)
	- [Strategies](#strategies)
- [Particle Swarm Optimization](#particle-swarm-optimization)
	- [Comparison on benchmark functions](#comparison-on-benchmark-functions)
		- [Rosenbrock](#rosenbrock)
		- [Rastrigin](#rastrigin)
		- [Conclusion](#conclusion)
	- [Population size and generations](#population-size-and-generations)

## Genetic algorithms
### Mutation and Crossover
In general it's known that using mutation the search will favor more the exploration and, on the other side crossover is more related to explotation. 

In particular, with high mutation rates the search lead to more exploration of the search space but can also slow down convergence; with high crossover rates we are encouraging exploration combining different solutions from the population but it can fail with too premature convergence. 

Following the [Choosing Mutation and Crossover Ratios for Genetic Algorithms—A Review with a New Dynamic Approach](https://www.mdpi.com/2078-2489/10/12/390) I've set the mutation rate = 0.03 and the crossover rate = 0.9.

Here a simpler comparison using only mutation and only crossover, with `Ackley` as benchmark function: 

| only mutation                                                       | only crossover                                                      |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| ![Pasted image 20240419094539](img/Pasted%20image%2020240419094539.png) | ![Pasted image 20240419094553](img/Pasted%20image%2020240419094553.png) |
| ![Pasted image 20240419094627](img/Pasted%20image%2020240419094627.png) | ![Pasted image 20240419094611](img/Pasted%20image%2020240419094611.png) |

Here we can easily see that in this case the mutation run is better than the alone crossover in term of final generation and size of the final population but as sad, the number of iterations is bigger. 

The different behavior between exploitation and exploration is evident and for example in the crossover case the premature convergence has happened. 

We have instead a better result **combining** them, even if the final population seems similar to the ones produced by the mutation alone, the combined search process embeds a exploration - exploitation tradeoff more efficient. 

Here the mutation + crossover combination: 

| ![Pasted image 20240419094716](img/Pasted%20image%2020240419094716.png) | ![Pasted image 20240419094722](img/Pasted%20image%2020240419094722.png) |
| ------------------------------------ | ------------------------------------ |

Here an experiment fixing the mutation rate at `0.9` and changing over the crossover rate with various values: 

| 0.0001                               | 0.01                                 |
| ------------------------------------ | ------------------------------------ |
| ![Pasted image 20240419100113](img/Pasted%20image%2020240419100113.png) | ![Pasted image 20240419100010](img/Pasted%20image%2020240419100010.png) |
| **0.1**                              | **0.9**                              |
| ![Pasted image 20240419095756](img/Pasted%20image%2020240419095756.png) | ![Pasted image 20240419100047](img/Pasted%20image%2020240419100047.png) |

With higher crossover rate (last pic) the algorithm may find better solutions initially due to increased exploration, exploring a larger portion of the search space.
- ex with `0.1` at generation 4 we have a more "compact" population compares with rate `0.01`

However a too high ration can lead to premature convergence falling in suboptimal solutions. 

If we have lower crossover rate the algorithm will be slow initially because of limited exploration. (see generation 4 of pic with `0.0001`)
Furthermore it might also limit the algorithm's ability to find high-quality solutions but it help prevent premature convergence keeping the population diversity. 


### Selective pressure
With a small tournament size, only a few individuals compete in each tournament selection so we have more **exploitation**, favoring the selection of the best individuals in the population.

A large tournament size involves more individuals in each tournament selection so it promotes **exploration** by allowing more individuals to participate in the selection process.

| `tournament_size = 1`                                                   | `tournament_size = 5`                                                   |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| ![Pasted image 20240419101306](img/Pasted%20image%2020240419101306.png) | ![Pasted image 20240419101238](img/Pasted%20image%2020240419101238.png) |
| ![Pasted image 20240419101329](img/Pasted%20image%2020240419101329.png) | ![Pasted image 20240419101326](img/Pasted%20image%2020240419101326.png) |

The first run shows that with low size the process maintains genetic diversity in the population, here at the last generations we have a worst solution but it may be more effective in escaping local optima and exploring the search space extensively. 

In the second run with an higher selective pressure,the algorithm tends to converge quickly towards the compact solutions but there is a loss of the genetic diversity over generations, reducing the exploration capability of the algorithm. 


### Benchmark functions
Based on the properties of the benchmark functions used the genetic algorithm varies its performance. 

In particular :
- with **functions with one global optimum** GAs converge quickly to the global optimum without many problems
- with **functions with multiple local optima** it's more complex for GAs to explore and exploit the search space effectively
- with **high dimensional functions** having a larger search space the difficulty or GAs increase a lot

Here the comparison of GA performance between `Rastrigin` function (multimodal, non-separable, high-dimensional) and `Rosenbrock` (unimodal, separable, low-dimensional)

| `Rosenbrock`                                                            | `Rastrigin`                                                             |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| ![Pasted image 20240419103035](img/Pasted%20image%2020240419103035.png) | ![Pasted image 20240419102901](img/Pasted%20image%2020240419102901.png) |
| ![Pasted image 20240419103025](img/Pasted%20image%2020240419103025.png) | ![Pasted image 20240419102853](img/Pasted%20image%2020240419102853.png) |

---

## Evolutionary Strategies
### Number of offspring
The number of offspring generated at each iteration seems **influence a lot** the search process.

In general, higher value leads to increased **exploration** of the search space, since with more offspring there is a greater chance of exploring diverse regions of the solution space. Instead, with lower value there is more **exploitation** due to the attention around the current best solutions.

Higher values of $\lambda$ generally lead to faster convergence since more solutions are generated and evaluated simultaneously but it may also lead to premature convergence on local optima. 

Really important: the **computational cost** increase with the increase of this parameter: more offspring need to be generated and evaluated.

Here the proof of these concepts (the population has size = 20).

| `num_offspring = 20`                 | `num_offspring = 200`                |
| ------------------------------------ | ------------------------------------ |
| ![Pasted image 20240419103923](img/Pasted%20image%2020240419103923.png) | ![Pasted image 20240419103845](img/Pasted%20image%2020240419103845.png) |
| ![Pasted image 20240419103930](img/Pasted%20image%2020240419103930.png) | ![Pasted image 20240419103852](img/Pasted%20image%2020240419103852.png) |

### Mixing number
The mixing parameter represents the number of parents involved in the creation of each offspring, this means that higher value of it leads to more exploration of the search space (each offspring is generated from a larger pool of parents) allowing for more diverse combinations of genetic material. Conversely a lower value focused the process more on exploitation using a smaller subset of parents.

Again it's a matter of **explotation - exploration tradeoff**. 

Here an example:

| `mixing_number = 1`                  | `mixing_number = 20`                 |
| ------------------------------------ | ------------------------------------ |
| ![Pasted image 20240419104438](img/Pasted%20image%2020240419104438.png) | ![Pasted image 20240419104511](img/Pasted%20image%2020240419104511.png) |


### Strategies

| None                                 | GLOBAL                               | INDIVIDUAL                           |
| ------------------------------------ | ------------------------------------ | ------------------------------------ |
| ![Pasted image 20240419104957](img/Pasted%20image%2020240419104957.png) | ![Pasted image 20240419104920](img/Pasted%20image%2020240419104920.png) | ![Pasted image 20240419104939](img/Pasted%20image%2020240419104939.png) |
| ![Pasted image 20240419105005](img/Pasted%20image%2020240419105005.png) | ![Pasted image 20240419104925](img/Pasted%20image%2020240419104925.png) | ![Pasted image 20240419104943](img/Pasted%20image%2020240419104943.png) |

- `None`
	- static search process : exploration and exploitation behavior remains unchanged over iterations
	- in this case it struggle with complex landscapes where adaptive strategies would be beneficial
- `GLOBAL`
	- pressure on global exploration of the search space
	- quickly adapt to changes in the landscape and explore promising regions more effectively
- `INDIVIDUAL`
	- focuses on individual exploration 
	- different solutions explore diverse regions of the search space independently 
	- struggle to maintain population diversity

---

## Particle Swarm Optimization

### Comparison on benchmark functions 

In order to compare Particle Swarm Optimization (PSO) with with Genetic Algorithms (GA) and Evolution Strategies (ES) I lunched all the search process on 2 benchmark functions:
1.  `Rosenbrock` : unimodal, separable and low-dimensional function
2.  `Rastrigin` : multimodal, non-separable and high-dimensional function 

Using these two functions is possible to observe the different behavior using one simple function and ones more complex. 

#### Rosenbrock

| `PSO`                                | `GA`                                                                    | `ES`                                 |
| ------------------------------------ | ----------------------------------------------------------------------- | ------------------------------------ |
| ![Pasted image 20240419110202](img/Pasted%20image%2020240419110202.png) | ![Pasted image 20240419103025](img/Pasted%20image%2020240419103025.png) | ![Pasted image 20240419110255](img/Pasted%20image%2020240419110255.png) |
| ![Pasted image 20240419110210](img/Pasted%20image%2020240419110210.png) | ![Pasted image 20240419103035](img/Pasted%20image%2020240419103035.png) | ![Pasted image 20240419110306](img/Pasted%20image%2020240419110306.png) |
#### Rastrigin

| `PSO`                                                                   | `GA`                                                                    | `ES`                                                                    |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| ![Pasted image 20240419170132](img/Pasted%20image%2020240419170132.png) | ![Pasted image 20240419110430](img/Pasted%20image%2020240419110430.png) | ![Pasted image 20240419110508](img/Pasted%20image%2020240419110508.png) |
| ![Pasted image 20240419170139](img/Pasted%20image%2020240419170139.png) | ![Pasted image 20240419110447](img/Pasted%20image%2020240419110447.png) | ![Pasted image 20240419110521](img/Pasted%20image%2020240419110521.png) |

#### Conclusion 
**PSO** is effective in finding global optima in smooth and convex fitness landscapes like the Rosenbrock function, in this case it is the most computationally efficient and the easier to implement. However, even with this function keeping population diversity can be challenging in PSO.
**GA** explore and exploit solutions effectively, it maintains population diversity but it require high number of generations to converge, making it computationally expensive. 
**ES** seems well-suited for continuous optimization problems with complex fitness landscapes like the Rosenbrock function but it require more computational resources compared to PSO and GA.

**PSO** struggle more with multimodal functions like Rastrigin due to premature convergence or insufficient exploration of the search space and again maintaining population diversity can be challenging in PSO, as we can see.
**GA** approach allows it to explore a wide range of solutions simultaneously, making it suitable for multimodal functions like Rastrigin and it is effective in maintaining population diversity. In terms of efficiency it seems the best. 
**ES** efficiently explore and exploit multiple regions of the search space simultaneously, leading to robust performance in multimodal functions and reaching the better solution, but it require more computational resources compared to PSO and GA

### Population size and generations

Here an example with `Ackley` showing different variations of  `pop_size` / `max_generations`:

| 125/20                                                              | 50/50                                                               | 20/125                                                              |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| ![Pasted image 20240419170836](img/Pasted%20image%2020240419170836.png) | ![Pasted image 20240419170525](img/Pasted%20image%2020240419170525.png) | ![Pasted image 20240419170803](img/Pasted%20image%2020240419170803.png) |
| ![Pasted image 20240419170840](img/Pasted%20image%2020240419170840.png) | ![Pasted image 20240419170529](img/Pasted%20image%2020240419170529.png) | ![Pasted image 20240419170808](img/Pasted%20image%2020240419170808.png) |

With other benchmark functions is possible to see different result so the optimal choice depends on the specific problem. One thing that we can say is that the **the adjustment of the population size have a more significant impact** on the algorithm's behavior. 

In general a larger population size allows for more diverse exploration of the solution space and that's really important for PSO to escape local optima.  More iterations can help particles to refine their movements, especially in complex and multi-modal optimization landscapes.


--- 
