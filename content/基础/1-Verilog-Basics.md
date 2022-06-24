---
title: 简介
weight: 5
date: 2022-06-24 17:25:09 +0800
---

<iframe width="560" height="315" src="https://www.youtube.com/embed/vVYOV9MP5BA" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Verilog 是一种硬件描述语言，可以用来描述数字电路的行为。比如下面这个 Flip-Flop，在时钟的上升沿处，其输出 Q 会等于输入 D。用 Verilog 描述为：

```verilog
always@(posedge clock)
  Q<=D;
```