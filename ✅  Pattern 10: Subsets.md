# Pattern 10: Subsets

A huge number of coding interview problems involve dealing with <b>Permutations</b> and <b>Combinations</b> of a given set of elements. This pattern describes an efficient <b>Breadth First Search (BFS)</b> approach to handle all these problems.

## Subsets (easy)
https://leetcode.com/problems/subsets/

> Given a set with distinct elements, find all of its distinct subsets.

To generate all subsets of the given set, we can use the <b>Breadth First Search (BFS)</b> approach. We can start with an empty set, iterate through all numbers one-by-one, and add them to existing sets to create new subsets.

Let’s take the example-2 mentioned above to go through each step of our algorithm:

Given set: `[1, 5, 3]`
1. Start with an empty set: `[[]]`
2. Add the first number `1` to all the existing subsets to create new subsets: `[[],`<b>`[1]`</b>`];`
3. Add the second number `5` to all the existing subsets: `[[], [1], `<b>`[5], [1,5]`</b>`]`;
4. Add the third number `3` to all the existing subsets: `[[], [1], [5], [1,5], `<b>`[3], [1,3], [5,3], [1,5,3]`</b>`]`.

Since the input set has distinct elements, the above steps will ensure that we will not have any duplicate subsets.
````js
function findSubsets(nums) {
  const subsets = [];
  
  //start by adding the empty subset
  subsets.push([])
  
  for(let i = 0; i < nums.length; i++) {
    const currentNumber = nums[i]
    
    //we will take all existing subsets and insert the current
    //number in them to create new subsets
    const n = subsets.length

    //create a new subset from the existing subset and insert
    //the current element to it
    for(let j = 0; j < n; j++) {
      
      //clone the permutation
      // const set1 = subsets[j].slice(0)
      
      // set1.push(currentNumber)
      subsets.push([...subsets[j], nums[i]])
    }
  }

  return subsets;
};


findSubsets([1, 3])
findSubsets([1, 5, 3])
````
- Since, in each step, the number of subsets doubles as we add each element to all the existing subsets, therefore, we will have a total of `O(2ᴺ)` subsets, where `N` is the total number of elements in the input set. And since we construct a new subset from an existing set, therefore, the time complexity of the above algorithm will be `O(N*2ᴺ)`.
- All the additional space used by our algorithm is for the output list. Since we will have a total of `O(2ᴺ)` subsets, and each subset can take up to `O(N)` space, therefore, the space complexity of our algorithm will be `O(N*2ᴺ)`.

## Subsets With Duplicates (medium)
https://leetcode.com/problems/subsets-ii/

> Given a set of numbers that might contain duplicates, find all of its distinct subsets.

This problem follows the <b>Subsets</b> pattern and we can follow a similar <b>Breadth First Search (BFS)</b> approach. The only additional thing we need to do is handle duplicates. Since the given set can have duplicate numbers, if we follow the same approach discussed in <b>Subsets</b>, we will end up with duplicate subsets, which is not acceptable. To handle this, we will do two extra things:
1. Sort all numbers of the given set. This will ensure that all duplicate numbers are next to each other.
2. Follow the same <b>BFS</b> approach but whenever we are about to process a duplicate (i.e., when the current and the previous numbers are same), instead of adding the current number (which is a duplicate) to all the existing subsets, only add it to the subsets which were created in the previous step.

Let’s take first Example mentioned below to go through each step of our algorithm:
````
Given set: [1, 5, 3, 3]  
Sorted set: [1, 3, 3, 5]
````
1. Start with an empty set: `[[]]`
2. Add the first number `1` to all the existing subsets to create new subsets: `[[], [1]]`;
3. Add the second number `3` to all the existing subsets: `[[], [1], [3], [1,3]]`.
4. The next number `3` is a duplicate. If we add it to all existing subsets we will get:
    ````
    [[], [1], [3], [1,3], [3], [1,3], [3,3], [1,3,3]]
    ````
````
We got two duplicate subsets: [3], [1,3]  
Whereas we only needed the new subsets: [3,3], [1,3,3] 
````
To handle this instead of adding `3` to all the existing subsets, we only add it to the new subsets which were created in the previous <b>3rd</b> step:
````
    [[], [1], [3], [1,3], [3,3], [1,3,3]]
````
5. Finally, add the forth number `5` to all the existing subsets: `[[], [1], [3], [1,3], [3,3], [1,3,3], [5], [1,5], [3,5], [1,3,5], [3,3,5], [1,3,3,5]]`

````js
function subsetsWithDupe(nums) {
  //sort the numbers to handle duplicates
  nums.sort((a,b) => a-b)
  
  const subsets = [];
  
  subsets.push([])
  
  let start = 0
  let end = 0
  
  for(let i = 0; i < nums.length; i++) {
    start = 0
    
    //if current and the previous elements are the same,
    //create new subsets only from the subsets
    //added in the previous step
    if(i > 0 && nums[i] === nums[i-1]) {
      start = end + 1
    }
    
    end = subsets.length - 1
    
    for(let j = start; j < end + 1; j++) {
      //create a new subset from the existing subset and add the
      //current element to it
      subsets.push([...subsets[j], nums[i]])
    }
  }
  
  return subsets;
};


subsetsWithDupe([1, 3, 3])
subsetsWithDupe([1, 5, 3, 3])
````
- Since, in each step, the number of subsets doubles (if not duplicate) as we add each element to all the existing subsets, therefore, we will have a total of `O(2ᴺ)` subsets, where `N` is the total number of elements in the input set. And since we construct a new subset from an existing set, therefore, the time complexity of the above algorithm will be `O(N*2ᴺ)`.
- All the additional space used by our algorithm is for the output list. Since, at most, we will have a total of `O(2ᴺ)` subsets, and each subset can take up to `O(N)` space, therefore, the space complexity of our algorithm will be `O(N*2ᴺ)`.
## Permutations (medium)
https://leetcode.com/problems/permutations/

> Given a set of distinct numbers, find all of its permutations.

Permutation is defined as the re-arranging of the elements of the set. For example, `{1, 2, 3}` has the following six permutations:
1. `{1, 2, 3}`
2. `{1, 3, 2}`
3. `{2, 1, 3}`
4. `{2, 3, 1}`
5. `{3, 1, 2}`
6. `{3, 2, 1}`

If a set has `n` distinct elements it will have `n!` permutations.

This problem follows the <b>Subsets</b> pattern and we can follow a similar <b>Breadth First Search (BFS)</b> approach. However, unlike <b>Subsets</b>, every permutation must contain all the numbers.

Let’s take the example mentioned below to generate all the permutations. Following a <b>BFS</b> approach, we will consider one number at a time:
1. If the given set is empty then we have only an empty permutation set: `[]`
2. Let’s add the first element `1`, the permutations will be: `[1]`
3. Let’s add the second element `3`, the permutations will be: `[3,1], [1,3]`
4. Let’s add the third element `5`, the permutations will be: `[5,3,1], [3,5,1], [3,1,5], [5,1,3], [1,5,3], [1,3,5]`
 
Let’s analyze the permutations in the 3rd and 4th step. How can we generate permutations in the 4th step from the permutations of the 3rd step?

If we look closely, we will realize that when we add a new number `5`, we take each permutation of the previous step and insert the new number in every position to generate the new permutations. For example, inserting `5` in different positions of `[3,1]` will give us the following permutations:
1. Inserting `5` before `3`: `[5,3,1]`
2. Inserting `5` between `3` and `1`: `[3,5,1]`
3. Inserting `5` after `1`: `[3,1,5]`

````js
function findPermutations(nums) {
  const result = [];
  let permutations = [[]]
  let numsLength = nums.length
  
   for(let i = 0; i < nums.length; i++) {
     const currentNumber = nums[i]
     
     //we will take all existing permutations an add the
     //current number to create a new permutation
     const n = permutations.length
     
     for(let p = 0; p < n; p++){
       const oldPermutation = permutations.shift()
       console.log(oldPermutation)
       
       //create a new permutation by adding the current number at every position
       for(let j  = 0; j < oldPermutation.length + 1; j++) {
         
         //clone the permutation
         const newPermutation = oldPermutation.slice(0)
         
         //insert the current number at index j
         newPermutation.splice(j, 0, currentNumber)
         
         if(newPermutation.length === numsLength) {
           result.push(newPermutation)
         } else {
           permutations.push(newPermutation)
         }
       }
     }
   }
  return result;
};

findPermutations([1, 3, 5])
````

- We know that there are a total of `N!` permutations of a set with `N` numbers. In the algorithm above, we are iterating through all of these permutations with the help of the two ‘for’ loops. In each iteration, we go through all the current permutations to insert a new number in them. To insert a number into a permutation of size ‘`N` will take `O(N)`, which makes the overall time complexity of our algorithm `O(N*N!)`.
- All the additional space used by our algorithm is for the `result` list and the `queue` to store the intermediate permutations. If you see closely, at any time, we don’t have more than `N!` permutations between the result list and the queue. Therefore the overall space complexity to store `N!` permutations each containing `N` elements will be `O(N*N!)`.

### Recursive Solution
````js
function permute(nums) {
  //recursion
  let subsets = []
 
  generatePermuationsRecursive(nums, 0, [], subsets)
  
  return subsets   
};

function generatePermuationsRecursive(nums, index, currentPermuation, subsets) {
    if(index === nums.length) {
      subsets.push(currentPermuation)
    } else {
      //create a new permuation by adding the current number at every position
      for(let i = 0; i < currentPermuation.length +1; i++) {
        let newPermutation = currentPermuation.slice(0)
        
        //insert nums[index] at index i
        newPermutation.splice(i, 0, nums[index])
        generatePermuationsRecursive(nums, index+1, newPermutation, subsets)
      }
    }  
  }
  ````
## String Permutations by changing case (medium)
https://leetcode.com/problems/letter-case-permutation/

> Given a string, find all of its permutations preserving the character sequence but changing case.

This problem follows the <b>Subsets</b> pattern and can be mapped to <b>Permutations</b>.

Let’s take Example-2 mentioned above to generate all the permutations. Following a <b>BFS</b> approach, we will consider one character at a time. Since we need to preserve the character sequence, we can start with the actual string and process each character (i.e., make it upper-case or lower-case) one by one:
1. Starting with the actual string: `"ab7c"`
2. Processing the first character `a`, we will get two permutations: `"ab7c", "Ab7c"`
3. Processing the second character `b`, we will get four permutations: `"ab7c", "Ab7c", "aB7c", "AB7c"`
4. Since the third character is a digit, we can skip it.
5. Processing the fourth character `c`, we will get a total of eight permutations: `"ab7c", "Ab7c", "aB7c", "AB7c", "ab7C", "Ab7C", "aB7C", "AB7C"`

Let’s analyze the permutations in the 3rd and the 5th step. How can we generate the permutations in the 5th step from the permutations in the 3rd step?

If we look closely, we will realize that in the 5th step, when we processed the new character `c`, we took all the permutations of the previous step (3rd) and changed the case of the letter `c` in them to create four new permutations.
````js
function findLetterCaseStringPermutations(str) {
  const permutations = [];
  permutations.push(str)
  
  //process every character of the string one by one
  for(let i = 0; i < str.length; i++) {
    //only characters we will skip digits
    if(isNaN(parseInt(str[i]), 10)){
      //we will take all exixting permutations and change the letter case appropriately
      const n = permutations.length
      
      for(let j = 0; j < n; j++) {
        //string to array
        const chs = permutations[j].split('')
        
        //if the current character is in upper case
        //change it to lower case or vice verse
        if(chs[i] === chs[i].toLowerCase()) {
          chs[i] = chs[i].toUpperCase()
        } else {
          chs[i] = chs[i].toLowerCase()
        }
        permutations.push(chs.join(''))
      }
    }
  }
  return permutations;
};


findLetterCaseStringPermutations("ad52")
findLetterCaseStringPermutations("ab7c")
````

- Since we can have`2ᴺ` permutations at the most and while processing each permutation we convert it into a character array, the overall time complexity of the algorithm will be `O(N*2ᴺ)`.
- All the additional space used by our algorithm is for the output list. Since we can have a total of `O(2ᴺ)` permutations, the space complexity of our algorithm is `O(N*2ᴺ)`.
## Balanced Parentheses (hard)
https://leetcode.com/problems/generate-parentheses/

> For a given number `N`, write a function to generate all combination of `N` pairs of balanced parentheses.

This problem follows the <b>Subsets</b> pattern and can be mapped to <b>Permutations</b>. We can follow a similar <b>BFS</b> approach.

Let’s take Example-2 mentioned above to generate all the combinations of balanced parentheses. Following a <b>BFS</b> approach, we will keep adding open parentheses `(` or close parentheses `)`. At each step we need to keep two things in mind:

- We can’t add more than `N` open parenthesis.
- To keep the parentheses balanced, we can add a close parenthesis `)` only when we have already added enough open parenthesis `(`. For this, we can keep a count of open and close parenthesis with every combination.

Following this guideline, let’s generate parentheses for `N=3`:

1. Start with an empty combination: `“”`
2. At every step, let’s take all combinations of the previous step and add `(` or `)` keeping the above-mentioned two rules in mind.
3. For the empty combination, we can add `(` since the count of open parenthesis will be less than `N`. We can’t add `)` as we don’t have an equivalent open parenthesis, so our list of combinations will now be: `(`
4. For the next iteration, let’s take all combinations of the previous set. For `(` we can add another `(` to it since the count of open parenthesis will be less than `N`. We can also add `)` as we do have an equivalent open parenthesis, so our list of combinations will be: `((`, `()`
5. In the next iteration, for the first combination `((`, we can add another `(` to it as the count of open parenthesis will be less than `N`, we can also add `)` as we do have an equivalent open parenthesis. This gives us two new combinations: `(((` and `(()`. For the second combination `()`, we can add another `(` to it since the count of open parenthesis will be less than `N`. We can’t add `)` as we don’t have an equivalent open parenthesis, so our list of combinations will be: `(((`, `(()`, `()(`
6. Following the same approach, next we will get the following list of combinations: `“((()”, “(()(”, “(())”, “()((”, “()()”`
7. Next we will get: `“((())”, “(()()”, “(())(”, “()(()”, “()()(”`
8. Finally, we will have the following combinations of balanced parentheses: `“((()))”, “(()())”, “(())()”, “()(())”, “()()()”`
9. We can’t add more parentheses to any of the combinations, so we stop here.

````js
class ParenthesesString {
  constructor(str, openCount, closeCount) {
    this.str = str;
    this.openCount = openCount;
    this.closeCount = closeCount;
  }
}


function generateValidParentheses(num) {
  let result = [];
  let queue = []
  
  queue.push(new ParenthesesString('', 0, 0))
  
  while(queue.length > 0) {
    const ps = queue.shift()
    
    //if we've reached the maximum number of open and closed
    //parentheses, add to the result
    if(ps.openCount === num && ps.closeCount === num) {
      result.push(ps.str)
    } else {
      if(ps.openCount < num) {
        //if we can add an open parentheses, add it
        queue.push(new ParenthesesString(`${ps.str}(`, ps.openCount + 1, ps.closeCount))  
      }
      if(ps.openCount > ps.closeCount) {
        //if we can add a close parentheses, add it
        queue.push(new ParenthesesString(`${ps.str})`, ps.openCount, ps.closeCount + 1))
      }
    }
  }
  return result;
};


generateValidParentheses(2)
generateValidParentheses(3)
````
- Let’s try to estimate how many combinations we can have for `N` pairs of balanced parentheses. If we don’t care for the ordering - that `)` can only come after `(` - then we have two options for every position, i.e., either put open parentheses or close parentheses. This means we can have a maximum of `2ᴺ` combinations. Because of the ordering, the actual number will be less than `2ᴺ`
- If you see the visual representation of Example-2 closely you will realize that, in the worst case, it is equivalent to a binary tree, where each node will have two children. This means that we will have `2ᴺ` leaf nodes and `2ᴺ-1` intermediate nodes. So the total number of elements pushed to the queue will be `2ᴺ−1`, which is asymptotically equivalent to `O(2ᴺ)`. While processing each element, we do need to concatenate the current string with `(` or `)`. This operation will take `O(N)`, so the overall time complexity of our algorithm will be `O(N*2ᴺ)`. <i>This is not completely accurate but reasonable enough to be presented in the interview.</i>

- All the additional space used by our algorithm is for the output list. Since we can’t have more than `O(2ᴺ)` combinations, the space complexity of our algorithm is `O(N*2ᴺ)`.

### Recursive Solution 
````js
function generateValidParentheses(num) {
  const result = [];
  const parenthesesString = Array(2 * num);
  generateValidParenthesesRecursive(num, 0, 0, parenthesesString, 0, result);
  return result;
}


function generateValidParenthesesRecursive(num, openCount, closeCount, parenthesesString, index, result) {
  // if we've reached the maximum number of open and close parentheses, add to the result
  if (openCount === num && closeCount === num) {
    result.push(parenthesesString.join(''));
  } else {
    if (openCount < num) { // if we can add an open parentheses, add it
      parenthesesString[index] = '(';
     generateValidParenthesesRecursive(num, openCount + 1, closeCount, parenthesesString, index + 1, result);
    }
    if (openCount > closeCount) { // if we can add a close parentheses, add it
      parenthesesString[index] = ')';
      generateValidParenthesesRecursive(num, openCount, closeCount + 1, parenthesesString, index + 1, result);
    }
  }
}

generateValidParentheses(2)
generateValidParentheses(3)
````
## 😕 Unique Generalized Abbreviations (hard)
https://leetcode.com/problems/generalized-abbreviation/

> Given a word, write a function to generate all of its unique generalized abbreviations.

A generalized abbreviation of a word can be generated by replacing each substring of the word with the count of characters in the substring. Take the example of `“ab”` which has four substrings: `“”, “a”, “b”, and “ab”`. After replacing these substrings in the actual word by the count of characters, we get all the generalized abbreviations: `“ab”, “1b”, “a1”, and “2”`.

<b>Note:</b> All contiguous characters should be considered one substring, e.g., we can’t take `“a”` and `“b”` as substrings to get `“11”`; since `“a”` and `“b”` are contiguous, we should consider them together as one substring to get an abbreviation `“2”`.

Let’s take Example-1 mentioned above to generate all unique generalized abbreviations. Following a BFS approach, we will abbreviate one character at a time. At each step, we have two options:

- Abbreviate the current character, or
- Add the current character to the output and skip the abbreviation.

Following these two rules, let’s abbreviate `BAT`:

1. Start with an empty word: `“”`
2. At every step, we will take all the combinations from the previous step and apply the two abbreviation rules to the next character.
3. Take the empty word from the previous step and add the first character to it. We can either abbreviate the character or add it (by skipping abbreviation). This gives us two new words: `_`, `B`.
4. In the next iteration, let’s add the second character. Applying the two rules on `_` will give us `_ _` and `1A`. Applying the above rules to the other combination `B` gives us `B_` and `BA`.
5. The next iteration will give us: `_ _ _, 2T, 1A_, 1AT, B _ _, B1T, BA_, BAT`
6. The final iteration will give us:`3, 2T, 1A1, 1AT, B2, B1T, BA1, BAT`

````js
class AbbreviatedWord {
  constructor(str, start, count) {
    this.str = str;
    this.start = start;
    this.count = count;
  }
}

function generateGeneralizedAbbreviation(word) {
  let wLength = word.length
  const result = [];
  const queue = [];
  queue.push(new AbbreviatedWord('', 0, 0))
  while(queue.length > 0) {
    const abWord = queue.shift()
    if(abWord.start === wLength){
      if(abWord.count !== 0) {
        abWord.str += abWord.count
      }
      result.push(abWord.str)
    } else {
      //continue abbreviating by incrementing the current abbreviation count
      queue.push(new AbbreviatedWord(abWord.str, abWord.start + 1, abWord.count + 1))
      
      //restart abbreviating, append the count and the current character to the string
      if(abWord.count !== 0){
        abWord.str += abWord.count
      }
      let newWord = abWord.str + word[abWord.start]
      queue.push(new AbbreviatedWord(newWord, abWord.start + 1, 0))
    }
  }
  return result;
};


generateGeneralizedAbbreviation("BAT")//"BAT", "BA1", "B1T", "B2", "1AT", "1A1", "2T", "3"

generateGeneralizedAbbreviation("code")// "code", "cod1", "co1e", "co2", "c1de", "c1d1", "c2e", "c3", "1ode", "1od1", "1o1e", "1o2", "2de", "2d1", "3e", "4"
````
- Since we had two options for each character, we will have a maximum of `2ᴺ` combinations. If you see the visual representation of Example-1 closely, you will realize that it is equivalent to a binary tree, where each node has two children. This means that we will have `2ᴺ` leaf nodes and `2ᴺ-1` intermediate nodes, so the total number of elements pushed to the queue will be `2ᴺ + 2ᴺ-1`, which is asymptotically equivalent to `O(2ᴺ)`. While processing each element, we do need to concatenate the current string with a character. This operation will take `O(N)`, so the overall time complexity of our algorithm will be `O(N*2ᴺ)`.
 - All the additional space used by our algorithm is for the output list. Since we can’t have more than `O(2ᴺ)` combinations, the space complexity of our algorithm is `O(N*2ᴺ)`.
### Recursive Solution 
````js
function generate_generalized_abbreviation(word) {
  const result = [];
  generate_abbreviation_recursive(word, '', 0, 0, result);
  return result;
}


function generate_abbreviation_recursive(word, abWord, start, count, result) {
  if (start === word.length) {
    if (count !== 0) {
      abWord += count;
    }
    result.push(abWord);
  } else {
    // continue abbreviating by incrementing the current abbreviation count
    generate_abbreviation_recursive(word, abWord, start + 1, count + 1, result);

    // restart abbreviating, append the count and the current character to the string
    if (count !== 0) {
      abWord += count;
    }
    const newWord = abWord + word[start];
    generate_abbreviation_recursive(word, newWord, start + 1, 0, result);
  }
}


console.log(`Generalized abbreviation are: ${generate_generalized_abbreviation('BAT')}`);
console.log(`Generalized abbreviation are: ${generate_generalized_abbreviation('code')}`);
````
## 🌟 Evaluate Expression (hard)
https://leetcode.com/problems/different-ways-to-add-parentheses/

> Given an expression containing digits and operations `(+, -, *)`, find all possible ways in which the expression can be evaluated by grouping the numbers and operators using parentheses.

This problem follows the Subsets pattern and can be mapped to Balanced Parentheses. We can follow a similar BFS approach.

Let’s take the first example to generate different ways to evaluate the expression.
1. We can iterate through the expression character-by-character.
2. we can break the expression into two halves whenever we get an operator `(+, -, *)`.
3. The two parts can be calculated by recursively calling the function.
4. Once we have the evaluation results from the left and right halves, we can combine them to produce all results.

````js
function diffWaysToEvaluateExpression(input) {
  const result = [];
  
  // base case: if the input string is a number, parse and add it to output.
  if(!(input.includes('+')) && !(input.includes('-')) && !(input.includes('*'))) {
     result.push(parseInt(input))
     } else {
       for(let i = 0; i < input.length; i++){
         const char = input[i];
         if(isNaN(parseInt(char, 10))){
        // if not a digit
        // break the equation here into two parts and make recursively calls
           const leftParts = diffWaysToEvaluateExpression(input.substring(0, i))
           const rightParts = diffWaysToEvaluateExpression(input.substring(i + 1))
         
           for (let l = 0; l < leftParts.length; l++) {
          for (let r = 0; r < rightParts.length; r++) {
            let part1 = leftParts[l],
              part2 = rightParts[r];
            if (char === '+') {
              result.push(part1 + part2);
            } else if (char === '-') {
              result.push(part1 - part2);
            } else if (char === '*') {
              result.push(part1 * part2);
            }
          }
        }
      }
    }
  }

  return result;
};


console.log(`Expression evaluations: ${diffWaysToEvaluateExpression("1+2*3")}`)//[7, 9],1+(2*3) => 7 and (1+2)*3 => 9
console.log(`Expression evaluations: ${diffWaysToEvaluateExpression("2*3-4-5")}`)//[8, -12, 7, -7, -3 ], 2*(3-(4-5)) => 8, 2*(3-4-5) => -12, 2*3-(4-5) => 7, 2*(3-4)-5 => -7, (2*3)-4-5 => -3
````

- The time complexity of this algorithm will be exponential and will be similar to Balanced Parentheses. Estimated time complexity will be `O(N*2^N)` but the actual time complexity `( O(4^n/\sqrt{n})` is bounded by the Catalan number and is beyond the scope of a coding interview. 
- The space complexity of this algorithm will also be exponential, estimated at `O(2^N)` though the actual will be `( O(4^n/\sqrt{n})`.

### Memoized Solution
The problem has overlapping subproblems, as our recursive calls can be evaluating the same sub-expression multiple times. To resolve this, we can use <b>memoization</b> and store the intermediate results in a <b>HashMap</b>. In each function call, we can check our map to see if we have already evaluated this sub-expression before
````js
function diffWaysToEvaluateExpression(input) {
  diffWaysToEvaluateExpressionRecursive({}, input)
}

function diffWaysToEvaluateExpressionRecursive(map, input) {
  
  if(input in map) {
    //issue with the hashmap here
    return map[input]
    // console.log(map[input])
  }
  const result = [];
  
  // base case: if the input string is a number
  //parse and add it to output.
  if(!(input.includes('+')) && !(input.includes('-')) && !(input.includes('*'))) {
     result.push(parseInt(input))
     } 
  else {
       for(let i = 0; i < input.length; i++){
         const char = input[i];
         if(isNaN(parseInt(char, 10))){
        // if not a digit
        // break the equation here into two parts and make recursively calls
           const leftParts = diffWaysToEvaluateExpression(input.substring(0, i))
           const rightParts = diffWaysToEvaluateExpression(input.substring(i + 1))
         // console.log(leftParts.length)
           for (let l = 0; l < leftParts.length; l++) {
          for (let r = 0; r < rightParts.length; r++) {
            let part1 = leftParts[l],
              part2 = rightParts[r];
            if (char === '+') {
              result.push(part1 + part2);
            } else if (char === '-') {
              result.push(part1 - part2);
            } else if (char === '*') {
              result.push(part1 * part2);
            }
          }
        }
      }
    }
  }
  
  map[input] = result
  
  return result;
};


console.log(`Expression evaluations: ${diffWaysToEvaluateExpression("1+2*3")}`)//[7, 9],1+(2*3) => 7 and (1+2)*3 => 9
console.log(`Expression evaluations: ${diffWaysToEvaluateExpression("2*3-4-5")}`)//[8, -12, 7, -7, -3 ], 2*(3-(4-5)) => 8, 2*(3-4-5) => -12, 2*3-(4-5) => 7, 2*(3-4)-5 => -7, (2*3)-4-5 => -3
````

## 🌟 Structurally Unique Binary Search Trees (hard)
https://leetcode.com/problems/unique-binary-search-trees-ii/

> Given a number `n`, write a function to return all structurally unique <b>Binary Search Trees (BST)</b> that can store values `1` to `n`?

This problem follows the <b>Subsets</b> pattern and is quite similar to <b>Evaluate Expression</b>. Following a similar approach, we can iterate from `1` to `n` and consider each number as the root of a tree. All smaller numbers will make up the left sub-tree and bigger numbers will make up the right sub-tree. We will make recursive calls for the left and right sub-trees

````js
class TreeNode {
  constructor(val, left = null, right = null) {
    this.val = val;
    this.left = left;
    this.right = right;
  }
}


function findUniqueTrees(n) {
   if (n <= 0) {
    return [];
  }
  return findUniqueTreesRecursive(1, n);
}

function findUniqueTreesRecursive(start, end) {
  const result = [];
  // let treeCount = 0
  // base condition, return 'null' for an empty sub-tree
  // consider n = 1, in this case we will have start = end = 1, this means we should have only one tree
  // we will have two recursive calls, findUniqueTreesRecursive(1, 0) & (2, 1)
  // both of these should return 'null' for the left and the right child
  if (start > end) {
    result.push(null);
    return result;
  }

  for (let i = start; i < end + 1; i++) {
    // making 'i' the root of the tree
    const leftSubtrees = findUniqueTreesRecursive(start, i - 1);
    const rightSubtrees = findUniqueTreesRecursive(i + 1, end);
    for (let p = 0; p < leftSubtrees.length; p++) {
      for (let q = 0; q < rightSubtrees.length; q++) {
        const root = new TreeNode(i, leftSubtrees[p], rightSubtrees[q]);
        result.push(root);
        // treeCount++
      }
    }
  }

  //return length of this instead, traverse tree?
  return result;
};


findUniqueTrees(2)//List containing root nodes of all structurally unique BSTs, Here are the 2 structurally unique BSTs storing all numbers from 1 to 2:
findUniqueTrees(3)//List containing root nodes of all structurally unique BSTs, Here are the 5 structurally unique BSTs storing all numbers from 1 to 3:
````
- The time complexity of this algorithm will be exponential and will be similar to Balanced Parentheses. Estimated time complexity will be `O(n*2^n)` but the actual time complexity `( O(4^n/\sqrt{n})` is bounded by the Catalan number and is beyond the scope of a coding interview. 
- The space complexity of this algorithm will be exponential too, estimated at `O(2^n)`, but the actual will be `( O(4^n/\sqrt{n})`.

#### Memoized Solution
Since our algorithm has overlapping subproblems, can we use memoization to improve it? We could, but every time we return the result of a subproblem from the cache, we have to clone the result list because these trees will be used as the left or right child of a tree. This cloning is equivalent to reconstructing the trees, therefore, the overall time complexity of the memoized algorithm will also be the same.

## 🌟 Count of Structurally Unique Binary Search Trees (hard)
https://leetcode.com/problems/unique-binary-search-trees/

> Given a number `n`, write a function to return the count of structurally unique <b>Binary Search Trees (BST)</b> that can store values `1` to `n`.

````js
class TreeNode {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null; 
  }
};


function countTrees(n) {
if (n <= 1) {
    return 1;
  }
  let count = 0;
  for (let i = 1; i < n + 1; i++) {
    // making 'i' the root of the tree
    const countOfLeftSubtrees = countTrees(i - 1);
    const countOfRightSubtrees = countTrees(n - i);
    count += (countOfLeftSubtrees * countOfRightSubtrees);
  }
  return count;
};


countTrees(2)//2, As we saw in the previous problem, there are 2 unique BSTs storing numbers from 1-2.
countTrees(3)//5, There will be 5 unique BSTs that can store numbers from 1 to 3.
````
- The time complexity of this algorithm will be exponential and will be similar to Balanced Parentheses. Estimated time complexity will be `O(n*2^n)` but the actual time complexity `( O(4^n/\sqrt{n})` is bounded by the Catalan number and is beyond the scope of a coding interview. 
- The space complexity of this algorithm will be exponential too, estimated `O(2^n)` but the actual will be `( O(4^n/\sqrt{n})`.

### Memoized version
Our algorithm has overlapping subproblems as our recursive call will be evaluating the same sub-expression multiple times. To resolve this, we can use memoization and store the intermediate results in a <b>HashMap</b>. In each function call, we can check our map to see if we have already evaluated this sub-expression before.
````js
class TreeNode {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null; 
  }
};

function countTrees(n) {
  return countTreesRec({}, n);
}

function countTrees(map, n) {
  
  //fix this hashmap
   if (n in map) {
    return map[n];
  }
  
  
if (n <= 1) {
    return 1;
  }
  let count = 0;
  for (let i = 1; i < n + 1; i++) {
    // making 'i' the root of the tree
    const countOfLeftSubtrees = countTrees(i - 1);
    const countOfRightSubtrees = countTrees(n - i);
    count += (countOfLeftSubtrees * countOfRightSubtrees);
  }
  
  map[n] = count;
  
  return count;
};


countTrees(2)//2, As we saw in the previous problem, there are 2 unique BSTs storing numbers from 1-2.
countTrees(3)//5, There will be 5 unique BSTs that can store numbers from 1 to 3.
````
- The time complexity of the memoized algorithm will be `O(n^2)`, since we are iterating from `1` to `n` and ensuring that each sub-problem is evaluated only once. 
- The space complexity will be `O(n)` for the memoization map.
