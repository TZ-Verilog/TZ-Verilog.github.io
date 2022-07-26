---
title: Introduction to Logic Circuits
weight: 15
date: 2022-07-24 12:54:41 +0800
math: true
---

本章会介绍：

- 逻辑表达式和电路
- 布尔运算
- 逻辑门和简单电路的综合
- CAD 和 VerilogHDL
- 逻辑表达式的化简与卡诺图（Karnaugh maps)

## 逻辑表达式

考虑一个简单的开关电路

```goat
        x
        |
      .-+-.
  +---+ S +-----+
--+-- '-+-'   .-+-.
              | L |
 -+-          '-+-'
  +             |
  +-------------+
```

S 表示这是一个开关，x是外部的输入信号，L表示灯。当 x=0 时，L=0；x=1时，L=1. 所以我们可以写成：

$$
L(x)=x
$$

```goat
       x1      x2
        |      |
      .-+-.  .-+-.
  +---+ S +--+ S +---+
--+-- '-+-'  '-+-' .-+-.
                   | L |
 -+-               '-+-'
  +                  |
  +------------------+
```

只有当 $x_1,x_2$ 同时为 1 时，L 才等于 1. 上面的电路则可以写成：

$$
L(x_1,x_2)=x_1\cdot x_2
$$

符号 $\cdot$ 称为 **与（AND）**。

```goat
          x1
          |
        .-+-.
  +-----+ S +------+
--+--   '-+-'    .-+-.
          |      | L |
 -+-    .-+-.    '-+-'
  +     | S |      |
  |     '-+-'      |
  |       |        |
  |       x2       |
  +----------------+
```

当 $x_1$ 或 $x_2$ 为 1 时，L 为 1. 上面的电路可以写成：

$$
L(x_1,x_2)=x_1+x_2
$$

符号 $+$ 称为 **或（OR）**。

```goat
      .------.
  +---+      +----+
--+-- '------'    |
              +---+---+
 -+-        .-+-.   .-+-.
  +      x -+ S |   | L |
  |         '-+-'   '-+-'
  |           +---+---+
  |               |
  +---------------+
```

当 x=0 时，L 为 1. 上面的电路可以写成：

$$
L(x)=\overline{x}
$$

上划线表示 **非（NOT）**，有一些其他表示方法，比如：

$$
\overline{x}=x'=!x=\sim x={\rm NOT}\; x
$$

## 真值表

我们介绍了基本的逻辑表达式，并给出了它们的电路定义，下面我们将给出了它们的真值表：

| $x_1,x_2$ | $x_1\cdot x_2$ | $x_1+x_2$ |
| :-------: | :------------: | :-------: |
|    0 0    |       0        |     0     |
|    0 1    |       0        |     1     |
|    1 0    |       0        |     1     |
|    1 1    |       1        |     1     |

真值表的最左边给出所有的输入组合，而右边则给出逻辑表达式的输出。真值表可以直观地展示出逻辑表达式。真值表会随着输入指数增长，比如当输入有 3 个，则真值表有 8 行。

| $x_1,x_2,x_3$ | $x_1\cdot x_2 \cdot x_3$ | $x_1+x_2+x_3$ |
| :-----------: | :----------------------: | :-----------: |
|     0 0 0     |            0             |       0       |
|     0 0 1     |            0             |       1       |
|     0 1 0     |            0             |       1       |
|     0 1 1     |            0             |       1       |
|     1 0 0     |            0             |       1       |
|     1 0 1     |            0             |       1       |
|     1 1 0     |            0             |       1       |
|     1 1 1     |            1             |       1       |

与和或都可以接受多个输入，与只有在输入都为 1 时才为 1，而或在输入中有一个为 1 时就为 1.

## 逻辑门

逻辑表达式在电路中可以通过晶体管/MOS管来实现，这在电路中可以用逻辑门来表示：

![logic gate](images/logic%20gate.svg)

通过组合逻辑门可以得到逻辑电路。

### 逻辑电路的分析

#### 例一

对于给定的逻辑电路，我们可以想象给定输入后得到什么输出，比如下面这个电路：$f=\overline{x}_1+x_1\cdot x_2$

![logic circuit](images/logic%20circuit.svg)

其真值表为：

| $x_1, x_2$ | $A,B$ | $f(x_1,x_2)$ |
| :--------: | :---: | :----------: |
|    0 0     |  1 0  |      1       |
|    0 1     |  1 0  |      1       |
|    1 0     |  0 0  |      0       |
|    1 1     |  0 1  |      1       |

其时序图为：

![time diagram](images/time%20diagram.svg)

时序图可以用于在实际电路中，通过逻辑分析仪，来观察其实际工作情况。

上面的逻辑表达式实际上等价于 $g=\overline{x}_1+x_2$，它们有相同的真值表。

#### 例二

考虑下面的逻辑电路：

![xor circuit](images/xor%20circuit.svg)

| $x,y$ |   L   |
| :---: | :---: |
|  0 0  |   0   |
|  0 1  |   1   |
|  1 0  |   1   |
|  1 1  |   0   |

只有当 x,y 不相同时，输出才为 1。我们把这个称为 **异或（XOR）**，记为 $x \oplus y$，用下面的逻辑门来表示：

![xor gate](images/xor%20gate.svg)

对异或门取反可以得到 **同或（NAND）**，记为 $x \odot y$，用下面的逻辑门来表示：

![nand gate](images/nand%20gate.svg)

#### 例三

在上一节我们介绍了二进制数，考虑两个一位的二进制数相加，则可能有四种结果；

$$
\begin{array}{r}
    a\\
   +b\\
   \hline
   s_1s_0
\end{array}
$$

| $a,b$ | $s_1s_0$ |
| :---: | :------: |
|  0 0  |   0 0    |
|  0 1  |   0 1    |
|  1 0  |   0 1    |
|  1 1  |   1 0    |

注意到 $s_0=a\oplus b$，$s_1=a\odot b$，所以可以画出如下逻辑电路：

![addition of binary numbers](images/add.svg)

这个电路称为 **半加器**。

## 布尔代数

George Boole 在 1849 年发表了用代数描述逻辑推理的方法，被后人称为 Boolean Algebra.

### 公理

假设有如下公理：

1. $0\cdot 0=0$
2. $1+1=1$
3. $1\cdot 1=1$
4. $0+0=0$
5. $0\cdot 1=1\cdot 0=0$
6. $1+0=0+1=1$
7. If $x=0$, then $\overline{x}=1$
8. If $x=1$, then $\overline{x}=0$

### 定理

#### 单变量定理

1. $x\cdot 0=0$
2. $x+1=1$
3. $x\cdot 1=0$
4. $x+0=x$
5. $x\cdot x=x$
6. $x+x=x$
7. $x\cdot\overline{x}=0$
8. $x+\overline{x}=1$
9. $\overline{\overline{x}}=x$

以上定理很容易证明，只需要代入 0 或 1 即可。

#### 对偶

某个逻辑表达式的 **对偶式** 可以通过将 与/或 互换，0/1 互换来得到。观察上面的公理和定理，可以发现一些式子是对偶的。比如：

$$
x+0=x \leftrightarrow x\cdot 1=x
$$

如果一个逻辑表达式是正确的，其对偶式也是正确的，它们的结果是一样的。因此，一般有两种方法来表示同一个逻辑表达式，通常其中一种会更简单。

#### 多变量定理

交换率（Commutative）

- $x\cdot y = y\cdot x$
- $x+y=y+x$

结合率（Associative）

- $x\cdot(y\cdot z)=(x\cdot y)\cdot z$
- $x+(y+z)=(x+y)+z$

分配律（Distributive）

- $x\cdot(y+z)=x\cdot y+x\cdot z$
- $x+y\cdot z=(x+y)\cdot(x+z)$

吸收率（Absorption）

- $x+x\cdot y=x$
- $x\cdot(x+y)=x$
- $x\cdot y+x\cdot\overline{y}=x$
- $(x+y)\cdot(x+\overline{y})=x$

德·摩根定律（DeMorgan's theorem）

- $\overline{x\cdot y}=\overline{x}+\overline{y}$
- $\overline{x+y}=\overline{x}\cdot\overline{y}$
- $x+\overline{x}\cdot y=x+y$
- $x\cdot(\overline{x}+y)=x\cdot y$

共识定理（Consensus）

- $x\cdot y+y\cdot z+\overline{x}\cdot z=x\cdot y+\overline{x}\cdot z$
- $(x+y)\cdot(y+z)\cdot(\overline{x}+z)=(x+y)\cdot(\overline{x}+z)$

### 例子

证明以下逻辑表达式：

$$
(x_1+x_2)\cdot(\overline{x}+\overline{x}_2)=x_1\cdot\overline{x}_2+\overline{x}_1\cdot x_2
$$

展开等式左边：

$$
\begin{align}
    {\rm LHS} &= (x_1+x_2)\cdot\overline{x}_1+(x_1+x_2)\cdot\overline{x}_2\\
    &=x_1\cdot\overline{x}_1+x_2\cdot\overline{x}_1+x_1\cdot\overline{x}_2+x_2\cdot\overline{x}_2\\
    &=0+x_2\cdot\overline{x}_1+x_1\cdot\overline{x}_2+0\\
    &=x_2\cdot\overline{x}_1+x_1\cdot\overline{x}_2
\end{align}
$$

### 韦恩图

韦恩图也可以用于证明逻辑表达式的正确性。这里就不花了，有兴趣的话可以试着证明一下共识定理。

## 用与/或/非门综合

有了以上知识，我们可以尝试用与/或/非门得到所希望的逻辑功能。考虑下面的真值表：

| $x_1,x_2$ | $f(x_1,x_2)$ |
| :-------: | :----------: |
|    0 0    |      1       |
|    0 1    |      1       |
|    1 0    |      0       |
|    1 1    |      1       |

要写出逻辑表达式，我们可以将 $f=1$ 的行相加：

$$
f(x_1,x_2)=x_1x_2+\overline{x}_1x_2+\overline{x}_1\overline{x}_2
$$

> 注：以后我们写逻辑表达式时将省略 $\cdot$ 号。

但这样得到的式子并不是最简的。我们可以采取下面的化简方式：

$$
\begin{align}
    f(x_1,x_2) &= x_1x_2 + \overline{x}_1x_2+ \overline{x}_1x_2+\overline{x}_1\overline{x}_2\\
    &=(x_1+\overline{x}_1)x_2+\overline{x}_1(\overline{x}_2+x_2)\\
    &=x_2+\overline{x}_1
\end{align}
$$

直接用定理化简并不直观，所以后面我们会介绍另一种方法叫“卡诺图”。

### 积的和、和的积

下面我们将用专业术语来描述逻辑函数综合的过程。为了方便描述，我们将“与”看作乘，“或”看作除

#### 最小项

如果一个函数有 $n$ 个变量，则包含这 $n$ 个变量的乘积称为 **最小项（Minterm）**，变量可能是 $x_i$ 或 $\overline{x}_i$。真值表中的每一行都可以用一个最小项来表示。

| 行号  | $x_1,x_2,x_3$ |                      Minterm                       |
| :---: | :-----------: | :------------------------------------------------: |
|   0   |     0 0 0     | $m_0=\overline{x}_1 \overline{x}_2 \overline{x}_3$ |
|   1   |     0 0 1     |      $m_1=\overline{x}_1 \overline{x}_2 x_3$       |
|   2   |     0 1 0     |      $m_2=\overline{x}_1 x_2 \overline{x}_3$       |
|   3   |     0 1 1     |            $m_3=\overline{x}_1 x_2 x_3$            |
|   4   |     1 0 0     |      $m_4=x_1 \overline{x}_2 \overline{x}_3$       |
|   5   |     1 0 1     |            $m_5=x_1 \overline{x}_2 x_3$            |
|   6   |     1 1 0     |            $m_6=x_1 x_2 \overline{x}_3$            |
|   7   |     1 1 1     |                 $m_7=x_1 x_2 x_3$                  |

#### 积的和

任何逻辑表达式可以用最小项的和来表示，即 **积的和（sum-of-products）**。如果我们给真值表的每一行编号，并将对应的最小值记为 $m_i$，则逻辑表达式 $f$ 可以用如下形式表示：

$$
f(x_1,x_2,x_3) = \sum (m_i)
$$

或

$$
f(x_1,x_2,x_3) = \sum m(i)
$$

其中，$i$ 是真值表中，$f=1$ 的行。

#### 最大项

根据对偶定理，如果一个逻辑表达式 $f$ 可以用真值表中 $f=1$ 的最小项的和来表示，那么也可以用真值表中 $f=0$ 的最大项的积来表示。最大项即包含所有变量的和的项，记为 $M_i$，见下表：

| 行号  | $x_1,x_2,x_3$ |                       Maxterm                        |
| :---: | :-----------: | :--------------------------------------------------: |
|   0   |     0 0 0     |                 $M_0=x_1+ x_2+ x_3$                  |
|   1   |     0 0 1     |            $M_1=x_1+ x_2+ \overline{x}_3$            |
|   2   |     0 1 0     |            $M_2=x_1+ \overline{x}_2+ x_3$            |
|   3   |     0 1 1     |      $M_3=x_1+ \overline{x}_2+ \overline{x}_3$       |
|   4   |     1 0 0     |            $M_4=\overline{x}_1+ x_2+ x_3$            |
|   5   |     1 0 1     |      $M_5=\overline{x}_1+ x_2+ \overline{x}_3$       |
|   6   |     1 1 0     |      $M_6=\overline{x}_1+ \overline{x}_2+ x_3$       |
|   7   |     1 1 1     | $M_7=\overline{x}_1+ \overline{x}_2+ \overline{x}_3$ |

注意每一行的最大项和最小项中是相反的，比如 $m_0=\overline{x}_1 \overline{x}_2 \overline{x}_3$，$M_0=x_1+ x_2+ x_3$，满足 $m_0 = \overline{M}_0$（德·摩根定律）

#### 和的积

对于一个逻辑表达式 $f=\sum (m_i)$，其中 $m_i$ 为真值表中 $f=1$ 的行，则 $\overline{f}=\sum (m_j)$，$j$ 为真值表中 $f=0$ 的行。于是我们有：

$$
\overline{\overline{f}} = \sum (\overline{m}_j)
$$

根据德·摩根定理，$\overline{m_j} = M_j$，因此，我们有：

$$
f = \overline{\overline{f}} = \prod (M_j)
$$

或

$$
f= \prod M(j)
$$

其中，$j$ 为真值表中 $f=0$ 的行。

#### 用与/或/非表示积的和、和的积

（略）

### 只包含 NAND 或 NOR 的逻辑电路

上面描述了如何用与/或/非门来综合逻辑表达式，实际上，只用与非 NAND ，或只用或非 NOR，也可以实现。

与非、或非相比于与、或的最大好处是，它们的底层晶体管结构更加简单。

下面来描述如何只用与非/或非来实现逻辑表达式。对于一个积的和

$$
f = \sum (m_i)
$$

根据德·摩根定理，有：

$$
f = \overline{\prod (\overline{m}_i)}
$$

那么就可以只用与非来实现。用图可能更好理解：

![logic circuit with nand gate](images/logic%20circuit%20with%20nand%20gate.svg)

同理，对于一个和的积，我们有：

$$
\begin{align}
  f &= \prod (M_i)\\
    &= \overline{\sum (\overline{M}_i)}
\end{align}
$$

![logic circuit with nor gate](images/logic%20circuit%20with%20nor%20gate.svg)

对于某些最小项、最大项，可能需要对变量取反，我们也可以利用 NAND/NOR 来实现 NOT

![use nand nor as not](images/use%20nand%20nor%20as%20not.svg)

## 设计示例

### 双输入选择器

功能：两个输入 $x_1,x_2$，开关 $s$ 控制输出 $f$ 等于哪个输入，即：

$$
f = 
\begin{cases}
  x_1 & s=0\\
  x_2 & s=1
\end{cases}
$$

列出真值表：

| $s,x_1,x_2$ |  $f$  |
| :---------: | :---: |
|    0 0 0    |   0   |
|    0 0 1    |   0   |
|    0 1 0    |   1   |
|    0 1 1    |   1   |
|    1 0 0    |   0   |
|    1 0 1    |   1   |
|    1 1 0    |   0   |
|    1 1 1    |   1   |

写出积的和：

$$
f=\overline{s}x_1\overline{x}_2+\overline{s}x_1x_2+s\overline{x}_1x_2+sx_1x_2
$$

利用分配律，可以写成：

$$
\begin{align}
  f&=\overline{s}x_1(\overline{x}_2+x_2)+sx_2(\overline{x}_1+x_1)\\
  &=\overline{s}x_1+sx_2
\end{align}
$$

可以推广到 $n$ 路选择器：

$$
f = \sum m_{si}x_i
$$

$m_{si}$ 为由选择信号组成的最小项，$x_i$ 为某组选择信号有效时的输出。

## CAD工具简介

上面所列举的逻辑电路比较简单，人手可以简化，但如果系统比较复杂，那么就必须用 CAD（Computer Aid Design），CAD 工具会完成如下几个任务：

- design entry
- logic synthesis & optimization
- simulation
- physical design

```goat
             .-----------------.
             |Design conception|
             '--------+--------'
                      |
+---------------------+
|                     |
|                     v
|    .----------------------------------.
|    |            Design entry          |
|    |  .----------------.     .-----.  |
|    | | Schematic capture|   |Verilog| |
|    |  '------+---------'     '--+--'  |
|    '---------+------------------+-----'
|              |   .---------.    |
^              +-->|Synthesis|<---+
|                  '----+----'
|                       |
|                       v
|            .---------------------.
|            |Functional simulation|
|            '----------+----------' 
|                       |
|                _______v_______
|           No  /               \
+-----<--------+ Design correct? +
|               \_______________/
|                   Yes |
|    +------------------+
|    |                  |
|    |                  v
|    |          .---------------.
|    |          |Physical design|
|    |          '-------+-------'
|    |                  |
|    |                  v
|    |         .-----------------.
^    ^         |Timing simulation|
|    |         '--------+--------'
|    |                  |
|    |       ___________v____________
|    |  No  /                        \
+----+-----+ Timing requirements met? +
            \___________+____________/
                        |
                        v
               .------------------.
               |Chip configuration|
               '------------------'
```

### Design Entry

设计逻辑电路的第一步是决定该电路的功能和结构，这显然是由人来设计的。然后我们需要向 EDA 工具描述这个电路，也就是 design entry.

Design entry 有两种方式：

- Schematic Capture：通过画电路图，如逻辑门等，以图形化的方式描述
  - 优点：易于使用
  - 缺点：难以描述大型电路
- Hardware Description Language：通过硬件描述语言，用代码来描述
  - 优点：可移植（不同工具都能用，无需改代码）、易读

这两种方式可以结合使用，我们可以用 HDL 描述底层逻辑，然后用 Schematic Capture 把各模块连接到一起。

我们所使用的 HDL 是 Verilog HDL

### Logic Synthesis

综合就是由 design entry 设计出逻辑电路。一开始转换得到的逻辑电路并不是最简的，所以 CAD 会自动优化以获得更好的电路。

### Functional Simulation

对逻辑表达式进行模拟以验证功能的正确性。这要求用户指明输入的数据。输出的结果一般是时序图，用户可以检查它是否满足需求。

### Physical Design

从物理上设计实现综合得到的逻辑电路，确定各个功能在芯片上的位置。

### Timing Simulation

用电路实现的逻辑门会存在延迟，输入到输出存在 “propagation delay”（传播延迟）。时序仿真会考虑这些延迟。如果仿真结果不满足需求，我们需要在 physical design 时对 CAD 工具增加时序限制。如果还是不满足，则需要优化综合过程、甚至优化最开始的设计。

### Circuit Implementation

如果一切都满足要求，那么就可以在实际芯片上实现电路（芯片制造或FPGA烧录）

## Verilog 简介

Verilog 中有多种描述目标电路的方式，一种是利用电路元件（比如逻辑门）来描述，称为 **structural representation**，另一种则是通过逻辑表达式来描述，称为 **behavioral representation**

### Structural Specification of Logic Circuits

Verilog 包含一组门级的语句，比如一个双输入（$x_1,x_2$）单输出（$y$）的与门：

```verilog
and(y, x1, x2)
```

四输入的或门：

```verilog
or(y, x1, x2, x3, x4)
```

完整的门级语句见下表：

|   名字   |               描述               |       用法        |
| :------: | :------------------------------: | :---------------: |
|   and    |      $f=(a\cdot b \cdots)$       | `and(f,a,b,...)`  |
|   nand   | $f=\overline{(a\cdot b \cdots)}$ | `nand(f,a,b,...)` |
|    or    |         $f=(a+b+\cdots)$         |  `or(f,a,b,...)`  |
|   nor    |   $f=\overline{(a+b+\cdots)}$    | `nor(f,a,b,...)`  |
|   xor    |   $f=(a\oplus b \oplus\cdots)$   | `xor(f,a,b,...)`  |
|   not    |         $f=\overline{a}$         |    `not(f,a)`     |
|   buf    |              $f=a$               |    `buf(f,a)`     |
| notif0() |   $f=(!e ? \overline{a}:'bz)$    |  `notif0(f,a,e)`  |
| notif1() |    $f=(e ? \overline{a}:'bz)$    |  `notif1(f,a,e)`  |
| bufif0() |         $f=(!e ? a:'bz)$         |  `bufif0(f,a,e)`  |
| bufif1() |         $f=(e ? a:'bz)$          |  `bufif1(f,a,e)`  |

一个逻辑电路以 `module` 的形式定义，先指明端口（输入、输出），然后定义内部的逻辑语句。以下面这个选择器为例：

![2 inputs multiplexer](images/2%20inputs%20multiplexer.svg)

```verilog
module multiplexer(x1, x2, s, f)
  input x1, x2, s;
  output f;

  not(k,s);
  and(g,k,x1);
  and(h,s,x2);
  or(f,g,h);

endmodule
```

我们也可以用 `~x` 来表示取反，因此可以不用 `not`：

```verilog
module multiplexer(x1, x2, s, f)
  input x1, x2, s;
  output f;

  and(g,~s,x1);
  and(h,s,x2);
  or(f,g,h);

endmodule
```

在 Veriog 中，输入、输出等量称为信号（Signal），信号和模块的命名都必须以字母开头，可以包含字母、`_`、`$`。Verilog 是大小写敏感的。

Verilog 的注释用 `//` 来表示。

### Behavioral Specification of Logic Circuits

用门级语句来描述电路太冗长了，我们可以用更简单的方式来描述，比如使用逻辑表达式

$$
f = \overline{s}x_1+ s x_2
$$

```verilog
module multiplexer(x1, x2, s, f)
  input x1, x2, s;
  output f;

  assign f=(~s$x1)|(s&x2);

endmodule
```

在 Verilog 中，与、或分别是 `&` 和 `|`，`assign f=` 表示连续赋值（continuous assignment），即 `=` 右边的信号变化时，会重新计算 `=` 左边的信号。

这种方式能简化代码，但有一种更高级的抽象方法，即使用 `if-else` 语句：

```verilog
if(s==0)
  f=x1;
else
  f=x2;
```

这种方式更加直观地说明了电路的功能。`if-else` 属于 Verilog 中的流程语句（procedural statement），流程语句必须包裹在 `always` 块中，因此完整的模块应该是：

```verilog
// Behavioral specification
module multiplexer(x1, x2, s, f)
  input x1, x2, s;
  output f;
  ref f;
  always@(x1 or x2 or s)
    if(s==0)
      f=x1;
    else
      f=x2;

endmodule
```

always 块中可以有多条语句，一个模块中也可以有多个 always 块。always 块内的语句是按顺序执行的，相比之下，always块外部的连续赋值语句是并行执行的。

always @ 后面括号内的是 敏感表（sensitivity list），当敏感表内的信号变化时，always 内的语句才会执行。这样做的好处是在仿真的时候，就不需要每次都执行所有语句了。

如果在流程语句中对信号赋值，则 Verilog 要求这个信号被声明为一个变量，用 `reg` 来声明。reg 表示 register（寄存器），赋值结束后，在下一次 always 语句执行前，reg 会一直存储这个值。reg 可以和 output 一起使用，比如：

```verilog
output reg f;
```

另外，在 Verilog 2001 标准中，信号的方向和类型可以写在括号内。另外，敏感表内也可以用 `,` 代替 `or`：

```verilog
// Behavioral specification
module multiplexer(input x1, x2, s, output reg f)

  always@(x1, x2, s)
    if(s==0)
      f=x1;
    else
      f=x2;

endmodule
```

还有，我们可以不用在敏感表中列出所有信号，而是用 `always@(*)` 或 `always@*` 让编译器自己找出哪些变量需要考虑。

### Verilog 的层级

对于大型的电路，可以划分为不同层级的模块，高层级的模块调用低层级的模块。比如下图包含两个底层模块，一个负责运算（将输入相加）另一个负责显示

![A logic circuit with two modules](images/A%20logic%20circuit%20with%20two%20modules.jpg)

```verilog
// An adder module
module adder (a, b, s1, s0);
  input a, b;
  output s1, s0;

  assign s1 = a & b;
  assign s0 = a ^ b;

endmodule
```

```verilog
// A module for driving a 7-segment display
module display (s1, s0, a, b, c, d, e, f, g);
  input s1, s0;
  output a, b, c, d, e, f, g;

  assign a = ~s0;
  assign b = 1;
  assign c = ~s1;
  assign d = ~s0;
  assign e = ~s0;
  assign f = ~s1 & s0;
  assign g = s1 & s0;

endmodule
```

```verilog
module adder_display (x, y, a, b, c, d, e, f, g);
  input x, y;
  output a, b, c, d, e, f, g;
  wire w1, w0;

  adder U1 (x, y, w1, w0);
  display U2 (w1, w0, a, b, c, d, e, f, g);
  
endmodule
```

在顶层模块中 `adder_display` 中，`w1, w0` 不是输入输出，而是中间的连线，所以声明为 `wire`。顶层模块内部实例化了一个 `adder` 和 `display`，分别命名为 `U1, U2`，并按照底层模块定义时的顺序在括号中传递信号。
