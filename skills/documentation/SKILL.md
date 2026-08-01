---
name: documentation
description: MUST USE when writing, reviewing, changing, or maintaining any software project or product documentation, including READMEs, CONTRIBUTING guides, ADRs, architecture and API docs, configuration guides, and inline documentation
---

# Documentation

Documentation transfers enough accurate understanding for readers to decide, act, and maintain software safely. Explain why the software exists, what it enables, how to use it, and how to keep it healthy. Prefer purpose, design, and practical use over line-by-line implementation detail.

Write for the reader at the current scope without assuming they entered at the repository root:

- Users need purpose, prerequisites, setup, and a path to success.
- Developers extending a library, framework, or tool need concepts, architecture, dependencies, integration points, constraints, and trade-offs.
- Maintainers need invariants, assumptions, decision history, pitfalls, testing expectations, and release practices.

Increase depth as documentation approaches the code. Keep every claim narrow enough to remain truthful as the code evolves. Avoid speculation and forward-looking claims unless they are explicitly identified.

## Workflow

Always work in this order: Discover -> User -> Developer -> Maintainer. Update only the artifacts relevant to the requested scope.

### Discover

Read all documentation in scope before writing, including relevant READMEs, `CONTRIBUTING.md`, ADRs, and `docs/` or architecture material. Understand the software, audience, intent, terminology, tone, and structure. Identify gaps, contradictions, outdated claims, and duplication.

Plan links to existing material instead of repeating it. Leave clear and accurate content untouched.

### User

Make the nearest relevant README useful as a reader's first entry point:

- A root `README.md` introduces purpose, audience, installation, and a quick start.
- A module, package, or feature `README.md` lives beside its code and explains that scope's intent and usage.
- Examples are minimal, runnable, directly relevant, and immediately explain new variables or placeholders.

Begin with the problem, audience, and value. Then show installation, setup, and use. Do not parrot implementation details.

### Developer

After usage guidance, explain the design conceptually. Cover architecture, dependencies, integration points, and extension constraints. Use concise summaries or diagrams when relationships would otherwise be hard to understand. Provide short runnable examples and contextual links to ADRs or API definitions.

Keep detailed internal explanations in technical README sections or, when their size warrants it, in `docs/` or `architecture/`.

### Maintainer

Explain design rationale, trade-offs, constraints, invariants, decision history, known pitfalls, and testing and release expectations. Keep this guidance candid and pragmatic.

Use `CONTRIBUTING.md` as the single authoritative home for contribution and maintenance guidance, including setup, testing, branching, review, and release expectations. Link to ADRs rather than duplicating decisions.

## Placement and navigation

Layer documentation so readers can enter at any depth:

- Root documentation introduces the project and gets readers started.
- Module and feature documentation explains local intent and usage and links to related or parent material.
- Utility, class, and inline documentation exists only where it clarifies local purpose, constraints, or non-obvious behavior.

When context belongs elsewhere, link to its canonical location and provide only enough local context to orient the reader. Prefer nearby sibling links before sending a module reader to a broader level. Give external links one sentence explaining why the destination matters. Use descriptive link text.

Start each section with orientation and end with a clear next action or confirmation that the reader can stop. The heading outline must communicate the logical flow on its own.

## Change safety

Documentation has continuity across readers, links, diffs, and history. Make surgical changes:

- Change only unclear, inaccurate, outdated, contradictory, or missing content.
- Add context or links before replacing whole sections. Extend examples before reformatting them unless clarity requires it.
- Match the existing tone and structure. Justify a major stylistic change by a concrete consistency or clarity need.
- Preserve valid anchors, links, and paths. Before removing text, verify that nothing depends on it.
- Record why the revision was made, not only what changed, in the eventual commit message or an appropriate inline comment.

Improve consistency and understanding without rewriting the document in your own voice.

## Writing discipline

Use a friendly, confident, plainspoken peer voice. Be approachable without oversimplifying. Avoid marketing language, exaggerated enthusiasm, emojis, decorative symbols, and empty adjectives such as "powerful" or "state of the art."

Each paragraph must answer a reader question. Use short declarative sentences. Name and link prerequisites or background knowledge rather than assuming shared context.

Use standard Markdown and ASCII punctuation. Do not use smart punctuation or punctuation-driven prose. Give each distinct what, why, or how idea its own heading. Use lists only for real sequences or sets; express conceptual hierarchies with nested headings and short explanations. Use code fences for commands, configuration, and exact UI output, and backticks for inline literals. Keep headings, links, and sentences legible in plain text and narrow terminals.

Examples prove usability. Keep them minimal, runnable, and close to the text they support. Split or simplify oversized code blocks and images. Test commands and snippets or label them clearly as illustrative.

## Configuration documentation

Treat configuration as a first-class topic. Explain what can be configured, why choices exist, and how they affect behavior. Lead from a safe minimal setup to advanced scenarios instead of presenting an unexplained option dump.

Put a `Configuration` section in the nearest relevant README. Move a long catalog to `docs/configuration.md` and link to it. Co-locate example files with the code that consumes them, and include a small `examples/` directory of runnable samples referenced by the docs. Pin examples to known-good versions where practical.

Provide:

- A minimal copyable configuration that works out of the box, followed by what it does and when to use it.
- Small configurations for common user goals and deployment modes.
- Supported formats and exact file locations or platform-specific search order, including multi-file merge rules.
- Dedicated environment-variable guidance that mirrors the option fields, with relevant systemd, container, and CI notes.
- Mappings among CLI flags, environment variables, and configuration keys, including exact precedence.
- Startup validation, representative errors, troubleshooting steps, and links to relevant logs or debug commands.
- A migration section with before and after examples when configuration changes between versions, linked to the ADR or release notes.

Group the option catalog by feature or component, not alphabetically. Use the same field order for every option:

1. Exact name as used by configuration files, CLI flags, or environment variables.
2. Purpose and why a reader would change it.
3. Type, allowed values, numeric units, and duration syntax.
4. Actual default, including the rule for a dynamic default.
5. Whether it is required and under which modes or conditions.
6. Supported sources, exact precedence, and merge behavior.
7. Whether changes apply immediately, on reload, or on restart, including any hot-reload trigger.
8. Security handling for sensitive values. Use clearly marked placeholders, never real secrets, and show safe sources such as environment variables, secret files, or vault references.
9. Interactions with other options, feature gates, or modes, including conditions where the option is ignored.
10. Version history for additions, changes, or deprecations, with relevant ADR or release-note links.
11. A minimal contextual example.

## Self-verification

Before presenting documentation, mark each item Pass or Fail. Revise and repeat until no Fail remains:

- Discovery covered all documentation in scope. The result contains no unresolved contradiction or unnecessary duplication.
- User, developer, and maintainer needs relevant to the scope are covered in their canonical artifacts.
- The root README is immediately usable, module READMEs exist where needed beside their code, and `CONTRIBUTING.md` is current for setup, testing, branching, review, and release guidance.
- Root, module, and low-level content is placed correctly and cross-linked without large duplicated sections.
- The heading outline is coherent. Sections orient readers and end with an action or clear stopping point.
- Tone is plainspoken and non-promotional. Paragraphs answer reader questions, prerequisites are explicit, and terminology is consistent.
- Examples are minimal, runnable, explained, and tested or clearly marked illustrative.
- Internal, external, anchor, and file-path links are descriptive, contextual, stable, and verified.
- Only necessary content changed. Existing tone and structure remain unless a justified clarity or consistency need required otherwise. Removed content has no dependents.
- Markdown, ASCII punctuation, code formatting, and exact UI output are correct. No smart punctuation or decorative symbols remain.
- Headings support screen readers. Images and code blocks remain scannable.
- Commands, defaults, precedence, security guidance, and other behavioral claims match the software or are qualified accurately. Version references are current or pinned with a rationale.
- The eventual commit message explains why the documentation changed.
- A heading-only skim, narrow-terminal view, link check, and ASCII check all pass. Readers can stop at natural points without missing required steps.
