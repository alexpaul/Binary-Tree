# Binary-Tree
Binary Tree in Swift.


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

The are 4 ways to travers a Binary Tree. 

* Breadth-order traversal or Level-order traversal 
* Depth-First Traversal - In-Order 

