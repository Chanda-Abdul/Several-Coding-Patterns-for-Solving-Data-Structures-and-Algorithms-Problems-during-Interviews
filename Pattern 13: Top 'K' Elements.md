# Pattern 13: Top 'K' Elements
* It has taken longer than planned for me to work through this pattern in JavaScript. I don't believe that heaps are the most effecient way to tackle these problems during an interview.  It seems like it would be easier to implement some kind of sorting algorithm like quicksort.  Please let me know your thoughts and opinions on this.
#

Any problem that asks us to find the <b>top/smallest/frequent K</b> elements among a given set falls under this pattern.

The best data structure that comes to mind to keep track of <b>K</b> elements is <s>Heap</s>. This pattern will make use of the <s><b>Heap</b></s> to solve multiple problems dealing with <b>K</b> elements at a time from a set of given elements.

### ❗ NOTE 

<b>JavaScript</b> does not have a built in method for <b>heaps</b>. It could be an ineffecient use of time during an actual interview to create a <b>Heap class</b> from scratch.  If you want to learn how to implement a <b>Heap Class</b> in <b>JavaScript</b> take a look at this [article](https://blog.bitsrc.io/implementing-heaps-in-javascript-c3fbf1cb2e65).

## 😕 Top 'K' Numbers (easy)
> Given an unsorted array of numbers, find the ‘K’ largest numbers in it.

<b>Note:</b> For a detailed discussion about different approaches to solve this problem, take a look at [Kth Smallest Number](#kth-smallest-number-easy).

A <b>brute force solution</b> could be to sort the array and return the <b>largest K numbers</b>. The time complexity of such an algorithm will be `O(N*logN)` as we need to use a sorting algorithm like <b>[Quicksort](https://github.com/Chanda-Abdul/leetcode/blob/master/%E2%9D%97Sort%20Algorithms.md#-quick-sort)</b>. Can we do better than that?

<s>The best data structure that comes to mind to keep track of top ‘K’ elements is Heap. Let’s see if we can use a heap to find a better algorithm.

If we iterate through the array one element at a time and keep ‘K’ largest numbers in a heap such that each time we find a larger number than the smallest number in the heap, we do two things:
1. Take out the smallest number from the heap, and
2. Insert the larger number into the heap.
  
This will ensure that we always have ‘K’ largest numbers in the heap. The most efficient way to repeatedly find the smallest number among a set of numbers will be to use a min-heap. As we know, we can find the smallest number in a min-heap in constant time `O(1)`, since the smallest number is always at the root of the heap. Extracting the smallest number from a min-heap will take `O(logN)` (if the heap has ‘N’ elements) as the heap needs to readjust after the removal of an element.</s>

Let’s take <b>Example 1</b> to go through each step of our algorithm:

Given array: `[3, 1, 5, 12, 2, 11]`, and `K=3`
<s>

1. First, let’s insert ‘K’ elements in the min-heap.
2. After the insertion, the heap will have three numbers `[3, 1, 5]` with ‘1’ being the root as it is the smallest element.
3. We’ll iterate through the remaining numbers and perform the above-mentioned two steps if we find a number larger than the root of the heap.
4. The 4th number is ‘12’ which is larger than the root (which is ‘1’), so let’s take out ‘1’ and insert ‘12’. Now the heap will have `[3, 5, 12]` with ‘3’ being the root as it is the smallest element.
5. The 5th number is ‘2’ which is not bigger than the root of the heap (‘3’), so we can skip this as we already have top three numbers in the heap.
6. The last number is ‘11’ which is bigger than the root (which is ‘3’), so let’s take out ‘3’ and insert ‘11’. Finally, the heap has the largest three numbers: [5, 12, 11]
  
As discussed above, it will take us `O(logK)` to extract the minimum number from the min-heap. So the overall time complexity of our algorithm will be `O(K*logK+(N-K)*logK)` since, first, we insert ‘K’ numbers in the heap and then iterate through the remaining numbers and at every step, in the worst case, we need to extract the minimum number and insert a new number in the heap. This algorithm is better than `O(N*logN)`.</s>

````js
function findKLargestNumbers(nums, k) {
  return quickSort(nums).slice(-k);
}

function quickSort(array) {
  //recursive base case
  if (array.length === 1) return array;

  //set pivot to end value
  const pivotPoint = array[array.length - 1];
  const firstHalf = [];
  const secondHalf = [];

  for (let i = 0; i < array.length - 1; i++) {
    if (array[i] < pivotPoint) {
      firstHalf.push(array[i]);
    } else {
      secondHalf.push(array[i]);
    }
  }

  //recursively sort
  if (firstHalf.length > 0 && secondHalf.length > 0) {
    return [...quickSort(firstHalf), pivotPoint, ...quickSort(secondHalf)];
  } else if (firstHalf.length > 0) {
    return [...quickSort(firstHalf), pivotPoint];
  } else {
    //secondHalf.length> 0
    return [pivotPoint, ...quickSort(secondHalf)];
  }
}

console.log(`Here are the top K numbers: ${findKLargestNumbers([3, 1, 5, 12, 2, 11], 3)}`);
console.log(`Here are the top K numbers: ${findKLargestNumbers([5, 12, 11, -1, 12], 3)}`);
````
- As discussed above, the time complexity of this algorithm is `O(K * log K +(N - K) * logK)`, which is asymptotically equal to `O(N*logK)`.
- The space complexity will be `O(K)` since we need to store the top `K` numbers in an array.
## Kth Smallest Number (easy)
https://leetcode.com/problems/kth-largest-element-in-an-array/
> Given an unsorted array of numbers, find `Kth` smallest number in it.
> 
> Please note that it is the `Kth` smallest number in the sorted order, not the `Kth` distinct element.
````js
function findKLargestNumbers(nums, k) {
  const finalIndex = nums.length - k;
  let start = 0;
  let end = nums.length - 1;

  while (start <= end) {
    //random number between start and end for pivot
    const pivot = Math.floor(Math.random() * (end - start + 1) + start);
    //final postion of the pivot in a sorted array
    const pivotIndex = sort(nums, pivot, start, end);
    if (pivotIndex === finalIndex) return nums[finalIndex];

    //if pivotIndex is smaller we undershot, so look only on the first half
    if (pivotIndex < finalIndex) start = pivotIndex + 1;
    //if pivotIndex is larger we overshot, so look only on the first half
    else end = pivotIndex - 1;
  }
}

function sort(array, pivot, start, end) {
  //swap the pivot to the end
  [array[pivot], array[end]] = [array[end], array[pivot]];

  let i = start;
  let j = start;

  while (j < end) {
    if (array[j] <= array[end]) {
      [array[i], array[j]] = [array[j], array[i]];
      i++;
    }
    j++;
  }

  //swap pivot to its final position
  [array[i], array[end]] = [array[end], array[i]];
  return i;
}

console.log(`Here is the top K number: ${findKLargestNumbers([3,2,3,1,2,4,5,5,6], 4)}`);
//4
console.log(`Here is the top K number: ${findKLargestNumbers([3, 1, 5, 12, 2, 11], 3)}`);
//5
console.log(`Here is the top K number: ${findKLargestNumbers([5, 12, 11, -1, 12], 3)}`);
//11
````

- As discussed above, the time complexity of this algorithm is `O(K * log K +(N - K) * logK)`, which is asymptotically equal to `O(N*logK)`.
- The space complexity will be `O(K)` since we need to store the top `K` numbers in an array.
## 'K' Closest Points to the Origin (easy)
https://leetcode.com/problems/k-closest-points-to-origin/
> Given an array of points in a 2D plane, find `K` closest points to the origin.

<b>Note:</b> For a detailed discussion about different approaches to solve this problem, take a look at [Kth Smallest Number](#kth-smallest-number-easy).
````js
function findClosestPoints(points, k) {
  let result = [];
  points = points.sort(
    ([x1, y1], [x2, y2]) =>
      Math.pow(x1, 2) + Math.pow(y1, 2) - (Math.pow(x2, 2) + Math.pow(y2, 2))
  );
  result = [...points.slice(0, k)];

  return result;
}
console.log(`"Here are the k points closest the origin: " ${findClosestPoints ([[1,2],[1,3]], 1)}`);
//The Euclidean distance between (1, 2) and the origin is sqrt(5).
//The Euclidean distance between (1, 3) and the origin is sqrt(10).
//Since sqrt(5) < sqrt(10), therefore (1, 2) is closer to the origin.

console.log(`"Here are the k points closest the origin: " ${findClosestPoints ([[1, 3], [3, 4], [2, -1]], 2)}`);
//[[1, 3], [2, -1]]

````
## Connect Ropes (easy)
https://leetcode.com/problems/minimum-cost-to-connect-sticks/
> Given ‘N’ ropes with different lengths, we need to connect these ropes into one big rope with minimum cost. The cost of connecting two ropes is equal to the sum of their lengths.
## 👩‍🦯 Top 'K' Frequent Numbers (medium)
https://leetcode.com/problems/top-k-frequent-elements/
> Given an unsorted array of numbers, find the top ‘K’ frequently occurring numbers in it.
## Frequency Sort (medium)
https://leetcode.com/problems/sort-characters-by-frequency/
> Given a string, sort it based on the decreasing frequency of its characters.
## Kth Largest Number in a Stream (medium)
https://leetcode.com/problems/kth-largest-element-in-a-stream/
> Design a class to efficiently find the Kth largest element in a stream of numbers.
> 
> The class should have the following two things:
> 
> 1. The constructor of the class should accept an integer array containing initial numbers from the stream and an integer ‘K’.
> 2. The class should expose a function add(int num) which will store the given number and return the Kth largest number.
## 'K' Closest Numbers (medium)
https://leetcode.com/problems/find-k-closest-elements/
> Given a sorted number array and two integers ‘K’ and ‘X’, find ‘K’ closest numbers to ‘X’ in the array. Return the numbers in the sorted order. ‘X’ is not necessarily present in the array.
## Maximum Distinct Elements (medium)
https://leetcode.com/problems/least-number-of-unique-integers-after-k-removals/
> Given an array of numbers and a number ‘K’, we need to remove ‘K’ numbers from the array such that we are left with maximum distinct numbers.
## Sum of Elements (medium)
https://www.geeksforgeeks.org/sum-elements-k1th-k2th-smallest-elements/
> Given an array, find the sum of all numbers between the K1’th and K2’th smallest elements of that array.
## Rearrange String (hard)
https://leetcode.com/problems/reorganize-string/
> Given a string, find if its letters can be rearranged in such a way that no two same characters come next to each other.
## 🌟 Rearrange String K Distance Apart (hard)
https://leetcode.com/problems/rearrange-string-k-distance-apart/
> Given a string and a number ‘K’, find if the string can be rearranged such that the same characters are at least ‘K’ distance apart from each other.
## 🌟 🔍 Scheduling Tasks (hard)
https://leetcode.com/problems/task-scheduler/
> You are given a list of tasks that need to be run, in any order, on a server. Each task will take one CPU interval to execute but once a task has finished, it has a cooling period during which it can’t be run again. If the cooling period for all tasks is ‘K’ intervals, find the minimum number of CPU intervals that the server needs to finish all tasks.
> 
> If at any time the server can’t execute any task then it must stay idle.
## 🌟Frequency Stack (hard)
https://leetcode.com/problems/maximum-frequency-stack/
> Design a class that simulates a Stack data structure, implementing the following two operations:
> 
> - push(int num): Pushes the number ‘num’ on the stack.
> - pop(): Returns the most frequent number in the stack. If there is a tie, return the number which was pushed later.

