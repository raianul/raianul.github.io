---
name: bangla-tech-writer
description: >
  Use this skill whenever the user wants to write a Bangla tech article in a storytelling style, or convert an English tech article/URL into a Bangla markdown post for a Bangladeshi audience. Triggers include: "এটা Bangla করো", "বাংলায় লেখো", "Bangla article বানাও", "convert to Bangla storytelling", sharing an English tech URL and asking for a Bangla version, or any request to write about a tech topic in Bangla for Bangladeshi readers. Always use this skill for Bangla tech writing — even if the user just says "write about X in Bangla" or "এই topic নিয়ে লেখো".
---

# Bangla Tech Writer

Convert English tech articles or topics into engaging Bangla storytelling-style markdown articles for a Bangladeshi audience.

## Core Philosophy

The goal is not translation — it is **adaptation**. A Bangladeshi reader should feel like they are reading a story written for them, not a translated document. Every concept needs a Bangladeshi anchor: a chai stall, a bKash transaction, an Eid rush, a Pathao ride. Extract the concept from the source, then rebuild it from scratch using the structure and style below.

---

## Writing Rules

1. **Never use double hyphens** (`--`) or **em dashes** (`—`). Use a colon (`:`), comma (`,`), or full stop (।) instead. Restructure the sentence if needed.
2. **Use "তুমি/তোমার"** — friendly and direct, not formal "আপনি".
3. **Start with a story** — every article opens with a relatable Bangladeshi scene before any technical concept appears.
4. **Bold the first mention** of every technical term when introduced.
5. **Numbered sections** using Bengali numerals: ১. ২. ৩.
6. **Prefer flowing prose** over bullet point lists. Bullets are fine for short enumerations, but prose tells the story better.
7. **End each section** with a sentence or question that flows naturally into the next concept — keep momentum going.
8. **Keep a conversational tone** — write as if explaining to a friend at a tea stall, not a textbook reader.
9. **Each article gets a unique Bangladeshi character and scene.** Never reuse the same person across articles. See the published articles table below for taken characters.

---

## Bangladeshi Analogy Bank

Pick whichever fits naturally, or invent new ones. The principle is always: find something from daily Bangladeshi life.

| Tech Concept | Bangladeshi Analogy |
|---|---|
| Single server | রাশেদ ভাইয়ের একা চায়ের দোকান |
| Traffic spike | ঈদের দিন হঠাৎ ৫০০ কাস্টমার |
| Vertical scaling | বড় চুলা কিনে আনা |
| Horizontal scaling | নতুন শাখা খোলা |
| Load balancer | দরজায় দারোয়ান যে ভাগ করে দেয় |
| Cache | আদা আগেই ফুটিয়ে রাখা |
| Database | অর্ডার খাতা |
| Message queue | টোকেন সিস্টেম |
| High-traffic app | bKash, Pathao, Shohoz Food, Daraz |
| Class / Blueprint | ছাঁচ (করিম মিয়ার কারখানা) |
| Object / Instance | ছাঁচ থেকে বানানো পাতিল |
| API | রেস্টুরেন্টের ওয়েটার |
| Microservices | আলাদা আলাদা দোকানের শপিং মল |
| Replication | খাতার ফটোকপি |
| Sharding | শাখা ভিত্তিক আলাদা খাতা |
| Failover | বিকল্প জেনারেটর |
| Timeout | দোকান বন্ধের সময় পেরিয়ে গেছে |

---

## Article Structure

Follow this structure for every article:

### Title
The title must be unique and story-specific. Format:
```
# [Topic]: [a short evocative phrase from the story]
## [Series Name] সিরিজ
```

Never use "গল্পের মতো করে বোঝা" — it is generic and repetitive. The subtitle should come from the specific story in the article. Examples:
- "স্কেলেবিলিটি: ঈদের দিন রাশেদ ভাইয়ের দোকান ডুবে গেল"
- "ক্লাস আর অবজেক্ট: একটা ছাঁচ, হাজার পাতিল"
- "Encapsulation: bKash জানে, তুমি জানো না"

### ভূমিকা (Opening Story)
Write 3–6 short paragraphs. Drop the reader into a scene — do not introduce the topic first, let the story reveal it. Use short, punchy sentences for dramatic effect. End the intro by naming the problem: "এই সমস্যাটার নাম **[Topic]**।"

### Numbered Concept Sections (১. ২. ৩. ...)
Each section covers one sub-concept:
- Open with the Bangladeshi analogy extended naturally from the intro
- Introduce the technical concept with a bold term
- Add a Mermaid diagram if the concept is structural or has a flow
- Cover trade-offs in **ভালো দিক** / **খারাপ দিক** format when relevant
- Close the section with a transition that creates curiosity for the next

### বাস্তব উদাহরণ (Real-World Walkthrough)
Use a Bangladeshi app (bKash, Pathao, Shohoz, Daraz) to walk through how the concepts apply step by step. Use numbered stages if the system evolves over time (e.g., "ধাপ ১", "ধাপ ২").

### সারসংক্ষেপ (Summary)
Always end with:
1. A two-column table mapping story language to tech language:
   `| গল্পের ভাষায় | প্রযুক্তির ভাষায় |`
2. Three to five **মূল শিক্ষা** (key takeaways) in bold sentences.

### Closing Hook
End with a teaser leading into the next article:
```
> পরবর্তী প্রশ্ন: [question that sets up next topic]? সেই ধারণার নাম **[Next Topic]।**

*[Series] সিরিজের পরবর্তী পর্ব: [Next Topic Title]*
```

---

## Mermaid Diagram Rules

Every diagram must use this exact header — no exceptions:

```
%%{init: {'theme': 'base', 'themeVariables': {'primaryColor': '#F5F5F5', 'primaryTextColor': '#000', 'primaryBorderColor': '#333', 'lineColor': '#333', 'background': '#fff'}}}%%
graph TD
    classDef box fill:#F5F5F5,color:#000,stroke:#333
```

Rules:
- Apply `:::box` to every node without exception
- Use Bengali text for all node labels
- Keep diagrams simple — 5 to 8 nodes maximum
- Label arrows in Bengali: `-->|"বাংলা লেখা"|`
- Use `subgraph` for side-by-side comparisons (e.g., stateful vs stateless)
- Prefer `graph TD` (top-down) for hierarchies, `graph LR` (left-right) for flows

---

## When Given a URL or English Article

1. Fetch and read the full source content
2. Identify: the core concept, sub-concepts, any diagrams or charts, and key examples
3. Do NOT translate literally — extract the ideas, then rewrite from scratch using the structure above
4. Replace all English examples with Bangladeshi equivalents from the analogy bank
5. Keep technical terms in English when they are standard (Load Balancer, Redis, Sharding, etc.) but always introduce them inside a Bengali sentence
6. If content is behind a paywall, use the Claude in Chrome plugin to access it

---

## Output

The blog uses **Jekyll** on GitHub Pages. Save the finished article as a `.md` file directly in the correct directory of the workspace (`/Users/raian/Personal/raianul.github.io`). No HTML conversion needed — Jekyll builds it automatically.

**Directory — pick the one that matches the series:**

| Series | Directory |
|---|---|
| OOP | `oop/` |
| Design Principles | `design-principles/` |
| Design Patterns | `design-patterns/` |
| Concurrency | `concurrency/` |
| API Fundamentals | `api-fundamentals/` |
| System Design | `system-design/` |
| Distributed Systems | `distributed-systems/` |
| Architectural Patterns | `architecture/` |
| Async Communication | `async/` |
| Caching | `caching/` |
| Database Fundamentals | `databases/` |
| Database Deep Dive | `database-deep-dive/` |
| DSA | `dsa/` |
| Networking | `networking/` |
| AI Fundamentals | `ai/fundamentals/` |
| AI Tools | `ai/tools/` |

**Filename format:** `[topic-in-english-lowercase-with-hyphens].md`

**Every article must start with this exact frontmatter** (update fields as appropriate):

```yaml
---
layout: post
title: "Article Title Here"
description: "One-line SEO description. No em dashes."
series: "OOP সিরিজ"
series_label: "OOP"
category: oop
tags: [Tag1, Tag2, Tag3]
date: YYYY-MM-DD
date_bn: "মাস ২০২৬"
read_time: "৬ মিনিট"
order: 8
excerpt: "Homepage card-এ যা দেখাবে। Em dash ছাড়া।"
---
```

**`category` and `series_label` values per series:**

| Series | category | series_label |
|---|---|---|
| OOP | `oop` | `OOP` |
| Design Principles | `design-principles` | `Design Principles` |
| Design Patterns | `design-patterns` | `Design Patterns` |
| Concurrency | `concurrency` | `Concurrency` |
| API Fundamentals | `api-fundamentals` | `API Fundamentals` |
| System Design | `system-design` | `System Design` |
| Distributed Systems | `distributed-systems` | `Distributed Systems` |
| Architectural Patterns | `architecture` | `Architecture` |
| Async Communication | `async` | `Async` |
| Caching | `caching` | `Caching` |
| Database Fundamentals | `databases` | `Databases` |
| Database Deep Dive | `database-deep-dive` | `Database Deep Dive` |
| DSA | `dsa` | `DSA` |
| Networking | `networking` | `Networking` |
| AI Fundamentals | `ai-fundamentals` | `AI Fundamentals` |
| AI Tools | `ai-tools` | `AI Tools` |

`order` must be the next number in the series (used for prev/next navigation). Check the published articles table below for the latest order number per series.

**Extra fields for AI categories:**
- `ai-fundamentals` articles: add `topic: "LLM"` (or `RAG`, `Neural Networks`, `Embeddings`, `Transformers`) — used for sidebar filtering on `/ai/fundamentals/`
- `ai-tools` articles: add `tool: "Gemini"` (or `ChatGPT`, `Claude`, `Cursor`, etc.) — used for sidebar filtering on `/ai/tools/`

**After saving the `.md` file, no other steps are needed.** The homepage (`index.html`) auto-discovers all articles with `layout: post` via Jekyll Liquid — no manual card additions or count updates required.

### Currently published articles (do not reuse their characters)

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
