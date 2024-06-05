# Report Lab. 11 Integer Linear programming and Dynamic Programming

notebook : [optimtech_lab11](optimtech_lab11.ipynb)

**Table of Contents**

- [The N-queens Problem | ILP](#the-n-queens-problem--ilp)
- [Traveling Salesman Problem | ILP](#traveling-salesman-problem--ilp)
- [Knapsack Problem | DP](#knapsack-problem--dp)
- [Code Implementations](#code-implementations)
	- [The N-queens Problem | ILP](#the-n-queens-problem--ilp)
	- [Traveling Salesman Problem | ILP](#traveling-salesman-problem--ilp)
	- [Knapsack Problem | DP](#knapsack-problem--dp)


## The N-queens Problem | ILP

In order to see how the complexity increase with the increasing of the number of queens (`N`) I plot the different values of the time to solve the problem (`elapsed_time = time.time() - initial_time`) at the different stage. 

![Pasted image 20240524104116](img/Pasted%20image%2020240524104116.png)

We can see that the using more than 60 queens the elapsed time start increasing quite fast and over the 80 the algorithm start struggling a lot. 

## Traveling Salesman Problem | ILP
Here we can see some result of the Traveling Salesman Problem using Miller-Tucker-Zemlin subtour elimination method.

These example show the resulting path using different numbers of nodes. 

|                                                                         |                                                                             |
| ----------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| ![Pasted image 20240524155208](img/Pasted%20image%2020240524155208.png) | ![Pasted image 20240524155219](img/Pasted%20image%2020240524155219.png)<br> |
| ![Pasted image 20240524155234](img/Pasted%20image%2020240524155234.png) | ![Pasted image 20240524155246](img/Pasted%20image%2020240524155246.png)     |
| ![Pasted image 20240524155649](img/Pasted%20image%2020240524155649.png) | ![Pasted image 20240524155438](img/Pasted%20image%2020240524155438.png)     |


## Knapsack Problem | DP
Here we can see how the number of subproblems changes, changing the total capacity and the number of objects:

![Pasted image 20240524115823](img/Pasted%20image%2020240524115823.png)

- As the capacity of the knapsack increases, the number of subproblems increase since with a larger capacity there are more possible combinations of items to consider.
- Similarly, as the number of objects increases, the number of subproblems also tends to increase because again with more objects there are more possible combinations to consider.
- Lower numbers of subproblems suggest that the algorithm is able to prune the search space effectively, while higher numbers may indicate inefficiencies or redundancies.

--- 

## Code Implementations 
### The N-queens Problem | ILP
```python
def get_NQueens_model(N):
    model = Model("")
    v = {f"x_{i}{j}": model.addVar(vtype='i', name=f"x_{i}{j}",lb=0,ub=1) for i in range(1,N+1) for j in range(1,N+1)}

    # rows constraint
    for i in range(1, N+1):
        # for each row (i) the sum of columns (j) must be equal to 1
        model.addCons(quicksum(v[f"x_{i}{j}"] for j in range(1, N+1)) == 1) 

    # columns constraint
    for j in range(1, N+1):
        # for each column (j) the sum of rows (i) must be equal to 1
        model.addCons(quicksum(v[f"x_{i}{j}"] for i in range(1, N+1)) == 1) 
    
    # diagonals constraint
    # diagonal 1
    for k in range(2, 2*N+1):
        # no two queens are on the same major diagonal (top-left to bottom-right)
        model.addCons(quicksum(v[f"x_{i}{j}"] for i in range(1, N+1) for j in range(1, N+1) if i+j == k) <= 1)
    # diagonal 2
        # no two queens are on the same minor diagonal (top-right to bottom-left)
    for k in range(-N + 1, N):
        model.addCons(quicksum(v[f"x_{i}{j}"] for i in range(1, N+1) for j in range(1, N+1) if i-j == k) <= 1)
        
    # objective
    model.setObjective(quicksum(v[f"x_{i}{j}"] for i in range(1, N+1) for j in range(1, N+1)), "maximize")

    return model
```

### Traveling Salesman Problem | ILP
```python 
def get_TSP_model(matrix, n_nodes):
    model = Model("TSP")

    # variables
    x = {(i, j): model.addVar(vtype='B', name=f"e_{i}{j}") for i in range(1, n_nodes + 1) for j in range(1, n_nodes + 1)}
    u = {i: model.addVar(vtype='C', name=f"u_{i}") for i in range(2, n_nodes + 1)}

    # constraints
    # 1. Leave every point for exactly one successor
    for i in range(1, n_nodes + 1):
        model.addCons(quicksum(x[i, j] for j in range(1, n_nodes + 1) if j != i) == 1, f"Outgoing_{i}")
    # 2. Reach every point from exactly one predecessor
    for j in range(1, n_nodes + 1):
        model.addCons(quicksum(x[i, j] for j in range(1, n_nodes + 1) if j != i) == 1, f"Incoming_{i}")

    # 3. Miller-Tucker-Zemlin Subtour elimination
    for i in range(2, n_nodes + 1):
        for j in range(2, n_nodes + 1):
            if i != j:
                model.addCons(u[i] - u[j] + n_nodes * x[i, j] <= n_nodes - 1, f"MTZ_{i}_{j}")

    # Objective function
    model.setObjective(quicksum(matrix[i - 1][j - 1] * x[i, j] for i in range(1, n_nodes + 1) for j in range(1, n_nodes + 1) if i != j))

    return model
```

### Knapsack Problem | DP
```python
def dp_solution(problem):
  K = problem.getCapacity()
  n = problem.getItemsNumber()
  v = np.zeros((K+1, n+1))
  S = np.zeros((K+1, n+1))
  sub_problem = 0

  for i in range(1, n+1):
      for w in range(1, K+1):
          sub_problem += 1
          weight, value = problem.getWeightandValue(i-1)
          if weight <= w and value + v[w-weight, i-1] > v[w, i-1]:
              v[w, i] = value + v[w-weight, i-1]
              S[w, i] = 1
          else:
              v[w, i] = v[w, i-1]

  return v, v[-1][-1], sub_problem
```