# Lab 03:INFORMED SEARCH

## A* SEARCH FOR THE 8-PUZZLE: HEURISTIC IMPLEMENTATION AND PERFORMANCE ANALYSIS

---

## 1. Learning Objectives

The main objectives of this practical were:

- To understand the state-space representation of the 8-puzzle problem.
- To implement the Misplaced Tiles and Manhattan Distance heuristics.
- To complete the A* search algorithm using a priority queue.
- To verify puzzle solvability and generate test states.
- To compare the performance of both heuristics using the number of expanded nodes.

---

## 2. State-Space Formulation of the 8-Puzzle

The 8-puzzle consists of a 3×3 board containing eight numbered tiles and one blank space. The objective is to move the tiles until the board reaches the required goal configuration.

### Goal State

```python
GOAL_STATE = (1, 2, 3, 4, 5, 6, 7, 8, 0)
```

The puzzle state is represented as a tuple of nine integers. The value `0` represents the blank space.

### Main Components of the Problem

| Component | Description |
|---|---|
| Initial State | Starting arrangement of the puzzle |
| Goal State | `(1, 2, 3, 4, 5, 6, 7, 8, 0)` |
| Actions | Up, Down, Left, Right |
| Blank Space | Represented by 0 |
| State Transition | Swapping the blank space with an adjacent tile |

### Movement Constraints

The blank space can move only within the board boundaries.

| Movement | Condition |
|---|---|
| Up | Blank moves one position upward |
| Down | Blank moves one position downward |
| Left | Blank moves one position left |
| Right | Blank moves one position right |

### Completed Helper Functions

The following helper functions were completed:

- `get_blank_position(state)` identifies the position of the blank space.
- `get_neighbors(state)` generates all valid neighboring states.
- `is_goal(state)` checks whether the current state matches the goal state.

### Boundary Verification

The goal state's blank space is located at the bottom-right corner. Therefore, only two movements are valid: Up and Left.

The test confirmed that exactly two neighboring states were generated.

---

## 3. Heuristic Design and Implementation

A heuristic estimates the remaining cost from the current state to the goal state. Two admissible heuristics were implemented and tested.

### A. Misplaced Tiles Heuristic

This heuristic counts the number of numbered tiles that are not in their correct positions. The blank space is excluded from the calculation.

```python
def misplaced_tiles(state):
    return sum(
        1 for i in range(9)
        if state[i] != 0 and state[i] != GOAL_STATE[i]
    )
```

### B. Manhattan Distance Heuristic

This heuristic calculates the total horizontal and vertical distance of each numbered tile from its correct goal position.

```python
def manhattan_distance(state):
    distance = 0

    for i, tile in enumerate(state):
        if tile != 0:
            goal_index = GOAL_STATE.index(tile)

            current_row, current_col = divmod(i, 3)
            goal_row, goal_col = divmod(goal_index, 3)

            distance += (
                abs(current_row - goal_row)
                + abs(current_col - goal_col)
            )

    return distance
```

### Heuristic Verification

The test state was:

```python
test_state = (1, 2, 3, 4, 5, 6, 7, 0, 8)
```

Both heuristics returned `1` for this state because only one numbered tile was out of position by one move.

For the goal state, both heuristics returned `0`.

Both heuristics are admissible because they never overestimate the actual minimum cost required to reach the goal.

---

## 4. Completion of the A* Search Algorithm

The A* algorithm selects the most promising state by calculating its total estimated cost:

$$
f(n) = g(n) + h(n)
$$

Where:

- `g(n)` = Actual cost from the initial state to the current state.
- `h(n)` = Estimated cost from the current state to the goal.
- `f(n)` = Total estimated cost.

### Completed Operations

The incomplete parts of the A* algorithm were completed using the following operations:

1. Skip outdated entries from the priority queue.
2. Expand the current state and generate its neighboring states.
3. Calculate the new path cost for each neighbor.
4. Update the best known cost when a shorter path is found.
5. Calculate the priority using `g(n) + h(n)`.
6. Insert the updated state into the priority queue.

A tie-breaker was also used in the priority queue to handle states having equal priority values.

### Algorithm Output

The algorithm returns:

- The solution path.
- The total solution cost.
- The number of expanded nodes.

---

## 5. Solvability Verification and Test-State Generation

Before applying A*, the puzzle's solvability was checked using the inversion count method.

An inversion occurs when a larger numbered tile appears before a smaller numbered tile, ignoring the blank space.

### Solvability Rule

For a 3×3 puzzle:

- Even inversion count means the puzzle is solvable.
- Odd inversion count means the puzzle is not solvable.

The following assertion was used to verify generated states:

```python
assert is_solvable(start), "Generated puzzle is not solvable"
```

### Generating Test States

The `generate_scramble()` function was used to generate random puzzle states by starting from the goal state and performing valid random moves.

Four scramble depths were selected for testing:

- 5 moves
- 10 moves
- 15 moves
- 20 moves

A scramble depth represents the number of random moves used to generate a state. It does not necessarily represent the shortest solution distance because some random moves may undo previous moves.

---

## 6. Experimental Design and Performance Measurement

The performance of both heuristics was evaluated using the A* search algorithm.

The experiment was conducted using the following settings:

| Parameter | Configuration |
|---|---|
| Search Algorithm | A* Search |
| Heuristics | Misplaced Tiles and Manhattan Distance |
| Scramble Depths | 5, 10, 15, 20 |
| Trials per Depth | 5 |
| Total Trials | 20 |
| Performance Measure | Average number of expanded nodes |

Both heuristics were tested on the same generated puzzle states to ensure a fair comparison.

The solution costs obtained from both heuristics were also checked. Since both heuristics are admissible, they should produce optimal solution costs.

---

## 7. Experimental Results

The following table shows the average number of expanded nodes for both heuristics over five trials at each scramble depth.

### Results Table

| Scramble Depth | Misplaced Tiles (Average Nodes) | Manhattan Distance (Average Nodes) |
|---:|---:|---:|
| 5 | 3.6 | 3.6 |
| 10 | 4.4 | 4.2 |
| 15 | 5.6 | 5.4 |
| 20 | 13.6 | 10.4 |

### Graphical Comparison

A comparison chart was generated in Google Colab using Pandas and Matplotlib.

- X-axis: Scramble Depth
- Y-axis: Average Number of Expanded Nodes
- First series: Misplaced Tiles
- Second series: Manhattan Distance

### Observations

- At scramble depth 5, both heuristics expanded the same average number of nodes.
- At depths 10 and 15, Manhattan Distance expanded slightly fewer nodes.
- The largest difference appeared at depth 20.
- At depth 20, Misplaced Tiles expanded an average of 13.6 nodes, while Manhattan Distance expanded 10.4 nodes.

These results indicate that Manhattan Distance performed better in most of the tested cases.

---

## 8. Analytical Discussion

### Q1. Why should both heuristics return zero for the goal state?

At the goal state, all numbered tiles are already in their correct positions. Therefore, the remaining cost is zero, and both heuristics return zero.

If a heuristic returned a positive value at the goal state, it would overestimate the actual remaining cost and violate admissibility.

### Q2. At which scramble depth was the performance difference most noticeable? Why?

The largest difference was observed at scramble depth 20.

Misplaced Tiles expanded an average of 13.6 nodes, while Manhattan Distance expanded 10.4 nodes.

Manhattan Distance provides more detailed information because it measures the actual horizontal and vertical distance of each tile. Misplaced Tiles only counts how many tiles are out of position.

This additional information helps A* prioritize more promising states and reduce unnecessary node expansions.

### Q3. Why is Manhattan Distance always greater than or equal to Misplaced Tiles? Does this make it inadmissible?

Every misplaced tile must move at least one step to reach its correct position. Manhattan Distance counts the total distance of each misplaced tile, while Misplaced Tiles only counts the number of misplaced tiles.

Therefore, Manhattan Distance is always greater than or equal to Misplaced Tiles.

However, a higher heuristic value does not automatically make a heuristic inadmissible. A heuristic is admissible if it never overestimates the actual minimum remaining cost.

Manhattan Distance is still admissible and provides a more informative estimate.

### Q4. Does Manhattan Distance completely solve the performance limitations of A*?

No. Manhattan Distance generally improves search efficiency, but it does not eliminate all limitations of A*.

For sufficiently difficult puzzles, A* may still need to explore and store a large number of states. Therefore, even with a better heuristic, memory and processing requirements can remain significant.

---

## 9. Conclusion

In this practical, the A* search algorithm was completed and tested on the 8-puzzle problem using two admissible heuristics: Misplaced Tiles and Manhattan Distance.

Both heuristics produced the same optimal solution costs for the tested puzzles. However, Manhattan Distance generally expanded fewer nodes, with the clearest improvement observed at scramble depth 20.

The experiment demonstrated that a more informative admissible heuristic can improve the efficiency of A* search without sacrificing the optimality of the solution.
