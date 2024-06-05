# Report Lab. 9 Design of Experiments

notebook : [optimtech_lab9](optimtech_lab9.ipynb)

**Table of Contents**

- [Sampling methods](#sampling-methods)
	- [Comments:](#comments)
	- [Another Experiment](#another-experiment)
- [Usage of DOEs](#usage-of-does)

## Sampling methods

Here the comparison between some different sampling methods applied to the same Ackley functions.
In particular, keeping the some parameters, I've reported the result of:
- Random sampling 
- Halton sequence
- Latin Hypercube sampling
- Full factorial sampling

| Random sampling                                                            | Halton sequence                                                              |
| -------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| ![Pasted image 20240510132329](img/Pasted%20image%2020240510132329.png)        | ![Pasted image 20240510132243](img/Pasted%20image%2020240510132243.png)          |
| Best Solution: [0.01303324 0.29914853]<br>Best Fitness: 2.1336081842324046 | Best Solution: [ 0.03564636 -0.22956562]<br>Best Fitness: 1.6287858045192576 |

| Latin Hypercube sampling                                                     | Full factorial sampling                                                      |
| ---------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| ![Pasted image 20240510155049](img/Pasted%20image%2020240510155049.png)      | ![Pasted image 20240510163621](img/Pasted%20image%2020240510163621.png)      |
| Best Solution: [-0.0677247  -0.02234126]<br>Best Fitness: 0.3319926063615566 | Best Solution: [ 0.10907822 -0.08808136]<br>Best Fitness: 0.8574829855404009 |
*Note: The result are taken using 30 iterations for each run*

These results suggest that for the Ackley function, which is multimodal with many local minima, methods like **Halton sequence and Full Factorial sampling may be more effective in finding optimal solutions**.

![Pasted image 20240510172640](img/Pasted%20image%2020240510172640.png)

### Comments:


The **random sampling** has the problem that we have no "control" on the space explored region, in fact sometimes it miss important regions of the search space and so the efficiency is low.

Here a simple example of the "instability" of this sampling methods:

| run 1                                | run 2                                |
| ------------------------------------ | ------------------------------------ |
| ![Pasted image 20240510165227](img/Pasted%20image%2020240510165227.png) | ![Pasted image 20240510165212](img/Pasted%20image%2020240510165212.png) |
Is evident that the behavioral change a lot in exploring the space, sometimes effectively and other not.  

---

The **Latin Hypercube sampling** appears to perform better than Random sampling but worse than Halton sequence and Full Factorial sampling in this case. This is because it struggle with highly non-linear or discontinuous functions like Ackley. 

---

Regarding the **Fully Factorial Sampling** it guarantees that all regions of the search space are explored. This work well in our experiment since there is a small number of dimensions. In general it's expensive and inefficient for high-dimensional problems.

---

Finally, since **Halton sequence** uses a deterministic, quasi-random sequence that fills the space more uniformly than random sampling, it has better exploration capabilities in this particular problem domain

---

### Another Experiment
In order to introduce also another benchmark function, I've also use the **Rastrigin function**, that is a non-convex function with numerous local minima. 

The conclusions are similar:
- **Random sampling**: quality of the solutions may vary widely, with some close to the global minimum and others far from it
- **Halton Sequence**: more systematic exploration of the search space compared to random sampling, finding solutions with lower fitness values
- **Latin Hypercube Sampling**: perform better in smoother regions, for this problem has slightly worst performance than Halton, in general
- **Full Factorial Sampling**: provide a comprehensive exploration of the search space by systematically evaluating all possible combinations of input variables, finding good result for this problem 

![Pasted image 20240510174016](img/Pasted%20image%2020240510174016.png)

| Random sampling                      | Halton sequence                      |
| ------------------------------------ | ------------------------------------ |
| ![Pasted image 20240605183344](img/Pasted%20image%2020240605183344.png) | ![Pasted image 20240605183326](img/Pasted%20image%2020240605183326.png) |

| Latin Hypercube sampling             | Full factorial sampling              |
| ------------------------------------ | ------------------------------------ |
| ![Pasted image 20240605183314](img/Pasted%20image%2020240605183314.png) | ![Pasted image 20240605183301](img/Pasted%20image%2020240605183301.png) |


--- 

## Usage of DOEs

Sampling methods that provide a more even coverage of the search space, such as Halton Sequence and Latin Hypercube Sampling, can help algorithms **converge faster**, since these methods reduce the likelihood of getting stuck in local optima and guide the optimization process towards promising regions.

Using sampling methods can lead to the **discovery of better solutions**, since they increase the chances of identifying the global optimum or high-quality solutions.

So, in general these methods **facilitate more effective exploration of the search space**, enabling optimization algorithms to identify and exploit promising regions more efficiently.

I report here the the Genetic Algorithms (GA) with and without the use of DOEs  methods on the **Rastrigin** function.

| GA without DOEs                                                         | GA with Halton sequence                                                 | GA with Latin Hypercube sampling                                        |
| ----------------------------------------------------------------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| ![Pasted image 20240510180802](img/Pasted%20image%2020240510180802.png) | ![Pasted image 20240510181124](img/Pasted%20image%2020240510181124.png) | ![Pasted image 20240510181231](img/Pasted%20image%2020240510181231.png) |
| Best Fitness: <br>1.2555                                                | Best Fitness: <br>0.4293                                                | Best Fitness: <br>0.0068                                                |

The advantages of using sampling methods are clear and respect what sad before.  For this experiment **DOEs methods always produce better result than not using them**, both in terms of better solutions found and in convergence speed.

Halton Sequence and Latin Hypercube Sampling perform well due to their ability to provide a more uniform coverage of the search space and Genetic Algorithms (GA), Particle Swarm Optimization (PSO), Evolution Strategies (ES) may benefit from these sampling methods as they tend to explore the search space more systematically.

The impact of Design of Experiments on search cost can vary depending on several factors, including the problem's complexity, dimensionality, and the effectiveness of the optimization algorithm, as always. There's **no one-size-fits-all approach**, and the choice of sampling method should be based on the specific characteristics and requirements of the optimization problem