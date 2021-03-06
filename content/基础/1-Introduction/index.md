---
title: Introduction
weight: 10
date: 2022-07-18 11:14:11 +0800
math: true
---

本章会介绍：

- 数字硬件的组成
- 数字电路的设计流程
- 二进制数字
- 信息的数字化表示

## 数字电路的设计流程

```goat
    .--------------.
   |Required product|
    '-------+------'
            |
            v
.-----------+----------.
|Define specificatioins|
'-----------+----------'
            |
            v
    .-------+------.
    |Initial design|
    '-------+------'
            |
            +---------<---------+
            |                   |
            v                   |
     .------+-----.        .----+-----.
     | Simulation |        | Redesign |
     '------+-----'        '----+-----'
            |                   ^
      ______v______             |
     /             \  No        |
    +Design correct?+------>----+----<----------+
     \_____________/                            |
            |                                   |
        Yes +-----------<-----------+           |
            |                       |           |
            v                       |           |
.-----------+------------.  .-------+--------.  |
|Prototype implementation|  |Make corrections|  |
'-----------+------------'  '----------------'  |
            |                       ^           |
            v                       |           |
     .------+------.          ______|______     |
     |   Testing   |         /             \    |
     '------+------'        + Minor errors? +---+
            |                \_____________/
   _________v__________             ^
  /                    \ No         |
 + Meets specification? +-----------+
  \____________________/
            |
        Yes |
            v
    .--------------.
   |Finished product|
    '-------+------'
```

## 信息的数字化表示

在数字电路中，通过电平高低来表示 0 和 1，一般 0V 表示 0，1V 表示 1. 下面来看看如何用 0 和 1 表示数字、字母和其他信息。

### 二进制数字

在十进制中，每一位数字都表示十的几次方，比如 8547 表示 $8\times 10^3+5\times 10^2+4\times 10^1+7\times 10^0$，对于任意的十进制数，比如：

$$
D = d_{n-1}d_{n-2}\cdots d_1d_0
$$

都可以表示为：

$$
V(D)=d_{n-1}\times 10^{n-1}+d_{n-2}\times 10^{n-2}+\cdots d_1\times 10^1+d_0 \times 10^0
$$

因此，十进制是以 10 为底数的数字系统。

而在数字电路中，由于只能使用 0 和 1，因此必须采用二进制。二进制中的每一位称为 **比特（bit）**，任意二进制数

$$
B=b_{n-1}b_{n-2}\cdots b_1b_0
$$

都可以写成：

$$
\begin{align}
    V(B)&=b_{n-1}\times 2^{n-1}+b_{n-2}\times 2^{n-2}+\cdots b_1\times 2^1+b_0 \times 2^0\\
    &=\sum_{i=0}^{n-1} b_i\times 2^i
\end{align}
$$

比如：$(1101)_2=1\times 2^3 + 1\times 2^2 + 0\times 2^1 + 1\times 2^0=(13)_{10}$，这里括号的下标表示这是几进制数。

n bits 的二进制表示 $0$ 到 $2^n-1$ 的数字。

### 十进制与二进制的转换

二进制转十进制只需要将其每一位展开为 $b_{n}\times 2^{n}$（n从0开始）即可（见上面的例子）。十进制转二进制则相反，需要除以 2，考虑一个二进制数：

$$
\begin{align}
V(B)&=b_{n-1}\times 2^{n-1}+b_{n-2}\times 2^{n-2}+\cdots b_1\times 2^1+b_0\\
V(B)/2&=b_{n-1}\times 2^{n-2}+b_{n-2}\times 2^{n-3}+\cdots b_1 &\cdots b_0
\end{align}
$$

其最低位（Least-Significant Bit, LSB）只可能是 0 或 1，除以 2 后恰好是余数。对商继续除以2，就能得到二进制的每一位数。注意最后得到的数才是最高位（Most-Significant Bit, MSB）。

示例：

$$
\begin{align}
(857)_{10}&\rightarrow (?)_2 &{\rm Remainder} &\\
    857 \div 2 &= 428 &1 &\;{\rm LSB}\\
    428 \div 2 &= 214 &0 &\\
    214 \div 2 &= 107 &0 &\\
    107 \div 2 &= 53  &1 &\\
     53 \div 2 &= 26  &1 &\\
     26 \div 2 &= 13  &0 &\\
     13 \div 2 &= 6   &1 &\\
      6 \div 2 &= 3   &0 &\\
      3 \div 2 &= 1   &1 &\\
      1 \div 2 &= 0   &1 &\;{\rm MSB}
\end{align}
$$

$$
(857)_{10} = (1101011001)_2
$$

## ASCII 字码

![ASCII 字码表](images/ASCII-Table-wide.svg)

ASCII 使用 7 bits 来表示 128 个字符，但在计算机中一个字节一般是 8 bits，多出来的一位可以设置成 0，或用于校验（如对其余 7 位进行[奇偶校验](https://www.cnblogs.com/winformasp/articles/11944018.html)）。

## 数字与模拟信信号

二进制数也可以表示模拟信号，通过数模转换电路（DAC），可以将数字转换为相应的电平。反之，通过模数转换电路（ADC），可以将模拟信号转为二进制数。
