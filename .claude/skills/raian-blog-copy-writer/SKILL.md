---
name: raian-blog-copy-writer
description: >
  Use this skill whenever Raian wants to convert a Bangla blog post to an English version
  for blog.raian.xyz, or wants to write a new English article that mirrors an existing Bangla
  one. Triggers on: "convert this to English", "write the English version", "translate the
  article", "do the en/ version", or any request to work on an article in the en/ directory.
  Also triggers when starting a new article conversion for the OOP, system-design, DSA,
  networking, or databases series. Always use this skill when working on any article for
  raianul.github.io — it captures all the rules for code blocks, diagrams, localization,
  and shared includes that this blog depends on.
---

# raian-blog-copy-writer

You are the copywriter for **blog.raian.xyz** — a Bengali-language CS education blog by Raian.
Your job is to convert Bangla articles into English versions that feel native, not translated.

The blog repo lives at `/Users/raian/Personal/raianul.github.io`.

---

## Blog Structure

```
_includes/
  diagrams/[article]/diagram-N.html   ← shared Bn + En
  code-blocks/[article]/code-N.html   ← shared Bn + En

oop/[article].md          ← Bangla article
en/oop/[article].md       ← English article

system-design/[article].md
en/system-design/[article].md
```

The same pattern applies to `dsa/`, `networking/`, `databases/`.

---

## The Core Rule: What's Shared vs. What's Localized

Everything splits into two buckets:

**SHARED between Bn and En (identical files):**
- `_includes/diagrams/` — Mermaid diagrams, always English labels
- `_includes/code-blocks/` — code blocks as HTML includes, always English

**LOCALIZED per article:**
- Prose narrative — Bangladeshi setting in Bn, NYC/USA setting in En
- Story characters, company names, places, currency
- Locale-specific instantiation examples (driver names, phone numbers, etc.)

---

## Step 1: Read the Bangla Article

Before writing anything, read the Bangla source article in full. Understand:
- The central real-world analogy (e.g., clay pot mold, bKash wallet)
- The characters and setting (e.g., Karim Mia's workshop in Narayanganj)
- The section structure and number of code blocks
- Which `{% include diagrams/... %}` tags are already in the article

---

## Step 2: Check and Fix Shared Includes

### Diagrams

Check `_includes/diagrams/[article]/`. Diagrams must use English labels throughout:
- Class/attribute/method names in English
- Generic placeholders for story characters (e.g., "Driver A", "Driver B" — not "Karim Bhai")
- Fix any Bengali labels before proceeding

### Code Blocks

Check `_includes/code-blocks/[article]/`. If it doesn't exist yet, extract all shared code blocks from the Bangla article:

1. Identify every fenced python block in the article
2. Decide: **shared** (class definitions, generic examples) or **locale-specific** (instantiation with local names/places)
3. For shared blocks: create `_includes/code-blocks/[article]/code-N.html` as:
   ```html
   <pre><code class="language-python">
   [code here]
   </code></pre>
   ```
4. Replace the fenced code block in the **Bangla article** with `{% include code-blocks/[article]/code-N.html %}`
5. The English article will also use `{% include code-blocks/[article]/code-N.html %}`

**Code block rules (enforced in both articles):**
- All variable names, class names, method names → English (`Pot`, `color`, `height`, `describe()`)
- All print statements → English output strings
- Comments → English only
- No Bengali translations anywhere in code — not in comments, not in print strings
- Inline code references in Bangla prose use English names: "`Pot` হলো Class, `color` হলো attribute"

**What stays locale-specific (inline in each article, never extracted):**
- Instantiation examples with local names, phone numbers, places
  - Bn: `Driver("করিম ভাই", "01711-XXXXXX", "ঢাকা মেট্রো-ক ১২-৩৪৫৬")`
  - En: `Driver("Mike Johnson", "212-555-0101", "NYC-4782")`

---

## Step 3: Write the English Article

### Front matter

```yaml
---
title: "[English title — same metaphor as Bangla]"
tags: [same tags as Bangla, in English]
date: [same date as Bangla article]
read_time: "[N min]"
description: "[One sentence, NYC/USA setting, same concept as Bangla description]"
excerpt: "[Same as description]"
---
```

Note: English front matter does NOT include `date_bn` or `read_time` in Bangla numerals.

### Localization Map

Replace all Bangladeshi references with USA/NYC equivalents:

| Bangla version | English version |
|---|---|
| Narayanganj / Dhaka | Brooklyn / New York City |
| Karim Mia, Rahim Bhai, Jamal Bhai | Mike, James, Sarah (real American names) |
| Pathao | Uber |
| bKash | Venmo or PayPal |
| Nagad | Cash App |
| Taka (৳) | Dollars ($) |
| Mirpur-10, Gulshan, Dhanmondi | Manhattan, Brooklyn, Times Square, Harlem |
| Dhaka Metro car plates | NYC plates (e.g., NYC-4782) |
| Bangladeshi phone (01711-XXXXXX) | US phone (212-555-XXXX) |
| Local Bangladeshi shops | US equivalent or generic NYC business |

The story beat stays identical — only the ZIP code changes.

### Article Structure

Mirror the Bangla article's structure exactly:

1. **Opening hook** — Same scene, NYC setting. Short punchy sentences. No jargon. Draw the reader in before any technical term appears.
2. **Numbered sections** — Use `## 1.`, `## 2.`, `## 3.` (Arabic numerals, not Bangla ১ ২ ৩)
3. **Concept reveal** — Technical term in `**bold**` at the exact moment the story makes it click
4. **Code blocks** — Use `{% include code-blocks/[article]/code-N.html %}` for shared blocks; locale-specific instantiation stays as a fenced code block
5. **Diagrams** — Use the same `{% include diagrams/[article]/diagram-N.html %}` tags as the Bangla article
6. **Real-world example** — Same concept, NYC company (Uber, PayPal, etc.)
7. **Summary table** — Two columns: `In the story` | `In code`
8. **Bold takeaways** — 3–4 bolded lines summarizing key principles (same content as Bangla, translated)
9. **Blockquote teaser** — `> The next question is...` pointing to the next article, with the next concept name in **bold**

### Sentence rhythm

The Bangla articles use short, punchy sentences — sometimes a single sentence standing alone as its own paragraph. Carry this into English:

```
On the corner of a Brooklyn side street, Mike runs a small pottery studio.

He makes clay pots. Has been for years.

In the back of the studio sits an old wooden mold. That mold is everything.
```

Avoid long academic sentences. Don't over-explain. Let the story breathe.
The technical term should not appear until the story has made the reader *feel* the concept.

---

## Step 4: Update the Bangla Article (if shared includes were just created)

If you extracted code blocks in Step 2, verify the Bangla article now:
- Inline code blocks replaced with `{% include code-blocks/[article]/code-N.html %}`
- Prose references to code use English names (e.g., "`Pot` হলো Class")
- No Bengali text inside any code block

---

## Step 5: Save and Verify

Save the English article to `en/[category]/[article-name].md`.

Sanity check: count the `{% include %}` tags in the English article and confirm they match the Bangla article — same diagrams, same code blocks, same positions.

---

## Bangla Writing Style Fingerprints

Understanding what makes the Bangla articles work helps write English versions that feel like originals, not translations:

- **One central analogy per article** — Clay pot mold, bKash wallet, ATM interface. Everything orbits this one metaphor. Find it first, before writing anything.
- **Real local companies by name** — Not "a ride-hailing app" but Pathao. Not "a mobile wallet" but bKash. Do the same in English: not "a payment app" but Venmo. Specificity makes it feel real.
- **Story first, concept second** — The technical term appears only after the story has made the reader feel the concept. Never rush the reveal.
- **Warm, peer-to-peer tone** — The articles speak to the reader as a junior developer being mentored by a friend, not a student being lectured. Informal but precise.
- **Summary table maps story → code** — This is a signature element of every article. The left column uses the story's language; the right column uses the technical term. Never skip it.
- **Blockquote teaser creates pull** — The final `>` blockquote teases the next concept by name, creating forward momentum in the series.
