# Binary-Tree

Binary Tree in Swift. A Binary Tree is an abstract data struture the is made up of a root node and a left and rigt subtree. At most an node has zero, one or tow child nodes. 


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

## Traversing a Tree 

The are 4 ways to traverse (iterate through each element of) a Binary Tree. 

* Breadth-order traversal or Level-order traversal 
* Depth-First Traversal - In-Order 
* Depth-First Traversal - Pre-Order 
* Depth-First Traversal - Post-Order 



