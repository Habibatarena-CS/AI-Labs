# INFORMED SEARCH

# LABORATORY EXERCISE 03

## A* SEARCH FOR THE 8-PUZZLE: HEURISTIC IMPLEMENTATION AND PERFORMANCE ANALYSIS

### 1. Learning Objectives

The main objectives of this practical were:

- To understand the state-space representation of the 8-puzzle problem.
- To implement the Misplaced Tiles and Manhattan Distance heuristics.
- To complete the A* search algorithm using a priority queue.
- To verify puzzle solvability and generate test states.
- To compare the performance of both heuristics using the number of expanded nodes.

### 2. State-Space Formulation of the 8-Puzzle

The 8-puzzle consists of a 3×3 board containing eight numbered tiles and one blank space. The objective is to move the tiles until the board reaches the required goal configuration.

**Goal State:**

```python
GOAL_STATE = (1, 2, 3, 4, 5, 6, 7, 8, 0)
