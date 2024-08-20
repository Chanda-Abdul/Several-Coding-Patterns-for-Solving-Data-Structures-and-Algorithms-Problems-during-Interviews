

Backtracking used for to get all possible solutions.

```
void backtrack(arguments) {
	if (condition == true) { // Condition when we should stop our exploration.
		result.push_back(current);
		return;
	}
	for (int i = num; i <= last; i++) {
		current.push_back(i); // Choose candidate.
		backtrack(arguments); // Explore more
		current.pop_back();   // Remove candidate.
	}
}
```
**Popular Questions**

 1. [All Possible Combinations](https://www.interviewbit.com/problems/all-possible-combinations/)
 2. [Subset](https://www.interviewbit.com/problems/subset/)
 3. [Combination Sum](https://www.interviewbit.com/problems/combination-sum/)
 4. [Combination Sum 2](https://www.interviewbit.com/problems/combination-sum-ii/)
 5. [Combinations](https://www.interviewbit.com/problems/combinations/)
 6. [Subset2](https://www.interviewbit.com/problems/subsets-ii/)

**All Possible Combinations**

Input 1:

```
A = ['ab', 'cd']
```
**Example Output**  

Output 1:

```
['ac', 'ad', 'bc', 'bd']
```
CODE -- ACCORDING TO OUR PATTERN
![image](https://github.com/user-attachments/assets/f00d62aa-1a01-4080-9dfb-c872e5e6b663)


**SUBSET**
**Example Input**  

A = [1, 2, 3]

  
  
**Example Output**  

[  
 [],  
 [1],  
 [1, 2],  
 [1, 2, 3],  
 [1, 3],  
 [2],  
 [2, 3],  
 [3],  
]

![image](https://github.com/user-attachments/assets/96978d2d-5be0-4b1b-b243-507484208465)

**Combination Sum**

**Example Input**  

Input 1:

A = [2, 3]
B = 2

Input 2:

A = [2, 3, 6, 7]
B = 7

**Example Output**  

Output 1:

[ [2] ]

Output 2:

[ [2, 2, 3], [7] ]

![image](https://github.com/user-attachments/assets/8d5d7fdd-b505-4cde-aa50-12084f495ae8)

**Combination Sum2**

Only diff in combination sum and this problem is duplicate not allowed so we move to next index asap

Input and output

![image](https://github.com/user-attachments/assets/ca4e43aa-ee52-4837-b970-4bea3e0718cf)

![image](https://github.com/user-attachments/assets/efe1c936-ba27-4522-925b-eec376c6fd59)

**SubSet2**

The only difference between this and the subset problem is, here we store results in set to avoid duplicates while in the set question we used ArrayList to get all subsets including duplicates

Input
![image](https://github.com/user-attachments/assets/6eb69ee9-7807-44e7-a5f9-a6f5e82ca8db)

![image](https://github.com/user-attachments/assets/1c500ee1-1200-4d9a-9d44-2aca157c7701)


**Letter Phone**

Input

![image](https://github.com/user-attachments/assets/8568e54f-6068-4f5f-823d-c1e8a30b3678)

Output

Used All Possible Sum Approach

![image](https://github.com/user-attachments/assets/e2ee6556-1c43-4797-8f61-dad21457162b)

**Permutation**

Input

![image](https://github.com/user-attachments/assets/44adba86-10e0-4adc-9af9-9c1220b3ab85)

Output

![image](https://github.com/user-attachments/assets/2a8d4705-41ad-40c5-8904-991549d2e877)

**N QUEEN**

![image](https://github.com/user-attachments/assets/0fc7b275-9e0b-4a57-84c1-63ae5a1efb62)

Output

![image](https://github.com/user-attachments/assets/d408d73c-520f-40ad-a029-ffbafed4364a)








