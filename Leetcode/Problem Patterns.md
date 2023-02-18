[Coding interview leetcode patterns](https://www.educative.io/blog/coding-interview-leetcode-patterns)

> Without considering the patterns associated with each question and answer, such an effort is futile given the sheer vastness of the computing subject.

> Coding patterns are a path to connecting an unknown problem to a known problem.

> Once you're familiar with patterns, then you can use that knowledge to solve a multitude of related problems instead of just the few encounter on LeetCode.

## Patterns:

### 1. Two pointers technique

#### pattern:
1. sorted array
2. fulfil certain constraints with a specific set of elements (the set of elements could be a pair, triplet, or a subarray)

#### Example:
> Given an array of **sorted numbers**  and **a target sum**, find **a pair** in the array whose sum is equal to the given target.

Naive way O(N<sup>2</sup>): 
2 nested loops and check all the possible pairs to find the one whose sum is equal to the target value.

Better way O(N):
1. First pointer set to the beginning , Second pointer set to the end of the sorted array
2. At every step, see if the numbers pointed by the 2 pointers add up to the target sum
	1. if so, return the 2 values pointed
	2. otherwise,
		1. if the sum is greater than the target, means you need a pair of smaller sum, move the 2nd pointer to the left neighbour
		2. if the sum is less than the target, move the 1st pointer to the right neighbour

### 2. Sliding windows

#### Pattern:
1. dealing with array or a linked list
2. find / calculate something among **all the subarrays** of **a given size**
3. A **window** is a sublist formed over a part of the iterable data structure

#### Example:
> Given an array, find the average of each subarray of 'K' contiguous elements in it

Naive way O(N<sup>2</sup>):
Two nested loops that the outer loop iterates over each window, and inner loop goes through each element in the window to find the sum

Better way O(N):
1. find the sum/average of the first window
2. sliding the window, by removing/subtracting the left most number and adding the new number to the right most position, and then find the sum/average


### 3. 0/1 Knapsack Pattern

### 4. Fast and slow pointers

#### Pattern:
1. Deal with cyclic linked list

#### Example:
> To determine if linked list contains a cycle or not


### 5. Merge intervals

#### Pattern:
1. deal with overlapping intervals
2. find/merge the overlaps 
3. only 6 scenarios could happen, say interval a, b:
	1. a overlaps b, and b ends after a's end -> [-----(--]--)
	2. a overlaps b, and b ends before a's end -> [-----(----)--]
	3. a overlaps b, and b begins before a's start -> (--[--)-----]
	4. a overlaps b, and b begins after a's start -> [---(---)--]
	5. a does not overlap b, and a starts before b starts -> [---] (---)
	6. a does not overlap b, and b starts before a starts -> (---) [----]

#### Example:
> Given a list of intervals, merge all the overlapping intervals to produce a list of mutually exclusive intervals

### 6. In-place reversal of a linked list

#### Pattern:
1. Reverse a linked list
2. doing it 'in-place'

solution: 
1. Keep track of 3 nodes: previous, current and next node
2. point current node to previous one
3. subsequently, the current becomes the previous, the next node becomes the current node, the next of 'current' becomes the next

### 7. Tree Breath First Search

#### Pattern:
1. All nodes are explored at the present depth before moving to nodes at the next depth level
2. Usually implemented using a queue

Solution:
1. Start by pushing **root** node into the queue
2. Keeps iterating until the queue becomes empty
	1. count the number of elements in the level, say **levelSize**
	2. remove the **levelSize** nodes from the queue
	3. push their value into an array representing the level
	4. insert the node's children into the queue


----

https://github.com/PinkyJie/leetcode-patterns

1. Sliding window
2. Two pointers
3. Fast slow pointers
4. Merge intervals
5. Cyclic sort
6. In place reversal of a linked list
7. BFS
8. DFS
9. Two heaps
10. Subsets
11. Binary search
12. Bitwise XOR
13. Top K elements
14. K-way merge
15. Dynamic programming
16. Topological sort
17. Divide and Conquer




