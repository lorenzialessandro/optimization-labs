# Report Lab. 8 Multi-Objective Optimization

notebook : [optimtech_lab8](optimtech_lab8.ipynb)

**Table of Contents**

- [Weighted sum of the objectives](#weighted-sum-of-the-objectives)
- [NSGA-II](#nsga-ii)
	- [Kursawe](#kursawe)
	- [Multi-disk clutch brake optimization](#multi-disk-clutch-brake-optimization)
- [GA vs NSGA-II](#ga-vs-nsga-ii)


### Weighted sum of the objectives
In multi-objective optimization adjusting the weights allows to **balance the importance of each objective**. In general, higher weights prioritize one objective over others, potentially leading to solutions that excel in that objective but may sacrifice performance in others.

Changing the weights can influence the **exploration-exploitation trade-off**: higher weights on certain objectives can lead the GA to focus more on exploiting regions of the search space that optimize those objectives, potentially neglecting other areas that might offer better overall solutions.

In terms of **convergence**, higher weights on certain objectives may lead to faster convergence toward solutions that optimize those objectives, while lower weights may result in a more diverse population and slower convergence.

Following I display **different weighs configurations** in order to emphasize the effect on the solutions, but also on the convergence and generations. The run is done using `run_ga` (simple Genetic Algorithm) on `Kursawe` problem. 

This first example show the use of equal weights for both objective, it may lead to solutions that are balanced in optimizing both objectives:

| Best Solution | [3.933, 5.0, 5.0] |
| ------------- | ----------------- |
| Best Fitness  | 5.246733462298993 |

![Pasted image 20240503103807](img/Pasted%20image%2020240503103807.png)

Here we start to analyze some configurations pushing more one objective function on the other in different ways. 

| Configuration: | Emphasize the first objective `[2, 1]`                                  | Emphasize the second objective `[1, 2]`                                 |
| -------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| Best Solution  | [5.0, 5.0, 5.0]                                                         | [3.574, 4.757, 5.0]                                                     |
| Best Fitness   | 10.005708135263578                                                      | 5.561420877894748                                                       |
|                | ![Pasted image 20240503104625](img/Pasted%20image%2020240503104625.png) | ![Pasted image 20240503104647](img/Pasted%20image%2020240503104647.png) |


| Configuration: | Emphasize the first, de-emphasize the second `[3, 0.5]`                 | Emphasize the second, de-emphasize the first `[0.5, 3]`                 |
| -------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| Best Solution: | [-5.0, 5.0, 5.0]                                                        | [-3.333, 4.786, 5.0]                                                    |
| Best Fitness:  | 14.94064446249574                                                       | 3..068164516783732                                                      |
|                | ![Pasted image 20240503104151](img/Pasted%20image%2020240503104151.png) | ![Pasted image 20240503104231](img/Pasted%20image%2020240503104231.png) |

| Configuration: | Ignore objective 2 `[1,0]`           | Ignore objective 1 `[0,1]`           |
| -------------- | ------------------------------------ | ------------------------------------ |
| Best Solution: | [-5.0, 5.0, -5.0]                    | [-1.165, -0.144, 3.228]              |
| Best Fitness:  | 4.8623346886842835                   | 0.0016965928319208                   |
|                | ![Pasted image 20240503105104](img/Pasted%20image%2020240503105104.png) | ![Pasted image 20240503105131](img/Pasted%20image%2020240503105131.png) |

These configurations demonstrate how adjusting the weights assigned to each objective can influence the optimization process and the characteristics of the obtained solutions:

-  **Emphasize the first (second) objective**: the algorithm tends to prioritize improving the first (second) objective, leading to a solution where all objectives are relatively high
- **Emphasize the first, de-emphasize the second**: leads to a solution where the first objective is maximized while the second is minimized (viceversa)
- **Ignore objective 1 (2)**: solution where the second (first) objective is minimized

### NSGA-II

#### Kursawe
For the `Kursawe` problem, applying the NSGA-II algorithm we obtain:

| Pareto front                                                            | All populations                                                         |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| ![Pasted image 20240503105804](img/Pasted%20image%2020240503105804.png) | ![Pasted image 20240503105810](img/Pasted%20image%2020240503105810.png) |

| Convergence Curve                                                       | Result                                                                           |
| ----------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| ![Pasted image 20240503110247](img/Pasted%20image%2020240503110247.png) | Best Solution: [-1.506, -1.526, -1.439] <br><br>Best Fitness: [-13.085, -10.697] |

#### Multi-disk clutch brake optimization
For the `Multi-disk clutch brake optimization` problem, applying the NSGA-II algorithm we obtain:

| Pareto front                                                            | All populations                                                         |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| ![Pasted image 20240503190723](img/Pasted%20image%2020240503190723.png) | ![Pasted image 20240503190728](img/Pasted%20image%2020240503190728.png) |

| Convergence Curve                    | Result                                                                     |
| ------------------------------------ | -------------------------------------------------------------------------- |
| ![Pasted image 20240503191157](img/Pasted%20image%2020240503191157.png) | Best Solution: [80, 90, 1.5] <br><br>Best Fitness: [0.187, 16.344]<br><br> |


We can observe that the **GA algorithm does not capture the diversity of solutions along the Pareto front like NSGA-II does**. It tends to converge to a single solution, potentially missing out on other promising solutions that represent different trade-offs between conflicting objective.

In both cases, **NSGA-II demonstrated its effectiveness in multi-objective optimization** by discovering diverse sets of non-dominated solutions representing various trade-offs between conflicting objectives.

### GA vs NSGA-II

Here the results of applying GA and NSGA-II algorithms on the same `Kursawe` problem:

|                | GA algorithm      | NSGA-II algorithm        |
| -------------- | ----------------- | ------------------------ |
| Best Solution: | [5.0, 5.0, 3.934] | [-1.506, -1.526, -1.439] |
| Best Fitness:  | 5.2467342894      | [-13.085, -10.697]       |

Here the results of applying GA and NSGA-II algorithms on the same `Multi-disk clutch brake optimization` problem:

|                | GA algorithm          | NSGA-II algorithm |
| -------------- | --------------------- | ----------------- |
| Best Solution: | [80, 97, 1.5, 960, 9] | [80, 90, 1.5]     |
| Best Fitness:  | 3.8862247407068       | [0.187, 16.344]   |

* *Note: GA runs with equal weight configuration.* 

We can observe that in general both the Kursawe and Multi-disk clutch brake optimization problems, **NSGA2 outperforms the simple GA**. Its ability to handle multiple objectives and maintain diversity in the solution set makes it a better choice for these types of optimization tasks.

GA is in fact not specifically designed for multi-objective optimization. It might converge to a single solution, loosing the diversity of the Pareto front.


NSGA-II is well suitable for both the problem, efficiently exploring the Pareto front and providing a diverse set of non-dominated solutions that capture the trade-offs between conflicting objectives in each problem.

**NSGA-II often converges faster than a standard GA** in multi-objective optimization problems due to its ability to maintain a diverse set of non-dominated solutions

---

