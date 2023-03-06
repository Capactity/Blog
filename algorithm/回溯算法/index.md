## 回溯算法

> **回溯法也可以叫做回溯搜索法，它是一种搜索的方式，也就是递归函数**。回溯法并不是什么高效的算法，**因为回溯的本质是穷举，穷举所有可能，然后选出我们想要的答案**。如果想让回溯法高效一些，可以加一些剪枝的操作

在解题中，我们可以把所有的回溯问题，都抽象为一个树形结构来考虑。如在集合中递归查找子集，集合的大小就构成了树的宽度，递归的深度构成的树的深度。

**回溯算法模版框架**：

```js
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```



#### 组合问题

- [77.组合(中等)](https://github.com/Capactity/blog/blob/master/algorithm/回溯算法/77-组合.md)
- [216.组合总和III(中等)](https://github.com/Capactity/blog/blob/master/algorithm/回溯算法/216-组合总和.md)
- [40.组合总和II](https://github.com/Capactity/blog/blob/master/algorithm/回溯算法/40-组合总和.md)
- 17.电话号码的字母组合

#### **排列问题**

- 46.全排列

#### 分割问题

- [131.分割回文串](https://github.com/Capactity/blog/blob/master/algorithm/回溯算法/131-分割回文串.md)
- 93.复原IP地址
- 139单词拆分

#### 棋盘问题

- 51.N皇后

#### 其他

- 22.括号生成
- 79.单词搜索

