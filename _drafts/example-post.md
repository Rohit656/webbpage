---
title: An example post
description: A template showing headings, math, and code. Move to _posts/ to publish.
---

This is a draft. Files in `_drafts/` are not published. To publish a post, put it in
`_posts/` with a name like `2026-10-27-my-title.md`; the date in the filename is the
post date and the rest becomes the URL slug.

## Math

Write math between double dollar signs. Inline: $$\mathsf{Gen}(1^\lambda) \to (pk, sk)$$
with $$\Pr[\mathcal{A} \text{ wins}] \le \tfrac{1}{2} + \mathsf{negl}(\lambda)$$.

A display equation goes on its own paragraph:

$$
\mathbf{A}\mathbf{s} + \mathbf{e} \approx_c \mathbf{u}, \qquad \mathbf{A} \leftarrow \mathbb{Z}_q^{m \times n}
$$

## Code

```python
def keygen(n, q):
    return sample_matrix(n, q)
```

## Links and images

Link to a [paper](https://eprint.iacr.org/2025/250.pdf). Images go in a folder like
`assets/` and are referenced as `{% raw %}![alt text]({{ '/assets/figure.png' | relative_url }}){% endraw %}`.
