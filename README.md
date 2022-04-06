# Several Coding Patterns for Solving Data Structures and Algorithms Problems during Interviews

These are my JavaScript notes from a course that categorizes coding interview problems into a set of <b>16 patterns</b>. 

|   |   |
|---|---|
| <b>[Pattern 1: Sliding Window](./✅%20%20Pattern%2001%20:%20Sliding%20Window.md)</b>|<b>[Pattern 9: Two Heaps](./✅%20🙃%20Pattern%2009:%20Two%20Heaps.md)</b>   |
|<b>[Pattern 2: Two Pointer](./✅%20%20Pattern%2002:%20Two%20Pointers.md)</b>|<b>[Pattern 10: Subsets](./✅%20%20Pattern%2010:%20Subsets.md)</b>|
|<b>[Pattern 3: Fast & Slow pointers](./✅%20%20Pattern%2003:%20Fast%20%26%20Slow%20pointers.md)</b>|<b>[Pattern 11: Modified Binary Search](./✅%20%20Pattern%2011:%20Modified%20Binary%20Search.md)</b>|
|<b>[Pattern 4: Merge Intervals](./✅%20%20Pattern%2004%20:%20Merge%20Intervals.md)</b>|<b>[Pattern 12: Bitwise XOR](./✅%20Pattern%2012:%20%20Bitwise%20XOR.md)</b>|
|<b>[Pattern 5: Cyclic Sort](./✅%20%20Pattern%2005:%20Cyclic%20Sort.md)</b>|<b>[Pattern 13: Top 'K' Elements](./Pattern%2013:%20Top%20'K'%20Elements.md)</b>|
|<b>[Pattern 6: In-place Reversal of a LinkedList](./✅%20%20Pattern%2006:%20In-place%20Reversal%20of%20a%20LinkedList.md)</b>|<b>[Pattern 14: K-way merge](./Pattern%2014:%20K-way%20merge.md)</b>|
|<b>[Pattern 7: Tree Breadth First Search](./✅%20%20Pattern%2007:%20Tree%20Breadth%20First%20Search.md)</b>|<b>[Pattern 15: 0/1 Knapsack (Dynamic Programming)](./%E2%9C%85%20Pattern%2015:%200-1%20Knapsack%20(Dynamic%20Programming).md)</b>|
|<b>[Pattern 8: Depth First Search (DFS)](./✅%20%20Pattern%2008:Tree%20Depth%20First%20Search.md)</b>|<b>[Pattern 16: Topological Sort (Graph)](./%E2%9C%85%20Pattern%2016:%20%F0%9F%94%8D%20Topological%20Sort%20(Graph).md)</b>|

## [Pattern 1: Sliding Window](./✅%20%20Pattern%2001%20:%20Sliding%20Window.md)

In many problems dealing with an array (or a LinkedList), we are asked to find or calculate something among all the contiguous subarrays (or sublists) of a given size. For example, take a look at this problem:

> Given an array, find the average of all contiguous subarrays of size ‘K’ in it.

Let’s understand this problem with a real input:

`Array: [1, 3, 2, 6, -1, 4, 1, 8, 2], K=5`

A brute-force algorithm will calculate the sum of every 5-element contiguous subarray of the given array and divide the sum by ‘5’ to find the average.

The efficient way to solve this problem would be to visualize each contiguous subarray as a sliding window of `‘5’` elements. This means that we will slide the window by one element when we move on to the next subarray. To reuse the sum from the previous subarray, we will subtract the element going out of the window and add the element now being included in the sliding window. This will save us from going through the whole subarray to find the sum and, as a result, the algorithm complexity will reduce to `O(N)`.

## [Pattern 2: Two Pointer](./✅%20%20Pattern%2002:%20Two%20Pointers.md)

In problems where we deal with sorted arrays (or LinkedLists) and need to find a set of elements that fulfill certain constraints, the Two Pointers approach becomes quite useful. The set of elements could be a pair, a triplet or even a subarray. For example, take a look at the following problem:

> Given an array of sorted numbers and a target sum, find a pair in the array whose sum is equal to the given target.

To solve this problem, we can consider each element one by one (pointed out by the first pointer) and iterate through the remaining elements (pointed out by the second pointer) to find a pair with the given sum. The time complexity of this algorithm will be `O(N^2)` where `‘N’` is the number of elements in the input array.

Given that the input array is sorted, an efficient way would be to start with one pointer in the beginning and another pointer at the end. At every step, we will see if the numbers pointed by the two pointers add up to the target sum. If they do not, we will do one of two things:
1. If the sum of the two numbers pointed by the two pointers is greater than the target sum, this means that we need a pair with a smaller sum. So, to try more pairs, we can decrement the end-pointer.
2. If the sum of the two numbers pointed by the two pointers is smaller than the target sum, this means that we need a pair with a larger sum. So, to try more pairs, we can increment the start-pointer.

## [Pattern 3: Fast & Slow pointers](./✅%20%20Pattern%2003:%20Fast%20%26%20Slow%20pointers.md)

The <b>Fast & Slow</b> pointer approach, also known as the <b>Hare & Tortoise algorithm</b>, is a pointer algorithm that uses two pointers which move through the array (or sequence/LinkedList) at different speeds. This approach is quite useful when dealing with cyclic LinkedLists or arrays.

By moving at different speeds (say, in a cyclic LinkedList), the algorithm proves that the two pointers are bound to meet. The fast pointer should catch the slow pointer once both the pointers are in a cyclic loop.

One of the famous problems solved using this technique was <b>Finding a cycle in a LinkedList</b>. Let’s jump onto this problem to understand the <b>Fast & Slow</b> pattern.

## [Pattern 4: Merge Intervals](./✅%20%20Pattern%2004%20:%20Merge%20Intervals.md)

This pattern describes an efficient technique to deal with overlapping intervals. In a lot of problems involving intervals, we either need to find overlapping intervals or merge intervals if they overlap.

Given two intervals (`a` and `b`), there will be six distinct ways the two intervals can relate to each other:
1. `a` and `b`do not overlap
2. `a` and `b` overlap, `b` ends after `a`
3. `a` completely overlaps `b`
4. `a` and `b` overlap, `a` ends after `b`
5. `b` completly overlaps `a`
6. `a` and `b` do not overlap

Understanding the above six cases will help us in solving all intervals related problems.
![](./images/mergeintervals.png)

## [Pattern 5: Cyclic Sort](./✅%20%20Pattern%2005:%20Cyclic%20Sort.md)

This pattern describes an interesting approach to deal with problems involving arrays containing numbers in a given range. For example, take the following problem:

>You are given an unsorted array containing numbers taken from the range 1 to ‘n’. The array can have duplicates, which means that some numbers will be missing. Find all the missing numbers.

To efficiently solve this problem, we can use the fact that the input array contains numbers in the range of `1` to `‘n’`. 
For example, to efficiently sort the array, we can try placing each number in its correct place, i.e., placing `‘1’` at index `‘0’`, placing `‘2’` at index `‘1’`, and so on. Once we are done with the sorting, we can iterate the array to find all indices that are missing the correct numbers. These will be our required numbers.

## [Pattern 6: In-place Reversal of a LinkedList](./✅%20%20Pattern%2006:%20In-place%20Reversal%20of%20a%20LinkedList.md)

In a lot of problems, we are asked to reverse the links between a set of nodes of a <b>LinkedList</b>. Often, the constraint is that we need to do this in-place, i.e., using the existing node objects and without using extra memory.

<b>In-place Reversal of a LinkedList pattern</b> describes an efficient way to solve the above problem.

## [Pattern 7: Tree Breadth First Search](./✅%20%20Pattern%2007:%20Tree%20Breadth%20First%20Search.md)
This pattern is based on the <b>Breadth First Search (BFS)</b> technique to traverse a tree.

Any problem involving the traversal of a tree in a level-by-level order can be efficiently solved using this approach. We will use a <b>Queue</b> to keep track of all the nodes of a level before we jump onto the next level. This also means that the space complexity of the algorithm will be `O(W)`, where `W` is the maximum number of nodes on any level.

## [Pattern 8: Depth First Search (DFS)](./✅%20%20Pattern%2008:Tree%20Depth%20First%20Search.md)

This pattern is based on the <b>Depth First Search (DFS)</b> technique to traverse a tree.

We will be using recursion (or we can also use a stack for the iterative approach) to keep track of all the previous (parent) nodes while traversing. This also means that the space complexity of the algorithm will be `O(H)`, where `‘H’` is the maximum height of the tree.

## [Pattern 9: Two Heaps](./✅%20🙃%20Pattern%2009:%20Two%20Heaps.md)

In many problems, where we are given a set of elements such that we can divide them into two parts. To solve the problem, we are interested in knowing the smallest element in one part and the biggest element in the other part. This pattern is an efficient approach to solve such problems.

This pattern uses two <b>Heaps</b> to solve these problems; A <b>Min Heap</b> to find the smallest element and a <b>Max Heap</b> to find the biggest element.

## [Pattern 10: Subsets](./✅%20%20Pattern%2010:%20Subsets.md)

A huge number of coding interview problems involve dealing with <b>Permutations</b> and <b>Combinations</b> of a given set of elements. This pattern describes an efficient <b>Breadth First Search (BFS)</b> approach to handle all these problems.

## [Pattern 11: Modified Binary Search](./✅%20%20Pattern%2011:%20Modified%20Binary%20Search.md)

As we know, whenever we are given a sorted <b>Array</b> or <b>LinkedList</b> or <b>Matrix</b>, and we are asked to find a certain element, the best algorithm we can use is the <b>Binary Search</b>.
## [Pattern 12: Bitwise XOR](./✅%20Pattern%2012:%20%20Bitwise%20XOR.md)

<b>XOR</b> is a logical bitwise operator that returns `0` (false) if both bits are the same and returns `1` (true) otherwise. In other words, it only returns `1` if exactly one bit is set to `1` out of the two bits in comparison.

## [Pattern 13: Top 'K' Elements](./Pattern%2013:%20Top%20'K'%20Elements.md)

Any problem that asks us to find the top/smallest/frequent ‘K’ elements among a given set falls under this pattern.

The best data structure that comes to mind to keep track of ‘K’ elements is Heap. This pattern will make use of the Heap to solve multiple problems dealing with ‘K’ elements at a time from a set of given elements.

## [Pattern 14: K-way merge](./Pattern%2014:%20K-way%20merge.md)

## [Pattern 15: 0/1 Knapsack (Dynamic Programming)](./%E2%9C%85%20Pattern%2015:%200-1%20Knapsack%20(Dynamic%20Programming).md)
<b>0/1 Knapsack pattern</b> is based on the famous problem with the same name which is efficiently solved using <b>Dynamic Programming (DP)</b>.

In this pattern, we will go through a set of problems to develop an understanding of <b>DP</b>. We will always start with a brute-force recursive solution to see the overlapping subproblems, i.e., realizing that we are solving the same problems repeatedly.

After the recursive solution, we will modify our algorithm to apply advanced techniques of <b>Memoization</b> and <b>Bottom-Up Dynamic Programming</b> to develop a complete understanding of this pattern.

## [Pattern 16: 🔍 Topological Sort (Graph)](./%E2%9C%85%20Pattern%2016:%20%F0%9F%94%8D%20Topological%20Sort%20(Graph).md)
<b>Topological Sort</b> is used to find a linear ordering of elements that have dependencies on each other. For example, if event `B` is dependent on event `A`, `A` comes before `B` in topological ordering.


