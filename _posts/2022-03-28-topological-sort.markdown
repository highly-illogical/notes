---
layout: post
title: topological sort
---

Suppose you have a directed acyclic graph (DAG) and you want to sort the nodes in such a way that if there is an edge from A to B, then A comes before B.

This is called a _topological sort_ and can be done using DFS.

(You can't do a topological sort on a directed graph that has cycles, because no matter what order you place the nodes in the cycle in, there will always be at least one edge going backward.)

### depth-first search

1. start at a given node
2. move to an unvisited neighbor of the node
3. repeat step 2 until there's nowhere to go
4. mark the current node as visited
5. backtrack until you get to a node from where you can get to another unvisited node
6. repeat steps 3-5 until there are no more nodes to backtrack to
7. if there are any other nodes in the graph, pick one of them and repeat steps 1-6 until there are no more nodes to explore

### how to do a topological sort using DFS

1. run DFS on the graph
2. each time a node is marked as visited, add it to the list
3. when the algorithm terminates, reverse the list and return it
