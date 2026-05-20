# AGENTS.md

This repository is for writing articles, essays, and documentation. Treat it as a writing workspace, not a software project unless code examples or supporting scripts are explicitly added later.

## Working Principles

- Help produce clear, disciplined technical writing.
- Prefer a narrow claim over a broad topic.
- Make the central argument visible early.
- Keep the reader's practical problem in view.
- Remove filler, throat-clearing, generic introductions, and corporate-blog phrasing.
- Preserve the author's direct, serious voice.
- Treat this repository as portable. Avoid embedding current local absolute paths in durable documentation.
- When a writing rule, terminology decision, or article organization decision should survive future sessions, reflect it in `README.md` and this file.

## Default Article Structure

When drafting or revising an article, use this skeleton unless the user asks for a different form:

1. Title
2. Opening hook
3. Context / problem statement
4. Core claim
5. Explanation with 3-5 supporting sections
6. Example, case, or mini-demonstration
7. Counterpoint or limitation
8. Practical takeaway
9. Ending with a broader final insight

The article should do three things:

- Give the reader a reason to care.
- Explain one central idea clearly.
- Leave the reader with something usable.

## Section Standards

### Title

The title should answer: what is the one thing this article is really about?

Use specific titles, not clever vague ones.

### Opening Hook

The first paragraph should answer: why should the reader care right now?

Start with one of these:

- A real problem
- A contradiction
- A practical frustration
- A surprising claim
- A serious question the target reader already has

Avoid generic openings such as "In recent years..." unless the history itself is the subject.

### Context / Problem Statement

Define the scope clearly:

- What specific issue is being discussed?
- In what context does it appear?
- Why is it hard, misunderstood, or important?
- What common wrong assumption is being challenged?
- What is outside the scope?

### Core Claim

State the main argument early. The reader should not have to infer the thesis.

Ask:

- What is the article trying to prove or explain?
- What should the reader remember if they remember one sentence?

### Explanation

The body should usually have 3-5 supporting sections. Each section should make one supporting point.

Use this local pattern:

1. Claim
2. Explanation
3. Example
4. Implication

### Example / Case / Mini-Demonstration

Prefer concrete demonstration over assertion.

Depending on the topic, use:

- A real project example
- A small code sample
- A failure case
- Expected vs. actual behavior
- A simple mathematical example before abstraction

### Counterpoint / Limitation

Add a serious limitation or counterargument. This builds trust and prevents the article from becoming marketing.

Ask:

- Where does the claim stop being true?
- What would a smart critic say?
- What trade-off is being simplified?

### Practical Takeaway

The article should not merely explain. It should change what the reader does or notices.

Ask:

- What action should the reader take?
- What rule of thumb follows?
- What should they evaluate differently?
- What should they stop doing?

### Ending

Do not end with "In conclusion."

End by widening the meaning slightly:

- What larger lesson does this reveal?
- Why does this matter beyond the immediate example?
- What should the reader keep thinking about?

## Paragraph Rule

Most body paragraphs should follow:

Point -> Why it matters -> Example -> Consequence

Before keeping a paragraph, check:

- Does it make one point?
- Does it explain why the point matters?
- Does it include or imply a concrete mechanism or example?
- Does it show what follows?

If not, revise or remove it.

## Topic Guidance

### ML / AI Articles

Add these checks:

- What problem does this method solve?
- What are people misunderstanding about it?
- What happens in real use, not benchmark fantasy?
- What are the failure modes?

### Code Generation Articles

Add these checks:

- What works in demos but fails in production?
- What is the hidden human cost?
- What tasks are actually suitable for generation?
- What requires real engineering judgment?

### Mathematics Articles

Add these checks:

- What intuition should the reader get first?
- What is the simplest nontrivial example?
- What common confusion should be removed?
- Why is the abstraction worth learning?

For math writing, prefer this order:

1. State the idea.
2. Give a simple example.
3. Explain what changes in a more advanced case.
4. Generalize only after the reader has intuition.

## Revision Checklist

Before considering a draft ready, check:

- The topic is narrow enough.
- The main claim appears early.
- Every section supports the claim.
- There is at least one concrete example.
- The article admits limitations.
- The reader leaves with a usable takeaway.
- The ending gives a final insight rather than a summary label.

## Style Preferences

- Be direct, serious, and practical.
- Prefer clear claims over decorative language.
- Avoid corporate tone.
- Avoid exaggerated claims about revolutions or inevitability.
- Avoid unnecessary terminology.
- Keep introductions short.
- Do not add history unless it directly serves the argument.

## Terminology

For articles about language-model coding tools, use both `AI` and `LLM`, but give each term a clear role.

- Use `LLM` when discussing the technical mechanism or capability, especially ChatGPT/Codex-style language models.
- Use `AI` when discussing the broader practice, user-facing experience, or general field of AI-assisted development.
- Prefer `LLM` in technical titles when the article is specifically about language models rather than AI in general.
- If both terms appear, define the distinction once near the beginning of the article.

Suggested definition:

> In this article, I use "LLM" for ChatGPT/Codex-style language models, and "AI" for the broader practice of AI-assisted development.

## File Organization

Use `README.md` for human-facing workspace guidance.

Use `AGENTS.md` for durable instructions to future AI coding and writing agents.

For future articles, prefer one directory per article if supporting material is needed:

```text
article-title/
  draft.md
  notes.md
  assets/
```

For a simple standalone essay, a single Markdown file at the repository root is acceptable.

## PDF Export

When exporting an article to PDF, do not include decorative or document-title headers and footers. Use page numbers only.

## Portability and GitHub Move

This repository may be moved to GitHub and to a different local directory.

- Keep internal links and instructions relative to the repository when possible.
- Do not depend on `/Users/...` or another machine-specific path.
- Article-specific snapshots of other projects, such as copied README or AGENTS files, should stay inside the article directory that uses them.
- Before publishing an article, recheck external URLs, copied project facts, and any claims that may have changed.
- If the repository gains build scripts, code examples, generated assets, or publishing automation later, document the workflow in `README.md` and add agent-facing cautions here.
