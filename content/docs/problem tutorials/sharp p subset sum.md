---
title: "Sharp P Subset Sum"
description: ""
summary: ""
date: 2024-02-06T21:52:02+08:00
lastmod: 2024-02-06T21:52:02+08:00
draft: false
menu:
  docs:
    parent: ""
    identifier: "sharp p subset sum-158bd720e1f57d0be2e75711cc46b131"
weight: 999
toc: true
seo:
  title: "" # custom title (optional)
  description: "" # custom description (recommended)
  canonical: "" # custom canonical URL (optional)
  noindex: false # false (default) or true
---

[problem link](https://judge.yosupo.jp/problem/sharp_p_subset_sum)

informal problem statement:
> Given {{< math >}}$N${{< /math >}} items with positive integer weight {{< math >}}$s_1, s_2, \dots, s_N${{< /math >}}, find {{< math >}}$\#${{< /math >}}ways to pick some subset of items with weight sums to {{< math >}}$k${{< /math >}} for all {{< math >}}$1 \le k \le T${{< /math >}}

write the problem into polynomial form, then the answer for some {{< math >}}$k${{< /math >}} is
```math {.text-center}
$$
[x^k]\prod\limits_{i = 1}^N (1 + x^{s_i})
$$
```

calculate this naively using DP would takes {{< math >}}$O(NT)${{< /math >}}, but we can do better.

let {{< math >}}$freq_i${{< /math >}} denote the frequency of {{< math >}}$i${{< /math >}} in sequence {{< math >}}$s_N${{< /math >}}, then we can rewrite the polynomial into this
```math {.text-center}
$$
\prod\limits_{i = 1}^T (1 + x^i)^{freq_i}
$$
```

consider using log-exp trick{{< math >}}$(f(x)g(x) = exp(log(f(x)) + log(g(x))))${{< /math >}}, we have
```math {.text-center}
$$\eqalign{
&\prod\limits_{i = 1}^T (1 + x^i)^{freq_i}\\
\\
= exp(&\sum\limits_{i = 1}^T freq_i \cdot log(1 + x^i))
}$$
```

Since {{< math >}}$log(f(x))' = \frac{f(x)'}{f(x)}${{< /math >}}, we have
```math {.text-center}
$$\eqalign{
log(1 + x^i)' = &\frac{ix^{i - 1}}{1 + x^i} \\
\\
= &\sum\limits_{j = 1}^{\infty}(-1)^{j + 1}ix^{ji - 1}= ix^{i - 1} -ix^{2i - 1}+ix^{3i-1}\dots \\
\\
\leftrightarrow log(1 + x^i) = &\sum\limits_{j = 1}^{\infty}(-1)^{j + 1}\frac{x^{ij}}{j} = \frac{x^i}{1} - \frac{x^{2i}}{2} + \frac{x^{3i}}{3}\dots
}$$
```

Since {{< math >}}$log(1 + x^i)${{< /math >}} would have {{< math >}}$O(\frac{T}{i})${{< /math >}} terms, expanding it for all {{< math >}}$i${{< /math >}} would take {{< math >}}$O(\sum\limits_{i = 1}^T\frac{T}{i}) = O(TlgT)${{< /math >}} times, then we just sum over them and take exp([this problem](https://judge.yosupo.jp/problem/exp_of_formal_power_series)), which also takes {{< math >}}$O(TlgT)${{< /math >}}, and the whole time complexity is {{< math >}}$O(TlgT)${{< /math >}}.


sidenote: sorry for the ugly layout, I'm not sure how to align them beautifully :(
