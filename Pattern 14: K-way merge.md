# Pattern 14: K-way merge
This pattern helps us solve problems that involve a list of sorted arrays.

Whenever we are given `K` sorted arrays, we can use a <b>Heap</b> to efficiently perform a sorted traversal of all the elements of all arrays. We can push the smallest (first) element of each sorted array in a <b>Min Heap</b> to get the overall minimum. While inserting elements to the <b>Min Heap</b> we keep track of which array the element came from. We can, then, remove the top element from the heap to get the smallest element and push the next element from the same array, to which this smallest element belonged, to the heap. We can repeat this process to make a sorted traversal of all elements.

## 👩‍🦯 Merge K Sorted Lists (medium)
https://leetcode.com/problems/merge-k-sorted-lists/
> Given an array of `K` sorted <b>LinkedLists</b>, merge them into one sorted list.
## Kth Smallest Number in M Sorted Lists (Medium)
> Given `M` sorted arrays, find the `Kth` smallest number among all the arrays.
## 🌴 Kth Smallest Number in a Sorted Matrix (Hard)
https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/
> Given an `N * N` matrix where each row and column is sorted in ascending order, find the `Kth` smallest element in the matrix.
## Smallest Number Range (Hard)
https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/
Given `M` sorted arrays, find the smallest range that includes at least one number from each of the `M` lists.

## 🌟 K Pairs with Largest Sums (Hard)
https://leetcode.com/problems/find-k-pairs-with-smallest-sums/
> Given two sorted arrays in descending order, find `K` pairs with the largest sum where each pair consists of numbers from both the arrays.
