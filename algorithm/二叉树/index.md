## 二叉树

> 基础理论：[二叉树的基本概念](https://github.com/Capactity/blog/blob/master/data-structure/二叉树.md)

####  二叉树的遍历方式

二叉树主要有两种遍历方式：

1. 深度优先遍历：先往深走，遇到叶子节点再往回走。

2. 广度优先遍历：一层一层的去遍历。

   那么从深度优先遍历和广度优先遍历进一步拓展，才有如下遍历方式：

   - 深度优先遍历

     - 前序遍历：中左右  --（递归法，迭代法）
     - 中序遍历：左中右  --（递归法，迭代法）
     - 后序遍历：左右中  --（递归法，迭代法）

   - 广度优先遍历

     - 层次遍历（迭代法  --  队列）

     

**递归三要素：**

1. **确定递归函数的参数和返回值：** 确定哪些参数是递归的过程中需要处理的，那么就在递归函数里加上这个参数， 并且还要明确每次递归的返回值是什么进而确定递归函数的返回类型。
2. **确定终止条件：** 写完了递归算法, 运行的时候，经常会遇到栈溢出的错误，就是没写终止条件或者终止条件写的不对，操作系统也是用一个栈的结构来保存每一层递归的信息，如果递归没有终止，操作系统的内存栈必然就会溢出。
3. **确定单层递归的逻辑：** 确定每一层递归需要处理的信息。在这里也就会重复调用自己来实现递归的过程。



### 相关题目

#### 递归处理

[101.对称二叉树(简单)](https://github.com/Capactity/blog/blob/master/algorithm/二叉树/101-对称二叉树.md)

[226.翻转二叉树(简单)](https://github.com/Capactity/blog/blob/master/algorithm/二叉树/226-翻转二叉树.md)

[104.二叉树的最大深度(简单)](https://github.com/Capactity/blog/blob/master/algorithm/二叉树/104-二叉树的最大深度.md)

[111.二叉树的最小深度](https://github.com/Capactity/blog/blob/master/algorithm/二叉树/111-二叉树的最小深度.md)

#### 完全二叉树

[222.完全二叉树的节点个数](https://github.com/Capactity/blog/blob/master/algorithm/二叉树/111-二叉树的最小深度.md)

[110.平衡二叉树](https://github.com/Capactity/blog/blob/master/algorithm/二叉树/111-二叉树的最小深度.md)

#### 路径问题（递归+回溯）

[257.二叉树的所有路径](https://github.com/Capactity/blog/blob/master/algorithm/二叉树/257-二叉树的所有路径.md)

[129.求根节点到叶子节点数字之和](https://github.com/Capactity/blog/blob/master/algorithm/二叉树/129-求根节点到叶子节点数字之和.md)

[404.左叶子之和](https://github.com/Capactity/blog/blob/master/algorithm/二叉树/404-左叶子之和.md)

[112.路径总和](https://github.com/Capactity/blog/blob/master/algorithm/二叉树/112-路径总和.md)

#### 层序遍历

[513.找左下角的值](https://github.com/Capactity/blog/blob/master/algorithm/二叉树/513-找左下角的值.md)

#### 修改与构建二叉树

[106.从中序与后序遍历序列构造二叉树](https://github.com/Capactity/blog/blob/master/algorithm/二叉树/106-从中序与后序遍历序列构造二叉树.md)

[226.翻转二叉树](https://github.com/Capactity/blog/blob/master/algorithm/二叉树/226-翻转二叉树.md)

[654.最大二叉树](https://github.com/Capactity/blog/blob/master/algorithm/二叉树/654-最大二叉树.md)

#### 二叉搜索树属性

[700.二叉搜索树中的搜索](https://github.com/Capactity/blog/blob/master/algorithm/二叉树/700-二叉搜索树中的搜索.md)

[98.验证二叉搜索树](https://github.com/Capactity/blog/blob/master/algorithm/二叉树/98-验证二叉搜索树.md)

[230.二叉搜索树中第K小的元素](https://github.com/Capactity/blog/blob/master/algorithm/二叉树/230-二叉搜索树中第K小的元素.md)

[450.删除二叉搜索树中的节点](https://github.com/Capactity/blog/blob/master/algorithm/二叉树/450-删除二叉搜索树中的节点.md)

