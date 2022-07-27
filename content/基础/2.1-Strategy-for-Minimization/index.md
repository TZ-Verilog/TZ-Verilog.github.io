---
title: Strategy for Minimization
weight: 20
date: 2022-07-26 16:44:38 +0800
math: true
---

## 简化逻辑表达式的策略

下面我们将介绍如何使用卡诺图（karnaugh map）来获得逻辑表达式的最简形式。

### 术语

- **Literal**: Each appearance of a variable in a product term, either uncomplemented or complemented, is called a literal.（即乘积中的变量）
- **Implicant**: A product term that indicates the input valuation(s) for which a given function is equal to 1 is called an implicant of the function.（即某个乘积为 1 时，$f=1$，则这个乘积就是 implicant）
- **Prime Implicant**: An implicant is called a prime implicant if it cannot be combined into another implicant that has fewer literals.（不能进一步化简的 implicant）
- **Cover**: A collection of implicants that account for all valuations for which a given function is equal to 1 is called a cover of that function. （能使 $f=1$ 的一组 implicants）
- **Cost**: The
number of gates plus the total number of inputs to all gates in the circuit. We will assume that primary inputs, namely,
the input variables, are available in both true and complemented forms at zero cost.（门+门的输入，注意正负变量一开始就有，是不算cost的）

关于 Cost，举个例子：

$$
f=x_1\overline{x}_2+x_3\overline{x}_4
$$

有 2 个 AND，1 个 OR. 三个门共有 6 个输入，所以总的 cost=9. 注意 $\overline{x}_2,\overline{x}_4$ 是本来就有的输入，不算 cost

### 简化流程

1. Generate all prime implicants for the given function f.
2. Find the set of essential prime implicants.
3. If the set of essential prime implicants covers all valuations for which f = 1, then this set is the desired cover of f. Otherwise, determine the nonessential prime implicants that should be added to form a complete minimum-cost cover.
