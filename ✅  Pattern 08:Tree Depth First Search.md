# Pattern 8: Tree Depth First Search (DFS)

This pattern is based on the <b>Depth First Search (DFS)</b> technique to traverse a tree.

We will be using <b>recursion</b> (or we can also use a stack for the iterative approach) to keep track of all the previous (parent) nodes while traversing. This also means that the space complexity of the algorithm will be `O(H)`, where `H` is the maximum height of the tree.

## Binary Tree Path Sum (easy)
https://leetcode.com/problems/path-sum/
> Given a binary tree and a number `S`, find if the tree has a path from root-to-leaf such that the sum of all the node values of that path equals `S`.

As we are trying to search for a  <i>root-to-leaf path</i> , we can use the <b>Depth First Search (DFS)</b> technique to solve this problem.

To recursively traverse a binary tree in a  <b>DFS</b>  fashion, we can start from the root and at every step, make two recursive calls one for the left and one for the right child.

Here are the steps for our [Binary Tree Path Sum](#binary-tree-path-sum-easy) problem:
1. Start  <b>DFS</b>  with the root of the tree.
2. If the current node is not a leaf node, do two things:
    - Subtract the value of the current node from the given number to get a new <i>sum</i> => `S = S - node.value`
    - Make two recursive calls for both the children of the current node with the new number calculated in the previous step.
3. At every step, see if the current node being visited is a leaf node and if its value is equal to the given number `S`. If both these conditions are true, we have found the required  <i>root-to-leaf path</i> , therefore return `true`.
4. If the current node is a leaf but its value is not equal to the given number `S`, return `false`.

````js
class TreeNode {
  constructor(value, left = null, right = null) {
    this.value = value;
    this.left = left;
    this.right = right;
  }
}

function hasPath(root, sum) {
  if (!root) {
    return false;
  }

  //start DFS with the root of the tree
  //if the current node is a leaf and it's value is
  //equal to the sum then we've found a path
  if (root.value === sum && root.left === null && root.right === null) {
    return true;
  }

  //recursively call to traverse the left and right sub-tree
  //return true if any of the two recursive calls return true
  return (
    hasPath(root.left, sum - root.value) ||
    hasPath(root.right, sum - root.value)
  );
}

const root = new TreeNode(12);
root.left = new TreeNode(7);
root.right = new TreeNode(1);
root.left.left = new TreeNode(9);
root.right.left = new TreeNode(10);
root.right.right = new TreeNode(5);
console.log(`Tree has path: ${hasPath(root, 23)}`);
console.log(`Tree has path: ${hasPath(root, 16)}`);
````
- The time complexity of the above algorithm is `O(N)`, where `N` is the total number of nodes in the tree. This is due to the fact that we traverse each node once.
- The space complexity of the above algorithm will be `O(N)` in the worst case. This space will be used to store the <b>recursion</b> stack. The worst case will happen when the given tree is a <i>linked list</i> (i.e., every node has only one child).

## All Paths for a Sum (medium)
https://leetcode.com/problems/path-sum-ii/
> Given a binary tree and a number `S`, find all paths from root-to-leaf such that the sum of all the node values of each path equals `S`.

This problem follows the <b>[Binary Tree Path Sum](#binary-tree-path-sum-easy)</b> pattern. We can follow the same <b>DFS</b> approach. There will be two differences:
1. Every time we find a  <i>root-to-leaf path</i> , we will store it in a list.
2. We will traverse all paths and will not stop processing after finding the first path.

````js
class TreeNode {
  constructor(value, left = null, right = null) {
    this.value = value;
    this.left = left;
    this.right = right; 
  }
};


function findPaths(root, sum) {
  let allPaths = []
  
  findAPath(root, sum, [], allPaths)

  return allPaths
};

function findAPath(currentNode, sum, currentPath, allPaths) {
  if(currentNode === null) {
    return
  }
  
  //add the current node to the path
  currentPath.push(currentNode.value)
  
  //if the current node is a leaf and it's value is 
  //equal to sum, save the current path
  if(currentNode.value === sum && currentNode.left === null && currentNode.right === null) {
    allPaths.push([...currentPath])
  } else {
    //traverse the left sub-tree
    findAPath(currentNode.left, sum - currentNode.value, currentPath, allPaths)
    //traverse the right sub-tree
    findAPath(currentNode.right, sum - currentNode.value, currentPath, allPaths)
  }
  
  //remove the current node for the path to backtrack,
  //we need to remove the currentNode while we are going 
  //up the recursive call stack
  currentPath.pop()
}

const root = new TreeNode(12)
root.left = new TreeNode(7)
root.right = new TreeNode(1)
root.left.left = new TreeNode(4)
root.right.left = new TreeNode(10)
root.right.right = new TreeNode(5)
findPaths(root, 23);
````
- The time complexity of the above algorithm is `O(N^2)`, where `N` is the total number of nodes in the tree. This is due to the fact that we traverse each node once (which will take `O(N)`), and for every leaf node, we might have to store its path (by making a copy of the current path) which will take `O(N)`.
  - We can calculate a tighter time complexity of `O(NlogN)` from the space complexity discussion below.
- If we ignore the space required for the `allPaths` list, the space complexity of the above algorithm will be `O(N)` in the worst case. This space will be used to store the <b>recursion</b> stack. The worst-case will happen when the given tree is a <i>linked list</i> (i.e., every node has only one child).

> 🌟 Given a binary tree, return all  <i>root-to-leaf</i> paths.

https://leetcode.com/problems/binary-tree-paths/

We can follow a similar approach. We just need to remove the “check for the path sum.”
````js
class TreeNode {
  constructor(value, left = null, right = null) {
    this.value = value;
    this.left = left;
    this.right = right; 
  }
};


function findPaths(root) {
  let allPaths = []
  
  findAPath(root, [], allPaths)

  return allPaths
};

function findAPath(currentNode, currentPath, allPaths) {
  if(currentNode === null) {
    return
  }
  
  //add the current node to the path
  currentPath.push(currentNode.value)
  
  //if the current node is not a leaf, save the current path
  if(currentNode.left === null && currentNode.right === null) {
    allPaths.push([...currentPath])
  } else {
    //traverse the left sub-tree
    findAPath(currentNode.left, currentPath, allPaths)
    //traverse the right sub-tree
    findAPath(currentNode.right, currentPath, allPaths)
  }
  
  //remove the current node from the path to backtrack,
  //we need to remove the currentNode while we are going 
  //up the recursive call stack
  currentPath.pop()
}

const root = new TreeNode(12)
root.left = new TreeNode(7)
root.right = new TreeNode(1)
root.left.left = new TreeNode(4)
root.right.left = new TreeNode(10)
root.right.right = new TreeNode(5)
findPaths(root);
````

> 🌟 Given a binary tree, find the  <i>root-to-leaf</i> path  with the maximum sum.

We need to find the path with the maximum sum. As we traverse all paths, we can keep track of the path with the maximum sum.
````js
class TreeNode {
  constructor(value, left = null, right = null) {
    this.value = value;
    this.left = left;
    this.right = right; 
  }
};


function maxPathSum(root) {
  let maxSum = -Infinity
  
  function findAPath(currentNode, currentPath) {
    if(currentNode === null) {
      return
    }
    //add the current node to the path
    currentPath.push(currentNode.value)

    let pathMax = 0
    //if the current node is not a leaf, save the current path
    if(currentNode.left === null && currentNode.right === null) {
      for(let i = 0; i < currentPath.length; i++) {
        pathMax += currentPath[i] 
      }
      maxSum = Math.max(maxSum, pathMax)
    } else {
      //traverse the left sub-tree
      findAPath(currentNode.left, currentPath)
      //traverse the right sub-tree
      findAPath(currentNode.right, currentPath)
    }

    //remove the current node from the path to backtrack,
    //we need to remove the currentNode while we are going 
    //up the recursive call stack
    currentPath.pop()  
  }
  
  findAPath(root, [], maxSum)

  return maxSum
};


const root = new TreeNode(12)
root.left = new TreeNode(7)
root.right = new TreeNode(1)
root.left.left = new TreeNode(4)
root.right.left = new TreeNode(10)
root.right.right = new TreeNode(5)
maxPathSum(root);
````

## Sum of Path Numbers (medium)
https://leetcode.com/problems/sum-root-to-leaf-numbers/
> Given a binary tree where each node can only have a digit (0-9) value, each  <i>root-to-leaf path</i>  will represent a number. Find the total sum of all the numbers represented by all paths.

This problem follows the <b>[Binary Tree Path Sum](#binary-tree-path-sum-easy)</b> pattern. We can follow the same <b>DFS</b> approach. The additional thing we need to do is to keep track of the number representing the current path.

How do we calculate the path number for a node? Taking the first example mentioned above, say we are at node `7`. As we know, the path number for this node is `17`, which was calculated by: `1 * 10 + 7 => 17`. We will follow the same approach to calculate the path number of each node.

````js
class TreeNode {
  constructor(value) {
    this.value = value
    this.left = null
    this.right = null
  }
}

function findSumOfPathNumbers(root) {
  return findRootToLeafPathNumbers(root, 0)
}

function findRootToLeafPathNumbers(currentNode, pathSum) {
    if(currentNode === null) {
      return 0
    } 
    
    //calculate the path number of the current node
    pathSum = 10 * pathSum + currentNode.value
    
    //if the currentNode is a leaf, return the current pathSum
    if(currentNode.left === null && currentNode.right === null) {
      return pathSum
    }
    
    //traverse the left and the right sub-tree
    return findRootToLeafPathNumbers(currentNode.left, pathSum) + findRootToLeafPathNumbers(currentNode.right, pathSum)
}

const root = new TreeNode(1)
root.left = new TreeNode(0)
root.right = new TreeNode(1)
root.left.left = new TreeNode(1)
root.right.left = new TreeNode(6)
root.right.right = new TreeNode(5)
console.log(`Total Sum of Path Numbers: ${findSumOfPathNumbers(root)}`)
````
- The time complexity of the above algorithm is `O(N)`, where `N` is the total number of nodes in the tree. This is due to the fact that we traverse each node once.
- The space complexity of the above algorithm will be `O(N)` in the worst case. This space will be used to store the <b>recursion</b> stack. The worst case will happen when the given tree is a <i>linked list</i> (i.e., every node has only one child).
## Path With Given Sequence (medium)
> Given a binary tree and a number sequence, find if the sequence is present as a <i>root-to-leaf path</i>  in the given tree.

This problem follows the <b>[Binary Tree Path Sum](#binary-tree-path-sum-easy)</b> pattern. We can follow the same <b>DFS</b> approach and additionally, track the element of the given sequence that we should match with the current node. Also, we can return false as soon as we find a mismatch between the sequence and the node value.
````js
class TreeNode {
  constructor(value) {
    this.value = value
    this.right = null
    this.left = null
  }
}

function findPath(root, sequence) {
  if(!root) {
    return sequence.length === 0
  }
  
  return findPathRecursive(root, sequence, 0)
}

function findPathRecursive(currentNode, sequence, sequenceIndex) {
  if(currentNode === null) return false
  
  const sequenceLength = sequence.length
  if(sequenceIndex >= sequenceLength || currentNode.value !== sequence[sequenceIndex]) return false
  
  //if the current node is a leaf, and it is the end of the sequence, then we have found a path
  if(currentNode.left === null && currentNode.right === null && sequenceIndex === sequenceLength - 1) return true
  
  //recursively call to traverse the left and right sub-tree
  //return true if any of the two recursive calls return true
  
  return findPathRecursive(currentNode.left, sequence, sequenceIndex + 1) || findPathRecursive(currentNode.right, sequence, sequenceIndex + 1)
}

const root = new TreeNode(1)
root.left = new TreeNode(0)
root.right = new TreeNode(1)
root.left.left = new TreeNode(1)
root.right.left = new TreeNode(6)
root.right.right = new TreeNode(5)

console.log(`Tree has path sequence: ${findPath(root, [1, 0, 7])}`)
console.log(`Tree has path sequence: ${findPath(root, [1, 1, 6])}`)
````
- The time complexity of the above algorithm is `O(N)`, where `N` is the total number of nodes in the tree. This is due to the fact that we traverse each node once.
- The space complexity of the above algorithm will be `O(N)` in the worst case. This space will be used to store the <b>recursion</b> stack. The worst case will happen when the given tree is a <i>linked list</i> (i.e., every node has only one child).
## Count Paths for a Sum (medium)
https://leetcode.com/problems/path-sum-iii/

> Given a binary tree and a number `S`, find all paths in the tree such that the sum of all the node values of each path equals `S`. Please note that the paths can start or end at any node but all paths must follow direction from parent to child (top to bottom).

This problem follows the <b>[Binary Tree Path Sum](#binary-tree-path-sum-easy)</b> pattern. We can follow the same <b>DFS</b> approach. But there will be four differences:
1. We will keep track of the current path in a list which will be passed to every recursive call.
2. Whenever we traverse a node we will do two things:

    - Add the current node to the current path.
    - As we added a new node to the current path, we should find the sums of all sub-paths ending at the current node. If the sum of any sub-path is equal to `S` we will increment our path count.
3. We will traverse all paths and will not stop processing after finding the first path.
4. Remove the current node from the current path before returning from the function. This is needed to <i>backtrack</i> while we are going up the recursive call stack to process other paths.
````js
class TreeNode {
  constructor(value, right = null, left = null) {
    this.value = value
    this.right = right
    this.left = left
  }
}

function countPaths(root, S) {
  let currentPath = []
  return countPathsRecursive(root, S, currentPath)
}

function countPathsRecursive(currentNode, S, currentPath) {
  if(currentNode === null) return 0
  
  //add the currentNode to the path
  currentPath.push(currentNode.value)
  
  let pathCount = 0
  let pathSum = 0
  
  //find the sums of all aub-paths in the current path list
  for(let i = currentPath.length - 1; i >= 0; i--) {
    pathSum += currentPath[i]
    
    //if the sum of any sub-path is equal S we increment our path count
    if(pathSum === S) {
      pathCount++
    }
  }
  
  //traverse the left sub-tree
  pathCount += countPathsRecursive(currentNode.left, S, currentPath)
  //traverse the right sub-tree
  pathCount += countPathsRecursive(currentNode.right, S, currentPath)
  
  //remove the current node from the path to backtrack
  //we need to remove the current node while we are going upp the recursive call stack
  currentPath.pop()
  return pathCount
}

const root = new TreeNode(12)
root.left = new TreeNode(7)
root.right = new TreeNode(1)
root.left.left = new TreeNode(4)
root.right.left = new TreeNode(10)
root.right.right = new TreeNode(5)
console.log(`Tree has ${countPaths(root, 11)} paths`)
````
- The time complexity of the above algorithm is `O(N²)` in the worst case, where `N` is the total number of nodes in the tree. This is due to the fact that we traverse each node once, but for every node, we iterate the current path. The current path, in the worst case, can be `O(N)` (in the case of a skewed tree). But, if the tree is balanced, then the current path will be equal to the height of the tree, i.e., `O(logN)`. So the best case of our algorithm will be `O(NlogN)`.
- The space complexity of the above algorithm will be `O(N)`. This space will be used to store the <b>recursion</b> stack. The worst case will happen when the given tree is a <i>linked list</i> (i.e., every node has only one child). We also need `O(N)` space for storing the currentPath in the worst case.  Overall space complexity of our algorithm is `O(N)`.
## 🌟 Tree Diameter (medium)
https://leetcode.com/problems/diameter-of-binary-tree/

> Given a binary tree, find the length of its diameter. The diameter of a tree is the number of nodes on the <b>longest path between any two leaf nodes</b>. The diameter of a tree may or may not pass through the root.
>
> Note: You can always assume that there are at least two leaf nodes in the given tree.

This problem follows the <b>Binary Tree Path Sum</b> pattern. We can follow the same <b>DFS</b> approach. There will be a few differences:
1. At every step, we need to find the height of both children of the current node. For this, we will make two recursive calls similar to <b>DFS</b>.
2. The height of the current node will be equal to the maximum of the heights of its left or right children, plus `1` for the `currentNode`.
3. The tree diameter at the `currentNode` will be equal to the height of the left child plus the height of the right child plus `1` for the current node: `diameter = leftTreeHeight + rightTreeHeight + 1`. To find the overall tree diameter, we will use a class level variable. This variable will store the maximum `diameter` of all the nodes visited so far, hence, eventually, it will have the final tree diameter.

````js
class TreeNode {
  constructor(value, left = null, right = null) {
    this.value = value;
    this.left = left;
    this.right = right; 
  }
};

class TreeDiameter {
  constructor() {
    this.treeDiameter = 0;
  }

  findDiameter(root) {
    this.calculateHeight(root)
    return this.treeDiameter
  }
  
   calculateHeight(currentNode) {
    if(currentNode === null) return 0
     
     const leftTreeHeight = this.calculateHeight(currentNode.left)
     const rightTreeHeight = this.calculateHeight(currentNode.right)
     
     //if the current node doesn't have a left or right sub-tree,
     //we can't have a path passing through it,
     //since we need a leaf node on each side
     if(leftTreeHeight !== 0 && rightTreeHeight !== 0) {
       //diameter at the currentNode will be equal to the height of the left 
       //sub-tree the height of sub-trees + 1 for the currentNode
       const diameter = leftTreeHeight + rightTreeHeight + 1
       
       //update the global tree diameter
       this.treeDiameter = Math.max(this.treeDiameter, diameter)
     }
     
     //height of the currentNode will be equal to the maximum of the heights of
     //left or right sub-trees plus 1
     return Math.max(leftTreeHeight, rightTreeHeight) + 1
  } 
};


const treeDiameter = new TreeDiameter()
const root = new TreeNode(1)
root.left = new TreeNode(2)
root.right = new TreeNode(3)
root.left.left = new TreeNode(4)
root.right.left = new TreeNode(5)
root.right.right = new TreeNode(6)
console.log(`Tree Diameter: ${treeDiameter.findDiameter(root)}`)
root.left.left = null
root.right.left.left = new TreeNode(7)
root.right.left.right = new TreeNode(8)
root.right.right.left = new TreeNode(9)
root.right.left.right.left = new TreeNode(10)
root.right.right.left.left = new TreeNode(11)
console.log(`Tree Diameter: ${treeDiameter.findDiameter(root)}`)
````

- The time complexity of the above algorithm is `O(N)`, where `N` is the total number of nodes in the tree. This is due to the fact that we traverse each node once.
- The space complexity of the above algorithm will be `O(N)` in the worst case. This space will be used to store the <b>recursion</b> stack. The worst case will happen when the given tree is a <i>linked list</i> (i.e., every node has only one child).


## 🌟 Path with Maximum Sum (hard)
https://leetcode.com/problems/binary-tree-maximum-path-sum/

> Find the path with the maximum sum in a given binary tree. Write a function that returns the maximum sum.
> 
> A path can be defined as a <b>sequence of nodes between any two nodes</b> and doesn’t necessarily pass through the root. The path must contain at least one node.

This problem follows the <b>Binary Tree Path Sum</b> pattern and shares the algorithmic logic with <b>Tree Diameter</b>. We can follow the same <b>DFS</b> approach. The only difference will be to ignore the paths with negative sums. Since we need to find the overall maximum sum, we should ignore any path which has an overall negative sum.

````js
class TreeNode {
  constructor(value, left = null, right = null) {
    this.value = value;
    this.left = left;
    this.right = right; 
  }
};



function findMaximumPathSum(root) {
  let globalMaximumSum = -Infinity
  findMaximumPathSumRecursive(root)
  
  
  function findMaximumPathSumRecursive(currentNode) {
    if(currentNode === null) return 0
    
    let maxPathSumLeft = findMaximumPathSumRecursive(currentNode.left)
    let maxPathSumRight = findMaximumPathSumRecursive(currentNode.right)
    
    //ignore paths with negative sums, since we need to find the maximum sum 
    //we should ignore any path which has an overall negative sum
    maxPathSumLeft = Math.max(maxPathSumLeft, 0)
    maxPathSumRight = Math.max(maxPathSumRight, 0)
    
    //maximum path sum at the currentNode will be equal to the sum from the
    //left subtree + the sum from the right subtree + value of the currentNode
    const localMaximumSum = maxPathSumLeft + maxPathSumRight + currentNode.value
    
    //update the globalMaximumSum 
    globalMaximumSum = Math.max(globalMaximumSum, localMaximumSum)
    
    //maximum sum of any path from the currentNode will be equal to the maximum
    //of the sums from left to right sub-tree plus the value of the currentNode
    return Math.max(maxPathSumLeft, maxPathSumRight) + currentNode.value
  }
  return globalMaximumSum
};

let root = new TreeNode(1)
root.left = new TreeNode(2)
root.right = new TreeNode(3)
console.log(`Maximum Path Sum: ${findMaximumPathSum(root)}`)//6

root.left.left = new TreeNode(1)
root.left.right = new TreeNode(3)
root.right.left = new TreeNode(5)
root.right.right = new TreeNode(6)
root.right.left.left = new TreeNode(7)
root.right.left.right = new TreeNode(8)
root.right.right.left = new TreeNode(9)
console.log(`Maximum Path Sum: ${findMaximumPathSum(root)}`)//31

root = new TreeNode(-1)
root.left = new TreeNode(-3)
console.log(`Maximum Path Sum: ${findMaximumPathSum(root)}`)
````

- The time complexity of the above algorithm is `O(N)`, where `N` is the total number of nodes in the tree. This is due to the fact that we traverse each node once.
- The space complexity of the above algorithm will be `O(N)` in the worst case. This space will be used to store the <b>recursion</b> stack. The worst case will happen when the given tree is a <i>linked list</i> (i.e., every node has only one child).


###### #DFS #DepthFirstSearch #JavaScript #GrokkingTheCodingInterviewPatterns #LeetCode #DataStructures #Algorithms
