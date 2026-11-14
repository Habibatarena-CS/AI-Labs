# Lab 04: Hill-Climbing Search with the N-Queens Problem

**Implementation of Basic Hill-Climbing and Random-Restart Hill-Climbing algorithms for solving the N-Queens problem.**

## 1. Lab Objectives

The main purpose of this lab is to implement Hill-Climbing and Random-Restart Hill-Climbing in Python and use them to solve the N-Queens problem.

By completing this practical, I learned how to represent a search state, calculate its evaluation score, and generate neighboring states. I also explored how Hill-Climbing improves a current state, why it may stop at a local maximum, and how random restarts provide further chances of finding a valid solution.

## 2. Background: Local Search and Hill-Climbing

Traditional search methods such as Breadth-First Search (BFS), Depth-First Search (DFS), and A* explore possible states to find a path from the starting state to the goal.

Local Search works differently. Instead of keeping track of a complete path, it concentrates on improving the current state.

Hill-Climbing is a greedy local search algorithm. It checks neighboring states and moves to a better one when its evaluation score is higher.

**Main steps of Hill-Climbing:**

1. Begin with an initial state.
2. Calculate its evaluation score.
3. Generate the neighboring states.
4. Select the neighbor with the highest score.
5. Move to it only if it improves the current score.
6. Stop when no better neighbor is available.

Hill-Climbing can reach a valid solution, but it may also stop at a local maximum that is not the goal. Therefore, success is not guaranteed from every initial state.

## 3. The N-Queens Problem

The N-Queens problem involves placing N queens on an N × N chessboard so that no two queens attack one another.

A queen can attack another queen if both occupy:

- The same row.
- The same column.
- The same diagonal.

A valid arrangement must have zero attacking pairs.

**Example: A valid arrangement for four queens**

```text
. Q . .
. . . Q
Q . . .
. . Q .
```

In this arrangement, no two queens attack each other.

## 4. Representing a State

For this practical, the chessboard is represented through a Python list.

**Example:**

```python
state = [1, 3, 0, 2]
```

In this representation:

- Each index identifies a column.
- The value at that index identifies the queen's row.
- Every list element represents one queen.

This method automatically places exactly one queen in each column, so column conflicts cannot occur.

**Task 1 — State Representation**

A list was created to store the queen positions and then printed to check the state representation.

## 5. Generating a Random Initial State

The function `random_state(n)` generates a starting arrangement by selecting a random row for each column.

It creates a list containing N positions, with one queen assigned to every column.

Since the row positions are selected randomly, running the function again can produce a different initial arrangement.

**Task 2 — Random Initial State**

Generated and displayed a random initial state for the eight-queen problem.

## 6. Defining the Evaluation Function

The evaluation function checks the quality of a state by counting the pairs of queens that attack each other.

Our goal is to reduce the number of conflicts to zero.

For Hill-Climbing, we use a negative conflict count as the evaluation score:

**Evaluation(state) = −count_conflicts(state)**

| Conflicts | Evaluation Score |
|---|---|
| 5 | -5 |
| 3 | -3 |
| 1 | -1 |
| 0 | 0 |

A higher score indicates a better arrangement. Thus, zero is the highest possible score and represents a valid solution.

**Task 3 — Conflict Counting**

Implemented `count_conflicts(state)` to identify:

- Queens in the same row.
- Queens on the same diagonal.

Since each column already contains exactly one queen, column conflicts are avoided by the state representation itself.

## 7. Generating Neighboring States

A neighboring state is produced by making one small change to the current arrangement.

In this implementation, one queen is moved to a different row in its existing column. All other queen positions remain unchanged.

The total number of neighbors for a board of size N is:

**N × (N − 1)**

For an eight-queen board:

**8 × 7 = 56 neighbors**

**Task 4 — Neighbor Generation**

Implemented `get_neighbors(state)` to generate every possible state obtained by moving one queen to another row in the same column.

## 8. Implementing Basic Hill-Climbing

The function `hill_climbing(n)` combines the state representation, evaluation function, and neighbor-generation function.

Its working procedure is:

1. Select a random initial arrangement.
2. Calculate the score of the current state.
3. Generate all possible neighboring states.
4. Find the neighbor with the highest evaluation score.
5. Move to that neighbor if its score is higher than the current score.
6. Stop if no neighbor provides an improvement.

The function also stores the evaluation scores during the search. These values are later used to display the progress in a graph.

## 9. Running Hill-Climbing

Basic Hill-Climbing was executed for the eight-queen problem.

The final arrangement, conflict count, evaluation score, and number of iterations were examined.

There are two possible outcomes:

- **Solved:** The final conflict count is zero.
- **Not Solved:** The algorithm stops while some conflicts remain because no better neighboring state is available.

This second outcome is related to the local maximum problem. A valid arrangement may exist elsewhere, but the algorithm may not reach it from its current starting state.

## 10. Visualizing the Chessboard

The function `print_board(state)` was used to display the queen arrangement in chessboard form.

The following symbols represent the board:

- `Q` — Queen.
- `.` — Empty position.

This visualization makes it easier to inspect the final state. If a valid solution is found, no two queens should attack each other.

## 11. Plotting the Evaluation Score

Matplotlib was used to draw a graph of the evaluation score across the iterations of Hill-Climbing.

This graph helps us understand how the algorithm changes the current state and when it stops improving.

### Questions and Answers

**Q1. Does the evaluation score increase when Hill-Climbing moves to a better state?**

Yes. Whenever the algorithm selects a better neighboring state, its evaluation score increases.

**Q2. Can the score decrease during an accepted move?**

No. In this implementation, a move is accepted only when it improves the current evaluation score.

**Q3. Why does the algorithm stop when it cannot find a better neighbor?**

It stops because none of the neighboring states has a higher score than the current state.

**Q4. What does an evaluation score of zero indicate?**

It indicates that there are no conflicts between any pair of queens. Therefore, a valid solution has been found.

**Q5. Can Hill-Climbing stop while its evaluation score is negative?**

Yes. If none of the neighboring states improves the current score, the algorithm can stop with a negative score. This means it has reached a local maximum that is not a valid solution.

## 12. Experimenting with Different Board Sizes

Basic Hill-Climbing was tested with different board sizes: **N = 4, 8, 10, and 12**.

Multiple runs were performed to observe how the initial random arrangement affects the final result.

### Experimental Results

| Board Size (N) | Run | Final Conflicts | Iterations | Solved? |
|---|---|---|---|---|
| 4 | 1 | 1 | 2 | No |
| 4 | 2 | 0 | 3 | Yes |
| 8 | 1 | 0 | 6 | Yes |
| 8 | 2 | 2 | 3 | No |
| 8 | 3 | 1 | 3 | No |
| 10 | 1 | 1 | 3 | No |
| 10 | 2 | 1 | 7 | No |
| 10 | 3 | 2 | 4 | No |

**Observation:**

The same board size can give different results in separate runs. This happens because each run may begin with a different random arrangement. As a result, the final conflicts and number of iterations can also change.

## 13. Improving the Search: Random-Restart Hill-Climbing

Basic Hill-Climbing begins with one random state. If it reaches a non-goal local maximum, that particular search ends without finding a valid solution.

Random-Restart Hill-Climbing improves this approach by starting new searches from different random initial states.

**Working procedure:**

1. Generate a random initial state.
2. Execute Basic Hill-Climbing.
3. Check whether the result is a valid solution.
4. If the solution is found, stop the search.
5. If not, generate another random initial state.
6. Repeat until a solution is found or the restart limit is reached.

### Probabilistic Completeness

Probabilistic completeness means that, under suitable conditions, the probability of finding a solution approaches 100% as the number of independent attempts increases.

In simple words, giving the algorithm more independent attempts can improve its chance of success.

However, this does not mean that it must find a solution within a fixed number of restarts.

If each independent attempt has a nonzero probability of success, the probability of finding at least one solution after k attempts is:

**P(success) = 1 − (1 − p)ᵏ**

Here, p represents the probability of success in one attempt.

This explains why repeated attempts can improve the chance of finding a solution. It does not guarantee success within a limited number of attempts.

## 14. Implementing Random-Restart Hill-Climbing

The function `random_restart_hill_climbing(n, max_restarts=100)` was implemented to run Hill-Climbing repeatedly.

During these attempts, the algorithm keeps the best state and its evaluation score.

The search ends in either of these situations:

- A valid solution is found.
- The maximum number of restarts is reached.

If the restart limit is reached without finding a solution, the algorithm returns the best state found during its attempts.

## 15. Plotting Random-Restart Progress

Matplotlib was used to plot the best evaluation score recorded after each restart.

This graph differs from the graph of Basic Hill-Climbing. Instead of showing the progress of one search, it shows the best result retained across multiple searches.

The score cannot decrease because the algorithm keeps the best state found so far.

When the score reaches zero, a valid solution has been obtained.

## 16. Comparing Basic and Random-Restart Hill-Climbing

Both algorithms were tested five times using an eight-queen board.

### Experimental Results

| Run | Basic Conflicts | Basic Iterations | Basic Solved? | Random-Restart Conflicts | Restarts | Solved? |
|---|---|---|---|---|---|---|
| 1 | 1 | 6 | No | 0 | 7 | Yes |
| 2 | 1 | 5 | No | 0 | 7 | Yes |
| 3 | 0 | 4 | Yes | 0 | 1 | Yes |
| 4 | 2 | 4 | No | 0 | 8 | Yes |
| 5 | 2 | 3 | No | 0 | 7 | Yes |

**Observation:**

In these experiments, Basic Hill-Climbing found a valid solution in only one out of five runs. Random-Restart, however, found a solution in all five runs.

This shows how trying different initial states can provide additional opportunities to find a valid arrangement.

### Comparison of Both Algorithms

| Basic Hill-Climbing | Random-Restart Hill-Climbing |
|---|---|
| Begins with one initial state. | Tries multiple random initial states. |
| Stops when no better neighbor is available. | Can begin another search after an unsuccessful run. |
| May stop at a non-goal local maximum. | Provides more opportunities to find a solution. |
| Usually performs one search attempt. | May need several restarts. |
| Does not guarantee a solution. | Improves the probability of finding a solution. |

### Local Search and Traditional Search

Traditional search algorithms such as BFS, DFS, and A* explore possible states to find a path from the initial state to the goal.

Local Search focuses on improving the current state instead of finding a complete path. Hill-Climbing belongs to this category because it repeatedly selects a better neighboring state.

## 17. Lab Questions

**Q1. What is the purpose of the evaluation function in Hill-Climbing?**

The evaluation function measures the quality of a state. In this problem, it counts the attacking pairs of queens and returns the negative conflict count. This helps the algorithm compare neighboring states and select a better one.

**Q2. Why is an evaluation score of zero the best result?**

A score of zero means that no two queens are attacking each other. Therefore, the N-Queens problem is solved.

**Q3. What is the difference between a local maximum and a global maximum?**

A local maximum is a state whose score is at least as high as the scores of its immediate neighbors. A global maximum is the highest possible score in the entire search space.

**Q4. Why can Basic Hill-Climbing stop without finding a solution even when a solution exists?**

Basic Hill-Climbing stops when none of the neighboring states improves the current score. A valid solution may exist elsewhere, but reaching it may require temporarily moving to a worse state.

**Q5. Does reaching a local maximum always mean that the algorithm has failed?**

No. If the local maximum is also the global maximum and represents the goal, the algorithm has succeeded. In this problem, a score of zero means success. A negative local maximum means that the algorithm stopped without finding a solution.

**Q6. For N = 8, how many neighbors does each state have in our implementation?**

Each queen can move to seven alternative rows in its column.

8 × (8 − 1) = 8 × 7 = **56 neighbors**.

**Q7. What information does the evaluation-score graph provide?**

The graph shows how the score changes during the search. It increases when the algorithm accepts a better move and stops changing when no better neighbor is available. A score of zero indicates that a solution has been found.

**Q8. Explain probabilistic completeness in your own words.**

Probabilistic completeness means that if a solution exists and each independent attempt has a nonzero probability of finding it, the chance of success approaches 100% as the number of attempts increases.

**Q9. Why does Random-Restart improve the chance of finding a solution?**

It uses different random initial states. This gives the algorithm more opportunities to find a solution instead of stopping at an unsuccessful local maximum.

**Q10. Does probabilistic completeness guarantee that a solution will be found within 100 restarts? Why?**

No. The probability of success increases as the number of attempts grows, but a fixed limit of 100 restarts can still end without finding a solution.

## 18. Extension Task: N = 20

In this extension, the same N-Queens experiment was performed with 20 queens.

Both Basic Hill-Climbing and Random-Restart Hill-Climbing were executed. The final conflicts, number of iterations or restarts, and solution status were recorded.

### Cell 1 — Basic Hill-Climbing for N = 20

```python
state_20, scores_20 = hill_climbing(20)

conflicts_20 = count_conflicts(state_20)

print("Basic Hill-Climbing for N = 20")
print("Final state:", state_20)
print("Final conflicts:", conflicts_20)
print("Iterations:", len(scores_20))
print("Solved:", conflicts_20 == 0)
```

**Sample Output:**

```text
Basic Hill-Climbing for N = 20
Final state: [1, 10, 16, 16, 3, 12, 0, 19, 6, 13, 5, 17, 15, 18, 14, 7, 9, 19, 2, 4]
Final conflicts: 4
Iterations: 8
Solved: False
```

**Basic Hill-Climbing Results:**

- Final conflicts: 4
- Number of iterations: 8
- Solution found: No

### Cell 2 — Random-Restart Hill-Climbing for N = 20

```python
solution_20, restarts_20, best_scores_20 = (
    random_restart_hill_climbing(20, max_restarts=100)
)

conflicts_rr_20 = count_conflicts(solution_20)

print("Random-Restart Hill-Climbing for N = 20")
print("Final state:", solution_20)
print("Final conflicts:", conflicts_rr_20)
print("Restarts:", restarts_20)
print("Solved:", conflicts_rr_20 == 0)
```

**Sample Output:**

```text
Random-Restart Hill-Climbing for N = 20
Final state: [8, 10, 13, 17, 3, 7, 19, 2, 14, 5, 11, 18, 1, 6, 12, 9, 16, 0, 15, 4]
Final conflicts: 0
Restarts: 45
Solved: True
```

**Random-Restart Hill-Climbing Results:**

- Final conflicts: 0
- Number of restarts: 45
- Solution found: Yes

### Comparison and Observation

In this sample execution, Basic Hill-Climbing stopped with four conflicts remaining. It could not find a better neighboring state from its final arrangement.

Random-Restart Hill-Climbing tried new initial states and found a valid arrangement after 45 restarts.

This experiment shows that random restarts can provide additional opportunities to solve a larger N-Queens problem. The exact number of iterations and restarts may change between executions because the starting states are random.

## 19. Final Learning Outcomes

After completing this practical, I learned how to:

- Describe the difference between traditional search and local search.
- Represent the N-Queens problem using a Python list.
- Create an evaluation function and count queen conflicts.
- Generate neighboring states by moving one queen.
- Implement Basic Hill-Climbing in Python.
- Display the chessboard and plot evaluation scores.
- Understand why Hill-Climbing may stop at a non-goal local maximum.
- Implement Random-Restart Hill-Climbing.
- Explain probabilistic completeness and its conditions.
- Compare both algorithms through repeated experiments.

**Key Takeaway**

Hill-Climbing improves the current arrangement by moving towards a better neighboring state. It may find a valid solution, but it can also stop at a non-goal local maximum. Random-Restart improves the chances of success by trying different initial states, although it cannot guarantee a solution within a fixed restart limit.
