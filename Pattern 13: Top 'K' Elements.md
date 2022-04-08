# Pattern 13: Top 'K' Elements
<!-- * It has taken longer than planned for me to work through this pattern in JavaScript. I don't believe that heaps are the most effecient way to tackle these problems during an interview.  I'm reviewing the leetcode discussions for each problem as I go along, and it seems like it would be easier to implement some kind of sorting algorithm like quicksort.  Please let me know your thoughts/opinions on this. -->
<!-- # -->

Any problem that asks us to find the <b>top/smallest/frequent K</b> elements among a given set falls under this pattern.

The best data structure that comes to mind to keep track of <b>K</b> elements is <s>Heap</s>. This pattern will make use of the <s><b>Heap</b></s> to solve multiple problems dealing with <b>K</b> elements at a time from a set of given elements.

### ❗ NOTE 

<b>JavaScript</b> does not have a built in method for <b>heaps</b>. It could be an ineffecient use of time during an actual interview to create a <b>Heap class</b> from scratch.  If you want to learn how to implement a <b>Heap Class</b> in <b>JavaScript</b> take a look at this [article](https://blog.bitsrc.io/implementing-heaps-in-javascript-c3fbf1cb2e65).

## 😕 Top 'K' Numbers (easy)
> Given an unsorted array of numbers, find the `K` largest numbers in it.

<b>Note:</b> For a detailed discussion about different approaches to solve this problem, take a look at [Kth Smallest Number](#kth-smallest-number-easy).

A <b>brute force solution</b> could be to sort the array and return the <b>largest K numbers</b>. The time complexity of such an algorithm will be `O(N*logN)` as we need to use a sorting algorithm like <b>[Quicksort](https://github.com/Chanda-Abdul/leetcode/blob/master/%E2%9D%97Sort%20Algorithms.md#-quick-sort)</b>. Can we do better than that?

<!-- <s>The best data structure that comes to mind to keep track of top `K` elements is Heap. Let's see if we can use a heap to find a better algorithm.

If we iterate through the array one element at a time and keep `K` largest numbers in a heap such that each time we find a larger number than the smallest number in the heap, we do two things:
1. Take out the smallest number from the heap, and
2. Insert the larger number into the heap.
  
This will ensure that we always have `K` largest numbers in the heap. The most efficient way to repeatedly find the smallest number among a set of numbers will be to use a min-heap. As we know, we can find the smallest number in a min-heap in constant time `O(1)`, since the smallest number is always at the root of the heap. Extracting the smallest number from a min-heap will take `O(logN)` (if the heap has `N` elements) as the heap needs to readjust after the removal of an element.</s>

Let's take <b>Example 1</b> to go through each step of our algorithm:

Given array: `[3, 1, 5, 12, 2, 11]`, and `K=3`
<s>

1. First, let's insert `K` elements in the min-heap.
2. After the insertion, the heap will have three numbers `[3, 1, 5]` with `1` being the root as it is the smallest element.
3. We`ll iterate through the remaining numbers and perform the above-mentioned two steps if we find a number larger than the root of the heap.
4. The 4th number is `12` which is larger than the root (which is `1`), so let's take out `1` and insert `12`. Now the heap will have `[3, 5, 12]` with `3` being the root as it is the smallest element.
5. The 5th number is `2` which is not bigger than the root of the heap (`3`), so we can skip this as we already have top three numbers in the heap.
6. The last number is `11` which is bigger than the root (which is `3`), so let's take out `3` and insert `11`. Finally, the heap has the largest three numbers: [5, 12, 11]
  
As discussed above, it will take us `O(logK)` to extract the minimum number from the min-heap. So the overall time complexity of our algorithm will be `O(K*logK+(N-K)*logK)` since, first, we insert `K` numbers in the heap and then iterate through the remaining numbers and at every step, in the worst case, we need to extract the minimum number and insert a new number in the heap. This algorithm is better than `O(N*logN)`.</s> -->

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
> Given `N` ropes with different lengths, we need to connect these ropes into one big rope with minimum cost. The cost of connecting two ropes is equal to the sum of their lengths.

````js
function minimumCostToConnectRopes(ropeLengths) {
  if (ropeLengths.length === 1) return 0;

  ropeLengths.sort((a, b) => a - b);

  let totalCost = 0;

  while (ropeLengths.length) {
    let firstRope = ropeLengths.shift();
    let secondRope = ropeLengths.shift();
    let currentCost = firstRope + secondRope;
    totalCost += currentCost;

    if (ropeLengths.length === 0) return totalCost;

    //Binary Search
    let start = 0;
    let end = ropeLengths.length;

    while (start < end) {
      let mid = start + Math.floor((end - start) / 2);

      if (currentCost < ropeLengths[mid]) end = mid;
      else start = mid + 1;
    }
    ropeLengths.splice(start, 0, currentCost);
  }
}

console.log(`Minimum cost to connect ropes: ${minimumCostToConnectRopes([1, 3, 11, 5])}`)
//33
//First connect 1+3(=4), then 4+5(=9), and then 9+11(=20). So the total cost is 33 (4+9+20)

console.log(`Minimum cost to connect ropes: ${minimumCostToConnectRopes([3, 4, 5, 6])}`);
//36
//First connect 3+4(=7), then 5+6(=11), 7+11(=18). Total cost is 36 (7+11+18)

console.log(`Minimum cost to connect ropes: ${minimumCostToConnectRopes([1, 3, 11, 5, 2])}`)
//42
//First connect 1+2(=3), then 3+3(=6), 6+5(=11), 11+11(=22). Total cost is 42 (3+6+11+22)
````
- Given `N` ropes, we need `O(N^2)` for the <b>Binary Search</b>.
- The space complexity will be `O(1)` .
## 👩‍🦯 Top 'K' Frequent Numbers (medium)
https://leetcode.com/problems/top-k-frequent-elements/
> Given an unsorted array of numbers, find the top `K` frequently occurring numbers in it.
````js
function findKFrequentNumbers(nums, k) {
  const numMap = new Map();
  for (number of nums) {
    numMap.set(number, (numMap.get(number) || 0) + 1);
  }

  const mapKeys = [...numMap.keys()];
  const finalIndex = mapKeys.length - k;

  let start = 0;
  let end = mapKeys.length - 1;

  //Quicksort
  while (start <= end) {
    const pivotPoint = Math.floor(Math.random() * (end - start + 1) + start);
    const pivotIndex = pivotHelper(pivotPoint, start, end);

    if (pivotIndex === finalIndex) {
      return mapKeys.slice(finalIndex);
    }
    if (pivotIndex < finalIndex) {
      start = pivotIndex + 1;
    } else {
      end = pivotIndex - 1;
    }
    function pivotHelper(pivotPoint, start, end) {
      //move the pivotPoint to the end
      [mapKeys[pivotPoint], mapKeys[end]] = [mapKeys[end], mapKeys[pivotPoint]];
      let swapIndex = start;

      for (let i = start; i < end; i++) {
        if (numMap.get(mapKeys[i]) < numMap.get(mapKeys[end])) {
          [mapKeys[i], mapKeys[swapIndex]] = [mapKeys[swapIndex], mapKeys[i]];
          swapIndex++;
        }
      }
      [mapKeys[end], mapKeys[swapIndex]] = [mapKeys[swapIndex], mapKeys[end]];
      return swapIndex;
    }
  }
}

console.log(`Here are the K frequent numbers: ${findKFrequentNumbers([1, 3, 5, 12, 11, 12, 11], 2)}`)

console.log(`Here are the K frequent numbers: ${findKFrequentNumbers([5, 12, 11, 3, 11], 2)}`)
````
## Frequency Sort (medium)
https://leetcode.com/problems/sort-characters-by-frequency/
> Given a string, sort it based on the decreasing frequency of its characters.
````js
function sortCharacterByFrequency(str) {
  let counts = {}
  for(let char of str){
    counts[char] ? counts[char]++ : counts[char] = 1
  }
  
  let sortedCharactersArray = Object.keys(counts).sort((a,b) => counts[b] -counts[a])
  
  let sortedString = ""
  
  for(let char of sortedCharactersArray){
    sortedString += char.repeat(counts[char])
  }
  
  return sortedString
};


console.log('string after sorting characters by frequency: ${sortCharacterByFrequency("Programming")}`)
//"rrggmmPiano"
//'r', 'g', and 'm' appeared twice, so they need to appear before any other character.

console.log('string after sorting characters by frequency: ${sortCharacterByFrequency("abcbab")}`)
//"bbbaac"
//'b' appeared three times, 'a' appeared twice, and 'c' appeared only once.
````
## Kth Largest Number in a Stream (medium)
https://leetcode.com/problems/kth-largest-element-in-a-stream/
> Design a class to efficiently find the `Kth` largest element in a stream of numbers.
> 
> The class should have the following two things:
> 
> 1. The constructor of the class should accept an integer array containing initial numbers from the stream and an integer `K`.
> 2. The class should expose a function `add(int num)` which will store the given number and return the Kth largest number.
````js
class KthLargest {
  constructor(k, nums) {
    this.k = k;
    this.nums = nums;
  }

  add(val) {
    this.nums = this.nums.sort((a, b) => a - b).splice(-this.k);
    if (val > this.nums[0] || this.nums.length < this.k) {
      //sort and return kth largest with Binary Search
      const insertNewNum = () => {
        let start = 0;
        let end = this.k;

        while (start < end) {
          const mid = Math.floor((start + end) / 2);
          if (this.nums[mid] === val) return mid;
          if (this.nums[mid] < val) {
            start = mid + 1;
          } else {
            end = mid;
          }
        }
        return start;
      };
      while (this.nums.length > this.k) this.nums.shift();
      this.nums.splice(insertNewNum(), 0, val);
    }

    if (this.nums.length >= this.k) return this.nums[this.nums.length - this.k];
    else return null;
  }
}

// Input: [3, 1, 5, 12, 2, 11], K = 4
// 1. Calling add(6) should return '5'.
// 2. Calling add(13) should return '6'.
// 2. Calling add(4) should still return '6'.

let kthLargest = new KthLargest(4, [3, 1, 5, 12, 2, 11],);
kthLargest.add(6); // return 5
kthLargest.add(13); // return 6
kthLargest.add(4); // return 6
````
- The time complexity of the above algorithm is `O(NlogN)`.
- The space complexity of the above algorithm is `O(1)`
## 'K' Closest Numbers (medium)
https://leetcode.com/problems/find-k-closest-elements/
> Given a sorted number array and two integers `K` and `X`, find `K` closest numbers to `X` in the array. Return the numbers in the sorted order. `X` is not necessarily present in the array.

This problem follows the Top `K` Numbers pattern. The biggest difference in this problem is that we need to find the closest (to `X`) numbers compared to finding the overall largest numbers. Another difference is that the given array is sorted.

Utilizing a similar approach, we can find the numbers closest to `X` through the following algorithm:

1. Since the array is sorted, we can first find the number closest to `X` through Binary Search. Let's say that number is `Y`.
2. The `K` closest numbers to `Y` will be adjacent to `Y` in the array. We can search in both directions of `Y` to find the closest numbers.
3. We can use a heap to efficiently search for the closest numbers. We will take `K` numbers in both directions of `Y` and push them in a Min Heap sorted by their absolute difference from `X`. This will ensure that the numbers with the smallest difference from `X` (i.e., closest to `X`) can be extracted easily from the Min Heap.
4. Finally, we will extract the top `K` numbers from the Min Heap to find the required numbers.

After finding the number closest to `X` through Binary Search, we can use the Two Pointers approach to find the `K` closest numbers. Let’s say the closest number is `Y`. We can have a left pointer to move back from `Y` and a right pointer to move forward from `Y`. At any stage, whichever number pointed out by the left or the right pointer gives the smaller difference from `X` will be added to our result list.

To keep the resultant list sorted we can use a <b>Queue</b>. So whenever we take the number pointed out by the left pointer, we will append it at the beginning of the list and whenever we take the number pointed out by the right pointer we will append it at the end of the list.

Here is what our algorithm will look like:

````js
function findClosestElements(arr, K, X) {
  let windowOfKClosest = [];
  let idx = binarySearch(arr, X);
  let windowStart = idx;
  let windowEnd = idx + 1;
  const n = arr.length;

  for (let i = 0; i < K; i++) {
    if (windowStart >= 0 && windowEnd < n) {
      let diffFromStart = Math.abs(X - arr[windowStart]);
      let diffFromEnd = Math.abs(X - arr[windowEnd]);

      if (diffFromStart <= diffFromEnd) {
        windowOfKClosest.unshift(arr[windowStart]);
        windowStart--;
      } else {
        windowOfKClosest.push(arr[windowEnd]);
        windowEnd++;
      }
    } else if (windowStart >= 0) {
      windowOfKClosest.unshift(arr[windowStart]);
      windowStart--;
    } else if (windowEnd < n) {
      windowOfKClosest.push(arr[windowEnd]);
      windowEnd++;
    }
  }

  return windowOfKClosest;

  function binarySearch(arr, X) {
    let lo = 0;
    let hi = arr.length - 1;

    while (lo <= hi) {
      const mid = Math.floor(lo + (hi - lo) / 2);

      if (arr[mid] === X) {
        return mid;
      }
      if (arr[mid] < X) {
        lo = mid + 1;
      } else {
        hi = mid - 1;
      }
    }
    if (lo > 0) {
      return lo - 1;
    }
    return lo;
  }
}

console.log(`'K' closest numbers to 'X' are: ${findClosestElements([5, 6, 7, 8, 9], 3, 7)}`)
//Output: [6, 7, 8]

console.log(`'K' closest numbers to 'X' are: ${findClosestElements([2, 4, 5, 6, 9], 3, 6)}`)
//Output: [4, 5, 6]

console.log(`'K' closest numbers to 'X' are: ${findClosestElements([2, 4, 5, 6, 9], 3, 10)}`)
//Output: [5, 6, 9]

console.log(`'K' closest numbers to 'X' are: ${findClosestElements([1,2,3,4,5], 4, 3)}`)
//Output: [1,2,3,4]

console.log(`'K' closest numbers to 'X' are: ${findClosestElements([1,2,3,4,5], 4, -1)}`)
//Output: [1,2,3,4]
````
- The time complexity of the above algorithm is `O(logN + K)`. We need `O(logN)` for Binary Search and `O(K)`for finding the `K` closest numbers using the two pointers.
- If we ignoring the space required for the output list, the algorithm runs in constant space `O(1)`.

## Maximum Distinct Elements (medium)
https://leetcode.com/problems/least-number-of-unique-integers-after-k-removals/
> Given an array of numbers and a number `K`, we need to remove `K` numbers from the array such that we are left with maximum distinct numbers.
````js
function findMaximumDistinctElements(nums, k) {
  let freqMap = new Map();

  nums.forEach((number) => {
    freqMap.set(number, freqMap.get(number) + 1 || 1);
  });

  let freq = Array.from(freqMap.values());
  freq.sort((a, b) => a - b);

  let results = freq.length;
  for (let n of freq) {
    if (k >= n) {
      k -= n;
      results--;
    } else return results;
  }
  
  return results;
}

console.log(`Maximum distinct numbers after removing K numbers: 
${findMaximumDistinctElements([5,5,4], 1)}`)
//1, Remove the single 4, only 5 is left.

console.log(`Maximum distinct numbers after removing K numbers: 
${findMaximumDistinctElements([4,3,1,1,3,3,2], 3)}`)
//2, Remove 4, 2 and either one of the two 1s or three 3s. 1 and 3 will be left.

console.log(`Maximum distinct numbers after removing K numbers: 
${findMaximumDistinctElements([7, 3, 5, 8, 5, 3, 3], 2)}`)
//3, We can remove two occurrences of 3 to be left with 3 distinct numbers [7, 3, 8], 
//we have to skip 5 because it is not distinct and appeared twice.
// Another solution could be to remove one instance of '5' and '3' each to be left with 
//three distinct numbers [7, 5, 8], in this case, we have to skip 3 because it appeared twice.

console.log(`Maximum distinct numbers after removing K numbers: 
${findMaximumDistinctElements([3, 5, 12, 11, 12], 3)}`)
//2, We can remove one occurrence of 12, after which all numbers will become distinct. 
//Then we can delete any two numbers which will leave us 2 distinct numbers in the result.

console.log(`Maximum distinct numbers after removing K numbers: 
${findMaximumDistinctElements([1, 2, 3, 3, 3, 3, 4, 4, 5, 5, 5], 2)}`)//3, We can remove one occurrence of '4' to get three distinct numbers.
````
- Since we will insert all numbers in a <b>HashMap</b>, this will take `O(N*logN)` where `N` is the total input numbers. While extracting numbers from the map, in the worst case, we will need to take out `K` numbers. This will happen when we have at least `K` numbers with a frequency of two. Since the heap can have a maximum of `N/2` numbers, therefore, extracting an element from the heap will take `O(logN)` and extracting `K` numbers will take `O(KlogN)`. So overall, the time complexity of our algorithm will be `O(N*logN + KlogN)`. We can optimize the above algorithm and only push `K` elements in the heap, as in the worst case we will be extracting `K` elements from the heap. This optimization will reduce the overall time complexity to `O(N*logK + KlogK)`.
- The space complexity will be `O(N)` as, in the worst case, we need to store all the `N` characters in the <b>HashMap</b>.


## Sum of Elements (medium)
https://www.geeksforgeeks.org/sum-elements-k1th-k2th-smallest-elements/
> Given an array, find the sum of all numbers between the `K1th` and `K2th` smallest elements of that array.

````js
function findSumOfElements(nums, k1, k2) {
  nums.sort((a, b) => a - b);

  let kSlice = nums.slice(k1, k2 - 1);
  let kSumBetween = 0;

  kSlice.forEach((n) => (kSumBetween += n));

  return kSumBetween;
}

console.log(`Sum of all numbers between k1 and k2 smallest numbers: ${findSumOfElements([1, 3, 12, 5, 15, 11], 3, 6)}`)
//23
//The 3rd smallest number is 5 and 6th smallest number 15.
// The sum of numbers coming between 5 and 15 is 23 (11+12).

console.log(`Sum of all numbers between k1 and k2 smallest numbers: ${findSumOfElements([3, 5, 8, 7], 1, 4)}`)
//12
//The sum of the numbers between the 1st smallest number (3)
//and the 4th smallest number (8) is 12 (5+7).
````
## Rearrange String (hard)
https://leetcode.com/problems/reorganize-string/
> Given a string, find if its letters can be rearranged in such a way that no two same characters come next to each other.
````js
function rearrangeString(str) {
  //Build a hashMap based on char count
  const strMap = new Map();

  for (const char of str) {
    strMap.set(char, strMap.get(char) + 1 || 1);
  }

  //sort based on char frequency in descending order
  const sortedMap = new Map([...strMap.entries()].sort((a, b) => b[1] - a[1]));

  //is first value of sortedMap > half of str.length,
  //because a character count that is larger than half of the string length is considered invalid
  if (sortedMap.values().next().value > (str.length + 1) / 2) return '';

  let result = [];
  let index = 0;

  for (let [key, value] of sortedMap) {
    while (value--) {
      //Start filling characters to all the even indexs, i.e. 0, 2, 4,...,
      result[index] = key;
      index += 2;
      // when we got to the end, start filling odd indexes i.e. 1,3,5,...
      //By filling the characters this way, we can make sure that no same characters will be adjacent to each other
      if (index >= str.length) index = 1;
    }
  }

  return result.join('');
}

console.log(`Rearranged string: ${rearrangeString('aappp')}`);
console.log(`Rearranged string: ${rearrangeString('Programming')}`);
console.log(`Rearranged string: ${rearrangeString('aapa')}`);
````
## 🌟 Rearrange String K Distance Apart (hard)
https://leetcode.com/problems/rearrange-string-k-distance-apart/
> Given a string and a number `K`, find if the string can be rearranged such that the same characters are at least `K` distance apart from each other.
## 🌟 🔍 Scheduling Tasks (hard)
https://leetcode.com/problems/task-scheduler/
> You are given a list of tasks that need to be run, in any order, on a server. Each task will take one CPU interval to execute but once a task has finished, it has a cooling period during which it can`t be run again. If the cooling period for all tasks is `K` intervals, find the minimum number of CPU intervals that the server needs to finish all tasks.
> 
> If at any time the server can`t execute any task then it must stay idle.
## 🌟Frequency Stack (hard)
https://leetcode.com/problems/maximum-frequency-stack/
> Design a class that simulates a Stack data structure, implementing the following two operations:
> 
> - push(int num): Pushes the number `num` on the stack.
> - pop(): Returns the most frequent number in the stack. If there is a tie, return the number which was pushed later.

