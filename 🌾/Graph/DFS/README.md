# DFS

- Disjoint set (union find)
  - check the connectivity
- Find all vertices and all paths between two vertices
  - DFS
    - explore all paths from the start vertex to all other vertices
- Traverse all vertices in a “graph”
- Traverse all paths between any two vertices in a “graph”

## Traversing all Vertices

- Time complexity: $O(V + E)$
  - We need to check every vertex and traverse through every edge in the graph
- Space complexity: $O(V)$
  - Either the manually created stack or the recursive call stack can store up to V vertices

## Traversing all paths between two vertices – Depth-First Search Algorithm

- The worst-case scenario, when trying to find all paths, is a complete graph
  - complete graph
    - a graph where every vertex is connected to every other vertex
- Time complexity: $O((V - 1)!)$
- Space complexity: $O(V^3)$

## Problems

- [Longest Path With Different Adjacent Characters](./LongestPathDifferentAdjChars.md)
- [Minimum Time to Collect All Apples in a Tree](./MinTimeCollectAllApplesTree.md)
- [Number of Nodes in the Sub-Tree With the Same Label](./NumNodesSubTreeSameLabel.md)
- [Employee Importance](./EmployeeImportance.md)
- [Nested List Weight Sum](./NestedListWeightSum.md)
- [Nested List Weight Sum II](./NestedListWeightSumII.md)
- [Max Area of Island](./MaxAreaOfIsland.md)
  - Need to return a sum from the recursive function
- [Number of Provinces](./NumberOfProvinces.md)
  - Adjacency matrix
- [Number Of Enclaves](./NumberOfEnclaves.md)
  - combines [Max Area of Island](./MaxAreaOfIsland.md) and [Number Of Closed Islands](./NumberOfClosedIslands.md)
- [Count Sub Island](./CountSubIslands.md)
  - Need to return a boolean value from the recursive function
- [Flood Fill](./FloodFill.md)
- [Pacific Atlantic Water Flow](./PacificAtlanticWaterFlow.md)
- [Find if Path Exists in Graph](./FindIfPathExistsInGraph.md)
- [All Paths from Source Lead to Destination](./AllPathsFromSourceLeadToDestination.md)
  - cycle detection: node-coloring
- [Find Eventual Safe States](./AllPathsFromSourceLeadToDestination.md)
  - detect all nodes in cycles
  - can use topological sort
- [Minimum Fuel Cost to Report to the Capital](./MinimumFuelCostToCapital.md)
- [Reconstruct Itinerary](./ReconstructItinerary.md)
- [Is Graph Bipartite?](./IsGraphBipartite.md)
- BT
  - [Maximum Depth of Binary Tree](./MaximumDepthBinaryTree.md)
  - [Invert Binary Tree](./InvertBinaryTree.md)
- BST
  - [Minimum Distance Between BST Nodes](./MinimumDistanceBetweenBSTNodes.md)
- [Keys and Rooms](./KeysAndRooms.md)
- [Reorder Routes to Make All Paths Lead to the City Zero](./ReorderRoutesToMakeAllPathsLeadToTheCityZero.md)
- [Most Stones Removed with Same Row or Column](./MostStonesRemovedWithSameRowOrColumn.md)
- [Maximum Number of Moves in a Grid](./MaximumNumberOfMovesInAGrid.md)

### HashMap

- [Clone Graph](./CloneGraph.md)

### Islands

- [Number Of Closed Islands](./NumberOfClosedIslands.md)
  - Need to return a boolean value from the recursive function
- [Surrounded Regions](./SurroundedRegions.md)
- [Number of Distinct Islands](./NumberOfDistinctIslands.md)
  - HashSet
- [Making A Large Island](./MakingALargeIsland.md)

### Build Graph

- [Detonate the Maximum Bombs](./DetonateTheMaximumBombs.md)

### Visited Array

- [The Maze](./TheMaze.md)

### Cycle Detection

- [Critical Connections in a Network](./CriticalConnectionsNetwork.md)

## Notes

- In DFS, we use a recursive function to explore nodes as far as possible along each branch. Upon reaching the end of a branch, we backtrack to the next branch and continue exploring
- what to create
  - visited
  - graph
    - the information of neighbors
