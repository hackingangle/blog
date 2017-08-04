---
title: scrapy-深度&广度优先
date: 2017-07-08 12:36:47
tags: scrapy
---

## 为什么用到了树的遍历？
学习scrapy爬虫，应用爬虫框架，是一个what阶段。

如何去更好的理解what阶段配置的参数的意义，其实就离不开一些基础的算法所代表的含义。

比如说：

在一个没有资源列表的网站，所有的资源都是松散的，不能通过分析规则的分页数据获取到所有的网站资源链接。

这种情况怎么破？

全站遍历（扫描），听起来是一个很好的思路，大概归结成流程如下：

> 获取&分析一个网页->提取网页下面所有链接->链接加入缓存策略->进行进行下一组扫描

其实是就是一个`带环的树`，我们如何把环去掉？

> 通过url去重可以去掉环

其实就是url每次`获取&分析`完毕后，加入到`已分析池`中。

那我们就把问题简化成：树的遍历

树的遍历做最简单的描述，我们用二叉树来做例子，对二叉树的遍历做探讨。

二叉树的遍历可以分为两种：深度优先和广度优先。


先给一个二叉树结构

![二叉树(深度广度优先)](/images/scrapy/二叉树_深度广度优先.jpeg)

## 深度优先

看上面的图，我们来说一下深度优先的逻辑：
深度优先是从跟节点开始，以左到右顺序，每一个节点都扫面到最子节点，然后返回，再访问兄弟节点。

顺序如下：

> ABDEICFGH

### 实现
深度优先的实现算法可以是`递归`

``` python
# 深度遍历
def depthTraversal(self, node):
    if node is not None:
        print(node.data)
    if node.left is not None:
        self.depthTraversal(node.left)
    if node.right is not None:
        self.depthTraversal(node.right)
```

### 深度优先坑

1. 优先递归顺序的保存，依赖了调用栈，所以数据量太大会导致溢出
    - 种类
        - 递归层级太深
        - 无法退出
    - 解决方法
        - 限制退出规则
        - 指定最大层级(此处在scrapy中有应用!!!)

# 广度优先

广度优先就是从左到右顺序一层一层的遍历

顺序如下：

> ABCDEFGHI

## 实现

`队列`

``` python
# 广度遍历
def breadthTraversal(self, node):
    cached = []
    if node is None:
        return
    cached.append(node)
    while cached:
        node = cached.pop(0)
        print(node.data)
        if node.left is not None:
            cached.append(node.left)
        if node.right is not None:
            cached.append(node.right)
```

### 广度优先坑

如深度优先

## 代码实现Python3

上面分别叙述了两种遍历二叉树的方法，下面我们来实践下，我给出的是可以在python3下面跑的代码，大家可以调试感受下算法运行，
构建了一个这样的二叉树：


![二叉树-代码](/images/scrapy/二叉树-代码.jpeg)

``` python
# 定义二叉树节点
class Node:
    def __init__(self, data, left, right):
        self.data = data
        self.left = left
        self.right = right

# 定义二叉树
class BinaryTree:

    # init
    def __init__(self):
        self.root = None

    # 根节点
    def initTree(self, node):
        self.root = node

    # 构建二叉树
    def addNode(self, node):
        treeNodes = []
        def doAddNode(parentNode, level, node):
            if parentNode.left is None:
                parentNode.left = node
                treeNodes.append(node)
                return
            elif parentNode.right is None:
                parentNode.right = node
                treeNodes.append(node)
                return
            else:
                treeNodes.append(parentNode.left)
                treeNodes.append(parentNode.right)
                doAddNode(treeNodes[level+1], level+1, node)
        treeNodes.append(self.root)
        doAddNode(self.root, 0, node)

    # 深度遍历
    def depthTraversal(self, node):
        if node is not None:
            print(node.data)
        if node.left is not None:
            self.depthTraversal(node.left)
        if node.right is not None:
            self.depthTraversal(node.right)

    # 广度遍历
    def breadthTraversal(self, node):
        cached = []
        if node is None:
            return
        cached.append(node)
        while cached:
            node = cached.pop(0)
            print(node.data)
            if node.left is not None:
                cached.append(node.left)
            if node.right is not None:
                cached.append(node.right)

if __name__ == '__main__':
    # 测试数据
    data = [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15]
    binaryTree = BinaryTree()
    for (key, value) in enumerate(data):
        node = Node(value, None, None)
        if key == 0:
            binaryTree.initTree(node)
        else:
            binaryTree.addNode(node)
    # 深度优先
    # binaryTree.depthTraversal(binaryTree.root)
    # 输出：1,2,4,8,9,5,10,11,3,6,12,13,7,14,15

    # 广度优先
    binaryTree.breadthTraversal(binaryTree.root)
    # 输出：12,3,4,5,6,7,8,9,10,11,12,13,14,15
```
