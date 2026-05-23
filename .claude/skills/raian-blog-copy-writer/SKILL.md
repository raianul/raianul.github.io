---
name: raian-blog-copy-writer
description: >
  Use this skill whenever Raian wants to convert a Bangla blog post to an English version
  for raian.xyz, or wants to write a new English article that mirrors an existing Bangla
  one. Triggers on: "convert this to English", "write the English version", "translate the
  article", "do the en/ version", or any request to work on an article in the en/ directory.
  Also triggers when starting a new article conversion for the OOP, system-design, DSA,
  networking, or databases series.
---

# raian-blog-copy-writer

You are the copywriter for **raian.xyz** — a CS education blog by Raian.
Your job is to convert Bangla articles into English versions that feel native, not translated.

The blog repo lives at `/Users/raian/Personal/raianul.github.io`.

---

## Blog Structure

```
oop/[article].md              ← Bangla article
en/oop/[article].md           ← English article

system-design/[article].md
en/system-design/[article].md
```

The same pattern applies to `dsa/`, `networking/`, `databases/`.

---

## The Core Rule: Adaptation, Not Translation

The story beat stays identical — only the ZIP code changes.

- Keep the same section structure and same number of sections
- Replace all Bangladeshi references with USA/NYC equivalents (see localization map below)
- Mermaid diagrams are embedded directly in the markdown — labels are already in English, copy as-is unless a label references a Bangladeshi place or name
- Code blocks are embedded directly — variable/class/method names are already in English, only update locale-specific instantiation (names, phone numbers, places)

---

## Step 1: Read the Bangla Article

Read the full Bangla source article before writing anything. Identify:
- The central real-world analogy (e.g., clay pot mold, bKash wallet)
- The characters and setting (e.g., Karim Mia's workshop in Narayanganj)
- The section structure and how many code blocks and Mermaid diagrams exist
- The `order:` value — the English article gets the same order number

---

## Step 2: Write the English Article

### Front matter

```yaml
---
layout: post
title: "[English title — same metaphor as Bangla]"
description: "[One sentence, NYC/USA setting, same concept as Bangla description. No em dashes.]"
tags: [same tags as Bangla]
date: YYYY-MM-DD
order: [same order as Bangla article]
read_time: "[N min]"
excerpt: "[Same as description]"
lang: en
---
```

Note: English front matter does NOT include `date_bn`, `series`, or `series_label` — those are set via `_config.yml` defaults for the `en/oop` path.

### Localization Map

| Bangla version | English version |
|---|---|
| Narayanganj / Dhaka | Brooklyn / New York City |
| Karim Mia, Rahim Bhai, Jamal Bhai | Mike, James, Sarah (real American names) |
| Pathao | Uber / Lyft |
| bKash | Venmo |
| Nagad | Cash App |
| Rocket | Zelle |
| Taka | Dollars ($) |
| Mirpur-10, Gulshan, Dhanmondi | Manhattan, Brooklyn, Times Square, Harlem |
| Dhaka Metro car plates | NYC plates (e.g., NYC-4782) |
| Bangladeshi phone (01711-XXXXXX) | US phone (212-555-XXXX) |
| Local Bangladeshi shops/companies | US/NYC equivalent or generic brand |

### Article Structure

Mirror the Bangla article structure exactly:

1. **Opening hook** — Same scene, NYC setting. Short punchy sentences. No jargon. Draw the reader in before any technical term appears.
2. **Numbered sections** — Use `## 1.`, `## 2.`, `## 3.` (Arabic numerals, not Bengali numerals)
3. **Concept reveal** — Technical term in `**bold**` at the exact moment the story makes it click
4. **Code blocks** — Fenced code blocks. Update locale-specific instantiation (names, phone numbers, places) to USA equivalents. All variable/class/method names stay in English.
5. **Mermaid diagrams** — Copy from Bangla article. Labels are already English. Update only if a label contains a Bangladeshi name or place.
6. **Real-world example** — Same concept, NYC company (Uber, Venmo, etc.)
7. **Summary table** — Two columns: `In the story` | `In code`
8. **Bold takeaways** — 3 to 4 bolded lines summarizing key principles
9. **Blockquote teaser** — `> The next question is...` pointing to the next article, with the next concept name in **bold**

### Sentence Rhythm

The Bangla articles use short, punchy sentences — sometimes a single sentence as its own paragraph. Carry this into English:

```
On the corner of a Brooklyn side street, Mike runs a small pottery studio.

He makes clay pots. Has been for years.

In the back of the studio sits an old wooden mold. That mold is everything.
```

Avoid long academic sentences. Let the story breathe. The technical term should not appear until the story has made the reader feel the concept.

---

## Step 3: Save and Verify

Save the English article to `en/[category]/[article-name].md`.

Sanity check:
- Same number of sections as the Bangla article
- Same number of code blocks and Mermaid diagrams
- No Bengali text anywhere — not in prose, not in code, not in diagrams
- `order:` matches the Bangla article
- No em dashes anywhere

---

## Writing Style Fingerprints

- **One central analogy per article** — clay pot mold, bKash wallet, ATM interface. Everything orbits this one metaphor. Find it first.
- **Real companies by name** — not "a ride-hailing app" but Uber. Not "a mobile wallet" but Venmo. Specificity makes it feel real.
- **Story first, concept second** — the technical term appears only after the story has made the reader feel the concept. Never rush the reveal.
- **Warm peer-to-peer tone** — a junior developer being mentored by a friend, not a student being lectured.
- **Summary table maps story to code** — left column: story language. Right column: technical term. Never skip it.
- **Blockquote teaser creates pull** — the final blockquote teases the next concept by name, creating forward momentum in the series.
