
# LAB:02
## UNINFORMED SEARCH — BFS AND DFS ON A 2D MAZE

---

## OBJECTIVE OF THE LAB

The objective of this lab is to implement Breadth-First Search (BFS) and Depth-First Search (DFS), find paths in a 2D maze, and compare their performance.

---

## 1. THE MAZE

A maze is represented using a 2D list (grid) of rows and columns.

| Value | Description |
|:---:|---|
| `0` | Open cell. The algorithm can move through it. |
| `1` | Wall. The algorithm cannot move through it. |
| Start | The initial position. |
| Goal | The destination. |

**Maze used in the practical:**

```python
maze = [
    [0, 0, 0, 0, 0],
    [0, 1, 1, 1, 0],
    [0, 0, 0, 1, 0],
    [0, 1, 0, 0, 0],
    [0, 1, 1, 1, 0]
]
```

### Maze Details

| Detail | Value |
|---|:---:|
| Number of Rows | 5 |
| Number of Columns | 5 |
| Indexing | Starts from `0` |
| Start Position | `(0, 0)` |
| Goal Position | `(4, 4)` |

For example, `(2, 1)` means row 2 and column 1.

---

## 2. MOVEMENTS

The algorithm can move in four directions.

| Direction | Row Change | Column Change |
|:---:|:---:|:---:|
| UP | `-1` | `0` |
| DOWN | `+1` | `0` |
| LEFT | `0` | `-1` |
| RIGHT | `0` | `+1` |

### Movement Coordinates

```python
directions = [
    (-1, 0),  # UP
    (1, 0),   # DOWN
    (0, -1),  # LEFT
    (0, 1)    # RIGHT
]
```

### Example

If the current position is `(2, 2)`:

| Direction | New Position |
|:---:|:---:|
| UP | `(1, 2)` |
| DOWN | `(3, 2)` |
| LEFT | `(2, 1)` |
| RIGHT | `(2, 3)` |

---

## 3. CHECKING A VALID MOVE

Before moving to a new cell, the algorithm checks whether the move is valid.

| Condition | Requirement |
|---|---|
| Row Boundary | The new row must be inside the maze. |
| Column Boundary | The new column must be inside the maze. |
| Destination Cell | The destination must not be a wall (`0`). |
| Visited Cell | The cell must not have been visited already. |

These checks prevent the algorithm from moving outside the maze or passing through walls.

---

## 4. BREADTH-FIRST SEARCH (BFS)

BFS explores the maze level by level. It first checks the nearest cells and then moves towards farther cells.

### Key Characteristics

| Feature | BFS |
|---|---|
| Data Structure | Queue |
| Rule | FIFO — First In, First Out |
| Selection | The cell added first is explored first. |
| Search Method | Explores nearby possibilities before going deeper. |
| Shortest Path | Guaranteed when every move has equal cost. |

### Important Code

```python
frontier = deque([start])
current = frontier.popleft()
```

| Code | Purpose |
|---|---|
| `deque()` | Creates a queue. |
| `popleft()` | Removes the oldest element from the front of the queue. |
| `append()` | Adds a new cell to the queue. |

### BFS Working

1. Add the starting position to the queue.
2. Remove the first position from the queue.
3. Check its valid neighboring cells.
4. Add unvisited valid cells to the queue.
5. Repeat until the goal is found or the queue becomes empty.

---

## 5. DEPTH-FIRST SEARCH (DFS)

DFS explores one direction deeply before returning to explore other possible directions.

### Key Characteristics

| Feature | DFS |
|---|---|
| Data Structure | Stack |
| Rule | LIFO — Last In, First Out |
| Selection | The cell added last is explored first. |
| Search Method | Follows a deep path before backtracking. |
| Shortest Path | Not guaranteed. |

### Important Code

```python
frontier = []
frontier.append(start)
current = frontier.pop()
```

| Code | Purpose |
|---|---|
| `append()` | Adds a new position to the stack. |
| `pop()` | Removes the last added position. |

### DFS Working

1. Add the starting position to the stack.
2. Remove the last position from the stack.
3. Check its valid neighboring cells.
4. Add unvisited valid cells to the stack.
5. Continue exploring deeply.
6. If a path is blocked, DFS backtracks and tries another direction.

---

## 6. VISUALIZING THE PATHS

The paths are displayed using the following symbols.

| Symbol | Meaning |
|:---:|---|
| `S` | Start |
| `G` | Goal |
| `*` | Path followed |
| `#` | Wall |
| `.` | Open cell |

**Starting Position:** `(0, 0)`  
**Goal Position:** `(4, 4)`

### BFS Path

BFS follows the path through the left side and then moves towards the goal.

**Path Coordinates:**

```text
(0,0) → (1,0) → (2,0) → (2,1)
→ (2,2) → (3,2) → (3,3) → (3,4) → (4,4)
```

**BFS Grid:**

```text
S  .  .  .  .
*  #  #  #  .
*  *  *  #  .
.  #  *  *  *
.  #  #  #  G
```

BFS follows the lower route through the left side of the maze.

### DFS Path

DFS follows the upper route first and continues towards the goal.

**Path Coordinates:**

```text
(0,0) → (0,1) → (0,2) → (0,3)
→ (0,4) → (1,4) → (2,4) → (3,4) → (4,4)
```

**DFS Grid:**

```text
S  *  *  *  *
.  #  #  #  *
.  .  .  #  *
.  #  .  .  *
.  #  #  #  G
```

DFS follows the upper route and then moves down towards the goal.

---

## 7. COMPARISON OF BFS AND DFS

Both algorithms were tested using the same maze, start position, and goal position.

### Results

| Measurement | BFS | DFS |
|---|:---:|:---:|
| Path Length | 8 | 8 |
| Maximum Frontier | 3 | 3 |

### Comparison Summary

| BFS | DFS |
|---|---|
| Explores level by level. | Explores one branch deeply. |
| Uses a Queue. | Uses a Stack. |
| Uses FIFO. | Uses LIFO. |
| Guarantees the shortest path when all moves have equal cost. | Does not guarantee the shortest path. |

### Questions and Answers

**Q1. Which algorithm found the shorter path?**

Both BFS and DFS found paths of equal length (8).

**Q2. Which algorithm had a larger maximum frontier?**

Both had the same maximum frontier size (3).

**Q3. Why did BFS and DFS find different paths in the same maze?**

BFS uses a Queue (FIFO), which explores the oldest discovered position first. DFS uses a Stack (LIFO), which explores the most recently discovered position first.

Because they follow different exploration strategies, they can reach the goal through different routes.

**Q4. Does DFS always find the shortest path? Why?**

No. DFS explores deeply and may find a longer path before a shorter one.

**Q5. Does BFS always find the shortest path if each move has equal cost? Why?**

Yes. BFS explores level by level, so it finds the shortest path first when every move has equal cost.

---

## 8. EXPERIMENT 1 — CHANGE NEIGHBOR ORDER

In this experiment, the direction order is changed to observe its effect on both BFS and DFS paths.

### Original Neighbor Order

```python
directions = [
    (-1, 0),  # UP
    (1, 0),   # DOWN
    (0, -1),  # LEFT
    (0, 1)    # RIGHT
]
```

**Original Order:** UP → DOWN → LEFT → RIGHT

### Original Paths

**BFS Original Path:**

```text
S  .  .  .  .
*  #  #  #  .
*  *  *  #  .
.  #  *  *  *
.  #  #  #  G
```

BFS follows the lower route.

**DFS Original Path:**

```text
S  *  *  *  *
.  #  #  #  *
.  .  .  #  *
.  #  .  .  *
.  #  #  #  G
```

DFS follows the upper route.

### Modified Neighbor Order

```python
directions = [
    (0, 1),   # RIGHT
    (1, 0),   # DOWN
    (0, -1),  # LEFT
    (-1, 0)   # UP
]
```

**Modified Order:** RIGHT → DOWN → LEFT → UP

### Modified Paths

**BFS Modified Path:**

```text
S  *  *  *  *
.  #  #  #  *
.  .  .  #  *
.  #  .  .  *
.  #  #  #  G
```

BFS now follows the upper route.

**DFS Modified Path:**

```text
S  .  .  .  .
*  #  #  #  .
*  *  *  #  .
.  #  *  *  *
.  #  #  #  G
```

DFS now follows the lower route.

### Original and Modified Path Comparison

| Algorithm | Original Order | Modified Order |
|---|---|---|
| BFS | Lower Route | Upper Route |
| DFS | Upper Route | Lower Route |

### Observation

After changing the neighbor order from Up, Down, Left, Right to Right, Down, Left, Up, I observed that the paths followed by BFS and DFS changed. The new neighbor order affected the sequence in which both algorithms explored the maze. In my experiment, DFS followed the path that BFS had followed previously, showing that the neighbor order can significantly affect the search path. However, BFS and DFS still use different search strategies: BFS explores level by level, while DFS explores one branch as deeply as possible before backtracking.

**Note:** Both algorithms still find a shortest path in this maze because all moves have equal cost. Changing the neighbor order changes which path is selected when multiple shortest paths are available.

---

## 9. EXPERIMENT 2 — CHANGE START AND GOAL

In this experiment, different starting and goal positions were tested to observe their effect on path length, maximum frontier size, and algorithm performance.

**Important:** Start and goal should be placed on open cells (`0`), not walls (`1`).

### Code Used

```python
directions = [
    (-1, 0),   # up
    (1, 0),    # down
    (0, -1),   # left
    (0, 1)     # right
]

# Try changing these to any open (0) cells in the maze
start = (0, 0)
goal = (2, 2)

bfs_path, bfs_frontier = bfs(maze, start, goal)
dfs_path, dfs_frontier = dfs(maze, start, goal)

print("BFS path:", bfs_path, "| length:", len(bfs_path) - 1, "| max frontier:", bfs_frontier)
print("DFS path:", dfs_path, "| length:", len(dfs_path) - 1, "| max frontier:", dfs_frontier)

print()
print("BFS solution:")
print_maze(maze, bfs_path)
print()
print("DFS solution:")
print_maze(maze, dfs_path)
```

### Experimental Results

| Start | Goal | BFS Length | BFS Max | DFS Length | DFS Max |
|:---:|:---:|:---:|:---:|:---:|:---:|
| `(2,0)` | `(4,4)` | 6 | 3 | 16 | 4 |
| `(0,4)` | `(3,2)` | 5 | 3 | 9 | 3 |
| `(1,2)` | `(3,4)` | 4 | 4 | 4 | 4 |

**Note:** In this experiment, the starting position `(1, 2)` is a wall because its value in the maze is `1`. However, both BFS and DFS still begin searching from this position because the code does not check whether the starting cell is valid. The `valid_move()` function checks only the neighboring cells, not the initial position. Therefore, the algorithms can produce a path even when the starting position is a wall.

### Important Explanation

| Point | Explanation |
|---|---|
| Start and Goal | Changing them can change the path length. |
| BFS | Generally finds the shortest path. |
| DFS | May find a longer path depending on the neighbor order. |
| Maximum Frontier | The largest number of positions waiting to be explored at one time. |

### Questions and Answers

**Q6. Does the path length remain the same if we run the algorithm again with the same start and goal?**

Yes. With the same maze, start, goal, and direction order, the path length remains the same.

**Q7. As the distance increases, what happens to the maximum frontier?**

The maximum frontier may increase because more cells may need to be explored. It depends on the maze structure and the search algorithm.

**Q8. Can BFS and DFS return the same path? What does this tell us?**

Yes. Both can return the same path in some cases. It means that their exploration order led them to the same route.

**Q9. What happens if the goal is unreachable?**

Both algorithms explore all reachable cells and return `None` for the path. The maximum frontier shows the maximum number of waiting positions reached during the search.

**Return Value:**

```python
(None, max_frontier)
```

`None` indicates that no solution path was found.

---

## 10. APPLICATIONS OF BFS AND DFS

| BFS Applications | DFS Applications |
|---|---|
| Finding the shortest path in an unweighted graph. | Maze solving. |
| Finding degrees of connection in social networks. | Graph traversal. |
| Exploring nearby web pages. | Cycle detection. |
| Finding the minimum number of steps between two points. | Topological sorting. |
| — | Backtracking problems. |
| — | File system traversal. |

Both BFS and DFS are uninformed search algorithms because they do not use extra knowledge (heuristics) about how close a position is to the goal.

---

## 11. KEY TAKEAWAY

The main difference between BFS and DFS is how they select the next position from the frontier.

| Feature | BFS | DFS |
|---|---|---|
| Data Structure | Queue | Stack |
| Working Method | Level by level | Deep branch first |
| Main Operation | `popleft()` | `pop()` |
| Shortest Path | Yes, with equal costs | Not guaranteed |
| Memory Usage | Typically more | Typically less |

### Conclusion

BFS explores the maze level by level and guarantees the shortest path when every move has equal cost. DFS explores one branch deeply before backtracking and does not guarantee the shortest path. Changing the neighbor order can change the path selected by DFS.
