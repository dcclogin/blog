---
title: C343 Code Reviw - Routing Wires 
date: 2024-04-22 15:37:43
---

This is for the code review session for the 3rd project (Routing Wires) for Data Structure (C343) Spring 2024.

## The Idea

The type signature of the `findPaths` function:
```java
public static ArrayList<Wire> findPaths(Board board, ArrayList<Endpoints> goals)
```
Question: What is a **goal**? What is a **Wire**?

- A goal is a pair of endpoints on the board waiting to be connected.
- A goal can either *fail* or *succeed*.
- A wire is a goal that succeeds (find a path between 2 endpoints with BFS etc).

Question: What constitutes a **solution**?

1. The chip board is **updated** (wires all connected / goals all succeed).
2. An **ordered** list of wires is returned.

Therefore, a solution can be represented as a **permutation** of goals.

### Example 1

Think about the following inital board configuration:

```text
0  0 2  0 3  0 0
0 -1 0 -1 0 -1 0
0  1 0  0 0  1 0
0 -1 0 -1 0 -1 0
0  0 2  0 3  0 0
```

Question: How many permutations of goals [1,2,3]?

```text
A(n) = n!
A(3) = 3! = 3*2 = 6

They are
[1,2,3] [1,3,2] [2,3,1] [2,1,3] [3,1,2] [3,2,1]
```

Question: How many *feasible solutions* (suppose we use BFS)?

```text
There are only 2:

[1,2,3] [1,3,2]

The updated board is the same:

2  2 2  0 3  3 3
2 -1 0 -1 0 -1 3
2  1 1  1 1  1 3
2 -1 0 -1 0 -1 3
2  2 2  0 3  3 3
```

Remarks:
- We `accept` only **one arbitrary** feasible solution (either `[1,2,3]` or `[1,3,2]`, not both).
- The initial order of `goals` matters a lot:
    - no need for backtracking if properly arranged a priori (`[1,2,3]` or `[1,3,2]`). 
    - a space for creative **heuristics**.
- We `prune` the solution space when a goal fails


### Example 2 (pruning)

Slogan:
> *You can only fail once* for a certain prefix of permutation!

Suppose we have a list of `goals` [1,2,3,4,5,6] with a certain board configuration. Following BFS, any permutations where the success of goal `2` always blocks the success of goal `1`:

```text
[2, 1, _, _, _, _]    => 5! permutations are pruned (starting with 2 is not feasible at all)
    X

[2, 3, 1, _, _, _]    => 4! permutations are pruned
       X

[2, 3, 4, 1, _, _]    => 3! permutations are pruned
          X
```

```java
for (Endpoints curr : goals) {
    if (bfsFindOnePath(board, curr) == null) {
        // continue;   => less aggressive pruning
        return null;   => more aggressive pruning
    } else {
        ...
    }
}
```

Remarks:
- returning `null` will fully prune the current subproblem right now.
- `continue` will prune a smaller subproblem later (less aggressive).
- no feasible solutions are pruned if we simply return `null`. (why?)

Observation:
If a path cannot be found in a board configuration `A`, then there is no chance we can find one with `A + an extra wire`!


## Instructor's Solution (BFS + Backtracking)

Here's a detailed demonstration of instructor's solution, which uses BFS and backtracking without heuristics.

### BFS

BFS is for finding a relatively short path between a pair of endpoints on the board. In other words, it determines whether a goal should *fail* or *succeed*, and in what way (path) it succeeds! 

Therefore, intuitively, the `bfs` function returns a `boolean` value:

```java
private static boolean bfs(Board board, Endpoints eps, Map<Coord, Coord> parents)
```

Remarks:
- `board` is not yet updated in `bfs` (wire is not placed onto the board).
- a path from `start` to `end` is recorded through updating `parents` map, if `true` is returned.
- a partial path from `start` to some dead end is also recorded (useless), if `false` is returned.

```java
private static boolean bfs(Board board, Endpoints eps, Map<Coord, Coord> parents) {
    Queue<Coord> queue = new LinkedList<>();
    Set<Coord> visited = new HashSet<>();
    queue.add(eps.start);
    while (!queue.isEmpty()) {
        Coord cur = queue.remove();
        visited.add(cur);
        if (cur.equals(eps.end))
            return true;
        for (Coord adj : board.adj(cur)) {
            if ((board.getValue(adj) == 0 || adj.equals(eps.end))
                    && !visited.contains(adj)) {
                queue.add(adj);
                parents.put(adj, cur);
            }
        }
    }
    return false;
}    
```

Question: Any other candidates of searching algorithms? Why not DFS?

The next step is to update the `board` (i.e. `placeWire` onto the board):
- if `bfs` returns `true`, then (re-)construct a `wire` with `parents` map and place it onto the board.
- if `bfs` returns `false`, nothing happens (no update).

```java
private static Wire bfsFindOnePath(Board board, Endpoints eps) {
    Map<Coord, Coord> parents = new HashMap<>();
    boolean found_dest = bfs(board, eps, parents);
    if (found_dest) {
        ArrayList<Coord> path = new ArrayList<Coord>();
        path.add(eps.end);
        Coord p = parents.get(eps.end);
        while (p != null) {
            path.add(p);
            p = parents.get(p);
        }
        java.util.Collections.reverse(path);
        Wire w = new Wire(eps.id, path);
        board.placeWire(w);
        return w;
    } else {
        return null;
    }
}
```


### Naive BFS (no backtracking, no heuristics)

This should pass 8-9 out of 11 autograder tests.

```java
private static ArrayList<Wire> bfsFindPaths(Board board, ArrayList<Endpoints> goals) {
    ArrayList<Wire> wires = new ArrayList<>();
    for (Endpoints endpoints : goals) {
        wires.add(bfsFindOnePath(board, endpoints));
    }
    return wires;
}
```


### Backtracking (no heuristics)

Basically, backtracking is baked into the recursion structure:
- we keep spliting the list of `goals` into a `curr` element and the `rest` elements.
- first try BFS with the `curr` element:
    - if succeed, a `wire` is placed -> try the recursive part (`rest`).
    - if fail, `continue`/`return null` (pruning the solution space)
- second try the `rest`, recursive part:
    - if succeed, it means a feasible solution is found -> return a list of `wire`
    - if fail, remove the current `wire` on the board -> try another `curr`-`rest`

```java
private static ArrayList<Wire> backtrackingFindPaths(Board board, ArrayList<Endpoints> goals) {
    if (goals.size() == 0) {
        return new ArrayList<>();
    } else {
        // Try to find a point for one of the end-points.
        for (Endpoints curr : goals) {
            Wire w = bfsFindOnePath(board, curr);
            if (w == null) { // failed
                // continue;
                return null;
            } else { // success
                // Recursively solve the rest
                ArrayList<Endpoints> rest = new ArrayList<>(goals);
                rest.remove(curr);
                ArrayList<Wire> result = backtrackingFindPaths(board, rest);
                if (result == null) {
                    // Undo the current wire.
                    board.removeWire(w);
                    continue;
                } else { // The rest succeeded, add this one and return
                    result.add(w);
                    return result;
                }
            }
        }
        // We never succeeded, so return null
        return null;
    }
}
```

## Students' solutions

Here are some interesting solutions from students.

### Heuristic: Increasing Order of Manhattan Distances

Recall:
- The initial order of `goals` matters.

We can reduce the chance for backtracking with a quasi-empirical fact:
- wires with less "lengths" are less likely to block the others,
- therefore we find paths for and place them first. 

One quantification of "length" is [Manhattan Distance](https://en.wikipedia.org/wiki/Taxicab_geometry).

```text
md:    4  3  2  1  3  7
       |  |  |  |  |  |
#goal [1, 2, 3, 4, 5, 6]

=> (sorting w.r.t. md)

md:    1  2  3  3  4  7
       |  |  |  |  |  |
#goal [4, 3, 2, 5, 1, 6]
```

### Try All Permutations (no pruning)

Recall:
- a solution can be represented as a **permutation** of goals as input.

Therefore it's possible to loop over all permutations until a feasible solution is found. Generally, there is no pruning in this solution, which means there might be some unnecessary fails. But it depends on the implementation of `nextPermutaion` and can be combined with heuristics.

```java
public static ArrayList<Wire> findPaths(Board board, ArrayList<Endpoints> goals) {
    ArrayList<Wire> solutions = new ArrayList<>(Collections.nCopies(goals.size(), null));
    List<Integer> indices = new ArrayList<>();
    for (int i = 0; i < goals.size(); i++) {
        indices.add(i);
    }

    do {
        // Before each permutation, ensure the board is clear of wires from previous attempts.
        clearWiresFromBoard(board, solutions);

        boolean isValidPermutation = true;
        for (int index : indices) {
            Wire wire = bfsFindPath(board, goals.get(index));
            if (wire != null) {
                board.placeWire(wire);
                solutions.set(index, wire);
            } else {
                // If a wire couldn't be placed, it's necessary to remove all wires placed during this permutation attempt.
                for (Wire w : solutions) {
                    if (w != null) board.removeWire(w);
                }
                // Clear solutions to ensure a clean state for the next attempt.
                Collections.fill(solutions, null);
                isValidPermutation = false;
                break;
            }
        }
        if (isValidPermutation) {
            return solutions;  // Found a valid configuration
        }
    } while (nextPermutation(indices));

    return new ArrayList<>();  // If all permutations fail, return an empty list
}
```

### Unusual Solution: Try-Catch (Exception Handler)


Here is a more "dynamic" and "on-the-fly" way to do backtracking and resolve conflicts:

- build path ("proposed path") for each goal *independently*.
- `try` to place them one by one.
- `catch` the wire-wire conflict exception 
- re-build a new path/wire for the latter goal:
    - if succeed -> move on to the next goal.
    - if cannot re-build a new path -> remove one previously placed wire on the board and retry.
- until all conflicts solved.

```java
public static ArrayList<Wire> findPaths(Board board, ArrayList<Endpoints> goals) {
    ArrayList<Wire> finalWires = new ArrayList<>();
    HashMap<Integer, Wire> proposedPaths = new HashMap<>();

    // find a "proposed path" for each pair independently first
    // notice that they might have conficts (wireWireException)!
    for (int i = 0; i < goals.size(); ++i) {
        Endpoints currgoal = goals.get(i);
        ArrayList<Coord> foundpath = bfs(board, currgoal.start, currgoal.end, currgoal.id);
        Wire newWire = null;
        if (foundpath != null) {
            newWire = new Wire(currgoal.id, foundpath);
        }
        proposedPaths.put(currgoal.id, newWire);
    }

    Queue<Wire> placedWires = new LinkedList<>();
    Queue<Wire> reAddWires = new LinkedList<>();
    for (int j = 1; j < goals.size()+1; ++j) {
        Wire toAdd = proposedPaths.get(goals.get(j-1).id);
        int okay = 0;
        while (okay == 0 || !reAddWires.isEmpty()) {
            try {
                if(!reAddWires.isEmpty()){
                    toAdd = reAddWires.remove();
                    ArrayList<Coord> newpath = bfs(board,toAdd.start(),toAdd.end(),toAdd.id);
                    toAdd = new Wire(toAdd.id,newpath);
                    board.placeWire(toAdd);
                    finalWires.add(toAdd);
                    placedWires.add(toAdd);
                    proposedPaths.put(toAdd.id, toAdd);
                    okay = 0;
                    if(reAddWires.isEmpty()){
                        okay = 1;
                    }
                } else{
                    toAdd = proposedPaths.get(goals.get(j-1).id);
                    if(!finalWires.contains(toAdd)){
                        board.placeWire(toAdd);
                        finalWires.add(toAdd);
                        placedWires.add(toAdd);
                        proposedPaths.put(toAdd.id, toAdd);
                        okay = 1;
                    } else{
                        okay = 1;
                    }
                }
            } catch (Board.WireWireException e) {
                ArrayList<Coord> newpath = bfs(board,toAdd.start(),toAdd.end(),toAdd.id);
                // if newpath is null then we have to remove a previous wire to make this try to work
                while(newpath == null){
                    Wire removedtotry = placedWires.remove();
                    board.removeWire(removedtotry);
                    reAddWires.add(removedtotry);
                    placedWires.remove(removedtotry);
                    finalWires.remove(removedtotry);
                    newpath = bfs(board,toAdd.start(),toAdd.end(),toAdd.id);
                }
                // we finally found that one works.
                Wire newwiretoAdd = new Wire(toAdd.id, newpath);
                board.placeWire(newwiretoAdd);
                finalWires.add(newwiretoAdd);
                placedWires.add(newwiretoAdd);
                proposedPaths.put(newwiretoAdd.id, newwiretoAdd);
                okay = 0;
            }
        }
    }
    return finalWires;
}
```