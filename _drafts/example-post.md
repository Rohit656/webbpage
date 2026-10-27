---
title: An example post
description: Shows every formatting feature from BLOG_STYLE_GUIDE.md. Drafts are never published.
---

This draft shows each formatting rule in `BLOG_STYLE_GUIDE.md`. Files in `_drafts/` are
never published; to publish, move a file into `_posts/` as `YYYY-MM-DD-slug.md`.

## Inline math

A scheme is secure if every $$\PPT$$ adversary $$\Adv$$ has advantage
$$\Pr[\Adv(pk) = b] - \tfrac{1}{2} \le \negl(\secpar)$$, where $$x_1 * y_2$$ and *real emphasis*
can share a line. Absolute values $$\abs{x}$$, norms $$\norm{\mathbf{e}}_\infty$$, conditioning $$\Pr[A \mid B]$$, sets
$$\{x \in \zo^n : x_1 = 0\}$$, and comparisons $$a < b > c$$ all work. A plain dollar sign is
written \$5.

$$n$$ parties: a paragraph may start with inline math as long as text follows on the same line.

## Display math

$$
\mathbf{A} \getsr \Z_q^{m \times n}, \quad \mathbf{s} \getsr \Z_q^n, \quad
(\mathbf{A}, \mathbf{A}\mathbf{s} + \mathbf{e}) \approx_c (\mathbf{A}, \mathbf{u})
$$

A macro used throughout one post can be defined once with `\gdef`:

$$
\gdef\Hyb{\mathsf{Hyb}}
\Hyb_0 \approx_c \Hyb_1 \tag{1}
$$

Multi-line derivations use `aligned`, and the `\Hyb` macro defined above still works:

$$
\begin{aligned}
\Pr[\Hyb_0 = 1] &= \Pr[\Hyb_1 = 1] \pm \negl(\secpar) \\
&\le \tfrac{1}{2} + \negl(\secpar).
\end{aligned}
$$

## Theorems and definitions

> **Definition 1 (One-way function).** A function $$f : \zo^* \to \zo^*$$ is one-way if for
> every $$\PPT$$ $$\Adv$$, $$\Pr_{x \getsr \zo^\secpar}[f(\Adv(f(x))) = f(x)] \le \negl(\secpar)$$.
{: .definition}

> **Theorem 2.** If one-way functions exist, then so do pseudorandom generators: for every
> $$\ell = \poly(\secpar)$$ there is $$G : \zo^\secpar \to \zo^{\ell}$$ with
>
> $$
> G(U_\secpar) \approx_c U_\ell.
> $$
{: .theorem}

*Proof sketch.* Apply Goldreich–Levin, then stretch. $$\blacksquare$$

> **Remark.** Remarks and asides use the plain grey box.
{: .remark}

## Lists

1. Sample $$x \getsr \zo^\secpar$$.
2. Compute

   $$
   y = f(x) \oplus h(r)
   $$

3. Output $$y$$.

## Figure

<figure>
  <img src="{{ '/assets/blog/example-post/diagram.svg' | relative_url }}" alt="Challenger sends pk to the adversary, which returns a guess" style="max-width: 70%">
  <figcaption>Figure 1. The IND-CPA game for \(\Enc\).</figcaption>
</figure>

## Table

| Assumption | Post-quantum? | Reference |
|---|:---:|---|
| LWE | yes | [Reg05](https://arxiv.org/abs/2401.03703) |
| DDH | no | — |
{: .table .table-sm}

## Code

```python
def keygen(n, q):
    return sample_matrix(n, q)
```

## Footnotes

This sentence has a footnote.[^lwe]

[^lwe]: Footnotes go at the end of the post, and can contain math like $$q = \poly(n)$$.
