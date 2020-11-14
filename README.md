# Binary Tree

Binary Tree in Swift. A Binary Tree is an abstract data struture the is made up of a root node and a left and rigt subtree. A node can have zero, one or two child nodes. 


## Components of a Binary Tree 

* data 
* left child node 
* right child node 

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

## Traversing a Tree 

The are 4 ways to traverse (iterate through each element of) a Binary Tree. 

* Breadth-First Order traversal or Level-order traversal 
* Depth-First Order Traversal - In-Order 
* Depth-First Order Traversal - Pre-Order 
* Depth-First Order Traversal - Post-Order 


## Breadth-First Order traversal or Level-order traversal

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
  
  mutating func dequeue() -> T? {
    guard !elements.isEmpty else {
      return nil
    }
    return elements.removeFirst()
  }
}
```

#### Breadth-First Order traversal

```swift 
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

## Depth-First Order Traversal - Post-Order

#### Write a function that take a Binary Tree node and prints the values using `post-order traversal`

```swift 
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


## Depth-First Order Traversal - Pre-Order




## Resources 

1. [Wikipedia - Binary Tree](https://en.wikipedia.org/wiki/Binary_tree#:~:text=In%20computer%20science%2C%20a%20binary,child%20and%20the%20right%20child.)
2. [GeekForGeeks - Binary Tree Data Structure](https://www.geeksforgeeks.org/binary-tree-data-structure/)

