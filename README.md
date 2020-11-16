# Binary Tree

A Binary Tree is an abstract data struture that is made up of a root node and a left and right subtree. A node can have zero, one or two child nodes. 

## Tree Structure Vocabulary

* root
* subtree
* ancestor (`a` is an ancestor of `b` if there is a path to `b` where `a` is the root node)
* descendents
* parent node
* leaf (a node that doen't have any children)
* depth or level (number of edges from the root node to a given node)
* height (number of edges from the root node to the furthest leaf node)
* diameter or width (the longest distance between any two nodes in a tree)  

![binary tree sketch](https://user-images.githubusercontent.com/1819208/99263430-3089c600-27ed-11eb-9295-e404463561f2.jpg)

## Components of a Binary Tree Node

* **data** type it holds e.g `Int` or `String` etc
* a pointer to a **left child** node 
* a pointer to a **right child** node 

## Common Operations 

* Traversal 
* Insertion
* Deletion 


## Binary Tree Node

```swift 
class BinaryTreeNode<T> {
  var value: T
  var left: BinaryTreeNode?
  var right: BinaryTreeNode?
  init(_ value: T) {
    self.value = value
  }
}
```

## Binary Tree 

```swift 
/*
        8
      /   \
     11    4
    /  \    \
   7   30    6
*/
```

#### Some facts about this Binary Tree: 

* The height of the tree is 2, we count the edges between each node from the deepest leaf node. 
* The depth of node 4 to the root is 1, again here we count the edges. 
* This in NOT a Binary Search Tree as the values in the left subtree is not all less than the root and also the rules for a BST on the right subtree does not hold as node 4 and 6 should be greater than the root's value 8. 

## Traversing a Tree 

The are 4 ways to traverse (iterate through each element of) a Binary Tree. 

* Breadth-First Order traversal or Level-order traversal 
* Depth-First Order Traversal - In-Order (left, root, right) 
* Depth-First Order Traversal - Pre-Order (root, left, right)
* Depth-First Order Traversal - Post-Order (left, right, root)


## Breadth-First Order traversal or Level-order traversal

With Bredth-First traversal a `Queue` is used since we want to print nodes by levels in which we visit them. We use the Queue to enqueue nodes and dequeue in the order we visited them. This ensures that we visit each node by the level in which it appears. 

```swift 
/*
        8
      /   \
     11    4
    /  \    \
   7   30    6
*/
```

The expected output when using `Breadth-First Traversal` is `8 11 4 7 30 6`. 

#### Queue

```swift 
struct Queue<T> {
  private var elements = [T]()
  
  public var isEmpty: Bool {
    return elements.isEmpty
  }
  
  public var peek: T? {
    return elements.first
  }
  
  mutating func enqueue(_ element: T) {
    elements.append(element)
  }
  
  mutating func dequeue() -> T? { // see Queue lesson for more optimized dequeue https://github.com/alexpaul/Queue
    guard !elements.isEmpty else {
      return nil
    }
    return elements.removeFirst()
  }
}
```

#### Breadth-First Order traversal

```swift 
/*
        8
      /   \
     11    4
    /  \    \
   7   30    6
*/

func breadthFirstTraversal<T>(_ treeNode: BinaryTreeNode<T>?) {
  var queue = Queue<BinaryTreeNode<T>>()
  guard let _ = treeNode else {
    return
  }
  queue.enqueue(treeNode!)
  print(treeNode!.value)
  while let node = queue.dequeue() {
    if let left = node.left {
      print(left.value)
      queue.enqueue(left)
    }
    if let right = node.right {
      print(right.value)
      queue.enqueue(right)
    }
  }
}

let rootNode = BinaryTreeNode(8)
let elevenNode = BinaryTreeNode(11)
let fourNode = BinaryTreeNode(4)
let sevenNode = BinaryTreeNode(7)
let thirtyNode = BinaryTreeNode(30)
let sixNode = BinaryTreeNode(6)

rootNode.left = elevenNode
rootNode.right = fourNode
elevenNode.left = sevenNode
elevenNode.right = thirtyNode
fourNode.right = sixNode

breadthFirstTraversal(rootNode)
// 8 11 4 7 30 6
```

## Depth-First Order Traversal - In-Order

#### Write a function that takes a Binary Tree node and prints all its values 

```swift 
func inOrderTraversal<T>(_ node: BinaryTreeNode<T>?) {
  if let left = node?.left {
    inOrderTraversal(left)
  }
  if let value = node?.value {
    print(value)
  }
  if let right = node?.right {
    inOrderTraversal(right)
  }
}

inOrderTraversal(rootNode) // 7 11 30 8 4 6
```

#### Write a function that takes a Binary Tree node and captures its values via a closure

```swift 
func inOrderTraversal<T>(_ node: BinaryTreeNode<T>?, visit: (BinaryTreeNode<T>) -> ()) {
  if let left = node?.left {
    inOrderTraversal(left, visit: visit)
  }
  if let node = node {
    visit(node)
  }
  if let right = node?.right {
    inOrderTraversal(right, visit: visit)
  }
}

inOrderTraversal(rootNode) { (node) in
  print(node.value) // 7 11 30 8 4 6
}
```

#### Extend Binary Tree node and write a funciton that captures all its values via a closure 

```swift 
extension BinaryTreeNode {
  func inOrderTraversal(_ visit: (BinaryTreeNode<T>) -> ()) {
    left?.inOrderTraversal(visit)
    visit(self)
    right?.inOrderTraversal(visit)
  }
}

rootNode.inOrderTraversal { (node) in
  print(node.value) // 7 11 30 8 4 6
}
```

In-order traversal can be used to find out if a given binary tree is a binary search tree, i.e in ascending order.

## Depth-First Order Traversal - Post-Order

#### Write a function that take a Binary Tree node and prints the values using `post-order traversal`

```swift 
/*
        8
      /   \
     11    4
    /  \    \
   7   30    6
*/

func postOrderTraversal<T>(_ treeNode: BinaryTreeNode<T>?) {
  if let left = treeNode?.left {
    postOrderTraversal(left)
  }
  if let right = treeNode?.right {
    postOrderTraversal(right)
  }
  if let root = treeNode {
    print(root.value)
  }
}

postOrderTraversal(rootNode) // 7 30 11 6 4 8
```

Post-order traversal can be used to delete a tree. 


## Depth-First Order Traversal - Pre-Order

```swift 
/*
        8
      /   \
     11    4
    /  \    \
   7   30    6
*/

func preOrderTraversal<T>(_ treeNode: BinaryTreeNode<T>?) {
  if let root = treeNode {
    print(root.value)
  }
  if let left = treeNode?.left {
    preOrderTraversal(left)
  }
  if let right = treeNode?.right {
    preOrderTraversal(right)
  }
}

preOrderTraversal(rootNode) // 8 11 7 30 4 6
```

Pre-order traversal can be used to make a copy of a tree. 

## Resources 

1. [Wikipedia - Tree Data Structure](https://en.wikipedia.org/wiki/Tree_(data_structure))
2. [Wikipedia - Binary Tree](https://en.wikipedia.org/wiki/Binary_tree#:~:text=In%20computer%20science%2C%20a%20binary,child%20and%20the%20right%20child.)
3. [GeekForGeeks - Binary Tree Data Structure](https://www.geeksforgeeks.org/binary-tree-data-structure/)
4. [HackerRank - Binary Trees and Binary Search Trees](https://www.hackerrank.com/topics/binary-trees-and-binary-search-trees)
5. [RW - Swift Algorithm Club - Binary Tree](https://github.com/raywenderlich/swift-algorithm-club/tree/master/Binary%20Tree)
6. [Video - HackerRank - Trees](https://www.youtube.com/watch?v=oSWTXtMglKE)

