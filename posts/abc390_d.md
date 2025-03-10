---
title: AtCoder abc390_d - Stone XOR
date: 2025/03/10
tags: [ABC, 搜索, 组合数学, 贝尔数]
pinned: false
head:
  - - meta
    - name: description
      content: 组合数学
  - - meta
    - name: keywords
      content: 贝尔数
---

新奇的 dfs 方式

---

[题目链接](https://atcoder.jp/contests/abc390/tasks/abc390_d)

## 问题描述

有 $N$ 个袋子，每个袋子里装有 $A_i$ 个石子

你可以随意的将一个袋子里的石子全部装到另一个袋子当中

求最终每个袋子里的石子个数进行异或运算后所有不相同的值的个数

即求 $B_1 \oplus B_2 \oplus \cdots \oplus B_N$ 的所有不同的可能的个数，其中 $B_i$ 是袋子 $i$ 中石头的最终数量。

## 数据范围

- $2 \leq N \leq 12$
- $1 \leq A_i \leq 10^{17}$

---

装袋子的操作即把两个袋子里的石子合并，可以认为问题是将任一一袋石子装到任一编号的盒子中，因此需要穷举袋子放进哪一个盒子的可能性。

这种划分方法的所有可能性的个数就是[贝尔数](https://oi-wiki.org/math/combinatorics/bell/)，这里提出贝尔数意在说明该题正确的搜索方式不会超出时限。

本题 $N=12$ 时有 $B(12)=4213597(\sim 4\times 10^6)$

如何列出所有可能性？常规的搜索剪枝固然可行，也有一种直接的思考方式。

```C++
void dfs(int i, int maxid) // 初始为 dfs(1, 1), i 表示当前选择的袋子, maxid 表示袋子 i 可选择的最大的编号的盒子
{
    if (i > n) // 所有的袋子都已经装进了盒子, 进入 calc() 函数计算出该可能性的结果
    {
        calc();
        return;
    }
    for (int id = 1; id <= maxid; id++) // 可以选择的盒子的编号为 1 ~ maxid
    {
        vis[i] = id;
        dfs(i + 1, max(maxid - 1, vis[i]) + 1); // 只有编号 a - 1 的盒子被选择了才可选择编号为 a 的盒子的编号
    }
}
```

这样能保证不多考虑或遗漏任何可能性，最后在 calc() 函数中模拟将 13 个袋子里的石子装进 13 个盒子中，再将 13 个盒子的数值异或

由于 calc() 计算得出的结果可能很大，需要使用哈希的方式记录结果是否出现，可以使用 unordered_set 存储计算的结果以去重

[非赛时 AC 代码](https://atcoder.jp/contests/abc390/submissions/62100589)
