# Pattern 11: Modified Binary Search

As we know, whenever we are given a sorted <b>Array</b> or <b>LinkedList</b> or <b>Matrix</b>, and we are asked to find a certain element, the best algorithm we can use is the <b>Binary Search</b>.

## Order-agnostic Binary Search (easy)
https://leetcode.com/problems/binary-search/
> Given a sorted array of numbers, find if a given number `key` is present in the array. Though we know that the array is sorted, we don’t know if it’s sorted in ascending or descending order. You should assume that the array can have duplicates.
>
> Write a function to return the index of the `key` if it is present in the array, otherwise return `-1`.

To make things simple, let’s first solve this problem assuming that the input array is sorted in ascending order. Here are the set of steps for <b>Binary Search</b>:
1. Let’s assume `start` is pointing to the first index and `end` is pointing to the last index of the input array (let’s call it `arr`). This means:
````
    int start = 0;
    int end = arr.length - 1;
````
2. First, we will find the `middle` of `start` and `end`. An easy way to find the middle would be: <i>middle=(start+end)/2</i>.  The safest way to find the middle of two numbers without getting an overflow is as follows:
````
     middle  = start + (end-start)/2
`````
3. Next, we will see if the `key` is equal to the number at index `middle`. If it is equal we return `middle` as the required index.
4. If `key` is not equal to number at index middle, we have to check two things:
- If `key < arr[middle]`, then we can conclude that the `key` will be smaller than all the numbers after index `middle` as the array is sorted in the ascending order. Hence, we can reduce our search to `end = mid - 1`.
- If `key > arr[middle]`, then we can conclude that the `key` will be greater than all numbers before index `middle` as the array is sorted in the ascending order. Hence, we can reduce our search to `start = mid + 1`.
- We will repeat steps 2-4 with new ranges of `start` to `end`. If at any time `start` becomes greater than `end`, this means that we can’t find the `key` in the input array and we must return `-1`.

If the array is sorted in the descending order, we have to update the step 4 above as:
- If `key > arr[middle]`, then we can conclude that the `key` will be greater than all numbers after index `middle` as the array is sorted in the descending order. Hence, we can reduce our search to `end = mid - 1`.
- If `key < arr[middle]`, then we can conclude that the `key` will be smaller than all the numbers before index `middle` as the array is sorted in the descending order. Hence, we can reduce our search to `start = mid + 1`.
Finally, how can we figure out the sort order of the input array? We can compare the numbers pointed out by `start` and `end` index to find the sort order. If `arr[start] < arr[end]`, it means that the numbers are sorted in ascending order otherwise they are sorted in the descending order.
````js
function binarySearch (arr, key) {
  let start = 0
  let end = arr.length -1
  
  //check to see if arr is sorted ascending or descending
  const isAscending = arr[start] < arr[end]
  
  while(start <= end) {
    //calculate the middle of the current range
    let middle = Math.floor(start + (end-start)/2)
    
    if(key === arr[middle]) {
      return middle
    }
    
    if(isAscending) {
      //ascending order
      if(key < arr[middle]) {
        //the key can be in the first half
        end = middle - 1
      } else {
        //key > arr[middle], so the key can be in the 
        //second half
        start = middle + 1
      }
    } 
    else {
      //descending order
      if(key > arr[middle]) {
        //the key can be in the first half
        end = middle -1
      } else {
        //key < arr[middle], the key can be in the 
        //second half
        start = middle + 1
      }
    }
  }
  
  // key not found
  return -1;
};

binarySearch([4, 6, 10], 10)//2
binarySearch([1, 2, 3, 4, 5, 6, 7], 5)//4
binarySearch([10, 6, 4], 10)//0
binarySearch([10, 6, 4], 4)//2
````
- Since, we are reducing the search range by half at every step, this means that the time complexity of our algorithm will be `O(log N)` where `N` is the total elements in the given array.
- The algorithm runs in constant space `O(1)`.

## Ceiling of a Number (medium)

> Given an array of numbers sorted in an ascending order, find the ceiling of a given number `key`. The ceiling of the `key` will be the smallest element in the given array greater than or equal to the `key`.
>
> Write a function to return the index of the ceiling of the `key`. If there isn’t any ceiling return `-1`.

This problem follows the <b>Binary Search</b> pattern. Since <b>Binary Search</b> helps us find a number in a sorted array efficiently, we can use a modified version of the <b>Binary Search</b> to find the ceiling of a number.

We can use a similar approach as discussed in <b>Order-agnostic Binary Search</b>. We will try to search for the `key` in the given array. If we find the `key`, we return its `index` as the ceiling. If we can’t find the `key`, the next big number will be pointed out by the index `start`. 

Since we are always adjusting our range to find the `key`, when we exit the loop, the start of our range will point to the smallest number greater than the `key` as shown in the above picture.

We can add a check in the beginning to see if the `key` is bigger than the biggest number in the input array. If so, we can return `-1`.

````js
function searchCeilingOfNumber(arr, key) {
  const n = arr.length 
  let start = 0;
  let end = n - 1;
  
  if (key > arr[end]) {
    return -1;
  }
  
  while (start <= end) {
    let mid = Math.floor(start + (end - start) / 2);
    
    if (arr[mid] > key) {
        // key is in first half
        end = mid - 1;
      } else if (arr[mid] < key) {
        //key is in second half
        start = mid + 1;
      } else {
        //found the key
        return mid
      }
  }
  
  // since the loop is running until 'start <= end', so at the end of the while loop, 'start === end+1'
  // we are not able to find the element in the given array, so the next big number will be arr[start]
  return start;
}

searchCeilingOfNumber([4, 6, 10], 6); 
//1, The smallest number greater than or equal to '6' is '6' having index '1'.
searchCeilingOfNumber([1, 3, 8, 10, 15], 12); 
//4, The smallest number greater than or equal to '12' is '15' having index '4'.
searchCeilingOfNumber([4, 6, 10], 17); 
//-1, There is no number greater than or equal to '17' in the given array.
searchCeilingOfNumber([4, 6, 10], -1); 
//0, The smallest number greater than or equal to '-1' is '4' having index '0'.
````

- Since, we are reducing the search range by half at every step, this means that the time complexity of our algorithm will be `O(log N)` where `N` is the total elements in the given array.
- The algorithm runs in constant space `O(1)`.

### Similar Problem
https://leetcode.com/problems/search-insert-position/
> Given an array of numbers sorted in ascending order, find the floor of a given number ‘key’. The floor of the ‘key’ will be the biggest element in the given array smaller than or equal to the ‘key’
>
> Write a function to return the index of the floor of the ‘key’. If there isn’t a floor, return -1.

````js
function searchFloorOfNumber(arr, key) {
  let start = 0;
  let end = arr.length - 1;

  if (key < arr[start]) {
    return -1;
  }

  while (start <= end) {
    let mid = Math.floor(start + (end - start) / 2);

    if (key < arr[mid]) {
      // key is in first half
      end = mid - 1;
    } else if (key > arr[mid]) {
      //key is in second half
      start = mid + 1;
    } else {
      //found the key
      return mid;
    }
  }

  // since the loop is running until 'start <= end', so at the end of the while loop, 'start === end+1'
  // we are not able to find the element in the given array, so the next smaller number will be arr[end]
  return end;
}

searchFloorOfNumber([4, 6, 10], 6); 
//1, The biggest number smaller than or equal to '6' is '6' having index '1'.
searchFloorOfNumber([1, 3, 8, 10, 15], 12); 
//3, The biggest number smaller than or equal to '12' is '10' having index '3'.
searchFloorOfNumber([4, 6, 10], 17); 
//2, The biggest number smaller than or equal to '17' is '10' having index '2'.
searchFloorOfNumber([4, 6, 10], -1); 
//-1, There is no number smaller than or equal to '-1' in the given array.

````
- Since, we are reducing the search range by half at every step, this means that the time complexity of our algorithm will be `O(log N)` where `N` is the total elements in the given array.
- The algorithm runs in constant space `O(1)`.

## Next Letter (medium)
https://leetcode.com/problems/find-smallest-letter-greater-than-target/

> Given an array of lowercase letters sorted in ascending order, find the <b>smallest letter</b> in the given array <b>greater than a given `key`</b>.
> 
> Assume the given array is a <b>circular list</b>, which means that the last letter is assumed to be connected with the first letter. This also means that the smallest letter in the given array is greater than the last letter of the array and is also the first letter of the array.
>
> Write a function to return the next letter of the given `key`.

The problem follows the <b>Binary Search</b> pattern. Since <b>Binary Search</b> helps us find an element in a sorted array efficiently, we can use a modified version of it to find the next letter.

We can use a similar approach as discussed in <b>Ceiling of a Number</b>. There are a couple of differences though:

1. The array is considered circular, which means if the `key` is bigger than the last letter of the array or if it is smaller than the first letter of the array, the `key`’s next letter will be the first letter of the array.
2. The other difference is that we have to find the next biggest letter which can’t be equal to the `key`. This means that we will ignore the case where `key == arr[middle]`. To handle this case, we can update our start range to `start = middle +1`.

In the end, instead of returning the element pointed out by start, we have to return the letter pointed out by `start % array.length`. This is needed because of point 2 discussed above. Imagine that the last letter of the array is equal to the `key`. In that case, we have to return the first letter of the input array.

````js
function searchNextLetter(letters, key) {
  let n = letters.length;
  let start = 0;
  let end = n - 1;

  while (start <= end) {
    let mid = Math.floor(start + (end - start) / 2);

    //in first half
    if (key < letters[mid]) {
      end = mid - 1;
    } else {
      //key > letters[mid]
      //in second half
      start = mid + 1;
    }
  }
  // since the loop is running until 'start <= end', so at the end of the while loop, 'start === end+1'
  return letters[start % n];
}

searchNextLetter(['a', 'c', 'f', 'h'], 'f'); 
//'h', The smallest letter greater than 'f' is 'h' in the given array.
searchNextLetter(['a', 'c', 'f', 'h'], 'b'); 
//'c', The smallest letter greater than 'b' is 'c'.
searchNextLetter(['a', 'c', 'f', 'h'], 'm'); 
//'a', As the array is assumed to be circular, the smallest letter greater than 'm' is 'a'.
searchNextLetter(['a', 'c', 'f', 'h'], 'h'); 
//'a', As the array is assumed to be circular, the smallest letter greater than 'h' is 'a'.
````
- Since, we are reducing the search range by half at every step, this means that the time complexity of our algorithm will be `O(log N)` where `N` is the total elements in the given array.
- The algorithm runs in constant space `O(1)`.
## Number Range (medium)
https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/

> Given an array of numbers sorted in ascending order, find the range of a given number `key`. The range of the `key` will be the first and last position of the `key` in the array.
> 
> Write a function to return the range of the `key`. If the `key` is not present return `[-1, -1]`.
The problem follows the <b>Binary Search</b> pattern. Since <b>Binary Search</b> helps us find a number in a sorted array efficiently, we can use a modified version of the <b>Binary Search</b> to find the first and the last position of a number.

We can use a similar approach as discussed in <b>Order-agnostic Binary Search</b>. We will try to search for the `key` in the given array; if the `key` is found (i.e. `key == arr[middle`) we have two options:

1. When trying to find the first position of the `key`, we can update `end = middle - 1` to see if the `key` is present before `middle`.
2. When trying to find the last position of the `key`, we can update `start = middle + 1` to see if the `key` is present after `middle`.
In both cases, we will keep track of the last position where we found the `key`. These positions will be the required range.
````js
function findRange(arr, key) {
  let result = [-1, -1]
  result[0] = binarySearch(arr, key, false)
  
  if(result[0] !== -1){
    //no need to search, if key is not present in the input array
    result[1] = binarySearch(arr, key, true)
  }
  return result;
}

function binarySearch(arr, key, findMaxIndex) {
  let keyIndex = -1;
  let start = 0;
  let end = arr.length - 1;

  while (start <= end) {
    let mid = Math.floor(start + (end - start) / 2);

    if (key < arr[mid]) {
      end = mid - 1;
    } else if (key > arr[mid]) {
      start = mid + 1;
    } else {
      //key === arr[mid];
      keyIndex = mid;
      if (findMaxIndex) {
        //search ahead to find the last index of key
        start = mid + 1;
      } else {
        //search behind to find the last index of key
        end = mid - 1;
      }
    }
  }

  return keyIndex;
}

findRange([4, 6, 6, 6, 9], 6); //[1, 3]
findRange([1, 3, 8, 10, 15], 10); //[3, 3]
findRange([1, 3, 8, 10, 15], 12); //[-1,-1]
````
- Since, we are reducing the search range by half at every step, this means that the time complexity of our algorithm will be `O(log N)` where `N` is the total elements in the given array.
- The algorithm runs in constant space `O(1)`.

## Search in a Sorted Infinite Array (medium)
https://leetcode.com/problems/search-in-a-sorted-array-of-unknown-size/

> Given an infinite sorted array (or an array with unknown size), find if a given number `key` is present in the array. Write a function to return the index of the `key` if it is present in the array, otherwise return `-1`.
> 
> Since it is not possible to define an array with infinite (unknown) size, you will be provided with an interface `ArrayReader` to read elements of the array. `ArrayReader.get(index)` will return the number at `index`; if the array’s size is smaller than the `index`, it will return `Integer.MAX_VALUE`.

The problem follows the <b>Binary Search</b> pattern. Since <b>Binary Search</b> helps us find a number in a sorted array efficiently, we can use a modified version of the <b>Binary Search</b>  to find the `key` in an infinite sorted array.

The only issue with applying binary search in this problem is that we don’t know the bounds of the array. To handle this situation, we will first find the proper bounds of the array where we can perform a binary search.

An efficient way to find the proper bounds is to start at the beginning of the array with the bound’s size as `1` and exponentially increase the bound’s size (i.e., double it) until we find the bounds that can have the key.
````js
class ArrayReader {
  constructor(arr) {
    this.arr = arr;
  }

  get(index) {
    if (index >= this.arr.length) return Number.MAX_SAFE_INTEGER;
    return this.arr[index];
  }
}

function searchInInfiniteArray(reader, key) {
  //1. find the proper bounds
  let start = 0;
  let end = 1;
  while (reader.get(end) < key) {
    let newStart = end + 1;
    end += (end - start + 1) * 2;

    //2. increase to double the bounds size
    start = newStart;
  }
  return binarySearch(reader, key, start, end);
}

function binarySearch(reader, key, start, end) {
  while (start <= end) {
    let mid = Math.floor(start + (end - start) / 2);

    if (key < reader.get(mid)) {
      end = mid - 1;
    } else if (key > reader.get(mid)) {
      start = mid + 1;
    } else {
      //found the key
      return mid;
    }
  }
  return -1;
}

reader = new ArrayReader([4, 6, 8, 10, 12, 14, 16, 18, 20, 22, 24, 26, 28, 30]);
searchInInfiniteArray(reader, 11); //-1, The key is not present in the array.
searchInInfiniteArray(reader, 16); // 6, The key is present at index '6' in the array.

reader = new ArrayReader([1, 3, 8, 10, 15]);
searchInInfiniteArray(reader, 15); //4, The key is present at index '4' in the array.
searchInInfiniteArray(reader, 200); //-1, The key is not present in the array.
````

- There are two parts of the algorithm. In the first part, we keep increasing the bound’s size exponentially (double it every time) while searching for the proper bounds. Therefore, this step will take `O(log N)` assuming that the array will have maximum `N` numbers. In the second step, we perform the binary search which will take `O(log N)`, so the overall time complexity of our algorithm will be` O(log N + log N)` which is asymptotically equivalent to `O(log N)`.

- The algorithm runs in constant space `O(1)`.

## Minimum Difference Element (medium)
https://leetcode.com/problems/minimum-absolute-difference/
> Given an array of numbers sorted in ascending order, find the element in the array that has the minimum difference with the given `key`.

The problem follows the <b>Binary Search</b> pattern. Since <b>Binary Search</b> helps us find a number in a sorted array efficiently, we can use a modified version of the <b>Binary Search</b> to find the number that has the minimum difference with the given `key`.

We can use a similar approach as discussed in <b>Order-agnostic Binary Search</b>. We will try to search for the `key` in the given array. If we find the `key` we will return it as the minimum difference number. If we can’t find the `key`, (at the end of the loop) we can find the differences between the `key` and the numbers pointed out by indices `start` and `end`, as these two numbers will be closest to the `key`. The number that gives minimum difference will be our required number.
````js
function searchMinDiffElement(arr, key) {
  let start = 0;
  let end = arr.length - 1;

  if (key <= arr[start]) {
    return arr[start];
  } else if (key >= arr[end]) {
    return arr[end];
  }

  while (start <= end) {
    let mid = Math.floor(start + (end - start) / 2);

    if (key < arr[mid]) {
      end = mid - 1;
    } else if (key > arr[mid]) {
      start = mid + 1;
    } else {
      return arr[mid];
    }
  }
  // at the end of the while loop, 'start === end+1'
  // we are not able to find the element in the given array
  // return the element which is closest to the 'key'
  if (arr[start] - key < key - arr[end]) {
    return arr[start];
  }
  return arr[end];
}

searchMinDiffElement([4, 6, 10], 7);
//6, The difference between the key '7' and '6' is minimum than any other number in the array
searchMinDiffElement([4, 6, 10], 4);
//4
searchMinDiffElement([1, 3, 8, 10, 15], 12);
//10
searchMinDiffElement([4, 6, 10], 17);
//10
````
- Since, we are reducing the search range by half at every step, this means the time complexity of our algorithm will be `O(logN)` where `N` is the total elements in the given array.
- The algorithm runs in constant space `O(1)`.
## Bitonic Array Maximum (easy)
> Find the maximum value in a given Bitonic array. An array is considered bitonic if it is monotonically increasing and then monotonically decreasing. Monotonically increasing or decreasing means that for any index `i` in the `array arr[i] != arr[i+1]`.

A <b>bitonic array</b> is a sorted array; the only difference is that its first part is sorted in <i>ascending order</i> and the second part is sorted in <i>descending order</i>. We can use a similar approach as discussed in <b>Order-agnostic Binary Search</b>. Since no two consecutive numbers are same (as the array is monotonically increasing or decreasing), whenever we calculate the `middle`, we can compare the numbers pointed out by the index `middle` and `middle+1` to find if we are in the ascending or the descending part. So:
1.  If `arr[middle] > arr[middle + 1]`, we are in the second (descending) part of the bitonic array. Therefore, our required number could either be pointed out by `middle` or will be before `middle`. This means we will be doing: `end = middle`.
2. If `arr[middle] < arr[middle + 1]`, we are in the first (ascending) part of the bitonic array. Therefore, the required number will be after `middle`. This means we will be doing: `start = middle + 1`.

We can break when `start == end`. Due to the two points mentioned above, both `start` and `end` will be pointing at the maximum number of the bitonic array.

````js
function findMaxInBitonicArray(arr) {
  let start = 0;
  let end = arr.length - 1;

  while (start < end) {
    let mid = Math.floor(start + (end - start) / 2);

    if (arr[mid] > arr[mid + 1]) {
      end = mid;
    } else {
      //arr[mid] < arr[mid+1]
      start = mid + 1;
    }
  }
  //at the end of the while loop start === end
  return arr[start];
}

findMaxInBitonicArray([1, 3, 8, 12, 4, 2]);
//12, The maximum number in the input bitonic array is '12'.

findMaxInBitonicArray([3, 8, 3, 1]);
//8
findMaxInBitonicArray([1, 3, 8, 12]);
//12
findMaxInBitonicArray([10, 9, 8]);
//10
````

- Since, we are reducing the search range by half at every step, this means the time complexity of our algorithm will be `O(logN)` where `N` is the total elements in the given array.
- The algorithm runs in constant space `O(1)`.
## 🌟 Search Bitonic Array (medium)
> Given a Bitonic array, find if a given `key` is present in it. An array is considered bitonic if it is monotonically increasing and then monotonically decreasing. Monotonically increasing or decreasing means that for any index `i` in the array `arr[i] != arr[i+1]`.
> 
> Write a function to return the index of the `key`. If the `key` is not present, return `-1`.

The problem follows the <b>Binary Search</b> pattern. Since <b>Binary Search</b> helps us efficiently find a number in a sorted array we can use a modified version of the <b>Binary Search</b> to find the `key` in the bitonic array.

Here is how we can search in a <b>bitonic array</b>:

1. First, we can find the index of the maximum value of the <b>bitonic array</b>, similar to <b>Bitonic Array Maximum</b>. Let’s call the index of the maximum number `maxIndex`.
2. Now, we can break the array into two sub-arrays:
    - Array from index `0` to `maxIndex`, sorted in ascending order.
    - Array from index `maxIndex+1` to `array_length-1`, sorted in descending order.
3. We can then call <b>Binary Search</b> separately in these two arrays to search the `key`. We can use the same <b>Order-agnostic Binary Search</b> for searching.
````js
function searchBitonicArray(arr, key) {
  const maxIndex = findMax(arr);
  const keyIndex = binarySearch(arr, key, 0, maxIndex);
  if (keyIndex !== -1) {
    return keyIndex;
  }

  return binarySearch(arr, key, maxIndex + 1, arr.length - 1);
}

//find index of the max value in bitonic array
function findMax(arr) {
  let start = 0;
  let end = arr.length - 1;

  while (start < end) {
    const mid = Math.floor(start + (end - start) / 2);

    if (arr[mid] > arr[mid + 1]) {
      end = mid;
    } else {
      //arr[mid] < arr[mid+1]
      start = mid + 1;
    }
  }
  //at the end of the while loop start === end
  return start;
}

//order-agnostic binary search
function binarySearch(arr, key, start, end) {
  //check to see if arr is sorted ascending or descending
  const isAscending = arr[start] < arr[end];

  while (start <= end) {
    //calculate the middle of the current range
    const middle = Math.floor(start + (end - start) / 2);

    if (key === arr[middle]) {
      return middle;
    }

    if (isAscending) {
      //ascending order
      if (key < arr[middle]) {
        //the key can be in the first half
        end = middle - 1;
      } else {
        //key > arr[middle], so the key can be in the
        //second half
        start = middle + 1;
      }
    } else {
      //descending order
      if (key > arr[middle]) {
        //the key can be in the first half
        end = middle - 1;
      } else {
        //key < arr[middle], the key can be in the
        //second half
        start = middle + 1;
      }
    }
  }

  // key not found
  return -1;
}

searchBitonicArray([1, 3, 8, 4, 3], 4);
//3
searchBitonicArray([3, 8, 3, 1], 8);
//1
searchBitonicArray([1, 3, 8, 12], 12);
//3
searchBitonicArray([10, 9, 8], 10);
//0
````
- Since, we are reducing the search range by half at every step, this means the time complexity of our algorithm will be `O(logN)` where `N` is the total elements in the given array.
- The algorithm runs in constant space `O(1)`.
## 🌟 Search in Rotated Array (medium)
https://leetcode.com/problems/search-in-rotated-sorted-array/
> Given an array of numbers which is sorted in ascending order and also rotated by some arbitrary number, find if a given ‘key’ is present in it.
>
> Write a function to return the index of the ‘key’ in the rotated array. If the ‘key’ is not present, return -1. You can assume that the given array does not have any duplicates.

The problem follows the <b>Binary Search</b> pattern. We can use a similar approach as discussed in <b>Order-agnostic Binary Search</b> and modify it similar to <b>Search Bitonic Array</b> to search for the `key` in the rotated array.

After calculating the middle, we can compare the numbers at indices `start` and `middle`. This will give us two options:
1. If `arr[start] <= arr[middle]`, the numbers from `start` to `middle` are sorted in ascending order.
2. Else, the numbers from `middle+1` to `end` are sorted in ascending order.

Once we know which part of the array is sorted, it is easy to adjust our ranges. For example, if option 1 is `true`, we have two choices:
1. By comparing the `key` with the numbers at index `start` and `middle` we can easily find out if the `key` lies between indices `start` and `middle`; if it does, we can skip the second part => `end = middle -1`.
2. Else, we can skip the first part => `start = middle + 1`.

Since there are no duplicates in the given array, it is always easy to skip one part of the array in each iteration. However, if there are duplicates, it is not always possible to know which part is sorted. We will look into this case in the <b>Similar Problems</b> section.

````js
function searchRotatedArray(arr, key) {
  let start = 0;
  let end = arr.length - 1;

  while (start <= end) {
    const mid = Math.floor(start + (end - start) / 2);

    if (key === arr[mid]) {
      return mid;
    }

    if (arr[start] <= arr[mid]) {
      //first half is sorted in ascending order
      if (key >= arr[start] && key < arr[mid]) {
        //the key can be in the first half
        end = middle - 1;
      } else {
        //key > arr[middle], so the key can be in the
        //second half
        start = mid + 1;
      }
    } else {
      //second half is in descending order
      if (key > arr[mid] && key <= arr[end]) {
        start = mid + 1;
      } else {
        end = mid - 1;
      }
    }
  }

  // key not found
  return -1;
}

searchRotatedArray([10, 15, 1, 3, 8], 15);
//1, '15' is present in the array at index '1'.
searchRotatedArray([4, 5, 7, 9, 10, -1, 2], 10);
//4,'10' is present in the array at index '4'.
````
- Since, we are reducing the search range by half at every step, this means the time complexity of our algorithm will be `O(logN)` where `N` is the total elements in the given array.
- The algorithm runs in constant space `O(1)`.
### Similar Problem
https://leetcode.com/problems/search-in-rotated-sorted-array-ii/
> How do we search in a sorted and rotated array that also has duplicates?
> 
> The code above will fail in the following example!

The only problematic scenario is when the numbers at indices `start`, `middle`, and `end` are the same, as in this case, we can’t decide which part of the array is sorted. In such a case, the best we can do is to skip one number from both ends: `start = start + 1` & `end = end - 1`.
````js
function searchRotatedArrayWithDuplicates(arr, key) {
  let start = 0;
  let end = arr.length - 1;

  while (start <= end) {
    const mid = Math.floor(start + (end - start) / 2);

    if (key === arr[mid]) {
      return mid;
    }
    // the only difference from the previous solution,
    // if numbers at indexes start, mid, and end are same, we can't choose a side
    // the best we can do, is to skip one number from both ends as key !== arr[mid]
    if (arr[start] === arr[mid] && arr[end] === arr[mid]) {
      start += 1;
      end -= 1;
    } else if (arr[start] <= arr[mid]) {
      //first half is sorted in ascending order
      if (key >= arr[start] && key < arr[mid]) {
        //the key can be in the first half
        end = middle - 1;
      } else {
        //key > arr[middle], so the key can be in the
        //second half
        start = mid + 1;
      }
    } else {
      //second half is in descending order
      if (key > arr[mid] && key <= arr[end]) {
        start = mid + 1;
      } else {
        end = mid - 1;
      }
    }
  }

  // key not found
  return -1;
}

searchRotatedArrayWithDuplicates([3, 7, 3, 3, 3], 7);
//1, '7' is present in the array at index '1'.

````
- This algorithm will run most of the times in `O(logN)`. However, since we only skip two numbers in case of duplicates instead of half of the numbers, the worst case time complexity will become `O(N)`.
- The algorithm runs in constant space `O(1)`.


## 🌟 Rotation Count (medium)
https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/



> Given an array of numbers which is sorted in ascending order and is rotated ‘k’ times around a pivot, find ‘k’.
>
> You can assume that the array does not have any duplicates.

This problem follows the <b>Binary Search</b> pattern. We can use a similar strategy as discussed in <b>Search in Rotated Array</b>.

In this problem, actually, we are asked to find the index of the minimum element. The number of times the minimum element is moved to the right will be equal to the number of rotations. An interesting fact about the minimum element is that it is the only element in the given array which is smaller than its previous element. Since the array is sorted in ascending order, all other elements are bigger than their previous element.

After calculating the `middle`, we can compare the number at index `middle` with its `previous` and `next` number. This will give us two options:
1. If `arr[middle] > arr[middle + 1]`, then the element at `middle + 1` is the smallest.
2. If `arr[middle - 1] > arr[middle]`, then the element at `middle` is the smallest.

To adjust the ranges we can follow the same approach as discussed in <b>Search in Rotated Array</b>. Comparing the numbers at indices `start` and `middle` will give us two options:
1. If `arr[start] < arr[middle]`, the numbers from `start` to `middle` are sorted.
2. Else, the numbers from `middle + 1` to `end` are sorted.
````js
function countRotations(arr) {
  let start = 0;
  let end = arr.length - 1;

  while (start < end) {
    const mid = Math.floor(start + (end - start) / 2);

    // if mid is greater than the next element
    if (mid < end && arr[mid] > arr[mid + 1]) {
      return mid + 1;
    }

    // if mid is smaller than the next element
    if (mid > start && arr[mid - 1] > arr[mid]) {
      return mid;
    }

    if (arr[start] < arr[mid]) {
      //first half is sorted, so pivot is in second half
      start = mid + 1;
    } else {
      //second half is sorted, so pivot is in first half

      start = mid + 1;
    }
  }

  // key not found
  return 0;
}

countRotations([10, 15, 1, 3, 8]);
//2, The array has been rotated 2 times.

countRotations([4, 5, 7, 9, 10, -1, 2]);
//5, The array has been rotated 5 times.

countRotations([1, 3, 8, 10]);
//0, The array has been not been rotated.
````
- Since, we are reducing the search range by half at every step, this means the time complexity of our algorithm will be `O(logN)` where `N` is the total elements in the given array.
- The algorithm runs in constant space `O(1)`.

### Similar Problem
https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/
> How do we find the rotation count of a sorted and rotated array that has duplicates too?
> 
> The above code will fail on the following example!

We can follow the same approach as discussed in <b>Search in Rotated Array</b>. The only difference is that before incrementing `start` or decrementing `end`, we will check if either of them is the smallest number.
````js
function countRotationsWithDuplicates(arr) {
  let start = 0;
  let end = arr.length - 1;

  while (start < end) {
    const mid = Math.floor(start + (end - start) / 2);

    // if mid is greater than the next element
    if (mid < end && arr[mid] > arr[mid + 1]) {
      return mid + 1;
    }

    // if mid is smaller than the next element
    if (mid > start && arr[mid - 1] > arr[mid]) {
      return mid;
    }

    // this is the only difference from the previous solution
    // if numbers at indices start, mid, and end are same, we can't choose a side
    // the best we can do is to skip one number from both ends if they are not the smallest number
    if (arr[start] === arr[mid] && arr[end] === arr[mid]) {
      if (arr[start] > arr[start + 1]) {
        // if element at start+1 is not the smallest
        return start + 1;
      }
      start++;
      if (arr[end - 1] > arr[end]) {
        // if the element at end is not the smallest
        return end;
      }
      end--;
    }
    // left side is sorted, so the pivot is on right side
    else if (
      arr[start] < arr[mid] ||
      (arr[start] == arr[mid] && arr[mid] > arr[end])
    ) {
      start = mid + 1;
    } else {
      // right side is sorted, so the pivot is on the left side
      end = mid - 1;
    }
  }

  // the array has not been rotated
  return 0;
}

countRotationsWithDuplicates([3, 3, 7, 3]);
//3, The array has been rotated 3 times
````
- This algorithm will run in `O(logN)` most of the times, but since we only skip two numbers in case of duplicates instead of the half of the numbers, therefore the worst case time complexity will become `O(N)`.
- The algorithm runs in constant space `O(1)`.


