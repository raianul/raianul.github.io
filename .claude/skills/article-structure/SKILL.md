---
name: article-structure
description: >
  Use this skill when writing or editing articles for raianul.github.io to understand
  how code blocks, diagrams, and includes work. Covers the _includes/ file system,
  how Bangla and English articles share the same includes, code-N.html format,
  diagram-N.html (Mermaid) format, and the in-article include tag pattern.
  Triggers on: "how do I add a code block", "how do diagrams work", "how does the
  include system work", "add a new code block for this article", "create the HTML include".
---

# article-structure

Reference for how code blocks and diagrams are structured in raianul.github.io articles.

The blog repo lives at `/Users/raian/Personal/raianul.github.io`.

---

## Overview

Articles do NOT embed code or Mermaid diagrams directly inline (except for a plain fenced
code block shown as fallback). Instead, each article has its own folder under `_includes/`
for reusable HTML snippets. The Bangla and English articles **share the same includes** —
code and diagrams are language-agnostic.

---

## Directory Structure

```
_includes/
  code-blocks/
    [article-slug]/
      code-1.html
      code-2.html
      code-3.html
      ...
  diagrams/
    [article-slug]/
      diagram-1.html
      diagram-2.html
      ...
```

The `[article-slug]` matches the markdown filename without extension:
- `oop/inheritance.md` → `_includes/code-blocks/inheritance/`
- `oop/relationships.md` → `_includes/code-blocks/relationships/`
- `system-design/caching.md` → `_includes/code-blocks/caching/`

---

## Code Block HTML Format

`_includes/code-blocks/[slug]/code-N.html` contains raw `<pre><code>`:

```html
<pre><code class="language-python">class Doctor:
    def __init__(self, name: str, degree: str):
        self.name = name
        self.degree = degree

    def check_vitals(self, patient: str):
        print(f"{self.name}: checking vitals for {patient}")
</code></pre>
```

Rules:
- **NO Bengali text anywhere in the HTML file** — no variable names, no string values, no comments. The file is shared by both articles.
- Class is always `language-python` (the blog uses Python for all examples)
- No indentation inside `<pre><code>` — code starts at column 0
- No HTML escaping needed for `<`, `>` in Python generics — keep as-is
- No trailing newline before `</code></pre>`

---

## Diagram HTML Format

`_includes/diagrams/[slug]/diagram-N.html` contains a Mermaid `<div>`:

```html
<div class="mermaid">
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#F5F5F5', 'primaryTextColor': '#000', 'primaryBorderColor': '#333', 'lineColor': '#333', 'background': '#fff'}}}%%
graph TD
    classDef box fill:#F5F5F5,color:#000,stroke:#333

    A["ClassName
method_one()
method_two()"]:::box
    B["AnotherClass
+ extra_method()"]:::box

    A -->|"relationship label"| B
</div>
```

Rules:
- **NO Bengali text anywhere in the HTML file** — no node labels, no arrow labels, no comments. The file is shared by both articles.
- Always include the full `%%{init: ...}%%` theme block — copy it verbatim from any existing diagram
- Always declare `classDef box` — all nodes use `:::box`
- Node labels use `"` quoted strings; newlines inside the label separate class name from methods
- `+ method()` prefix means "added by this subclass" (use in inheritance/composition diagrams)
- Arrow labels go inside `|"label"|`
- All labels in English — both Bangla and English articles use the same diagram file

Common graph types used:
- `graph TD` — top-down class hierarchy (inheritance, composition)
- `graph LR` — left-right flow (dependency, aggregation)

UML arrow styles in Mermaid:
| Relationship | Mermaid syntax |
|---|---|
| Association | `A --> B` |
| Aggregation | `A o-- B` |
| Composition | `A *-- B` |
| Dependency | `A ..> B` |
| Realization / implements | `A ..|> B` |
| Inheritance / extends | `A --|> B` |

---

## In-Article Include Pattern

In the markdown article, each code block or diagram uses an include tag immediately
followed by a plain fenced code block (the fenced block acts as a readable fallback
in raw markdown / GitHub preview):

```markdown
{% include code-blocks/relationships/code-1.html %}

```python
selim = Supplier("সেলিম ভাই")
sumon = Worker("সুমন", supplier=selim)

sumon.request_item("সুতি কাপড়")
# সেলিম ভাই: preparing সুতি কাপড়
```
```

And for diagrams:

```markdown
{% include diagrams/relationships/diagram-1.html %}
```

Diagrams do NOT get a fenced fallback — just the include tag alone on its own line.

---

## Bangla vs English: What Differs in Includes

Both `oop/relationships.md` (Bangla) and `en/oop/relationships.md` (English) reference
**the exact same include files**. Nothing in the HTML includes is language-specific.

**Hard rule: HTML include files are English-only, always.** Bengali text never goes inside
`code-N.html` or `diagram-N.html` — not in class names, variable names, string values,
comments, or diagram labels.

The only place Bengali text appears is in the **fenced fallback code block** inside the
Bangla `.md` article itself (the one written directly in the markdown, not the include):

| | Bangla article | English article |
|---|---|---|
| Include tags | identical | identical |
| code-N.html | shared — English only | shared — English only |
| diagram-N.html | shared — English only | shared — English only |
| Fenced fallback code | Bengali strings/comments allowed | English/NYC names only |

So when writing the English article, the fenced fallback code block (under the include)
should use English variable names and NYC-localized values, even though the include file
itself is shared.

---

## Checklist: Adding a New Code Block

1. Write the Python code
2. Create `_includes/code-blocks/[slug]/code-N.html` with `<pre><code class="language-python">...</code></pre>`
3. In the Bangla article: add `{% include code-blocks/[slug]/code-N.html %}` then the fenced block (Bengali names ok)
4. In the English article: add the same include tag then the fenced block (English/NYC names)

## Checklist: Adding a New Diagram

1. Design the Mermaid graph
2. Create `_includes/diagrams/[slug]/diagram-N.html` with the `<div class="mermaid">` block
3. In both Bangla and English articles: add `{% include diagrams/[slug]/diagram-N.html %}` (no fenced fallback)
