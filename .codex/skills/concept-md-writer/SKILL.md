---
name: concept-md-writer
description: Write or improve Korean Markdown concept notes for Java, frontend, Vue, Nuxt, web rendering, browser behavior, OOP, JVM, collections, streams, and similar developer-study topics. Use when Codex needs to create a new .md concept note, rewrite an existing note, organize explanations for beginners, add code examples, compare related concepts, or make a knowledge-wiki page consistent and readable.
---

# Concept MD Writer

## Goal

Create concept notes that help a developer remember, explain, and use the idea later. Favor clear Korean, practical examples, and a stable structure over encyclopedic completeness.

## Workflow

1. Identify the reader level and topic boundary from the existing folder, filename, and nearby documents.
2. Pick the relevant template from `references/concept-note-template.md`.
3. Write in UTF-8 Markdown with Korean as the primary language.
4. Keep one main idea per section. Remove duplicated paragraphs and chat-like fragments.
5. Connect every abstract definition to at least one concrete example.
6. End with a short comparison, mistake checklist, or memory summary when it helps.

## Writing Rules

- Start with the shortest useful definition.
- Explain the "why" before deep implementation details.
- Use analogies only when they make the code easier to understand, then return to the same analogy in the code example.
- Prefer realistic developer examples over toy examples when the topic is practical.
- Keep code examples small enough to read in one glance.
- For Java notes, include compile/runtime behavior when it matters.
- For frontend notes, include browser, network, rendering, or framework lifecycle behavior when it matters.
- For comparison notes, use the same example domain across both sides so the reader can compare cleanly.
- Use `---` only between major blocks, not after every small paragraph.
- Avoid unexplained jargon. If a term is necessary, define it the first time it appears.

## Quality Bar

Before finishing, check:

- The title matches the actual topic.
- The first section answers "what is this?"
- The middle sections answer "how does it work?" and "when do I use it?"
- The examples compile or are clearly pseudocode.
- The final summary is short enough to memorize.
- No Korean text is mojibake or mixed with broken encoding.
- No accidental conversation text remains inside code blocks or prose.

## Subagent Use

When the user explicitly wants subagents or parallel review, use one subagent as an independent reviewer after drafting. Ask it to check only for clarity, technical correctness, missing beginner context, and whether the examples match the explanation. Do not pass hidden conclusions; pass the draft file path and the requested review criteria.
