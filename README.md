# Articles

This repository is a workspace for writing articles, essays, and technical documentation.

It is intentionally a writing repository, not a software project. Treat code examples,
project snapshots, and supporting files as article material unless a future article
adds an explicit build or test workflow.

The goal is to make writing repeatable. Each piece should answer a clear sequence of questions instead of depending on inspiration:

- Why should the reader care?
- What specific problem or question is being addressed?
- What is the central claim?
- Why is the claim true?
- What concrete example shows it?
- Where does the argument have limits?
- What should the reader do or think differently?

## Repository Layout

- `README.md`: human-facing workspace guide and publishing checklist.
- `AGENTS.md`: durable instructions for AI coding and writing agents.
- `*/notes.md`: source notes, observations, research material, and article planning.
- `*/draft.md`: working drafts, usually rougher than final articles.
- `*/article.md`: cleaner article version intended for review or publication.
- `*/assets/`: article-specific images, data, screenshots, or other support files when needed.

Prefer one directory per article when there is supporting material:

```text
article-title/
  notes.md
  draft.md
  article.md
  assets/
```

For a short standalone essay, a single Markdown file at the repository root is acceptable.

## Article Skeleton

Use this structure for most Medium-style technical articles.

1. **Title**
   - What is the one thing this article is really about?
   - Prefer specific titles over clever or vague ones.

2. **Opening Hook**
   - Why should the reader care right now?
   - Start with a real problem, contradiction, frustration, surprising claim, or serious question.
   - Avoid textbook definitions and generic openings.

3. **Context / Problem Statement**
   - What exactly is the problem or question?
   - Define the scope.
   - Say what the article is and is not about.

4. **Core Claim**
   - What is the main argument?
   - Make the thesis visible early.
   - The reader should not have to guess the point.

5. **Explanation**
   - Why is the claim true?
   - Use 3-5 subsections.
   - Each subsection should make one supporting point.
   - A useful internal pattern is: claim, explanation, example, implication.

6. **Example / Case / Mini-Demonstration**
   - Can the argument be shown instead of only asserted?
   - Use a real project example, small code sample, failure case, comparison, or simple mathematical example.

7. **Counterpoint / Limitation**
   - Where does the claim stop being true?
   - Admit nuance, trade-offs, and serious objections.

8. **Practical Takeaway**
   - What should the reader do with this?
   - Give a rule of thumb, action, evaluation criterion, or changed way of thinking.

9. **Ending**
   - What is the final insight?
   - Do not end with "In conclusion."
   - Widen the meaning slightly beyond the immediate example.

## Universal Paragraph Pattern

Most body paragraphs should follow this shape:

**Point -> Why it matters -> Example -> Consequence**

Before keeping a paragraph, ask:

- What is the point of this paragraph?
- Why should the reader care?
- What example or mechanism supports it?
- What follows from it?

If a paragraph cannot answer these questions, it is probably filler.

## Topic-Specific Prompts

### ML / AI

- What problem does this method solve?
- What are people misunderstanding about it?
- What happens in real use, not benchmark fantasy?
- What are the failure modes?

### Code Generation

- What works in demos but fails in production?
- What is the hidden human cost?
- What tasks are actually suitable for generation?
- What requires real engineering judgment?

### Mathematics

- What intuition should the reader get first?
- What is the simplest nontrivial example?
- What common confusion should be removed?
- Why is the abstraction worth learning?

For math writing, prefer this order:

1. State the idea.
2. Give a simple example.
3. Explain what changes in a more advanced case.
4. Generalize only after the reader has intuition.

## Terminology

For articles about language-model coding tools, use both `AI` and `LLM`, but keep their roles clear.

- Use `LLM` when discussing the technical mechanism or capability, especially ChatGPT/Codex-style language models.
- Use `AI` when discussing the broader practice, user-facing experience, or general field of AI-assisted development.
- Prefer `LLM` in technical titles when the article is specifically about language models rather than AI in general.
- If both terms appear, define the distinction once near the beginning of the article.

Suggested definition:

> In this article, I use "LLM" for ChatGPT/Codex-style language models, and "AI" for the broader practice of AI-assisted development.

## What To Avoid

- Do not start with history unless history is the point.
- Do not define everything before saying anything interesting.
- Do not write several paragraphs of throat-clearing.
- Do not sound like a corporate blog.
- Do not promise revolution every time.
- Do not confuse technical writing with terminology-heavy writing.

## Publishing Checklist

Before publishing, ask:

- Is the topic narrow enough?
- Is the main claim visible early?
- Does every section support that claim?
- Is there at least one concrete example?
- Are limitations or counterarguments admitted?
- Does the reader leave with a usable takeaway?
- Does the ending give a final insight rather than a summary label?

If any answer is no, the article is not ready.
