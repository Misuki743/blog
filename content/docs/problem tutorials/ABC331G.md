---
title: "ABC331G"
description: ""
summary: ""
date: 2024-02-05T00:06:03+08:00
lastmod: 2024-02-05T00:06:03+08:00
draft: false
menu:
  docs:
    parent: ""
    identifier: "ABC331G-4254e79a4c5bcb4cbab6a2866730e016"
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

[problem link](https://atcoder.jp/contests/abc331/tasks/abc331_g)

### intro

A standard problem on {{< math >}}$EV${{< /math >}} where you need to pick item randomly and put it back until you've got all of them, and you need to calculate {{< math >}}$E[\#\text{ operations done}]${{< /math >}}.

### prerequisite

- linearity of expection
- principle of inclusion-exclusion
- NTT
- basic understanding on how to use OGF to count things

### solution

Consider the process as a walk on absorbing markov chain with {{< math >}}$2^M${{< /math >}} vertices indexed with all possible set of cards we could have, then the thing we want to count is {{< math >}}$E[\text{start from vertex }\varnothing\text{, }\#\text{operation needed to reach vertex } T = [1..M]]${{< /math >}}.

For every walk {{< math >}}$\varnothing \rightarrow u \rightarrow v \rightarrow w \rightarrow \dots \rightarrow T${{< /math >}}, the length of walk can be decompose into sum over ({{< math >}}$\#${{< /math >}} edges on the walk going to {{< math >}}$v${{< /math >}}) for all {{< math >}}$v${{< /math >}}, thus by linearity of expectation, we have

```math {.text-center}
$$\eqalign{
&E[\text{start from vertex }\varnothing\text{, }\#\text{operations needed to reach vertex } T = [1..M]] \\
\\
=&\sum\limits_{S \subset [1..M]}E[\#\text{operations done when we are at vertex }S]
}$$
```

sidenote: Actually this kind of way using linearity to decompose expected value of a problem into sum of expected value of smaller problems are quite prevalent in CP, it is indeed a strong weapon to deal with {{< math >}}$EV${{< /math >}} problems.

but it seems easier to deal {{< math >}}$E[\#\text{operations done when we are at vertices }\subseteq S]${{< /math >}} rather than the above one, so let's consider using PIE(principle of inclusion-exclusion), then we have

```math {.text-center}
$$\eqalign{
&\sum\limits_{S \subset [1..M]}E[\#\text{operations done when we are at vertex }S]
\\
=&\sum\limits_{S \subset [1..M]} (-1)^{M - |S| - 1}\text{ }E[\#\text{operations done when we are at vertices }\subseteq S]
}$$
```

let {{< math >}}$f(S)${{< /math >}} denote {{< math >}}$\#${{< /math >}} cards belong to {{< math >}}$S${{< /math >}}, then it is quite obvious that we can calculate {{< math >}}$Pr[(\#\text{operations done when we are at vertices }\subseteq S) > k]${{< /math >}} for some fixed {{< math >}}$k${{< /math >}}, so let's consider applying another well-known trick ({{< math >}}$E[X] = \sum\limits_{k = 0}^{\infty}Pr[X > k]${{< /math >}}) to decompose {{< math >}}$EV${{< /math >}} further.

```math {.text-center}
$$\eqalign{
&\sum\limits_{S \subset [1..M]} (-1)^{M - |S| - 1}\text{ }E[\#\text{operations done when we are at vertices }\subseteq S]
\\
\\
=&\sum\limits_{S \subset [1..M]} (-1)^{M - |S| - 1}\text{ }\sum\limits_{k = 0}^{\infty} Pr[(\#\text{operations done when we are at vertices }\subseteq S) > k]
\\
\\
=&\sum\limits_{S \subset [1..M]} (-1)^{M - |S| - 1}\text{ }\sum\limits_{k = 0}^{\infty} (\frac{f(S)}{N})^{k}
\\
\\
=&\sum\limits_{S \subset [1..M]} (-1)^{M - |S| - 1}\text{ }\frac{1}{1 - \frac{f(S)}{N}} \qquad (\text{by the sum of geometric progression})
}$$
```

finally, observe that {{< math >}}$f(S)${{< /math >}} can have value only in {{< math >}}$[1..M]${{< /math >}}, let consider group {{< math >}}$S${{< /math >}} with same {{< math >}}$f(S)${{< /math >}} and calculate them at once, also only the parity of {{< math >}}$M - |S|${{< /math >}} is important, we can group {{< math >}}$S${{< /math >}} with same ({{< math >}}$M - |S| \bmod 2${{< /math >}}) together, and we have

```math {.text-center}
$$\eqalign{
&\sum\limits_{S \subset [1..M]} (-1)^{M - |S| - 1}\text{ }\frac{1}{1 - \frac{f(S)}{N}}
\\
\\
=&\sum\limits_{k = 0}^{N - 1}\frac{1}{1 - \frac{k}{N}}((\# S \text{ s.t. }(M - |S| \bmod 2 = 1)\text{ and } f(S) = k) - (\# S \text{ s.t. }(M - |S| \bmod 2 = 0)\text{ and } f(S) = k))
}$$
```

and the problem boiled down to the following well-known problem

> Given M items each have some non-negative integer weight, and the sum of weight is N, for each k in [0..N-1], count # ways to pick some subset of items have sum of weight equal to k

Assume the weight of items are {{< math >}}$w_1, w_2, \dots, w_M${{< /math >}}, consider writing it in polynomial form, we can know that the answer for any {{< math >}}$k${{< /math >}} is

```math {.text-center}
$$
[x^k]\prod\limits_{i = 1}^{M}(1 + x^{w_i})
$$
```

and the above polynomial can be calculate using D&C + NTT in {{< math >}}$O(NlgNlgM)${{< /math >}}, to deal with parity of {{< math >}}$|S|${{< /math >}}, just negate {{< math >}}$x^{w_i}${{< /math >}} and multiply the final polynomial with {{< math >}}$(-1)${{< /math >}} when {{< math >}}$M${{< /math >}} is even, then we are done.

(if you are not familier with it, see this: [library checker - Product of Polynomial Sequence](https://judge.yosupo.jp/problem/product_of_polynomial_sequence))

sidenote: actually this problem can be solved in {{< math >}}$O(NlgN)${{< /math >}} using log-exp trick. ([detail description](https://misuki743.github.io/blog/docs/problem-tutorials/sharp-p-subset-sum/))
