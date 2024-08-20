# Pattern 9: Two Heaps

In many problems, where we are given a set of elements such that we can divide them into two parts. To solve the problem, we are interested in knowing the smallest element in one part and the biggest element in the other part. This pattern is an efficient approach to solve such problems.

This pattern uses two <b>Heaps</b> to solve these problems; A <b>Min Heap</b> to find the smallest element and a <b>Max Heap</b> to find the biggest element.

Although this course uses <b>Heaps</b> to solve <b>Top 'K' Elements</b> problems, <b>JavaScript</b> does not have a built in method for <b>Heaps/Priority Queues</b>. It can be very time consuming to implement a <b>Heap class</b> from scratch, especially during an interview. After reviewing the <i>JavaScript</i> solutions on <i>Leetcode</i> the most effecient way to solve a <b>Top 'K' Elements</b> problem is usually with <b>[QuickSort](https://github.com/Chanda-Abdul/leetcode/blob/master/0%20%E2%9D%97Sort%20Algorithms.md#-quick-sort)</b>, <b>[BinarySearch](https://github.com/Chanda-Abdul/leetcode/blob/master/0%20%E2%9D%97Sort%20Algorithms.md#binary-search)</b>, <b>[BucketSort](https://initjs.org/bucket-sort-in-javascript-dc040b8f0058)</b>, <b>[Greedy Algorithms](https://github.com/Chanda-Abdul/Grokking-Algorithm-Book-Notes/blob/main/8.%20Greedy%20Algoritms.md)</b>, or <b>[HashMaps](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)</b>. 

## Find the Median of a Number Stream (medium)
https://leetcode.com/problems/find-median-from-data-stream/
> Design a class to calculate the median of a number stream. The class should have the following two methods:
> 1. `insertNum(int num)`: stores the number in the class
> 2. `findMedian()`: returns the median of all numbers inserted in the class
> If the count of numbers inserted in the class is even, the median will be the average of the middle two numbers.

As we know, the median is the middle value in an ordered integer list. So a brute force solution could be to maintain a sorted list of all numbers inserted in the class so that we can efficiently return the median whenever required. Inserting a number in a sorted list will take `O(N)` time if there are `N` numbers in the list. This insertion will be similar to the <b>Insertion sort</b>. Can we do better than this? Can we utilize the fact that we don’t need the fully sorted list - we are only interested in finding the middle element?

Assume ‘x’ is the median of a list. This means that half of the numbers in the list will be smaller than (or equal to) ‘x’ and half will be greater than (or equal to) ‘x’. This leads us to an approach where we can divide the list into two halves: one half to store all the smaller numbers (let’s call it `smallNumList`) and one half to store the larger numbers (let’s call it `largNumList`). The median of all the numbers will either be the largest number in the `smallNumList` or the smallest number in the `largNumList`. If the total number of elements is even, the median will be the average of these two numbers.

The best data structure that comes to mind to find the smallest or largest number among a list of numbers is a <b>Heap</b>. Let’s see how we can use a heap to find a better algorithm.

1. We can store the first half of numbers (i.e., `smallNumList`) in a <b>Max Heap</b>. We should use a Max Heap as we are interested in knowing the largest number in the first half.
2. We can store the second half of numbers (i.e., `largeNumList`) in a <b>Min Heap</b>, as we are interested in knowing the smallest number in the second half.
3. Inserting a number in a heap will take `O(logN)`, which is better than the brute force approach.
4. At any time, the median of the current list of numbers can be calculated from the top element of the two heaps.

Let’s take the Example-1 mentioned above to go through each step of our algorithm:
1. `insertNum(3)`: We can insert a number in the <b>Max Heap</b> (i.e. first half) if the number is smaller than the top (largest) number of the heap. After every insertion, we will balance the number of elements in both heaps, so that they have an equal number of elements. If the count of numbers is odd, let’s decide to have more numbers in max-heap than the Min Heap.
2. `insertNum(1)`: As ‘1’ is smaller than ‘3’, let’s insert it into the <b>Max Heap</b>.

Now, we have two elements in the <b>Max Heap</b> and no elements in <b>Min Heap</b>. Let’s take the largest element from the Max Heap and insert it into the <b>Min Heap</b>, to balance the number of elements in both heaps.

3. `findMedian()`: As we have an even number of elements, the median will be the average of the top element of both the heaps ➡️ `(1+3)/2 = 2.0(1+3)/2=2.0`
4. `insertNum(5)`: As ‘5’ is greater than the top element of the <b>Max Heap</b>, we can insert it into the <b>Min Heap</b>. After the insertion, the total count of elements will be odd. As we had decided to have more numbers in the <b>Max Heap</b> than the <b>Min Heap</b>, we can take the top (smallest) number from the <b>Min Heap</b> and insert it into the <b>Max Heap</b>.
5. `findMedian()`: Since we have an odd number of elements, the median will be the top element of <b>Max Heap</b> ➡️ `3`. An odd number of elements also means that the <b>Max Heap</b> will have one extra element than the <b>Min Heap</b>.
6. `insertNum(4)`: Insert ‘4’ into <b>Min Heap</b>.
7. `findMedian()`: As we have an even number of elements, the median will be the average of the top element of both the heaps ➡️ `(3+4)/2 = 3.5(3+4)/2=3.5`

### JavaScript Custom Heap Class
````
/** 
 *  custom Heap class
 */

class Heap {
	constructor(comparator) {
		this.size = 0;
		this.values = [];
		this.comparator = comparator || Heap.minComparator;
	}

	add(val) {
		this.values.push(val);
		this.size ++;
		this.bubbleUp();
	}

	peek() {
		return this.values[0] || null;
	}

	poll() {
		const max = this.values[0];
		const end = this.values.pop();
		this.size --;
		if (this.values.length) {
			this.values[0] = end;
			this.bubbleDown();
		}
		return max;
	}

	bubbleUp() {
		let index = this.values.length - 1;
		let parent = Math.floor((index - 1) / 2);

		while (this.comparator(this.values[index], this.values[parent]) < 0) {
			[this.values[parent], this.values[index]] = [this.values[index], this.values[parent]];
			index = parent;
			parent = Math.floor((index - 1) / 2);
		}
	}

	bubbleDown() {
		let index = 0, length = this.values.length;

		while (true) {
			let left = null,
				right = null,
				swap = null,
				leftIndex = index * 2 + 1,
				rightIndex = index * 2 + 2;

			if (leftIndex < length) {
				left = this.values[leftIndex];
				if (this.comparator(left, this.values[index]) < 0) swap = leftIndex;
			}

			if (rightIndex < length) {
				right = this.values[rightIndex];
				if ((swap !== null && this.comparator(right, left) < 0) || (swap === null && this.comparator(right, this.values[index]))) {
					swap = rightIndex;
				}
			}
			if (swap === null) break;

			[this.values[index], this.values[swap]] = [this.values[swap], this.values[index]];
			index = swap;
		}
	}
}

/** 
 *  Min Comparator
 */
Heap.minComparator = (a, b) => { return a - b; }

/** 
 *  Max Comparator
 */
Heap.maxComparator = (a, b) => { return b - a; }
````
### Solution 
````js
class MedianOfAStream {
  constructor(){
     this.maxHeap = new Heap(Heap.maxComparator);
     this.minHeap = new Heap(Heap.minComparator);
  }
  
  insertNum(num) {
   if(this.maxHeap.size === 0 || this.maxHeap.peek() >= num){
     this.maxHeap.add(num)
   } else {
     this.minHeap.add(num)
   }
    
    //either both the heaps will have = numnber of elements
    //or maxHeap will have one or more elements than minHeap
    if(this.maxHeap.size > this.minHeap.size + 1){
      this.minHeap.add(this.maxHeap.poll())
    } else if(this.maxHeap.size < this.minHeap.size) {
      this.maxHeap.add(this.minHeap.poll())
    }
  }

  findMedian() {
    if(this.maxHeap.size > this.minHeap.size){
      //because maxHeap will have one more element 
      //than the minHeap
      return this.maxHeap.peek()
    } else if(this.maxHeap.size < this.minHeap.size){
      return this.minHeap.peek()
    } else {
      //we have an even number of elements
      //so take the average of the middle two elements
      return (this.maxHeap.peek() + this.minHeap.peek()) / 2.0
    } 
  }
};


const medianOfAStream = new MedianOfAStream()
medianOfAStream.insertNum(3)
medianOfAStream.insertNum(1)
console.log(`The median is: ${medianOfAStream.findMedian()}`)//2
medianOfAStream.insertNum(5)
console.log(`The median is: ${medianOfAStream.findMedian()}`)//3
medianOfAStream.insertNum(4)
console.log(`The median is: ${medianOfAStream.findMedian()}`)//3.5
````
- The time complexity of the `insertNum()` will be `O(logN)` due to the insertion in the heap. The time complexity of the `findMedian()` will be `O(1)` as we can find the median from the top elements of the heaps.
- The space complexity will be `O(N)` because, as at any time, we will be storing all the numbers.

## 😕 Sliding Window Median (hard)
https://leetcode.com/problems/sliding-window-median/

> Given an array of numbers and a number ‘k’, find the median of all the ‘k’ sized sub-arrays (or windows) of the array.

### Example 1:
#### Input: 
`nums=[1, 2, -1, 3, 5], k = 2`
#### Output: 
`[1.5, 0.5, 1.0, 4.0]`
#### Explanation: 
Lets consider all windows of size ‘2’:
<pre>
[<b>1, 2</b>, -1, 3, 5] -> median is 1.5
[1, <b>2, -1</b>, 3, 5] -> median is 0.5
[1, 2, <b>-1, 3</b>, 5] -> median is 1.0
[1, 2, -1, <b>3, 5</b>] -> median is 4.0
</pre>
### Example 2:
#### Input: `nums=[1, 2, -1, 3, 5], k = 3`
#### Output: `[1.0, 2.0, 3.0]`
#### Explanation: 
Lets consider all windows of size ‘3’:
<pre>
[<b>1, 2, -1</b>, 3, 5] -> median is 1.0
[1, <b>2, -1, 3</b>, 5] -> median is 2.0
[1, 2, <b>-1, 3, 5</b>] -> median is 3.0
</pre>

This problem follows the <b>Two Heaps</b> pattern and share similarities with <b>Find the Median of a Number Stream</b>. We can follow a similar approach of maintaining a <b>max-heap</b> and a <b>min-heap</b> for the list of numbers to find their median.

The only difference is that we need to keep track of a sliding window of ‘k’ numbers. This means, in each iteration, when we insert a new number in the heaps, we need to remove one number from the heaps which is going out of the sliding window. After the removal, we need to rebalance the heaps in the same way that we did while inserting.
### JavaScript Custom Heap Class
````
/** 
 *  custom Heap class
 */

class Heap {
	constructor(comparator) {
		this.size = 0;
		this.values = [];
		this.comparator = comparator || Heap.minComparator;
	}

	add(val) {
		this.values.push(val);
		this.size ++;
		this.bubbleUp();
	}

	peek() {
		return this.values[0] || null;
	}

	poll() {
		const max = this.values[0];
		const end = this.values.pop();
		this.size --;
		if (this.values.length) {
			this.values[0] = end;
			this.bubbleDown();
		}
		return max;
	}

	bubbleUp() {
		let index = this.values.length - 1;
		let parent = Math.floor((index - 1) / 2);

		while (this.comparator(this.values[index], this.values[parent]) < 0) {
			[this.values[parent], this.values[index]] = [this.values[index], this.values[parent]];
			index = parent;
			parent = Math.floor((index - 1) / 2);
		}
	}

	bubbleDown() {
		let index = 0, length = this.values.length;

		while (true) {
			let left = null,
				right = null,
				swap = null,
				leftIndex = index * 2 + 1,
				rightIndex = index * 2 + 2;

			if (leftIndex < length) {
				left = this.values[leftIndex];
				if (this.comparator(left, this.values[index]) < 0) swap = leftIndex;
			}

			if (rightIndex < length) {
				right = this.values[rightIndex];
				if ((swap !== null && this.comparator(right, left) < 0) || (swap === null && this.comparator(right, this.values[index]))) {
					swap = rightIndex;
				}
			}
			if (swap === null) break;

			[this.values[index], this.values[swap]] = [this.values[swap], this.values[index]];
			index = swap;
		}
	}
  remove(value) {
    let idx;
    for (let i of this.index[value]) {
      idx = i;
      break;
    }
    this.index[value].delete(idx);
    if (idx === this.values.length - 1) return this.value.pop();
    this.values[idx] = this.value.pop()
    this.idxs[this.values[idx]].delete(this.value.length);
    this.idxs[this.store[idx]].add(idx);
    this.heapifyDown(this.heapifyUp(idx));
  }
}

/** 
 *  Min Comparator
 */
Heap.minComparator = (a, b) => { return a - b; }

/** 
 *  Max Comparator
 */
Heap.maxComparator = (a, b) => { return b - a; }
````
### Solution 😕(review)
````

class SlidingWindowMedian {
  constructor(){
    this.maxHeap = new Heap(Heap.maxComparator)
    this.minHeap = new Heap(Heap.minComparator)
  }
  
    balance() {
    // either both the heaps will have equal number of elements or max-heap will have
    // one more element than the min-heap
    if (this.maxHeap.size > this.minHeap.size + 1) {
      this.minHeap.add(this.maxHeap.values.pop());
    } else if (this.maxHeap.size < this.minHeap.size) {
      this.maxHeap.add(this.minHeap.values.pop());
    }
  }

  findSlidingWindowMedian(nums, k) {
    const result = new Array(nums.length - k + 1).fill(0.0);
    console.log(result)
    
    let n = nums.length
    console.log(n)
    
    for(let i = 0; i < n; i++){
      console.log(nums[i])
      if(this.maxHeap.size === 0 || nums[i] <= this.maxHeap.peek()){
        this.maxHeap.add(nums[i])
        console.log(this.maxHeap.peek())
      } else {
        this.minHeap.add(nums[i])
        console.log(this.minHeap.peek())
      }
      
      this.balance()
      
      if(i - k + 1 >= 0){
         //if we have at least k elements in the sliding window
        //then add the median to the result array
        if(this.maxHeap.size() === this.minHeap.size()){
          //we have an enven number of elements
          //take the average of middle two elements
          result[i - k + 1] = (this.maxHeap.peek() + this.min.Heap.peek())/2
        } else {
          //because maxHeap will have one more element than the minHeap
          result[i - k + 1] = this.maxHeap.peek()
        }
        
        //remove the element going out of the sliding window
        const elementToBeRemoved = nums[i - k + 1]
        if(elementToBeRemoved <= this.maxHeap.peek()){
          //delete from heap
          this.maxHeap.remove(elementToBeRemoved)
        } else {
          //delete from heap
          this.minHeap.remove(elementToBeRemoved)
        }
        this.balance()
     }
    }
   return result
  }
};



const slidingWindowMedian = new SlidingWindowMedian()
let result = slidingWindowMedian.findSlidingWindowMedian(
  [1, 2, -1, 3, 5], 2)
console.log(`Sliding window medians are: ${result}`)//[1.5, 0.5, 1.0, 4.0]

slidingWindowMedian = new SlidingWindowMedian()
result = slidingWindowMedian.findSlidingWindowMedian(
  [1, 2, -1, 3, 5], 3)
console.log(`Sliding window medians are: ${result}`)//[1.0, 2.0, 3.0]
````

- The time complexity of our algorithm is `O(N*K)`where `N` is the total number of elements in the input array and `K` is the size of the sliding window. This is due to the fact that we are going through all the `N` numbers and, while doing so, we are doing two things:
	1. Inserting/removing numbers from heaps of size `K`. This will take `O(logK)`.
	2. Removing the element going out of the sliding window. This will take `O(K)` as we will be searching this element in an array of size `K` (i.e., a heap).
- Ignoring the space needed for the output array, the space complexity will be `O(K)` because, at any time, we will be storing all the numbers within the sliding window.

## 😕 Maximize Capital (hard)
https://leetcode.com/problems/ipo/

> Given a set of investment projects with their respective profits, we need to find the most profitable projects. We are given an initial `capital` and are allowed to invest only in a fixed number of projects. Our goal is to choose projects that give us the maximum profit. Write a function that returns the maximum total capital after selecting the most profitable projects.
> 
> We can start an investment project only when we have the required capital. Once a project is selected, we can assume that its profit has become our capital.
> 
> <b>Example 1</b>
> 
> Input: 
> 
> `Project Capitals=[0,1,2], Project Profits=[1,2,3], Initial Capital=1, Number of Projects=2`
> 
> Output: 
> `6`
> 
> Explanation
> 
> With initial capital of ‘1’, we will start the second project which will give us profit of ‘2’. 
> 
> Once we selected our first project, our total capital will become 3 (profit + initial capital).
> 
> With ‘3’ capital, we will select the third project, which will give us ‘3’ profit.
> 
> After the completion of the two projects, our total capital will be 6 (1+2+3).
> 
> <b>Example 2</b>
> 
> Input
> 
> `Project Capitals=[0,1,2,3], Project Profits=[1,2,3,5], Initial Capital=0, Number of Projects=3`
> 
> Output
> 
>  `8`
>  
>  Explanation
>  
>  With ‘0’ capital, we can only select the first project, bringing out capital to 1.
>  
>  Next, we will select the second project, which will bring our capital to 3.
>  
>  Next, we will select the fourth project, giving us a profit of 5.
>  
>  After selecting the three projects, our total capital will be 8 (1+2+5).

While selecting projects we have two constraints:

1. We can select a project only when we have the required capital.
2. There is a maximum limit on how many projects we can select.
Since we don’t have any constraint on time, we should choose a project, among the projects for which we have enough capital, which gives us a maximum profit. Following this greedy approach will give us the best solution.

While selecting a project, we will do two things:
1. Find all the projects that we can choose with the available capital.
2. From the list of projects in the 1st step, choose the project that gives us a maximum profit.
We can follow the Two Heaps approach similar to Find the Median of a Number Stream. Here are the steps of our algorithm:
1. Add all project capitals to a min-heap, so that we can select a project with the smallest capital requirement.
2. Go through the top projects of the min-heap and filter the projects that can be completed within our available capital. Insert the profits of all these projects into a max-heap, so that we can choose a project with the maximum profit.
3. Finally, select the top project of the max-heap for investment.
4. Repeat the 2nd and 3rd steps for the required number of projects.
````
/** 
 *  custom Heap class
 */

class Heap {
	constructor(comparator) {
		this.size = 0;
		this.values = [];
		this.comparator = comparator || Heap.minComparator;
	}

	add(val) {
		this.values.push(val);
		this.size ++;
		this.bubbleUp();
	}

	peek() {
		return this.values[0] || null;
	}

	poll() {
		const max = this.values[0];
		const end = this.values.pop();
		this.size --;
		if (this.values.length) {
			this.values[0] = end;
			this.bubbleDown();
		}
		return max;
	}

	bubbleUp() {
		let index = this.values.length - 1;
		let parent = Math.floor((index - 1) / 2);

		while (this.comparator(this.values[index], this.values[parent]) < 0) {
			[this.values[parent], this.values[index]] = [this.values[index], this.values[parent]];
			index = parent;
			parent = Math.floor((index - 1) / 2);
		}
	}

	bubbleDown() {
		let index = 0, length = this.values.length;

		while (true) {
			let left = null,
				right = null,
				swap = null,
				leftIndex = index * 2 + 1,
				rightIndex = index * 2 + 2;

			if (leftIndex < length) {
				left = this.values[leftIndex];
				if (this.comparator(left, this.values[index]) < 0) swap = leftIndex;
			}

			if (rightIndex < length) {
				right = this.values[rightIndex];
				if ((swap !== null && this.comparator(right, left) < 0) || (swap === null && this.comparator(right, this.values[index]))) {
					swap = rightIndex;
				}
			}
			if (swap === null) break;

			[this.values[index], this.values[swap]] = [this.values[swap], this.values[index]];
			index = swap;
		}
	}
   remove(value) {
    let idx;
    for (let i of this.index[value]) {
      idx = i;
      break;
    }
    this.index[value].delete(idx);
    if (idx === this.values.length - 1) return this.value.pop();
    this.values[idx] = this.value.pop()
    this.idxs[this.values[idx]].delete(this.value.length);
    this.idxs[this.store[idx]].add(idx);
    this.heapifyDown(this.heapifyUp(idx));
  }
}

/** 
 *  Min Comparator
 */
Heap.minComparator = (a, b) => { return a - b; }

/** 
 *  Max Comparator
 */
Heap.maxComparator = (a, b) => { return b - a; }
````
````js
function findMaximumCapital(capital, profits, numberOfProjects, initialCapital) {
  const minCapitalHeap = new Heap(Heap.minComparator);
  const maxProfitHeap = new Heap(Heap.maxComparator);

  // insert all project capitals to a min-heap
  for (i = 0; i < profits.length; i++) {
    minCapitalHeap.add([capital[i], i]);
  }

  // let's try to find a total of 'numberOfProjects' best projects
  let availableCapital = initialCapital;
  for (i = 0; i < numberOfProjects; i++) {
    // find all projects that can be selected within the available capital and insert them in a max-heap
    while (minCapitalHeap.size > 0 && minCapitalHeap.peek()[0] <= availableCapital) {
      const [capital, index] = minCapitalHeap.values.pop();
      maxProfitHeap.add([profits[index], index]);
    }

    // terminate if we are not able to find any project that can be completed within the available capital
    if (maxProfitHeap.size === 0) {
      break;
    }

    // select the project with the maximum profit
    availableCapital += maxProfitHeap.pop()[0];
  }

  return availableCapital;
}


console.log(`Maximum capital: ${findMaximumCapital([0, 1, 2], [1, 2, 3], 2, 1)}`);
console.log(`Maximum capital: ${findMaximumCapital([0, 1, 2, 3], [1, 2, 3, 5], 3, 0)}`);
````

- Since, at the most, all the projects will be pushed to both the heaps once, the time complexity of our algorithm is `O(NlogN + KlogN)`, where `N` is the total number of projects and `K` is the number of projects we are selecting.
- The space complexity will be `O(N)` because we will be storing all the projects in the heaps.
## 🌟 😴 Next Interval (hard)
https://leetcode.com/problems/find-right-interval/

> Given an array of intervals, find the next interval of each interval. In a list of intervals, for an interval ‘i’ its next interval ‘j’ will have the smallest ‘start’ greater than or equal to the ‘end’ of ‘i’.

Write a function to return an array containing indices of the next interval of each input interval. If there is no next interval of a given interval, return -1. It is given that none of the intervals have the same start point.

Example 1:

Input: Intervals [[2,3], [3,4], [5,6]]
Output: [1, 2, -1]
Explanation: The next interval of [2,3] is [3,4] having index ‘1’. Similarly, the next interval of [3,4] is [5,6] having index ‘2’. There is no next interval for [5,6] hence we have ‘-1’.

Example 2:

Input: Intervals [[3,4], [1,5], [4,6]]
Output: [2, -1, -1]
Explanation: The next interval of [3,4] is [4,6] which has index ‘2’. There is no next interval for [1,5] and [4,6].

A brute force solution could be to take one interval at a time and go through all the other intervals to find the next interval. This algorithm will take `O(N^2)` where `N` is the total number of intervals. Can we do better than that?

We can utilize the Two Heaps approach. We can push all intervals into two heaps: one heap to sort the intervals on maximum start time (let’s call it maxStartHeap) and the other on maximum end time (let’s call it maxEndHeap). We can then iterate through all intervals of the maxEndHeap to find their next interval. Our algorithm will have the following steps:

Take out the top (having highest end) interval from the maxEndHeap to find its next interval. Let’s call this interval topEnd.
Find an interval in the maxStartHeap with the closest start greater than or equal to the start of topEnd. Since maxStartHeap is sorted by ‘start’ of intervals, it is easy to find the interval with the highest ‘start’. Let’s call this interval topStart.
Add the index of topStart in the result array as the next interval of topEnd. If we can’t find the next interval, add ‘-1’ in the result array.
Put the topStart back in the maxStartHeap, as it could be the next interval of other intervals.
Repeat steps 1-4 until we have no intervals left in maxEndHeap.
😴(needs review)
````js
class Interval {
  constructor(start, end) {
    this.start = start;
    this.end = end;
  }
}


function find_next_interval(intervals) {
  const n = intervals.length;

  // heaps for finding the maximum start and end
  const maxStartHeap = new Heap([], null, ((a, b) => a[0] - b[0]));
  const maxEndHeap = new Heap([], null, ((a, b) => a[0] - b[0]));

  const result = Array(n).fill(0);
  for (endIndex = 0; endIndex < n; endIndex++) {
    maxStartHeap.push([intervals[endIndex].start, endIndex]);
    maxEndHeap.push([intervals[endIndex].end, endIndex]);
  }

  // go through all the intervals to find each interval's next interval
  for (i = 0; i < n; i++) {
    // let's find the next interval of the interval which has the highest 'end'
    const [topEnd, endIndex] = maxEndHeap.pop();
    result[endIndex] = -1; // defaults to -1
    if (maxStartHeap.peek()[0] >= topEnd) {
      let [topStart, startIndex] = maxStartHeap.pop();
      // find the the interval that has the closest 'start'
      while (maxStartHeap.length > 0 && maxStartHeap.peek()[0] >= topEnd) {
        [topStart, startIndex] = maxStartHeap.pop();
      }
      result[endIndex] = startIndex;
      // put the interval back as it could be the next interval of other intervals
      maxStartHeap.push([topStart, startIndex]);
    }
  }
  return result;
}


result = find_next_interval([new Interval(2, 3), new Interval(3, 4), new Interval(5, 6)]);
console.log(`Next interval indices are: ${result}`);

result = find_next_interval([new Interval(3, 4), new Interval(1, 5), new Interval(4, 6)]);
console.log(`Next interval indices are: ${result}`);
````
- The time complexity of our algorithm will be `O(NlogN)`, where `N` is the total number of intervals.
- The space complexity will be `O(N)` because we will be storing all the intervals in the heaps.
