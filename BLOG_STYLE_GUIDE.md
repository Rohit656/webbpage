# Blog post style guide

Instructions for writing and formatting posts for Rohit Chatterjee's blog at
`https://rohit656.github.io/webbpage/blog/`. Follow every rule in this file exactly. The rules
reflect how this site is built (GitHub Pages, Jekyll 3.10, kramdown in GFM mode, KaTeX for math),
and several of them exist because the alternatives silently produce broken output.

A complete working example of every feature below is in `_drafts/example-post.md`. When unsure,
copy the pattern from there.

---

## 1. Deliverables

For each post, produce:

1. **One Markdown file** named `YYYY-MM-DD-slug.md`.
   - `YYYY-MM-DD` is the publication date. It becomes the post's date on the site.
   - `slug` is lowercase words joined by hyphens, ASCII only, ideally 3–6 words
     (e.g. `2026-11-03-minrank-public-key-encryption.md`). It becomes the URL:
     `/webbpage/blog/2026/11/minrank-public-key-encryption/`.
   - Never change the filename of a published post: the URL would change and old links would break.
2. **Any figures** for the post, placed in `assets/blog/<slug>/` (same slug as the filename,
   without the date).

Where the file goes:

- `_drafts/<slug>.md` (no date in the name) for a draft. Drafts are **not** published.
- `_posts/YYYY-MM-DD-slug.md` to publish. Pushing a file here makes it live within a minute or two.

Unless told otherwise, deliver posts as drafts.

---

## 2. Front matter

Every post starts with exactly this block, with nothing before it (no blank line, no BOM):

```yaml
---
title: "Public-key encryption from MinRank"
description: "One sentence, under ~160 characters, shown on the blog index, in search results and in RSS."
---
```

- `title` — Title Case or sentence case, but be consistent within a post. Quote it if it contains
  a colon, `#`, or starts with a special character. **No math in the title**: titles are shown in
  browser tabs and RSS readers, which do not render math. Write "LWE" or "MinRank", not `$$\mathbb{Z}_q$$`.
- `description` — plain text, no Markdown, no math. Required.
- Do **not** add `layout:`; it is set automatically.
- Optional: `date: 2026-11-03 09:00:00 +0800` only if a specific time of day matters. Otherwise omit.

The title is rendered by the page template. **Do not repeat the title as a heading in the body.**

---

## 3. Structure and headings

- Start with one or two plain paragraphs that say what the post is about. No heading before them.
- Use `##` for sections and `###` for subsections. **Never use `#`** (that level is the site name)
  and avoid `####` or deeper.
- Headings are sentence case: `## Why the reduction fails`, not `## Why The Reduction Fails`.
- No math in headings; describe it in words instead.
- Do not number headings manually ("1. Introduction"). If the post has numbered results, number
  the results (Theorem 1, Lemma 2), not the sections.
- Leave one blank line before and after every heading, paragraph, list, block quote, table,
  code block and display equation.

---

## 4. Math

Math is written in LaTeX and rendered by KaTeX. KaTeX supports most of standard LaTeX math, but
**not** packages, `\usepackage`, `\label`/`\ref`, `\eqref`, `\cite`, or text-mode commands like
`\textbf` outside math. Check unusual commands against <https://katex.org/docs/supported.html>.

### 4.1 The one delimiter: `$$ ... $$`

Both inline and display math use **double dollar signs**. The position decides which one you get.

**Never use single dollar signs** (`$x$`). They are not converted and appear as literal `$x$`
text. Also never use `\( \)`, `\[ \]`, or `\begin{equation}` directly in Markdown.

### 4.2 Inline math

Put `$$...$$` inside a line of text:

```markdown
Every $$\PPT$$ adversary $$\Adv$$ has advantage at most $$\negl(\secpar)$$.
```

- An inline formula may start a paragraph, as long as text follows it on the same line.
- Keep inline math short. If a formula is longer than about a third of a line or contains a
  fraction with large parts, sums with limits, or matrices, make it a display equation.
- Punctuation that ends a sentence goes **outside** inline math: `$$x$$.` not `$$x.$$`.
- Use `\tfrac` instead of `\frac` inline, so the line height stays even.

### 4.3 Display math

Put `$$` on its own line, the formula on the following lines, and `$$` on its own closing line,
with a **blank line before and after** the whole block:

```markdown
The LWE assumption states that

$$
(\mathbf{A}, \mathbf{A}\mathbf{s} + \mathbf{e}) \approx_c (\mathbf{A}, \mathbf{u}).
$$

Here $$\mathbf{A} \getsr \Z_q^{m \times n}$$.
```

- **The blank lines are mandatory.** Without them, the block silently turns into inline math
  inside the surrounding paragraph.
- A paragraph consisting only of `$$...$$` on a single line also becomes display math. If you
  want display math, use the multi-line form above, so the intent is obvious.
- Punctuation that ends the sentence goes **inside** display math, as in the example (`.)`).
- For multi-line derivations, use `aligned` (not `align`, `eqnarray`, or `align*`):

  ```markdown
  $$
  \begin{aligned}
  \Pr[\mathsf{Hyb}_0 = 1] &= \Pr[\mathsf{Hyb}_1 = 1] \pm \negl(\secpar) \\
  &\le \tfrac{1}{2} + \negl(\secpar).
  \end{aligned}
  $$
  ```

- Case analyses use `cases`; matrices use `pmatrix` or `bmatrix`.
- There are no equation numbers. To refer back to an equation, name it in the text ("the
  hybrid bound above") or put a tag on it with `\tag{1}` and refer to "(1)".

### 4.4 Characters that break inline math

The Markdown converter reads inline math **before** KaTeX does, and two character sequences in
inline math are misread:

- **Absolute values:** not `|x|` but `\abs{x}` or `\lvert x \rvert`.
- **Conditioning / "such that":** not `A | B` but `A \mid B`.
- **Norms:** not `||v||` or `\|v\|` but `\norm{v}` or `\lVert v \rVert`.

A vertical bar in inline math can make the converter treat the whole line as a table row,
which destroys the paragraph. Separately, the sequences `{{` and `{%` must not appear anywhere
in the file (text, math or code) except in the figure snippet of section 8: the site's template
engine treats them as commands. Write `{ {` with a space instead (e.g. `\mathbb{Z}_{ {q} }`,
though usually the extra braces can simply be dropped).

The `|` character is safe inside **display** math blocks, but use `\mid`, `\abs`, `\norm` there
too for consistency.

Underscores, asterisks, `<` and `>` are safe inside math. A literal dollar sign in text is
written `\$` (e.g. `costs \$5`).

### 4.5 Macros available in every post

These macros are defined site-wide (in `_includes/head.html`). Use them for consistency:

| Macro | Renders as | Macro | Renders as |
|---|---|---|---|
| `\secpar` | λ (security parameter) | `\getsr` | ← with \$ above (uniform sampling) |
| `\negl` | negl | `\poly` | poly |
| `\PPT` | PPT | `\Adv` | 𝒜 (adversary) |
| `\Sim` | Sim | `\zo` | {0,1} |
| `\Gen`, `\Enc`, `\Dec` | Gen, Enc, Dec | `\Sign`, `\Vrfy` | Sign, Vrfy |
| `\N`, `\Z`, `\F`, `\R` | ℕ, ℤ, 𝔽, ℝ | `\E` | 𝔼 (expectation, takes subscripts) |
| `\abs{x}` | \|x\| | `\norm{x}` | ‖x‖ |

Conventions:

- Algorithm and experiment names: `\mathsf{...}` (`\mathsf{Hyb}_1`, `\mathsf{Exp}^{\mathsf{cpa}}`).
- Vectors and matrices: bold, `\mathbf{s}`, `\mathbf{A}`.
- Sampling: `x \getsr S` for uniform; `x \leftarrow D` for a distribution `D`.
- Computational indistinguishability: `\approx_c`; statistical: `\approx_s`.
- Need another macro used throughout one post? Define it once with `\gdef` in the post's first
  display equation, e.g. `\gdef\Kg{\mathsf{KeyGen}}`. To make it available site-wide, add it to
  `window.katexMacros` in `_includes/head.html`.

---

## 5. Inline text formatting

- **Bold** (`**text**`) only for result labels ("**Theorem 1.**") and, rarely, a key term at
  its definition. Do not bold for general emphasis.
- *Italics* (`*text*`) for emphasis and for introducing a term: "a *one-way function* is …".
- `Code` (backticks) for code, file names, commands and identifiers only. Never for math:
  write `$$\mathsf{Enc}$$`, not `` `Enc` ``.
- Links: `[descriptive text](https://...)`. Never "click here". Prefer stable URLs (ePrint,
  arXiv abstract pages, DOIs) over PDF links where possible.
- Spell out abbreviations on first use, e.g. "learning with errors (LWE)".
- Use an en dash for ranges ("pages 3–7") and an em dash without spaces for asides
  ("the result—surprisingly—holds"). Use typographic quotes or plain `"` consistently.
- Write one paragraph per line, or wrap lines anywhere; both render the same. Never insert a
  blank line inside a paragraph.

---

## 6. Theorems, definitions, proofs

Theorem-like statements are block quotes with a style class on the line directly after them:

```markdown
> **Theorem 1 (Name, optional).** Statement, with math like $$f : \zo^n \to \zo^m$$.
{: .theorem}

> **Definition 2 (One-way function).** A function $$f$$ is one-way if …
{: .definition}

> **Remark.** An aside.
{: .remark}
```

- Classes: `.theorem` (blue; for theorems, lemmas, corollaries, claims, propositions),
  `.definition` (green; definitions, assumptions, constructions), `.remark` (grey; remarks,
  notes, examples).
- Every line of the statement starts with `> `, including lines inside a display equation
  and the blank lines between paragraphs (`>` alone).
- The `{: .class}` line must come **immediately** after the last `>` line, with no blank line.
- Numbering is manual and shared across all kinds: Definition 1, Theorem 2, Lemma 3 ….
- Proofs are **not** boxed. Start with `*Proof.*` (or `*Proof sketch.*`) as a normal paragraph
  and end with `$$\blacksquare$$` at the end of the last proof paragraph.

---

## 7. Lists

- Use `-` for bullets and `1.` for numbered lists. Leave a blank line before and after the list.
- A display equation inside a list item must be indented by **three spaces** (to line up with the
  item text) with blank lines around it:

  ```markdown
  1. Sample $$x \getsr \zo^\secpar$$.
  2. Compute

     $$
     y = f(x) \oplus h(r)
     $$

  3. Output $$y$$.
  ```

- Protocol and algorithm descriptions: a numbered list, one step per item, with the party in
  bold at the start if there are several parties ("**Sender:** sends …").

---

## 8. Figures

Put the image file in `assets/blog/<slug>/` and insert it with this exact HTML block,
surrounded by blank lines:

```html
<figure>
  <img src="{{ '/assets/blog/<slug>/<file>' | relative_url }}" alt="What the figure shows, in words" style="max-width: 70%">
  <figcaption>Figure 1. Caption sentence.</figcaption>
</figure>
```

- This is the one place where `{{ ... }}` is required (it fixes the path for the site's
  `/webbpage` prefix). Copy it exactly; only change `<slug>`, `<file>`, the alt text, the width
  and the caption.
- Never use a bare `/assets/...` path or a plain Markdown image `![...](...)` with an absolute
  path; it will 404 on the live site.
- Always write meaningful `alt` text.
- `max-width`: 100% for wide diagrams, 50–70% for most figures, 30–40% for small ones.
- Formats: SVG for diagrams (preferred), PNG for screenshots and plots, JPEG for photos.
  Keep each file under ~500 KB; resize photos to at most 1600 px wide.
- Lowercase, hyphenated file names: `ind-cpa-game.svg`.
- Captions start with "Figure N." and are plain text. Math in captions is allowed only as `\( ... \)`
  (inside the HTML block `$$` is not converted).
- Number figures in order and refer to them in the text as "Figure 1".

---

## 9. Tables

Use Markdown tables followed by the Bootstrap class line:

```markdown
| Assumption | Post-quantum? | Reference |
|---|:---:|---|
| LWE | yes | [Reg05](https://...) |
| DDH | no | — |
{: .table .table-sm}
```

- `{: .table .table-sm}` goes on the line directly after the last row.
- `:---:` centres a column, `---:` right-aligns it.
- **Never put `|` inside a table cell**, including inside math (use `\mid`, `\abs`, `\norm`).
- Keep tables narrow (≤ 5 columns); they do not scroll on phones.

---

## 10. Code

Use fenced code blocks with a language name, so they get syntax highlighting:

````markdown
```python
def keygen(n, q):
    return sample_matrix(n, q)
```
````

Common names: `python` (also use this for SageMath), `c`, `rust`, `bash`, `text`. Pseudocode goes in a numbered list (section 7), not a code block, so it can contain math.

---

## 11. Footnotes and references

- Footnotes: `text.[^short-name]`, and the definition `[^short-name]: Footnote text.` at the end
  of the file. They are numbered automatically. Use them sparingly, for asides.
- Citations are links in the text: `[CMV25](https://eprint.iacr.org/...)` or "as shown by
  [Regev](https://...)". Use alphabetic-style keys (authors' initials + two-digit year).
- If a post cites more than about five works, end with a `## References` section: a bullet list,
  one entry per work, formatted as
  `- [Key] Authors. *Title*. Venue Year. [link](https://...)`.

---

## 12. Things never to do

- Single-dollar math `$...$`, `\(...\)` or `\[...\]` (except `\(...\)` in figure captions), or LaTeX environments outside `$$`.
- `|` in inline math or in table cells.
- `{{` or `{%` anywhere except the figure snippet.
- Raw HTML other than the `<figure>` block.
- `#` headings, or repeating the title as a heading.
- Math, Markdown, or emoji in `title` and `description`.
- Absolute image paths, images hosted elsewhere (hotlinking), or files outside `assets/blog/<slug>/`.
- Editing files outside the post's own `.md` file and its `assets/blog/<slug>/` folder
  (layouts, `_config.yml`, `index.html`) unless explicitly asked.

---

## 13. Final checklist

Before handing a post over, check:

- [ ] Filename is `YYYY-MM-DD-slug.md` (or `_drafts/slug.md`), lowercase, hyphenated.
- [ ] Front matter has `title` and `description`, plain text, nothing before the opening `---`.
- [ ] No `#` heading; sections are `##`, subsections `###`.
- [ ] Every display `$$` block has a blank line before and after, and `$$` on its own lines.
- [ ] No single `$` delimiters; literal dollar signs are `\$`.
- [ ] No `|` in inline math or tables; no `{{` / `{%` outside the figure snippet.
- [ ] Theorem-like blocks have every line prefixed with `>` and the `{: .class}` line right after.
- [ ] Figures use the `<figure>` snippet with `relative_url`, alt text and a numbered caption,
      and the files exist in `assets/blog/<slug>/`.
- [ ] All links work and use stable URLs.

To preview locally before publishing (from the repository folder):

```bash
bundle exec jekyll serve --drafts
```

Then open `http://localhost:4000/webbpage/blog/`.
