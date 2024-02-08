---
title: "ARC147E Examination"
description: ""
summary: ""
date: 2024-02-09T02:55:26+08:00
lastmod: 2024-02-09T02:55:26+08:00
draft: false
menu:
  docs:
    parent: ""
    identifier: "ARC147E Examination-8e7daa5c4ab00820c73f5236dc8d3160"
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

[Problem Link](https://atcoder.jp/contests/arc147/tasks/arc147_e)

informal statement:
> Given integer sequence {{< math >}}$A_N, B_N${{< /math >}}, find maximum {{< math >}}$\#${{< /math >}}fix points we can have when permuting {{< math >}}$A_N${{< /math >}}, and after permuting, {{< math >}}$A_i \geq B_i\text{ }\forall i${{< /math >}} should be satisfied. ({{< math >}}$N \leq 3 \times 10^5${{< /math >}})

First, sort sequence by {{< math >}}$B_i${{< /math >}} since order doesn't matters, then consider fix some set of points that would move, then it's obvious the optimal choice is to sort these points by {{< math >}}$A_i${{< /math >}}(you can prove it by exchange argument), and if we choose all elements and still can't satisfied the condition, there is no solution, otherwise there always exist one.

Second, notice for any {{< math >}}$A_i${{< /math >}}, it can only "match" some prefix of {{< math >}}$B_i${{< /math >}}, it sound like some balance bracket sequence, doesn't it?

So let's think {{< math >}}$A_i${{< /math >}} as right bracket located at position {{< math >}}$A_i + 0.5${{< /math >}}, and {{< math >}}$B_i${{< /math >}} as left bracket sequence located at {{< math >}}$B_i${{< /math >}}, and we want to shuffle min {{< math >}}$\#${{< /math >}}pair of brackets to make it a balanced bracket sequence.

Then just consider go through these brackets from left to right, and when reach a right bracket sequence from a "bad" pair(i.e. {{< math >}}$A_i < B_i${{< /math >}}), we need to match it with some left bracket before it from a "good" pair, and then the right bracket from that "good" pair would become unmatched, and it's obvious that we want to choose the "good" pair of bracket which have the rightmost right bracket, then we are done.

The implementation is a bit tricky though.

## The key

In this problem, the important part is to notice there are two kind of elements, and one kind of element can match prefix of another kind, which is just what RBS does, and rephrase the problem at the perspective of bracket sequence, then everything become trivial.

## Some problem related to the key

[CF GR12 pE - Bomb](https://codeforces.com/contest/1326/problem/E)

[SEERC 2022 pJ - Joyful Death](https://codeforces.com/gym/104114/problem/J)
