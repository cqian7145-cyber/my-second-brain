# AI ENTRYPOINT

> This file is the primary entry point for any AI system working inside this Obsidian Vault.

This Vault is not a simple note archive.

It is a personal AI Knowledge OS composed of:

- Long-term knowledge
    
- Prompt assets
    
- Project memory
    
- Decision records
    
- Workflows
    
- Experiments
    
- Reviews
    
- AI collaboration rules
    

Any AI entering this Vault should treat existing notes as structured context, not as arbitrary text files.

---

# 1. Primary Objective

Your job is to:

1. understand the user's current request;
    
2. retrieve only the context needed;
    
3. respect existing decisions and conventions;
    
4. perform the task;
    
5. evaluate the result;
    
6. write useful knowledge back when appropriate.
    

The desired loop is:

```text
Request
↓
Understand
↓
Retrieve Context
↓
Clarify if necessary
↓
Execute
↓
Evaluate
↓
Record
↓
Distill Knowledge
```

Do not read the entire Vault unless the task genuinely requires it.

Prefer selective context retrieval.

---

# 2. Mandatory Reading Order

For non-trivial tasks, inspect the following files in this order.

## Level 1 — System Rules

Read:

```text
00_System/AI_RULES.md
00_System/CLARIFICATION_PROTOCOL.md
00_System/OUTPUT_STANDARDS.md
```

These define how AI should work in this Vault.

---

## Level 2 — Task-Specific Protocol

Depending on the task, read one or more:

```text
00_System/KNOWLEDGE_PROTOCOL.md
00_System/PROMPT_PROTOCOL.md
00_System/PROJECT_PROTOCOL.md
```

Examples:

Prompt work:

```text
PROMPT_PROTOCOL.md
```

Project work:

```text
PROJECT_PROTOCOL.md
```

Knowledge organization:

```text
KNOWLEDGE_PROTOCOL.md
```

---

## Level 3 — User Context

Read:

```text
00_System/USER_CONTEXT.md
```

only when personal preferences, long-term goals, technical stack, working style, or other persistent user context materially affects the task.

Do not force personal context into unrelated tasks.

The current user request always overrides older context.

Priority:

```text
Current Request
>
Current Project Context
>
Explicit Current Constraints
>
Vault Context
>
Past Decisions
>
General Assumptions
```

---

# 3. Project Context Protocol

When working on an existing project, locate its folder under:

```text
04_Projects/
```

Prefer reading:

```text
00_CONTEXT.md
```

first.

Then inspect only the files relevant to the task.

Typical project structure:

```text
00_CONTEXT.md
01_PROBLEM.md
02_RESEARCH.md
03_REQUIREMENTS.md
04_PLAN.md
05_DECISIONS.md
06_PROMPTS.md
07_EXPERIMENTS.md
08_OUTPUT.md
09_RETROSPECTIVE.md
```

Do not automatically read every project file.

Use:

```text
00_CONTEXT.md
```

as the project index.

---

# 4. Existing Decisions

Before making a major architectural, product, workflow, or technical decision, inspect:

```text
06_Decisions/
```

and the project's own:

```text
05_DECISIONS.md
```

Do not silently reverse an existing decision.

If recommending a change:

1. identify the existing decision;
    
2. explain what changed;
    
3. compare old and new assumptions;
    
4. state why revision is justified.
    

---

# 5. Prompt System

Prompt files live primarily in:

```text
03_Prompts/
```

The Prompt library is an asset repository, not a collection of random prompt text.

Prompt composition concept:

```text
Role
+
Goal
+
Context
+
Task
+
Constraints
+
Examples
+
Output Schema
+
Evaluation
```

Before creating a new Prompt:

1. search for an existing Prompt;
    
2. determine whether it can be reused;
    
3. determine whether it should be extended;
    
4. create a new Prompt only when needed.
    

Avoid duplicates.

---

# 6. Prompt Selection

For complex AI tasks, consider whether existing components exist in:

```text
03_Prompts/Core/
03_Prompts/Roles/
03_Prompts/Tasks/
03_Prompts/Output/
03_Prompts/Evaluators/
03_Prompts/Meta/
```

If a Prompt Router exists, use its rules.

Do not blindly apply large prompts when a smaller reusable component is sufficient.

---

# 7. Prompt Modification Rules

When improving an existing Prompt:

Do not create:

```text
prompt-v2.md
prompt-final.md
prompt-final2.md
```

Instead update the original Prompt's:

```yaml
version:
updated:
quality_score:
```

and add to:

```text
Changelog
```

Use semantic-style incremental versions when practical:

```text
1.0
1.1
1.2
2.0
```

Use a major version when the Prompt's fundamental behavior changes.

---

# 8. Clarification Policy

Before executing a non-trivial task, perform a private Clarification Check.

Evaluate:

```text
Goal
Context
Audience
Constraints
Output
Quality Bar
```

Do not automatically ask questions.

Decision process:

```text
Task clear?
→ Execute.

Important information missing?
→ Can a safe assumption solve it?

Yes
→ state the assumption if important and continue.

No
→ ask only the highest-value questions.
```

Ask at most 3–5 questions.

Never ask information already available in:

- current conversation;
    
- project context;
    
- system files;
    
- existing notes.
    

---

# 9. Context Retrieval Strategy

Use progressive disclosure.

Start with the minimum context required.

Preferred sequence:

```text
Current Request
↓
System Rules
↓
Project Context
↓
Related Decisions
↓
Related Knowledge
↓
Related Prompts
```

Avoid loading unrelated notes.

More context is not automatically better context.

The objective is:

> highest relevance per token.

---

# 10. Knowledge Search

Reusable knowledge is primarily stored in:

```text
02_Knowledge/
```

Typical domains may include:

```text
AI
Prompt-Engineering
Coding
Product
Business
Marketing
English
Robotics
Finance
Meta-Learning
```

When using Vault knowledge:

- distinguish facts from assumptions;
    
- distinguish evergreen knowledge from old project context;
    
- prefer the most recently updated relevant source;
    
- do not treat AI-generated notes as unquestionable truth.
    

---

# 11. Writing Back to the Vault

Do not modify files merely because you can.

Write back when the work creates durable value.

Examples:

### Write to project files when:

- requirements changed;
    
- a major decision was made;
    
- an experiment occurred;
    
- project state changed;
    
- new output was produced.
    

### Write to Knowledge when:

- a reusable principle was learned;
    
- a general method was discovered;
    
- a technical lesson applies beyond one project;
    
- research produced durable insight.
    

### Write to Decisions when:

- a meaningful long-term tradeoff was resolved.
    

### Write to Experiments when:

- a hypothesis was tested;
    
- Prompt A/B testing occurred;
    
- an implementation approach was compared.
    

---

# 12. Knowledge Distillation

Do not copy large AI responses directly into permanent Knowledge.

Distill them.

Ask:

```text
What is reusable?

What is project-specific?

What is evidence?

What is an assumption?

What changed my understanding?

What should be connected to other notes?
```

Evergreen knowledge should normally contain:

```text
Summary
Core Idea
Why It Matters
Details
Examples
Application
Related Notes
Sources
```

---

# 13. AI Output Evaluation

For important work, evaluate the result before finalizing.

Check:

```text
Correctness
Completeness
Relevance
Clarity
Actionability
Specificity
Consistency
```

Do not expose hidden chain-of-thought.

If useful, provide concise evaluation such as:

```text
Assumptions
Known gaps
Risks
Recommended verification
```

---

# 14. File Modification Safety

Before modifying files:

1. inspect existing content;
    
2. preserve valuable information;
    
3. prefer incremental edits;
    
4. avoid unnecessary rewrites;
    
5. do not delete files unless explicitly instructed.
    

When uncertain:

prefer preservation over deletion.

---

# 15. Archive Policy

The system follows:

> Archive, don't destroy.

Old material belongs in:

```text
09_Archive/
```

Deprecated Prompts belong primarily in:

```text
03_Prompts/Deprecated/
```

Completed projects should remain traceable.

Historical reasoning has value.

---

# 16. Inbox Policy

`01_Inbox` is temporary.

Do not turn it into permanent storage.

Inbox lifecycle:

```text
Capture
↓
Process
↓
Distill
↓
Move
```

---

# 17. Model Independence

This system may be used by:

- ChatGPT
    
- Claude
    
- Codex
    
- Cursor
    
- Gemini
    
- DeepSeek
    
- future AI systems
    

Do not create unnecessary model-specific dependencies.

Core principle:

> Models are replaceable. Knowledge assets are persistent.

---

# 18. Tool-Specific Files

If you are Claude Code, also read:

```text
CLAUDE.md
```

If you are a coding or autonomous agent using AGENTS conventions, also read:

```text
AGENTS.md
```

Tool-specific rules extend this file.

They do not override the user's explicit request.

---

# 19. Default Completion Pattern

For complex work, aim to finish with:

```text
What was done
What changed
Why
Validation
Remaining risks
Recommended next action
```

For simple work, be concise.

Do not generate unnecessary reports for trivial tasks.

---

# 20. Core Principle

The Vault exists to create a compounding loop:

```text
Knowledge
↓
Better Context
↓
Better Prompts
↓
Better AI Output
↓
Better Decisions
↓
Better Projects
↓
Experience
↓
Better Knowledge
```

Every meaningful interaction should strengthen this loop rather than add noise.