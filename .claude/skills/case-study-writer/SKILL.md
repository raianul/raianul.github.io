---
name: case-study-writer
description: >
  Use this skill when the user shares a URL from a real engineering blog (Netflix, Uber, Discord, Meta, etc.) and wants a Bengali article inspired by it. Triggers include: sharing a link from external.md and saying "এটা নিয়ে লেখো", "case study বানাও", "Bengali article", or any request to write a Bengali article based on a real-world engineering article. This skill fetches the original article, extracts the core concept, and writes a fully original Bengali article — NOT a translation — with the original source cited at the end.
---

# Case Study Writer

Fetch a real engineering blog article, extract the core concept, and write an original Bengali storytelling article inspired by it — with full attribution to the original source.

## Workflow

### Step 1 — Fetch and Understand

Fetch the article URL using WebFetch. From the article, extract:

- **Core problem**: what challenge was the company solving?
- **Core concept**: what technique, pattern, or system did they use?
- **Key insight**: what is the surprising or non-obvious thing they discovered?
- **Scale/numbers**: any impressive numbers that make the story real (e.g., "trillions of messages", "40 million reads/sec")

Do NOT translate the article. Just extract the ideas.

### Step 2 — Plan the Bengali Article

Decide:
- What is the **one core concept** a Bengali CS student should learn from this?
- What **Bangladeshi analogy** fits the core problem?
- What **character and scene** will open the article?

The article must teach the concept — the real-world company example is the proof that the concept matters.

### Step 3 — Write the Bengali Article

Follow the full **bangla-tech-writer** style and structure:

1. Open with a Bangladeshi scene (not the company, not the tech — a local story first)
2. Introduce the tech concept through the story
3. Show how the concept works with diagrams if needed
4. Include a **বাস্তব উদাহরণ** section — this is where the real company story comes in:
   - Name the company
   - Describe the problem they faced (in Bengali)
   - Describe how they solved it (in Bengali)
   - Include the impressive scale numbers
5. End with সারসংক্ষেপ table and মূল শিক্ষা

### Step 4 — Add Source Attribution

At the very end of the article, after the closing hook, add this section:

```markdown
---

## 📖 মূল উৎস

এই আর্টিকেলটি লেখা হয়েছে নিচের real-world engineering case study থেকে অনুপ্রাণিত হয়ে:

- **[Article Title]** — [Company] Engineering Blog, [Year]
  [Original URL]

> এটি একটি অনুপ্রাণিত শিক্ষামূলক আর্টিকেল। মূল concept এবং implementation [Company]-এর। বাংলা ব্যাখ্যা এবং উপস্থাপনা সম্পূর্ণ নতুনভাবে তৈরি করা হয়েছে।
```

---

## Writing Rules

All rules from `bangla-tech-writer` apply here:

- Never use `--` or `—`
- Use `তুমি/তোমার`
- Start with a Bangladeshi scene
- Bold first mention of every technical term
- Numbered sections with Bengali numerals: ১. ২. ৩.
- Prose over bullet points
- Unique character per article — check the published articles table below

---

## Mermaid Diagram Rules

Every diagram must use this exact header:

```
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#F5F5F5', 'primaryTextColor': '#000', 'primaryBorderColor': '#333', 'lineColor': '#333', 'background': '#fff'}}}%%
graph TD
    classDef box fill:#F5F5F5,color:#000,stroke:#333
```

- Apply `:::box` to every node
- Bengali text for all node labels
- 5 to 8 nodes maximum
- Label arrows in Bengali

---

## Output

Save the finished article as a `.md` file in the correct directory.

**Directory:** All case study articles go in `case-studies/`

**Filename format:** `[topic-in-english-lowercase-with-hyphens].md`

**Frontmatter:**

```yaml
---
layout: post
title: "Article Title Here"
description: "One-line SEO description. No em dashes."
series: "Real World সিরিজ"
series_label: "Real World"
category: system-design
tags: [Tag1, Tag2, CompanyName]
date: YYYY-MM-DD
date_bn: "মাস ২০২৬"
read_time: "৮ মিনিট"
order: 1
excerpt: "Homepage card-এ যা দেখাবে।"
source_company: "Netflix"
source_url: "https://..."
source_title: "Original Article Title"
---
```

`category`: always use `case-study`
`series`: always `"Case Studies সিরিজ"`
`series_label`: always `"Case Study"`

---

## Published Articles (do not reuse characters)

| File | Character | Date | Order |
|---|---|---|---|
| `system-design/scalability.md` | রাশেদ ভাই | 2022-07-15 | 1 |
| `oop/classes-and-objects.md` | করিম মিয়া | 2022-08-15 | 1 |
| `oop/enums.md` | রিনা আপা | 2022-09-15 | 2 |
| `oop/interfaces.md` | (Pathao/payment scenario) | 2022-10-15 | 3 |
| `oop/encapsulation.md` | (bKash wallet scenario) | 2022-11-15 | 4 |
| `oop/abstraction.md` | (Pathao/rideshare scenario) | 2022-12-15 | 5 |
| `oop/inheritance.md` | ফারুক স্যার | 2023-01-15 | 6 |
| `oop/polymorphism.md` | রনি ভাই | 2023-02-15 | 7 |
