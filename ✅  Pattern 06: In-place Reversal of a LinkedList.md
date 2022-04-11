# Pattern 6: In-place Reversal of a LinkedList

In a lot of problems, we are asked to reverse the links between a set of nodes of a <b>LinkedList</b>. Often, the constraint is that we need to do this </i>in-place</i>, i.e., using the existing node objects and without using extra memory.

<b></i>in-place</i> Reversal of a LinkedList pattern</b> describes an efficient way to solve the above problem.

## Reverse a LinkedList (easy)
https://leetcode.com/problems/reverse-linked-list/
> Given the head of a Singly <b>LinkedList</b>, reverse the <b>LinkedList</b>. Write a function to return the new head of the reversed <b>LinkedList</b>.

To reverse a <b>LinkedList</b>, we need to reverse one node at a time. We will start with a variable `current` which will initially point to the head of the <b>LinkedList</b> and a variable `previous` which will point to the previous node that we have processed; initially `previous` will point to `null`.

In a stepwise manner, we will reverse the `current` node by pointing it to the `previous` before moving on to the next node. Also, we will update the `previous` to always point to the previous node that we have processed. 

````js
class Node {
  constructor(value, next=null) {
    this.value = value;
    this.next = next
  }
  
  printList() {
    let result = ""
    let temp = this
    while(temp !== null) {
      result += temp.value + " "
      temp = temp.next
    }
    return result
  }
}

function reverse(head) {
  let current = head
  let previous = null
  
  while(current !== null) {
    //temporarily store the next node
    next = current.next
    
    //reverse the current node
    current.next = previous
    
    //before we move to the next node, 
    //point previous to the current node
    previous = current
    
    //move on to the next node
    current = next
  }
  
  return previous
}

head = new Node(2)
head.next = new Node(4);
head.next.next = new Node(6);
head.next.next.next = new Node(8);
head.next.next.next.next = new Node(10);

console.log(`Nodes of original LinkedList are: ${head.printList()}`)
console.log(`Nodes of reversed LinkedList are: ${reverse(head).printList()}`)
````

- The time complexity of our algorithm will be `O(N)` where `N’` is the total number of nodes in the <b>LinkedList</b>.
- We only used constant space, therefore, the space complexity of our algorithm is `O(1)`.

## Reverse a Sub-list (medium)
https://leetcode.com/problems/reverse-linked-list-ii/
> Given the head of a <b>LinkedList</b> and two positions `p` and `q`, reverse the <b>LinkedList</b> from position `p` to `q`.

The problem follows the <b></i>in-place</i> Reversal</b> of a <b>LinkedList</b> pattern. We can use a similar approach as discussed in <b>Reverse a LinkedList</b>. Here are the steps we need to follow:
1. Skip the first `p-1` nodes, to reach the node at position `p`.
2. Remember the node at position `p-1` to be used later to connect with the reversed sub-list.
3. Next, reverse the nodes from `p` to `q` using the same approach discussed in <b>Reverse a LinkedList</b>.
4. Connect the `p-1` and `q+1` nodes to the reversed sub-list.

````js
class Node {
  constructor(value, next = null) {
    this.value = value
    this.next = next
  }
  
  getList() {
    let result = ""
    let temp = this
    while(temp !== null) {
      result += temp.value + " "
      temp = temp.next
    }
    return result
  }
}

function reverseSubList(head, p, q) {
  if(p === q) {
    return head
  }
  
  //after skipping p-1 nodes, current will 
  //point to the p th node
  
  let current = head
  let previous = null
  
  let i = 0
  
  while(current !== null && i < p - 1) {
    previous = current
    current = current.next
    i++
  }
  
  //we are interested in three parts ofthe LL, 
  //1. the part before index p
  //2. the part between p and q
  //3. and the part after index q
  
  const lastNodeOfFirstPart = previous
  
  //after reversing the LL current will
  //become the last node of the subList
  const lastNodeOfSubList = current
  
  //will be used to temporarily store the next node
  let next = null
  
  i = 0
  //reverse nodes between p and q
  
  while (current !== null && i < q - p + 1) {
    next = current.next
    current.next = previous
    previous = current
    current = next
    i++
  }
  
  //connect with the first part
  if(lastNodeOfFirstPart !== null) {
    //previous is now the first node of the sub list
    lastNodeOfFirstPart.next = previous
    //this means p === 1 i.e., we are changing
    //the first node(head) of the LL
  } else {
    head = previous
  }
  
  //connect with the last part
  lastNodeOfSubList.next = current
  return head
}

head = new Node(1)
head.next = new Node(2);
head.next.next = new Node(3);
head.next.next.next = new Node(4);
head.next.next.next.next = new Node(5);

console.log(`Nodes of original LinkedList are: ${head.getList()}`)
console.log(`Nodes of reversed LinkedList are: ${reverseSubList(head, 2, 4).getList()}`)
````
- The time complexity of our algorithm will be `O(N)` where `N` is the total number of nodes in the <b>LinkedList</b>.
- We only used constant space, therefore, the space complexity of our algorithm is `O(1)`.

> 🌟 Reverse the first `k` elements of a given <b>LinkedList</b>.

This problem can be easily converted to our parent problem; to reverse the first `k` nodes of the list, we need to pass `p=1` and `q=k`.

> 🌟 Given a <b>LinkedList</b> with `n` nodes, reverse it based on its size in the following way:
> 1. If `n` is even, reverse the list in a group of `n/2` nodes.
> 2. If `n` is odd, keep the middle node as it is, reverse the first `n/2` nodes and reverse the last `n/2` nodes.

When `n` is even we can perform the following steps:
1. Reverse first `n/2` nodes: `head = reverse(head, 1, n/2)`
2. Reverse last `n/2` nodes: `head = reverse(head, n/2 + 1, n)`

When `n` is odd, our algorithm will look like:
1. `head = reverse(head, 1, n/2)`
2. `head = reverse(head, n/2 + 2, n)`
Please note the function call in the second step. We’re skipping two elements as we will be skipping the middle element.

## Reverse every K-element Sub-list (medium)
https://leetcode.com/problems/reverse-nodes-in-k-group/

> Given the head of a <b>LinkedList</b> and a number ‘k’, <b>reverse every ‘k’ sized sub-list</b> starting from the head.
> If, in the end, you are left with a sub-list with less than ‘k’ elements, reverse it too.

The problem follows the <b></i>in-place</i> Reversal of a LinkedList</b> pattern and is quite similar to <b>Reverse a Sub-list</b>. The only difference is that we have to reverse all the sub-lists. We can use the same approach, starting with the first sub-list (i.e. `p=1, q=k`) and keep reversing all the sublists of size ‘k’.

````js
class Node {
  constructor(value, next=null) {
    this.value = value
    this.next = next
  }
  
  getList() {
    let result = ""
    let temp = this
    while(temp !== null) {
      result += temp.value + " "
      temp = temp.next
    }
    return result
  }
}

function reverseEveryKElements(head, k) {
  //edge cases
  if(k <= 1 || head === null) {
    return head
  }
  
  let current = head
  let previous = null
  
  while(true) {
    const lastNodeOfPreviousPart = previous
    
    //after reversing the LL current will
    //become the last node of the sublist
    const lastNodeOfSubList = current
    
    //will be used to temporaily store the next node
    let next = null
    
    let i = 0;
    
    //reverse k nodes
    while(current !== null && i < k) {
      next = current.next
      current.next = previous
      previous = current
      current = next
      i++
    }
    
    //connect with the previous part
    if(lastNodeOfPreviousPart !== null) {
      lastNodeOfPreviousPart.next = previous
    } else {
      head = previous
    }
    
    //connect with the next part
    lastNodeOfSubList.next = current
    
    if(current === null) {
      break
    }
    previous = lastNodeOfSubList
  }
  return head
}

head = new Node(1)
head.next = new Node(2)
head.next.next = new Node(3)
head.next.next.next = new Node(4)
head.next.next.next.next = new Node(5)
head.next.next.next.next.next = new Node(6)
head.next.next.next.next.next.next = new Node(7)
head.next.next.next.next.next.next.next = new Node(8)

console.log(`Nodes of original LinkedList are: ${head.getList()}`)
console.log(`Nodes of reversed LinkedList are: ${reverseEveryKElements(head, 3).getList()}`)
````
- The time complexity of our algorithm will be `O(N)` where `N` is the total number of nodes in the <b>LinkedList</b>. 
- We only used constant space, therefore, the space complexity of our algorithm is `O(1)`. 

## 🌟 Reverse alternating K-element Sub-list (medium)
> Given the head of a <b>LinkedList</b> and a number `K`, <b>reverse every alternating `K` sized sub-list</b> starting from the head.
> 
> If, in the end, you are left with a sub-list with less than `K` elements, reverse it too.

The problem follows the <b></i>in-place</i> Reversal of a LinkedList</b> pattern and is quite similar to <b>Reverse every K-element Sub-list</b>. The only difference is that we have to skip `K` alternating elements. We can follow a similar approach, and in each iteration after reversing `K` elements, we will skip the next `K` elements.

````class Node {
  constructor(value, next = null) {
    this.value = value
    this.next = next
}

  printList() {
    let temp = this
    while(temp !== null) {
      process.stdout.write(`${temp.value} `);
      temp = temp.next
    }
    console.log()
  }
}

function reverseAlternateKElements(head, k) {
  if(head === null || k <= 1) return head
  
  let current = head
  let previous = null
  
  while (current !== null) {
    //break if we've reached the end of the list
    const lastNodeOfPreviousPart = previous
    
    //after reversing the LinkedList current will become the last node of the sub-list
    const lastNodeOfSubList = current
    
    //will be used to temporarily store the next node
    let next = null
    
    //reverse k nodes
    let i = 0
    while(current !== null && i < k) {
      next = current.next
      current.next = previous
      previous = current
      current = next
      i++
    }
    
    //connect with the previous part
    if(lastNodeOfPreviousPart !== null) {
      lastNodeOfPreviousPart.next = previous
    } else {
      head = previous
    }
    
    //connect with the next part
    lastNodeOfSubList.next = current
 
  
    //skip k nodes
    i = 0
    while (current !== null && i < k){
      previous = current
      current = current.next
      i++
     }
  } 
  return head
};



let head = new Node(1);
head.next = new Node(2);
head.next.next = new Node(3);
head.next.next.next = new Node(4);
head.next.next.next.next = new Node(5);
head.next.next.next.next.next = new Node(6);
head.next.next.next.next.next.next = new Node(7);
head.next.next.next.next.next.next.next = new Node(8);

process.stdout.write('Nodes of original LinkedList are: ');
head.printList();
result =  reverseAlternateKElements(head, 2);
process.stdout.write('Nodes of reversed LinkedList are: ');
result.printList();
````

- The time complexity of our algorithm will be `O(N)`where  `N’` is the total number of nodes in the <b>LinkedList</b>.
- We only used constant space, therefore, the space complexity of our algorithm is `O(1)`.
## 🌟 Rotate a LinkedList (medium)
https://leetcode.com/problems/rotate-list/

> Given the head of a Singly <b>LinkedList</b> and a number `K`, rotate the <b>LinkedList</b> to the right by `K` nodes.

Another way of defining the rotation is to take the sub-list of `K` ending nodes of the <b>LinkedList</b> and connect them to the beginning. Other than that we have to do three more things:

1. Connect the last node of the <b>LinkedList</b> to the head, because the list will have a different tail after the rotation.
2. The new head of the <b>LinkedList</b> will be the node at the beginning of the sublist.
3. The node right before the start of sub-list will be the new tail of the rotated <b>LinkedList</b>.

````js
class Node {
  constructor(value, next=null){
    this.value = value;
    this.next = next;
  }

  getList() {
    let result = "";
    let temp = this;
    while (temp !== null) {
      result += temp.value + " ";
      temp = temp.next;
    }
    return result;
  }
};


function rotate(head, rotations) {
  if(head === null || head.next === null || rotations <= 0) return head
  
  //find the length and the last node of the list
  let lastNode = head
  let listLength = 1
  
  while(lastNode.next !== null) {
    lastNode = lastNode.next
    listLength++
  }
  
  //connect the last node with the head to make it a circular list
  lastNode.next = head
  
  //no need to do rotations more than the length of the list
  rotations %= listLength
  let skipLength = listLength - rotations
  let lastNodeOfRotatedList = head
  
  for(let i = 0; i < skipLength - 1; i++) {
    lastNodeOfRotatedList = lastNodeOfRotatedList.next
  }
  
  //lastNodeOfRotatedList.next is pointing to the sub-list of k ending nodes
  head = lastNodeOfRotatedList.next
  lastNodeOfRotatedList.next = null
  
  return head
};


head = new Node(1)
head.next = new Node(2)
head.next.next = new Node(3)
head.next.next.next = new Node(4)
head.next.next.next.next = new Node(5)
head.next.next.next.next.next = new Node(6)

console.log(`Nodes of original LinkedList are: ${head.getList()}`)
console.log(`Nodes of rotated LinkedList are: ${rotate(head, 3).getList()}`)
````
- The time complexity of our algorithm will be `O(N)` where `N’` is the total number of nodes in the <b>LinkedList</b>.
- We only used constant space, therefore, the space complexity of our algorithm is `O(1)`.
