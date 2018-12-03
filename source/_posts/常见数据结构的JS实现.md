---
title: 常见数据结构的JS实现
date: 2018-07-23 21:12:15
tags:
- web前端
- JS
- 数据结构

categories:
- IT
---

数据结构和算法是一位优秀的程序员的基本功，最近在看《学习JavaScript数据结构与算法》一书，总结了常见的数据结构。
<!-- more -->

#### 数组 (Array)
数组数据结构(array data structure),简称数组,由相同类型的元素的集合所组成,分配一块连续的内存来存储.利用元素的索引(index)可计算出元素对应的存储地址.
```js
   let arr = []
   arr.push(1) // [1]
   arr.push(2) // [1,2]
   arr.pop()   // [1]
```
在数组添加,查找,删除元素的时间复杂度都是O(n),根据索引(index)查找复杂度为O(1)


#### 栈 (Stack)
栈(stack)是一种特殊的串列形式的抽象数据类型.特殊之处在于只允许在数组的一端(栈顶:top)进行加入数据和输出数据.
以下我们使用函数实现一个简单的栈:
```js
function Stack() {
    this.list = [] //存储元素
    this.top = 0  // 栈顶下标
    this.pop = function() {
        return this.list[--this.top]
    }
    this.push = function(item) {
        this.list[this.top++] = item
    }
    this.empty = function() {
        this.top = 0
    }
    this.isEmpty = function() {
        return this.top === 0
    }
    this.peek = function() {
        return this.list[this.top - 1]
    }
    let stack = new Stack()
    stack.push(1) // stack.list -> [1]
    stack.push(2) // stack.list -> [1,2]
    stack.push(3) // stack.list -> [1,2,3]
    stack.pop()   // stack.list -> [1,2]
    stack.isEmpty() // false
    stack.peek() // 2
    stack.empty() // stack.list -> [] 
```
在栈中添加,删除元素的时间复杂度都是O(1),不存在查找方法
#### 队列 (Queue)
队列是先进先出(FIFO,first-in-fisrt-out)的线性表.队列只允许在后端进行插入操作,在前端进行删除操作.
我们使用Es6的class来实现一个简单队列
```js
class Queue {
    constructor() {
        // 定义一个数组来保存队列里面的元素
        this.items = []
    }
    // 在队列尾部添加一个或者多个元素
    enqueue(item) {
        this.items.push(item)
    }
    // 移除队列顶部的元素，并返回被移除的元素
    dequeue() {
        return this.items.shift()
    }
    // 清空队列
    empty() {
        this.items = []
    }
    // 返回队列顶部的元素，不对队列本身做修改
    front() {
        return this.items[0]
    }
    // 判断队列是否为空
    isEmpty() {
        return this.items.length === 0
    }
    // 返回队列里面元素的个数
    length() {
        return this.item.length
    }
}
const queue = new Queue()
queue.isEmpty() // true
queue.enqueue(1)
queue.enqueue(2)
queue.dequeue() // 返回移除的元素1 
queue.front() // 2
queue.length() // 1
queue.isEmpty() // false
queue.remove() // 清空元素
queue.isEmpty() // true
```
和栈一样,在队列中添加,删除元素的时间复杂度都是O(1),不存在查找方法
#### 链表 (Linked List)
链表是一种常见的基础数据结构,也是一种线性表,但是不会按线性的顺序存储数据,而是在每一个节点里存储下一个节点的指针.由于不必须按顺序存储,链表在插入数据时可以达到O(1)的复杂度.
以下我们以数组为基础,实现一个双向链表:
1. 首先创建一个Node类,element保存节点数据.next保存指向下个节点的链接,prev保存指向上一个节点的链接

```js
// 节点
function Node(element) {
    this.element = element
    this.next = null
    this.prev = null
}

//链表类
function LinkedList() {
    //头节点
    this.head = new Node('head')
    this.size = 0;
    //查找节点,从头遍历
    this.find = function(item) {
        if (item == null) {
            return null
        }
        var currNode = this.head;
        while (currNode && currNode.element !== item) {
            currNode = currNode.next;
        }
        return currNode;
    }
    //插入节点,1.查找要插入的位置 2.将
    this.insert = function(newEl, item) {
        var newNode = new Node(newEl);
        var finder = item ? this.find(item) : null;
        if (!finder) {
            var last = this.findLast();
            newNode.prev = last;
            last.next = newNode;
        } else {
            newNode.next = finder.next;
            newNode.prev = finder;
            if (finder.next) {
                finder.next.prev = newNode;
            }
            finder.next = newNode;
        }
        this.size++;

    }
    //删除节点
    this.remove = function(item) {
        if (item) {
            var node = this.find(item);
            if (node == null) {
                return;
            }
            if (node.next === null) {
                node.prev.next = null;
                node.prev = null;
            } else {
                node.prev.next = node.next;
                node.next.prev = node.prev;
                node.next = null;
                node.prev = null;
            }
            this.size--;
        }
    }
    //查找链表中的最后一个元素,遍历查找
    this.findLast = function() {
        var currNode = this.head;
        while (currNode.next) {
            currNode = currNode.next;
        }
        return currNode;
    }
    // 打印元素
    this.display = function() {
        var currNode = this.head;
        // 元素不存在
        if (currNode === null) {
            return
        }
        while (currNode) {
            console.log(currNode.element)
            currNode = currNode.next;
        }
        console.log('---------'+this.size+'------------')
    }
}
```
链表的优点是：add和remove操作时间上都是O(1)的；缺点是：get和set操作时间上都是O(N)的，而且需要额外的空间存储指向其他数据地址的项。
#### 散列表 (Hash Table)
Hash table，也叫哈希表），是根据键（Key）而直接访问在内存存储位置的数据结构。也就是说，它通过计算一个关于键值的函数，将所需查询的数据映射到表中一个位置来访问记录，这加快了查找速度。这个映射函数称做散列函数，存放记录的数组称做散列表。在散列表上插入、删除和取用数据都非常快，但是对于查找操作来说却效率低下，比如查找一组数据中的最大值和最小值。
```js
function HashTable() {
    var table = [];
    var loseloseHashCode = function(key) {
        var hash = 0;
        for (var i = 0; i < key.length; i++) {
            hash += key.charCodeAt(i)
        }
        return hash % 37  // 取hash值除以37的余数作为值的地址，目的减少数组长度
    }
    this.put = function(key, value) {
        var position = loseloseHashCode(key);
        table[position] = value
    }
    this.get = function(key) {
        return table[loseloseHashCode(key)];
    }
    ;
    this.remove = function(key) {
        table[loseloseHashCode(key)] = undefined;
    }
}

```

最大问题是冲突,假如有多个键值得到的散列值相等，那么后面的元素会覆盖前面前面相同散列值的元素，这就是hash碰撞.

所以散列表最重要的是找到一个好的散列函数.常见的有以下几种:
-  loselosehashCode
loseloseHashCode(key): key映射成数值，用ascii码的转换进行处理，借助key.charCodeAt(i)方法。

-  djb2Hash
put/remove/get方法没有变。
function djb2Hash(key):同样是利用key的每个字母的ascii码进行处理，不过hash的初值、每次线性相乘的参数、以及最后取余操作的基数发生了改变。这些参数是经过研究的可以使散列表效率更高的数值。
```js
var djb2HashCode = function(key) {
  var hash = 5381; //初始化一个hash变量并赋值为一个质数5381
  for(var i = 0; i < key.length; i++) { //遍历
    hash = hash * 33 + key.charCodeAt(i);
  }
  return hash % 1013;
}
```

-  线性探查 
这种冲突的解决方法，在向表中添加一个新的元素的时候，如果索引为index的位置已经被占据了，则尝试index+1的位置，如果index+1的也被占据了，则放在index+2的位置，以此类推。同样需要重写put/get/remove，主要是有一个查找的过程。同样需要用ValuePair的实例来表示值。移除以及查找的算法显然有不足，但这个不足依赖于插入算法弥补。不是完美的方法。

```js
this.put = function(key, value) {
  var position = loseloseHashCode(key);
  if(table[position] === undefined) { //这个位置没有被占据
    table[position] = new valuePair(key, value);
  } else {
    var index = ++position; //寻找下一个位置
    while(table[index] !== undefined) { //被占据继续寻找下一个位置
      index ++;
    }
    table[index] = new valuePair(key, value);
  }
};

this.get = function(key) {
  var position = loseloseHashCode(key);
  if(table[position] !== undefined) {
    if(table[position].key === key) { //举例位置5
      return table[position].value; 
    } else {
      var index = ++position;
      while(table[index] !== undefined && (table[index] && table[index].key !== key)) { //该位置有元素但是不是要寻找的元素
        index ++; //索引增加
      }
      if(table[index] && table[index].key === key) { //确认正确性
        return table[index].value; //找到7
      }
    }
  }
  return undefined;
};
```


#### 树 (Tree)
树是一种分层数据的抽象模型。一个树的结构包含一系列存在父子关系的节点。每个节点都有一个父节点（除了顶部的第一个节点）以及零个或多个子节点。二叉树中的节点最多只能有两个节点：一个是左侧子节点，另一个是右侧子节点。

二叉搜索树
二叉搜索树BST 是一种特殊的二叉树,二叉查找树（BST：Binary Search Tree）是一种特殊的二叉树，它改善了二叉树节点查找的效率。一句话就是左节点比父节点小，右节点比父节点大，还有一个特性就是”中序遍历“可以让结点有序。

 
```js
function Node(value) {
    this.value = value
    this.left = null
    this.right = null
}

// 二叉搜索树
function BST() {
    var root = null
    this.insert = function (value) {
        var newNode = new Node(value)
        if (root === null) { //根节点为空，作为根节点
            root = newNode
        } else {
            insertNode(root, newNode) //插入节点操作
        }
    }

    function insertNode(node, newNode) {
        if (newNode.value < node.value) {
            if (node.left === null) { //如果没有左侧节点就插入新的节点
                node.left = newNode
            } else { //有的话递归
                insertNode(node.left, newNode)
            }
        } else {
            if (node.right === null) {  //如果没有右侧节点就插入新的节点
                node.right = newNode
            } else { //有的话递归
                insertNode(node.right, newNode)
            }
        }
    }

    this.getRoot = function () {
        return root
    }

    this.exist = function (value) {  //是否存在
        return searchNode(root, value) //搜索操作
    }
    var searchNode = function (node, value) {
        if (node === null) {
            return false
        }
        if (value < node.value) { //如果小于继续从左边搜索
            return searchNode(node.left, value)
        } else if (value > node.value) { //如果大于继续从右边搜索
            return searchNode(node.right, value)
        } else { //命中
            return true
        }
    }
    this.remove = function (element) {
        root = removeNode(root, element)
    }
    var removeNode = function (node, element) { //移除一个节点
        if (node === null) {
            return null
        }
        if (element < node.value) {
            node.left = removeNode(node.left, element)
            return node
        } else if (element > node.value) {
            node.right = removeNode(node.right, element)
            return node
        } else { //命中后分三种情况
            //移除叶子节点，即该节点没有左侧或者右侧子节点的叶结点
            if (node.left === null && node.right === null) {
                node = null
                return node
            }
            //移除有一个左侧或者右侧子节点的节点
            if (node.left === null && node.right) {
                node = node.right //把引用改为子节点的引用，下同
                return node
            }
            if (node.right === null && node.left) {
                node = node.left
                return node
            }
            //移除有两个子节点的节点
            var aux = findMinNode(node.right) //找到右边子树的最小节点
            node.value = aux.value //改变节点的键，更新节点的值
            node.right = removeNode(node.right, aux.value) //移除有相同键的节点
            return node //返回更新后节点的引用
        }
    }
    this.min = function () { //找最小键
        return minNode(root)
    }
    var minNode = function (node) {
        if (node) {
            while (node && node.left !== null) {
                node = node.left
            }
            return node.value
        }
        return null
    }
    this.max = function () { //找最大键
        return maxNode(root)
    }
    var maxNode = function (node) {
        if (node) {
            while (node && node.right !== null) {
                node = node.right
            }
            return node.value
        }
        return null
    }
    var findMinNode = function (node) { //返回节点
        while (node && node.left !== null) {
            node = node.left
        }
        return node
    }
    var findMaxNode = function (node) { //返回节点
        while (node && node.right !== null) {
            node = node.right
        }
        return node
    }
    this.display = function (root) {
        display(root)
    }
    var display = function (root) {
        if (!root) {
            return
        }
        console.log(root.value) // 先序遍历
        display(root.left)
        // console.log(root) // 中序遍历
        display(root.right)
        // console.log(root) // 后序遍历  // 三者都是深度优先遍历
    }
}

(function test() {
    var bst = new BST()
    bst.insert(1)
    bst.insert(0)
    bst.insert(2)
    bst.insert(6)
    bst.insert(9)
    bst.insert(3)
    bst.insert(8)
    // bst.display(bst.getRoot())
    console.log(JSON.stringify(bst.getRoot()))
    console.log(bst.exist(6))
    bst.remove(6)
    bst.display(bst.getRoot())
    console.log(bst.min() === 0)
    console.log(bst.max() === 9)
})()

```
