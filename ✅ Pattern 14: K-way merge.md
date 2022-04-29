# Pattern 14: K-way merge

This pattern helps us solve problems that involve a list of sorted arrays.

Whenever we are given `K` sorted arrays, we can use a <b>Heap</b> to efficiently perform a sorted traversal of all the elements of all arrays. We can push the smallest (first) element of each sorted array in a <b>Min Heap</b> to get the overall minimum. While inserting elements to the <b>Min Heap</b> we keep track of which array the element came from. We can, then, remove the top element from the <b>heap</b> to get the smallest element and push the next element from the same array, to which this smallest element belonged, to the <b>heap</b>. We can repeat this process to make a sorted traversal of all elements.

Although this course uses <b>Heaps</b> to solve <b>Top 'K' Elements</b> problems, <b>JavaScript</b> does not have a built in method for <b>Heaps/Priority Queues</b>. It can be very time consuming to implement a <b>Heap class</b> from scratch, especially during an interview. After reviewing the <i>JavaScript</i> solutions on <i>Leetcode</i> the most effecient way to solve a <b>Top 'K' Elements</b> problem is usually with <b>[QuickSort](https://github.com/Chanda-Abdul/leetcode/blob/master/0%20%E2%9D%97Sort%20Algorithms.md#-quick-sort)</b>, <b>[BinarySearch](https://github.com/Chanda-Abdul/leetcode/blob/master/0%20%E2%9D%97Sort%20Algorithms.md#binary-search)</b>, <b>[BucketSort](https://initjs.org/bucket-sort-in-javascript-dc040b8f0058)</b>, <b>[Greedy Algorithms](https://github.com/Chanda-Abdul/Grokking-Algorithm-Book-Notes/blob/main/8.%20Greedy%20Algoritms.md)</b>, or <b>[HashMaps](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)</b>. 

## 👩🏽‍🦯 🌴 😐📖 Merge K Sorted Lists (medium)

https://leetcode.com/problems/merge-k-sorted-lists/

> Given an array of `K` sorted <b>LinkedLists</b>, merge them into one sorted list.

### Array Sort

```js
class ListNode {
  constructor(value, next = null) {
    this.value = value;
    this.next = next;
  }
}

function mergeLists(lists) {
  //push to list
  if (!lists || !lists.length) return null;
  let mergeArr = [];
  let resultList = new ListNode(-1);

  lists.forEach((list) => {
    let curr = list;
    while (curr) {
      mergeArr.push(curr.value);
      curr = curr.next;
    }
  });
  let curr = resultList;

  //sort list
  mergeArr
    .sort((a, b) => a - b)
    .forEach((n) => {
      let temp = new ListNode(n);
      curr.next = temp;
      curr = curr.next;
    });

  return resultList.next;
}

let l1 = new ListNode(2);
l1.next = new ListNode(6);
l1.next.next = new ListNode(8);

let l2 = new ListNode(3);
l2.next = new ListNode(6);
l2.next.next = new ListNode(7);

l1.next = new ListNode(6);

let l3 = new ListNode(1);
l3.next = new ListNode(3);
l3.next.next = new ListNode(4);

l2.next = new ListNode(6);
result = mergeLists([l1, l2, l3]);

let output = "Here are the elements form the merged list: ";
while (result != null) {
  output += result.value + " ";
  result = result.next;
}
console.log(output);
```

### Compare Each Node One by One

```js
function mergeLists(lists) {
  //compare one by one
  if (!lists || !lists.length) return null;

  function findMinNode(lists) {
    let index = -1;
    let min = Infinity;

    for (let i = 0; i < lists.length; i++) {
      if (!lists[i]) continue;
      if (lists[i].value <= min) {
        min = lists[i].value;
        index = i;
      }
    }

    let resultNode = null;

    if (index !== -1) {
      resultNode = lists[index];
      lists[index] = lists[index].next;
    }

    return resultNode;
  }

  let resultList = new ListNode(-1);
  let curr = resultList;
  let temp = findMinNode(lists);

  while (temp) {
    curr.next = temp;
    curr = curr.next;
    temp = findMinNode(lists);
  }

  return resultList.next;
}
```

## 🔎 Kth Smallest Number in M Sorted Lists (Medium)

> Given `M` sorted arrays, find the `Kth` smallest number among all the arrays.

```js
function findKthSmallest(lists, k) {
  let minHeap = new Heap();

  // put the 1st element of each list in the min heap
  for (let i = 0; i < lists.length; i++) {
    minHeap.insert(lists[i][0]);
  }
  console.log(minHeap);
  // take the smallest(i.e., top) element form the min heap, if the running count is equal to k return the number
  let numberCount = 0,
    number = 0;
  while (minHeap.size > 0) {
    [number, i, list] = minHeap.remove();
    numberCount += 1;
    if (numberCount === k) {
      break;
    }
    // if the array of the top element has more elements, add the next element to the heap
    if (list.length > i + 1) {
      minHeap.insert([list[i + 1], i + 1, list]);
    }
  }
  return number;
}

console.log(
  `Kth smallest number is: ${findKthSmallest(
    [
      [2, 6, 8],
      [3, 6, 7],
      [1, 3, 4],
    ],
    5
  )}`
);
```

### Similar Problems

### 🔎 🌴 Median of Two Sorted Arrays

https://leetcode.com/problems/median-of-two-sorted-arrays/

> Given `M` sorted arrays, find the median number among all arrays.

<b>Solution:</b> This problem is similar to our parent problem with K=Median. So if there are `N` total numbers in all the arrays we need to find the `Kth` minimum number where `K=N/2`.

### 👩🏽‍🦯 🌴 Merge K Sorted Arrays

https://leetcode.com/problems/merge-k-sorted-lists/

> Given a list of `K` sorted arrays, merge them into one sorted list.

<b>Solution:</b> This problem is similar to [Merge K Sorted Lists](#🔎-median-of-two-sorted-arrays) except that the input is a list of arrays compared to LinkedLists. To handle this, we can use a similar approach as discussed in our parent problem by keeping a track of the array and the element indices.

## 🔎 🌴 Kth Smallest Number in a Sorted Matrix (Hard)

https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/

> Given an `N * N` matrix where each row and column is sorted in ascending order, find the `Kth` smallest element in the matrix.

This problem follows the [K-way merge pattern](#pattern-14-k-way-merge) and can be easily converted to [Kth Smallest Number in M Sorted Lists](#🔎-kth-smallest-number-in-m-sorted-lists-medium). As each row (or column) of the given matrix can be seen as a sorted list, we essentially need to find the `Kth` smallest number in `N` sorted lists.

Since each row and column of the matrix is sorted, is it possible to use <b>Binary Search</b> to find the `Kth`smallest number?

The biggest problem to use <b>Binary Search</b>, in this case, is that we don’t have a straightforward sorted array, instead, we have a matrix. As we remember, in <b>Binary Search</b>, we calculate the middle index of the search space <i>(‘1’ to ‘N’)</i> and see if our required number is pointed out by the middle index; if not we either search in the lower half or the upper half. In a sorted matrix, we can’t really find a middle. Even if we do consider some index as middle, it is not straightforward to find the search space containing numbers bigger or smaller than the number pointed out by the middle index.

An alternative could be to apply the <b>Binary Search</b> on the <i>“number range”</i> instead of the <i>“index range”</i>. As we know that the smallest number of our matrix is at the top left corner and the biggest number is at the bottom right corner. These two numbers can represent the <i>“range”</i> i.e., the `start` and the `end` for the <b>Binary Search</b>. Here is how our algorithm will work:

1. Start the <b>Binary Search</b> with `start = matrix[0][0]` and `end = matrix[n-1][n-1]`.
2. Find `middle` of the `start` and the `end`. This middle number is NOT necessarily an element in the matrix.
3. Count all the numbers smaller than or equal to `middle` in the matrix. As the matrix is sorted, we can do this in `O(N)`.
4. While counting, we can keep track of the <i>“smallest number greater than the middle”</i> (let’s call it `n1`) and at the same time the <i>“biggest number less than or equal to the middle” </i>(let’s call it `n2`). These two numbers will be used to adjust the <i>“number range”</i> for the <b>Binary Search</b> in the next iteration.
5. If the count is equal to <b>`K`</b>, `n2` will be our required number as it is the <i>“biggest number less than or equal to the middle”</i>, and is definitely present in the matrix.
   If the count is less than <b>`K`</b>, we can update `start = n2` to search in the higher part of the matrix and if the count is greater than <b>`K`</b>, we can update `end = n1` to search in the lower part of the matrix in the next iteration.

![](./images/kwaymatrix.png)

```js
function findKthsmallest(matrix, k) {
  const n = matrix.length;
  let start = matrix[0][0];
  let end = matrix[n - 1][n - 1];

  while (start < end) {
    const mid = Math.floor(start + (end - start) / 2);

    const [count, smaller, larger] = countLessEqual(
      matrix,
      mid,
      matrix[0][0],
      matrix[n - 1][n - 1]
    );

    if (count === k) return smaller;
    if (count < k) {
      //search higher
      start = larger;
    } else {
      //search lower
      end = smaller;
    }
  }

  return start;
}

function countLessEqual(matrix, mid, smaller, larger) {
  let count = 0;
  let n = matrix.length;
  let row = n - 1;
  let col = 0;

  while (row >= 0 && col < n) {
    if (matrix[row][col] > mid) {
      //as matrix[row][col] is bigger than the mid,
      //keep track of the smallest number greater than the mid
      larger = Math.min(larger, matrix[row][col]);
      row--;
    } else {
      // as matrix[row][col] is <= mid
      // keep track of the biggest number <= mid
      smaller = Math.max(smaller, matrix[row][col]);
      count += row + 1;
      col++;
    }
  }
  return [count, smaller, larger];
}

console.log(
  `Kth smallest number is: ${findKthsmallest(
    [
      [1, 4],
      [2, 5],
    ],
    2
  )}`
);

console.log(`Kth smallest number is: ${findKthsmallest([[-5]], 1)}`);

console.log(
  `Kth smallest number is: ${findKthsmallest(
    [
      [2, 6, 8],
      [3, 7, 10],
      [5, 8, 11],
    ],
    5
  )}`
);

console.log(
  `Kth smallest number is: ${findKthsmallest(
    [
      [1, 5, 9],
      [10, 11, 13],
      [12, 13, 15],
    ],
    8
  )}`
);
```

- The <b>Binary Search</b> could take `O(log(max-min ))` iterations where `max` is the largest and `min` is the smallest element in the matrix and in each iteration we take `O(N)`
  for counting, therefore, the overall time complexity of the algorithm will be `O(N*log(max-min))`.
- The algorithm runs in constant space `O(1)`.

## 📍 Smallest Number Range (Hard)

https://leetcode.com/problems/smallest-range-covering-elements-from-k-lists/

> Given `M` sorted arrays, find the smallest range that includes at least one number from each of the `M` lists.

This problem follows the [K-way merge pattern](#pattern-14-k-way-merge) and we can follow a similar approach as discussed in [Merge K Sorted Lists](#👩🏽‍🦯-🌴-merge-k-sorted-arrays).

We can start by inserting the first number from all the arrays in a `min-heap()`. We will keep track of the largest number that we have inserted in the heap (let’s call it `maxNumber`).

In a loop, we’ll take the smallest (top) element from the `min-heap()` and `maxNumber` has the largest element that we inserted in the heap. If these two numbers give us a smaller range, we’ll update our range. Finally, if the array of the top element has more elements, we’ll insert the next element to the heap.

We can finish searching the minimum range as soon as an array is completed or, in other terms, the <b>heap</b> has less than `M` elements.

```js
class MinHeap {
  constructor() {
    this.list = []; // a list of [num, groupID]
    this.size = 0;
  }

  push(item) {
    const list = this.list;
    const size = ++this.size;

    list[size - 1] = item;
    this.bubbleUp(size - 1);
  }

  pop() {
    if (this.size === 0) return;

    const list = this.list;
    const size = this.size;
    const item = list[0];

    [list[0], list[size - 1]] = [list[size - 1], list[0]];
    this.size--;
    this.bubbleDown(0);
    return item;
  }

  bubbleUp(index) {
    const list = this.list;
    const size = this.size;
    const parent = Math.floor((index - 1) / 2);

    if (parent < 0 || parent >= size) return;
    if (index < 0 || index >= size) return;

    if (list[index][0] < list[parent][0]) {
      [list[index], list[parent]] = [list[parent], list[index]];
      this.bubbleUp(parent);
    }
  }

  bubbleDown(index) {
    if (index < 0 || index >= this.size) return;

    const list = this.list;
    const size = this.size;
    const left = index * 2 + 1;
    const right = index * 2 + 2;
    let minVal = list[index][0];
    let minIndex = index;

    if (left >= 0 && left < size) {
      if (list[left][0] < minVal) {
        minVal = list[left][0];
        minIndex = left;
      }
    }
    if (right >= 0 && right < size) {
      if (list[right][0] < minVal) {
        minVal = list[right][0];
        minIndex = right;
      }
    }
    if (minIndex !== index) {
      [list[index], list[minIndex]] = [list[minIndex], list[index]];
      this.bubbleDown(minIndex);
    }
  }
}

function smallestRange(nums) {
  const minHeap = new MinHeap();
  const pointers = Array(nums.length).fill(0);
  let rangeStart = 0;
  let rangeEnd = Infinity;
  let maxNumber = -Infinity;

  // put the first element of each array into the heap
  for (let i = 0; i < nums.length; i++) {
    minHeap.push([nums[i][0], i]);
    maxNumber = Math.max(maxNumber, nums[i][0]);
  }

  // take the smallest(top) element from the heap, if it gives us smaller range, update the ranges
  // if the array of the top element has more elements, insert the next element in the heap
  while (true) {
    let [minNumber, group] = minHeap.pop();

    if (maxNumber - minNumber < rangeEnd - rangeStart) {
      rangeStart = minNumber;
      rangeEnd = maxNumber;
    }

    pointers[group]++;
    if (pointers[group] >= nums[group].length) break;

    // insert the next element into the heap
    minHeap.push([nums[group][pointers[group]], group]);
    maxNumber = Math.max(maxNumber, nums[group][pointers[group]]);
  }

  return [rangeStart, rangeEnd];
}

console.log(`Smallest range is: 
${smallestRange([
  [4, 10, 15, 24, 26],
  [0, 9, 12, 20],
  [5, 18, 22, 30],
])}`);
// [20,24]
// List 1: [4, 10, 15, 24,26], 24 is in range [20,24].
// List 2: [0, 9, 12, 20], 20 is in range [20,24].
// List 3: [5, 18, 22, 30], 22 is in range [20,24].

console.log(`Smallest range is: 
${smallestRange(
  (nums = [
    [1, 2, 3],
    [1, 2, 3],
    [1, 2, 3],
  ])
)}`);
// [1,1]

console.log(`Smallest range is: 
${smallestRange([
  [1, 5, 8],
  [4, 12],
  [7, 8, 10],
])}`);
// The range [4, 7] includes 5 from L1, 4 from L2 and 7 from L3.

console.log(`Smallest range is: 
${smallestRange([
  [1, 9],
  [4, 12],
  [7, 10, 16],
])}`);
// The range [9, 12] includes 9 from L1, 12 from L2 and 10 from L3.
```

- Since, at most, we’ll be going through all the elements of all the arrays and will remove/add one element in the heap in each step, the time complexity of the above algorithm will be `O(N*logM)` where `N` is the total number of elements in all the `M` input arrays.
- The space complexity will be `O(M)` because, at any time, our `min-heap()` will be store one number from all the `M` input arrays.

## 🌟 K Pairs with Largest Sums (Hard)

https://leetcode.com/problems/find-k-pairs-with-smallest-sums/

> Given two sorted arrays in descending order, find `K` pairs with the largest sum where each pair consists of numbers from both the arrays.

This problem follows the [K-way merge pattern](#pattern-14-k-way-merge) and we can follow a similar approach as discussed in [Merge K Sorted Lists](#👩🏽‍🦯-🌴-merge-k-sorted-arrays).

We can go through all the numbers of the two input arrays to create pairs and initially insert them all in the <b>heap</b> until we have `K` pairs in <b>Min Heap</b>. After that, if a pair is bigger than the top (smallest) pair in the <b>heap</b>, we can remove the smallest pair and insert this pair in the <b>heap</b>.

We can optimize our algorithms in two ways:

1. Instead of iterating over all the numbers of both arrays, we can iterate only the first `K` numbers from both arrays. Since the arrays are sorted in descending order, the pairs with the maximum sum will be constituted by the first `K` numbers from both the arrays.
2. As soon as we encounter a pair with a sum that is smaller than the smallest (top) element of the <b>heap</b>, we don’t need to process the next elements of the array. Since the arrays are sorted in descending order, we won’t be able to find a pair with a higher sum moving forward.

😕
```js
class Heap {
  constructor() {
    this.values = [];
  }
  get size() {
    return this.values.length;
  }

  insert(elem) {
    this.values.push(elem);
    let index = this.size - 1;
    if (index === 0) return;

    let parentIndex = Math.floor((index - 1) / 2);

    while (
      this.values[parentIndex] &&
      this.values[parentIndex].val < this.values[index].val
    ) {
      [this.values[parentIndex], this.values[index]] = [
        this.values[index],
        this.values[parentIndex],
      ];
      index = parentIndex;
      parentIndex = Math.floor((index - 1) / 2);
    }
    // console.log(this.values[0].index)
    // this.values.sort((a, b) => b[0] - a[0]))
  }

  extract() {
    😕
    // let parentIndex = this.size - 1;
    // let lastIndex = 0;
    // [this.values[parentIndex], this.values[lastIndex]] = [
    //   this.values[lastIndex],
    //   this.values[parentIndex],
    // ];
    // let result =
        this.values.pop();

//     let leftChildIndex = 2 * parentIndex + 1;
//     let rightChildIndex = 2 * parentIndex + 2;

//     while (
//       (this.values[leftChildIndex] !== undefined &&
//         this.values[leftChildIndex].val > this.values[parentIndex].val) ||
//       (this.values[rightChildIndex] !== undefined &&
//         this.values[rightChildIndex].val > this.values[parentIndex].val)
//     ) {
//       let highestChildIndex;
//       if (
//         this.values[leftChildIndex] === undefined ||
//         this.values[rightChildIndex] === undefined
//       ) {
//         highestChildIndex = leftChildIndex || rightChildIndex;
//       } else {
//         highestChildIndex =
//           this.values[leftChildIndex].val > this.values[rightChildIndex].val
//             ? leftChildIndex
//             : rightChildIndex;
//       }
//       [this.values[parentIndex], this.values[highestChildIndex]] = [
//         this.values[highestChildIndex],
//         this.values[parentIndex],
//       ];
//       parentIndex = highestChildIndex;
//       leftChildIndex = 2 * parentIndex + 1;
//       rightChildIndex = 2 * parentIndex + 2;
//     }
    return result;
  }
}

function findKLargestPairs(nums1, nums2, k) {
  if (!nums1.length || !nums2.length) return [];

  let heap = new Heap();

    for (let i = 0; i < Math.min(k, nums1.length); i++) {
        for (let j = 0; j < Math.min(k, nums2.length); j++) {
            let elem = {
                index: [nums1[i], nums2[j]],
                val: nums1[i] + nums2[j]
            }
            console.log(elem.index, elem.val, heap.values[heap.size-1])
            if (heap.size < k) {
                heap.insert(elem);
            } else if (elem.val < heap.values[heap.size-1]) {
              😕
              // if the sum of the two numbers from the two arrays is smaller than the smallest(top)
      // element of the heap, we can 'break' here. Since the arrays are sorted in the
      // descending order, we'll not be able to find a pair with a higher sum moving forward
                heap.extract();
                heap.insert(elem);


            } else {
               // we have a pair with a larger sum, remove top and insert this pair in the heap
                break;
            }
        }
    }

    return heap.values.map(n => n.index);
};


console.log(`Pairs with largest sum are:
${findKLargestPairs([9, 8, 2], [6, 3, 1], 3)}`);
//[9, 3], [9, 6], [8, 6]
// These 3 pairs have the largest sum. No other pair has a sum larger than any of these.
console.log(
  `Pairs with largest sum are: ${findKLargestPairs([5, 2, 1], [2, -1], 3)}`
);
//[5, 2], [5, -1], [2, 2]
```

- Since, at most, we’ll be going through all the elements of both arrays and we will add/remove one element in the heap in each step, the time complexity of the above algorithm will be `O(N∗M∗logK)` where `N` and `M` are the total number of elements in both arrays, respectively.  If we assume that both arrays have at least `K` elements then the time complexity can be simplified to `O(K^2logK)`, because we are not iterating more than `K` elements in both arrays.
- The space complexity will be `O(K)` because, at any time, our <b>Min Heap</b> will be storing `K` largest pairs.

## 💫 Kth Smallest Number (hard)

https://leetcode.com/problems/find-k-pairs-with-smallest-sums/

> Given an unsorted array of numbers, find `Kth` smallest number in it.

Please note that it is the `Kth` smallest number in the sorted order, not the `Kth` distinct element.

### Example 1:

```
Input: [1, 5, 12, 2, 11, 5], K = 3
Output: 5
Explanation: The 3rd smallest number is '5', as the first two smaller numbers are [1, 2].
```

### Example 2:

```
Input: [1, 5, 12, 2, 11, 5], K = 4
Output: 5
Explanation: The 4th smallest number is '5', as the first three smaller numbers are
[1, 2, 5].
```

### Example 3:

```
Input: [5, 12, 11, -1, 12], K = 3
Output: 11
Explanation: The 3rd smallest number is '11', as the first two small numbers are [5, -1].
```

This is a well-known problem and there are multiple solutions available to solve this. A few other similar problems are:

1. [Find the `Kth` largest number in an unsorted array.](https://www.geeksforgeeks.org/kth-smallestlargest-element-unsorted-array/)
2. [Find the median of an unsorted array.](https://www.geeksforgeeks.org/median-of-an-unsorted-array-in-liner-time-on/)
3. [Find the `K` smallest or largest numbers in an unsorted array.](https://www.geeksforgeeks.org/kth-smallestlargest-element-unsorted-array/)

Let’s discuss different algorithms to solve this problem and understand their time and space complexity. Similar solutions can be devised for the above-mentioned three problems.

### 1. Brute-force

The simplest brute-force algorithm will be to find the `Kth` smallest number in a step by step fashion. This means that, first, we will find the smallest element, then 2nd smallest, then 3rd smallest and so on, until we have found the `Kth` smallest element. Here is what the algorithm will look like:

```js
function findKthSmallestNumber(nums, k) {
  // to handle duplicates, we will keep track of previous smallest number and its index
  let previousSmallestNum = -Infinity,
    previousSmallestIndex = -1;
  (currentSmallestNum = Infinity), (currentSmallestIndex = -1);
  for (i = 0; i < k; i++) {
    for (j = 0; j < nums.length; j++) {
      if (nums[j] > previousSmallestNum && nums[j] < currentSmallestNum) {
        // found the next smallest number
        currentSmallestNum = nums[j];
        currentSmallestIndex = j;
      } else if (nums[j] === previousSmallestNum && j > previousSmallestIndex) {
        // found a number which is equal to the previous smallest number; since numbers can repeat,
        // we will consider 'nums[j]' only if it has a different index than previous smallest
        currentSmallestNum = nums[j];
        currentSmallestIndex = j;
        break; // break here as we have found our definitive next smallest number
      }
    }
    // current smallest number becomes previous smallest number for the next iteration
    previousSmallestNum = currentSmallestNum;
    previousSmallestIndex = currentSmallestIndex;
    currentSmallestNum = Infinity;
  }
  return previousSmallestNum;
}

console.log(
  `Kth smallest number is: ${findKthSmallestNumber([1, 5, 12, 2, 11, 5], 3)}`
);

// since there are two 5s in the input array, our 3rd and 4th smallest numbers should be a '5'
console.log(
  `Kth smallest number is: ${findKthSmallestNumber([1, 5, 12, 2, 11, 5], 4)}`
);

console.log(
  `Kth smallest number is: ${findKthSmallestNumber([5, 12, 11, -1, 12], 3)}`
);
```

- The time complexity of the above algorithm will be `O(N∗K)`. The algorithm runs in constant space `O(1)`.

### 2. Brute-force using Sorting

We can use an <i>in-place sort</i> like a <b>HeapSort</b> to sort the input array to get the `Kth` smallest number. 

Following is the code for this solution:

```js
function findKthSmallestNumber(nums, k) {
  nums.sort((a, b) => a - b);

  return nums[k - 1];
}

console.log(
  `Kth smallest number is: ${findKthSmallestNumber([1, 5, 12, 2, 11, 5], 3)}`
);

// since there are two 5s in the input array, our 3rd and 4th smallest numbers should be a '5'
console.log(
  `Kth smallest number is: ${findKthSmallestNumber([1, 5, 12, 2, 11, 5], 4)}`
);

console.log(
  `Kth smallest number is: ${findKthSmallestNumber([5, 12, 11, -1, 12], 3)}`
);
```

- Sorting will take `O(NlogN)` and if we are not using an in-place sorting algorithm, we will need `O(N)`space.

### 3. Using Max-Heap

As discussed in [Kth Smallest Number](#💫-kth-smallest-number-hard), we can iterate the array and use a <b>Max Heap</b> to keep track of `K` smallest number. In the end, the root of the heap will have the `Kth` smallest number.

Here is what this algorithm will look like:

```js
const Heap = require("./collections/heap"); //http://www.collectionsjs.com

function findKthSmallestNumber(nums, k) {
  maxHeap = new Heap();
  // put first k numbers in the max heap
  for (i = 0; i < k; i++) {
    maxHeap.push(nums[i]);
  }

  // go through the remaining numbers of the array, if the number from the array is smaller than the
  // top(biggest) number of the heap, remove the top number from heap and add the number from array
  for (i = k; i < nums.length; i++) {
    if (nums[i] < maxHeap.peek()) {
      maxHeap.pop();
      maxHeap.push(nums[i]);
    }
  }

  // the root of the heap has the Kth smallest number
  return maxHeap.peek();
}

console.log(
  `Kth smallest number is: ${findKthSmallestNumber([1, 5, 12, 2, 11, 5], 3)}`
);

// since there are two 5s in the input array, our 3rd and 4th smallest numbers should be a '5'
console.log(
  `Kth smallest number is: ${findKthSmallestNumber([1, 5, 12, 2, 11, 5], 4)}`
);

console.log(
  `Kth smallest number is: ${findKthSmallestNumber([5, 12, 11, -1, 12], 3)}`
);
```

- The time complexity of the above algorithm is `O(K*logK + (N-K)*logK)`which is asymptotically equal to `O(N∗logK)`. The space complexity will be `O(K)` because we need to store `K` smallest numbers in the heap.

### 4. Using Min-Heap

Also discussed in `Kth` Smallest Number, we can use a Min Heap to find the `Kth` smallest number. We can insert all the numbers in the min-heap and then extract the top `K` numbers from the heap to find the `Kth` smallest number.

- Building a heap with `N`elements will take `O(N)`and extracting `K` numbers will take `O(K∗logN)`. Overall, the time complexity of this algorithm will be `O(N+K∗logN)`and the space complexity will be `O(N)`.

### 5. Using Partition Scheme of <b>Quicksort</b>

[Quicksort](https://github.com/Chanda-Abdul/leetcode/blob/master/0%20%E2%9D%97Sort%20Algorithms.md#-quick-sort) picks a number called <b>pivot</b> and partition the input array around it. After <i>partitioning</i>, all numbers smaller than the <b>pivot</b> are to the left of the <b>pivot</b>, and all numbers greater than or equal to the <b>pivot</b> are to the right of the <b>pivot</b>. This ensures that the <b>pivot</b> has reached its correct sorted position.

We can use this <i>partitioning</i> scheme to find the `Kth` smallest number. We will recursively partition the input array and if, after <i>partitioning</i>, our <b>pivot</b> is at the `K-1` index we have found our required number; if not, we will choose one the following option:

1. If <b>pivot</b>’s position is larger than `K-1`, we will recursively partition the array on numbers lower than the <b>pivot</b>.
2. If <b>pivot</b>’s position is smaller than `K-1`, we will recursively partition the array on numbers greater than the <b>pivot</b>.

Here is what our algorithm will look like:

```js
function findKthSmallestNumber(nums, k) {
  return findKthSmallestNumber_rec(nums, k, 0, nums.length - 1);
}

function findKthSmallestNumber_rec(nums, k, start, end) {
  const p = partition(nums, start, end);

  if (p === k - 1) {
    return nums[p];
  }

  if (p > k - 1) {
    // search lower part
    return findKthSmallestNumber_rec(nums, k, start, p - 1);
  }

  // search higher part
  return findKthSmallestNumber_rec(nums, k, p + 1, end);
}

function partition(nums, low, high) {
  if (low === high) {
    return low;
  }

  const pivot = nums[high];
  for (i = low; i < high; i++) {
    // all elements less than 'pivot' will be before the index 'low'
    if (nums[i] < pivot) {
      [nums[low], nums[i]] = [nums[i], nums[low]];
      low += 1;
    }
  }

  // put the pivot in its correct place
  [nums[low], nums[high]] = [nums[high], nums[low]];
  return low;
}

console.log(
  `Kth smallest number is: ${findKthSmallestNumber([1, 5, 12, 2, 11, 5], 3)}`
);

// since there are two 5s in the input array, our 3rd and 4th smallest numbers should be a '5'
console.log(
  `Kth smallest number is: ${findKthSmallestNumber([1, 5, 12, 2, 11, 5], 4)}`
);

console.log(
  `Kth smallest number is: ${findKthSmallestNumber([5, 12, 11, -1, 12], 3)}`
);
```

- The above algorithm is known as <b>QuickSelect</b> and has a Worst case time complexity of `O(N^2)`. The best and average case is `O(N)`., which is better than the best and average case of <b>Quicksort</b>. Overall, <b>QuickSelect</b> uses the same approach as <b>Quicksort</b> i.e., <i>partitioning</i> the data into two parts based on a <b>pivot</b>. However, contrary to <b>Quicksort</b>, instead of recursing into both sides <b>QuickSelect</b> only recurses into one side – the side with the element it is searching for. This reduces the average and best case time complexity from `O(N∗logN)` to `O(N)`.

- The worst-case occurs when, at every step, the partition procedure splits the N-length array into arrays of size `1` and `N−1`. This can only happen when the input array is sorted or if all of its elements are the same. This <i>“unlucky”</i> selection of <b>pivot</b> elements requires `O(N)`recursive calls, leading to an `O(N^2)` worst-case.

- Worst-case space complexity will be `O(N)`used for the recursion stack. See details under <b>Quicksort</b>.

### 6. Using Randomized <i>partitioning</i> Scheme of Quicksort

As mentioned above, the worst case for <b>Quicksort</b> occurs when the partition procedure splits the `N-length` array into arrays of size `1` and `N−1`. To mitigate this, instead of always picking a fixed index for <b>pivot</b> (e.g., in the above algorithm we always pick `nums[high]` as the <b>pivot</b>), we can randomly select an element as <b>pivot</b>. After randomly choosing the <b>pivot</b> element, we expect the split of the input array to be reasonably well balanced on average.

Here is what our algorithm will look like (only the highlighted lines have changed):

```js
function findKthSmallestNumber(nums, k) {
  return findKthSmallestNumber_rec(nums, k, 0, nums.length - 1);
}

function findKthSmallestNumber_rec(nums, k, start, end) {
  const p = partition(nums, start, end);

  if (p === k - 1) {
    return nums[p];
  }
  if (p > k - 1) {
    // search lower part
    return findKthSmallestNumber_rec(nums, k, start, p - 1);
  }
  // search higher part
  return findKthSmallestNumber_rec(nums, k, p + 1, end);
}

function partition(nums, low, high) {
  if (low === high) {
    return low;
  }

  const pivotIndex = Math.floor(Math.random() * (high - low + 1)) + low;
  [nums[pivotIndex], nums[high]] = [nums[high], nums[pivotIndex]];

  const pivot = nums[high];
  for (i = low; i < high; i++) {
    // all elements less than 'pivot' will be before the index 'low'
    if (nums[i] < pivot) {
      [nums[low], nums[i]] = [nums[i], nums[low]];
      low += 1;
    }
  }

  // put the pivot in its correct place
  [nums[low], nums[high]] = [nums[high], nums[low]]; //swap
  return low;
}

console.log(
  `Kth smallest number is: ${findKthSmallestNumber([1, 5, 12, 2, 11, 5], 3)}`
);

// since there are two 5s in the input array, our 3rd and 4th smallest numbers should be a '5'
console.log(
  `Kth smallest number is: ${findKthSmallestNumber([1, 5, 12, 2, 11, 5], 4)}`
);

console.log(
  `Kth smallest number is: ${findKthSmallestNumber([5, 12, 11, -1, 12], 3)}`
);
```

- The above algorithm has the same worst and average case time complexities as mentioned for the previous algorithm. But choosing the <b>pivot</b> randomly has the effect of rendering the worst-case very unlikely, particularly for large arrays. Therefore, the expected time complexity of the above algorithm will be `O(N)`, but the absolute worst case is still `O(N^2)`. Practically, this algorithm is a lot faster than the non-randomized version.

### 7. Using the [Median of Medians algorithm](https://en.wikipedia.org/wiki/Median_of_medians)

We can use the <b>[Median of Medians algorithm](https://www.w3resource.com/javascript-exercises/fundamental/javascript-fundamental-exercise-88.php)</b> to choose a good <b>pivot</b> for the <i>partitioning</i> algorithm of the [Quicksort](https://github.com/Chanda-Abdul/leetcode/blob/master/0%20%E2%9D%97Sort%20Algorithms.md#-quick-sort). This algorithm finds an approximate median of an array in linear time `O(N)`. When this approximate median is used as the <b>pivot</b>, the worst-case complexity of the <i>partitioning</i> procedure reduces to linear `O(N)`, which is also the asymptotically optimal worst-case complexity of any sorting/selection algorithm. This algorithm was originally developed by <i>Blum, Floyd, Pratt, Rivest, and Tarjan</i> and was describe in their [1973 paper](http://people.csail.mit.edu/rivest/pubs/BFPRT73.pdf).

This is how the <i>partitioning</i> algorithm works:

1. If we have 5 or less than 5 elements in the input array, we simply take its first element as the <b>pivot</b>. If not then we divide the input array into subarrays of five elements (for simplicity we can ignore any subarray having less than five elements).
2. Sort each subarray to determine its median. Sorting a small and fixed numbered array takes constant time. At the end of this step, we have an array containing medians of all the subarray.
3. Recursively call the <i>partitioning</i> algorithm on the array containing medians until we get our <b>pivot</b>.
4. Every time the partition procedure needs to find a <b>pivot</b>, it will follow the above three steps.

Here is what this algorithm will look like:

```js
function findKthSmallestNumber(nums, k) {
  return findKthSmallestNumber_rec(nums, k, 0, nums.length - 1);
}

function findKthSmallestNumber_rec(nums, k, start, end) {
  const p = partition(nums, start, end);

  if (p === k - 1) {
    return nums[p];
  }

  if (p > k - 1) {
    // search lower part
    return findKthSmallestNumber_rec(nums, k, start, p - 1);
  }

  // search higher part
  return findKthSmallestNumber_rec(nums, k, p + 1, end);
}

function partition(nums, low, high) {
  if (low === high) {
    return low;
  }

  const median = median_of_medians(nums, low, high);
  // find the median in the array and swap it with 'nums[high]' which will become our pivot
  for (i = low; i < high; i++) {
    if (nums[i] === median) {
      [nums[i], nums[high]] = [nums[high], nums[i]];
      break;
    }
  }

  const pivo = nums[high];
  for (i = low; i < high; i++) {
    // all elements less than 'pivot' will be before the index 'low'
    if (nums[i] < pivot) {
      [nums[low], nums[i]] = [nums[i], nums[low]];
      low += 1;
    }
  }
  // put the pivot at its correct place
  [nums[low], nums[high]] = [nums[high], nums[low]];
  return low;
}

function median_of_medians(nums, low, high) {
  n = high - low + 1;
  // if we have less than 5 elements, ignore the partitioning algorithm
  if (n < 5) {
    return nums[low];
  }

  // partition the given array into chunks of 5 elements
  // for simplicity, lets ignore any partition with less than 5 elements
  const partitions = [];
  for (let i = 0; i < nums.length; i += 5) {
    if (i + 5 <= nums.length) {
      partitions.push(nums.slice(i, i + 5));
    }
  }

  // sort all partitions
  partitions.forEach((p) => {
    p.sort((a, b) => a - b);
  });

  // find median of all partitions; the median of each partition is at index '2'
  const medians = [];
  partitions.forEach((p) => {
    medians.push(p[2]);
  });

  return partition(medians, 0, medians.length - 1);
}

console.log(
  `Kth smallest number is: ${findKthSmallestNumber([1, 5, 12, 2, 11, 5], 3)}`
);

// since there are two 5s in the input array, our 3rd and 4th smallest numbers should be a '5'
console.log(
  `Kth smallest number is: ${findKthSmallestNumber([1, 5, 12, 2, 11, 5], 4)}`
);

console.log(
  `Kth smallest number is: ${findKthSmallestNumber([5, 12, 11, -1, 12], 3)}`
);
```

- The above algorithm has a guaranteed `O(N)`worst-case time. Please see the proof of its running time here and under <i>“Selection-based pivoting”</i>. The worst-case space complexity is `O(N)`.

### Conclusion

Theoretically, the [Median of Medians](#7-using-the-median-of-medians-algorithmhttpsenwikipediaorgwikimedianofmedians) algorithm gives the best time complexity of `O(N)`but practically both the [Median of Medians](#7-using-the-median-of-medians-algorithmhttpsenwikipediaorgwikimedianofmedians)  and the [Randomized partitioning algorithms](#6-using-randomized-ipartitioningi-scheme-of-quicksort) nearly perform equally.

In the context of <b>Quicksort</b>, given an `O(N)`selection algorithm using the [Median of Medians](#7-using-the-median-of-medians-algorithmhttpsenwikipediaorgwikimedianofmedians), one can use it to find the ideal <b>pivot</b> (the median) at every step of <b>Quicksort</b> and thus produce a sorting algorithm with `O(NlogN)`running time in the worst-case. Though practical implementations of this variant are considerably slower on average, they are of theoretical interest because they show that an optimal selection algorithm can yield an optimal sorting algorithm.
