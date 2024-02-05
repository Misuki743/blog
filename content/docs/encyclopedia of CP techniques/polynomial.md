---
title: "Polynomial"
description: ""
summary: ""
date: 2024-02-06T01:31:04+08:00
lastmod: 2024-02-06T01:31:04+08:00
draft: false
menu:
  docs:
    parent: ""
    identifier: "polynomial-3e933f3d2a93d8d94f3ee877fe9386e6"
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

## product of polynomials

[problem link](https://judge.yosupo.jp/problem/product_of_polynomial_sequence)

using divide and conquer to calculate it, i.e. split sequence of polynomial into two part, and convolve the result of them using FFT/NTT.

## operations on sparse polynomial

### inverse

[problem link](https://judge.yosupo.jp/problem/inv_of_formal_power_series_sparse)

start from the definition of inverse, we have

```math {.text-center}
$$\eqalign{
&f(x)g(x) \equiv 1 \text{ }(\bmod x^N) \\
\\
\leftrightarrow &\sum\limits_{j = 0}^{k} f_{k - j}g_j =
\begin{cases}
1 & \text{if k = 0} \\
0 & \text{if k > 0}
\end{cases}
}$$
```

so for {{< math >}}$k = 0${{< /math >}}, it is just {{< math >}}$g_0 = \frac{1}{f_0}${{< /math >}}, for the rest, we have
```math {.text-center}
$$\eqalign{
&\sum\limits_{j = 0}^{k - 1} f_{k - j}g_j + f_0 g_k = 0 \\
\\
\leftrightarrow &g_k = \frac{-\sum\limits_{j = 0}^{k - 1}f_{k - j}g_j}{f_0}
}
```

so it can be calculated in {{< math >}}$O(N \cdot \deg(f))${{< /math >}} in increasing order of {{< math >}}$k${{< /math >}}
