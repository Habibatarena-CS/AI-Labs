# Lab 02: Uninformed Search --- BFS and DFS on a 2D Maze

## Goal of the Lab

The goal of this lab is to implement Breadth-First Search (BFS) and
Depth-First Search (DFS), find paths in a 2D maze, and compare their
performance.

------------------------------------------------------------------------

## 1. The Maze

A maze is represented using a 2D list (grid) of rows and columns.

-   `0` represents an open cell. The algorithm can move through it.
-   `1` represents a wall. The algorithm cannot move through it.
-   **Start** is the initial position.
-   **Goal** is the destination.

### Maze used in the practical

``` python
maze = [
    [0, 0, 0, 0, 0],
    [0, 1, 1, 1, 0],
    [0, 0, 0, 1, 0],
    [0, 1, 0, 0, 0],
    [0, 1, 1, 1, 0]
]
```

### Important details

-   The maze has 5 rows and 5 columns.
-   Each cell is identified by its row and column index.
-   Indexing starts from `0`, not `1`.
-   Start position: `(0, 0)`
-   Goal position: `(4, 4)`

For example, `(2, 1)` means row 2 and column 1.

------------------------------------------------------------------------

## 2. Movements

The algorithm can move in four directions:

  Direction     Row Change   Column Change
  ----------- ------------ ---------------
  UP                    -1               0
  DOWN                  +1               0
  LEFT                   0              -1
  RIGHT                  0              +1

### Movement coordinates

``` python
directions = [
    (-1, 0),  # UP
    (1, 0),   # DOWN
    (0, -1),  # LEFT
    (0, 1)    # RIGHT
]
```

For example, if the current position is `(2, 2)`:

-   UP → `(1, 2)`
-   DOWN → `(3, 2)`
-   LEFT → `(2, 1)`
-   RIGHT → `(2, 3)`

------------------------------------------------------------------------

## 3. Checking a Valid Move

Before moving to a new cell, the algorithm checks whether the move is
valid.

A move is valid if:

1.  The new row is inside the maze.
2.  The new column is inside the maze.
3.  The destination cell is open (`0`), not a wall (`1`).

The algorithm also avoids visiting an already visited cell. These checks
prevent it from moving outside the grid or through walls.

------------------------------------------------------------------------

## 4. Breadth-First Search (BFS)

BFS explores the maze level by level. It checks the nearest cells first
and then moves towards farther cells.

### Important points

-   BFS uses a **Queue (FIFO)**.
-   FIFO means **First In, First Out**.
-   The position added first is removed and explored first.
-   BFS explores nearby possibilities before moving to the next level.
-   BFS finds a shortest path when every move has equal cost.

### Important code lines

``` python
frontier = deque([start])
current = frontier.popleft()
```

-   `deque()` creates a queue.
-   `popleft()` removes the oldest element from the front of the queue.
-   New positions are added using `append()`.

### How BFS works

1.  Add the starting position to the queue.
2.  Remove the first position from the queue.
3.  Check its valid neighboring cells.
4.  Add unvisited valid neighbors to the queue.
5.  Repeat until the goal is found or the queue becomes empty.

------------------------------------------------------------------------

## 5. Depth-First Search (DFS)

DFS explores one direction deeply before returning to explore other
possible directions.

### Important points

-   DFS uses a **Stack (LIFO)**.
-   LIFO means **Last In, First Out**.
-   The position added last is removed and explored first.
-   DFS follows a deep path before backtracking.
-   DFS does not guarantee the shortest path.

### Important code lines

``` python
frontier = []
frontier.append(start)
current = frontier.pop()
```

-   `append()` adds a new position to the stack.
-   `pop()` removes the last added position.
-   DFS continues exploring deeply until it reaches the goal or has no
    more options.

### How DFS works

1.  Add the starting position to the stack.
2.  Remove the last position from the stack.
3.  Check its valid neighboring cells.
4.  Add unvisited valid neighbors to the stack.
5.  Continue exploring deeply.
6.  If a path is blocked, DFS backtracks and tries another direction.

------------------------------------------------------------------------

## 6. Visualizing the Paths

The paths are displayed using the following symbols:

   Symbol  Meaning
  -------- ---------------
    `S`    Start
    `G`    Goal
    `*`    Path followed
    `#`    Wall
    `.`    Open cell

**Starting position:** `(0, 0)`\
**Goal position:** `(4, 4)`

### BFS Path

BFS follows the path through the left side and then moves towards the
goal.

**BFS path coordinates:**

``` text
(0,0) → (1,0) → (2,0) → (2,1)
→ (2,2) → (3,2) → (3,3) → (3,4) → (4,4)
```

**BFS grid:**

``` text
S  .  .  .  .
*  #  #  #  .
*  *  *  #  .
.  #  *  *  *
.  #  #  #  G
```

BFS follows the lower route through the left side of the maze.

### DFS Path

DFS follows the upper route first and continues towards the goal.

**DFS path coordinates:**

``` text
(0,0) → (0,1) → (0,2) → (0,3)
→ (0,4) → (1,4) → (2,4) → (3,4) → (4,4)
```

**DFS grid:**

``` text
S  *  *  *  *
.  #  #  #  *
.  .  .  #  *
.  #  .  .  *
.  #  #  #  G
```

DFS follows the upper route and then moves down towards the goal.

------------------------------------------------------------------------

## 7. Compare BFS and DFS

Both algorithms were tested using the same maze, start position, and
goal position.

### Results

  Measurement          BFS   DFS
  ------------------ ----- -----
  Path Length            8     8
  Maximum Frontier       3     3

### Comparison

-   BFS explores the maze level by level using a queue.
-   DFS explores one branch deeply using a stack.
-   Both found a path of length 8 in this practical.
-   Both had a maximum frontier size of 3.

### Questions and Answers

**Q1. Which algorithm found the shorter path?**

Both BFS and DFS found paths of equal length (8).

**Q2. Which algorithm had a larger maximum frontier?**

Both had the same maximum frontier size (3).

**Q3. Why did BFS and DFS find different paths in the same maze?**

BFS uses a Queue (FIFO --- First In, First Out), so it removes and
explores the position that was added first. DFS uses a Stack (LIFO ---
Last In, First Out), so it removes and explores the position that was
added last. Because they explore positions in different orders, BFS
searches level by level while DFS follows one branch deeply before
backtracking. Therefore, they can find different paths in the same maze.

**Q4. Does DFS always find the shortest path? Why?**

No. DFS explores deeply and may find a longer path before a shorter one.

**Q5. Does BFS always find the shortest path if each move has equal
cost? Why?**

Yes. BFS explores level by level, so it finds the shortest path first
when every move has equal cost.

------------------------------------------------------------------------

## 8. Experiment 1 --- Change Neighbor Order

In this experiment, the direction order is changed to observe its effect
on both BFS and DFS paths.

### Original Neighbor Order

The teacher's original direction order is:

``` python
directions = [
    (-1, 0),  # UP
    (1, 0),   # DOWN
    (0, -1),  # LEFT
    (0, 1)    # RIGHT
]
```

**Original order:** UP → DOWN → LEFT → RIGHT

**BFS --- Original path:**

``` text
S  .  .  .  .
*  #  #  #  .
*  *  *  #  .
.  #  *  *  *
.  #  #  #  G
```

BFS follows the lower route.

**DFS --- Original path:**

``` text
S  *  *  *  *
.  #  #  #  *
.  .  .  #  *
.  #  .  .  *
.  #  #  #  G
```

DFS follows the upper route.

### Modified Neighbor Order

The direction order is changed to:

``` python
directions = [
    (0, 1),   # RIGHT
    (1, 0),   # DOWN
    (0, -1),  # LEFT
    (-1, 0)   # UP
]
```

**Modified order:** RIGHT → DOWN → LEFT → UP

**BFS --- Modified path:**

``` text
S  *  *  *  *
.  #  #  #  *
.  .  .  #  *
.  #  .  .  *
.  #  #  #  G
```

BFS now follows the upper route.

**DFS --- Modified path:**

``` text
S  .  .  .  .
*  #  #  #  .
*  *  *  #  .
.  #  *  *  *
.  #  #  #  G
```

DFS now follows the lower route.

### Comparison of Original and Modified Paths

  Algorithm   Original Order   Modified Order
  ----------- ---------------- ----------------
  BFS         Lower route      Upper route
  DFS         Upper route      Lower route

### Observation

After changing the neighbor order from UP, DOWN, LEFT, RIGHT to RIGHT,
DOWN, LEFT, UP, I observed that the paths followed by both BFS and DFS
changed. In the original order, BFS followed the lower route and DFS
followed the upper route. After changing the order, BFS followed the
upper route, while DFS followed the lower route, which was the path
previously followed by BFS.

The new neighbor order affected the sequence in which both algorithms
explored the maze. However, BFS still explores level by level, while DFS
explores one branch as deeply as possible before backtracking.

------------------------------------------------------------------------

## 9. Experiment 2 --- Change Start and Goal

In this experiment, different starting and goal positions were tested to
observe how changing them affects:

-   Path length
-   Maximum frontier size
-   BFS and DFS performance

**Important:** The start and goal should be placed on open cells (`0`),
not walls (`1`).

### Code used in the practical

``` python
directions = [
    (-1, 0),  # UP
    (1, 0),   # DOWN
    (0, -1),  # LEFT
    (0, 1)    # RIGHT
]

# Try changing these to any open (0) cells in the maze
start = (0, 0)
goal = (2, 2)

bfs_path, bfs_frontier = bfs(maze, start, goal)
dfs_path, dfs_frontier = dfs(maze, start, goal)

print("BFS path:", bfs_path, "| length:", len(bfs_path) - 1,
      "| max frontier:", bfs_frontier)
print("DFS path:", dfs_path, "| length:", len(dfs_path) - 1,
      "| max frontier:", dfs_frontier)

print()
print("BFS solution:")
print_maze(maze, bfs_path)
print()
print("DFS solution:")
print_maze(maze, dfs_path)
```

### Experimental Results

   Start   Goal     BFS Length   BFS Max   DFS Length   DFS Max
  ------- ------- ------------ --------- ------------ ---------
   (2,0)   (4,4)             6         3           16         4
   (0,4)   (3,2)             5         3            9         3
   (1,2)   (3,4)             4         4            4         4

**Note about the third test:** In the given maze, `(1, 2)` is a wall
because its value is `1`. However, both algorithms still begin searching
from this position because the code checks the validity of neighboring
cells, not the initial start position. Therefore, the algorithms can
produce a path even when the starting position is a wall. This test does
not follow the practical instruction to choose an open starting cell.

### Important Explanation

-   Changing the start and goal can change the path length.
-   BFS generally finds the shortest path.
-   DFS may find a longer path depending on the neighbor order.
-   Maximum frontier shows the largest number of positions waiting to be
    explored at one time.

### Questions and Answers

**Q6. Does the path length remain the same if we run the algorithm again
with the same start and goal?**

Yes. With the same maze, start, goal, and direction order, the path
length remains the same.

**Q7. As the distance increases, what happens to the maximum frontier?**

The maximum frontier may increase because more cells may need to be
explored. It depends on the maze structure and the search algorithm.

**Q8. Can BFS and DFS return the same path? What does this tell us?**

Yes. Both algorithms can return the same path. This means that, in some
cases, their exploration order can lead them to the same route.

For example, the recorded observation for start `(2,0)` and goal `(4,4)`
says that both algorithms return the same path. **However, the results
table records different path lengths for this pair (BFS = 6 and DFS =
16). These two details do not agree, so the practical output should be
checked before submission.**

**Q9. What happens if the goal is unreachable?**

Both algorithms explore all reachable cells and return `None` for the
path. The maximum frontier shows the maximum number of waiting positions
reached during the search.

They return:

``` python
(None, max_frontier)
```

`None` indicates that no solution path was found.

------------------------------------------------------------------------

## 10. Where This Is Used

### BFS Applications

-   Finding the shortest path in an unweighted graph.
-   Finding degrees of connection in social networks.
-   Exploring nearby web pages.
-   Finding the minimum number of steps between two points.

### DFS Applications

-   Maze solving.
-   Graph traversal.
-   Cycle detection.
-   Topological sorting.
-   Backtracking problems.
-   File system traversal.

Both BFS and DFS are uninformed search algorithms because they do not
use extra knowledge (heuristics) about how close a position is to the
goal.

------------------------------------------------------------------------

## 11. Key Takeaway

The main difference between BFS and DFS is how they select the next
position from the frontier.

  Feature          BFS                     DFS
  ---------------- ----------------------- -------------------
  Data Structure   Queue                   Stack
  Working Method   Level by level          Deep branch first
  Main Operation   `popleft()`             `pop()`
  Shortest Path    Yes, with equal costs   Not guaranteed
  Memory Usage     Typically more          Typically less

### Conclusion

BFS explores the maze level by level and guarantees the shortest path
when every move has equal cost. DFS explores one branch deeply before
backtracking and does not guarantee the shortest path. Changing the
neighbor order can change the path selected by both algorithms when
multiple routes are available.
