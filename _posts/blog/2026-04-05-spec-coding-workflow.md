---
layout: post
title: "Vibe vs Spec Coding: Why Structure Matters"
categories: [Blog]
tags: [build, ai, workflow]
---

When you're building with AI coding assistants, you fall into one of two patterns:

**Vibe Coding**: "Make this button bigger. Actually, move it left. No wait, make it a dropdown. Can you add a modal?"

**Spec Coding**: "Here's a specification with 5 acceptance criteria. Implement it. Verify each criterion. Done."

I spent a week building [SudoSkills](https://sudoskills.geraldtui.com/) using Spec-Driven Development. Here's what I learned.

## The Problem with Vibe Coding

Vibe coding feels natural but its the "trust me bro" approach to building software. Here's what happens:

```
You: "Build a terminal component"
AI: *Creates basic terminal*
You: "Add command history"
AI: *Adds command history*
You: "The filesystem isn't persisting"
AI: *Patches filesystem*
You: "Now validation is broken"
AI: *Patches validation*
```

<img src="https://content.imageresizer.com/images/memes/same-spider-man-7-meme-4.jpg" alt="Vibe coding in a nutshell" style="width: 50%; display: block; margin: 0 auto;">

<p style="text-align: center;"><em>Vibe coding in a nutshell</em></p>

Three hours later, you've got spaghetti code and a finger pointing contest between you and your AI friend. Worse yet, if your agent struggles with keeping context, your next prompt will be met with "Sorry, new number, who dis?!"

Why? LLMs are [non-deterministic](https://www.youtube.com/watch?v=CQmI4XKTa0U&t=1508s). Without structure, you're navigating a probability space. The exact same prompt is a fresh roll of the dice.

## What is Spec-Driven Development?

Define the feature requirements. Let AI handle the implementation.

Checkout this spec for SudoSkills' virtual filesystem:

```markdown
# Spec 1.1: Virtual Filesystem

**Status**: Approved
**Last Modified**: 2026-03-25

## User Story
As a learner, I want commands to operate on a simulated filesystem,
So that I can practice Linux commands without affecting my real system.

## Technical Specification

**File**: `src/engine/filesystem.ts`
- Class `VirtualFilesystem` with methods: mkdir, touch, ls, cd, pwd, cat, rm
- Path resolution handles /, ./, ../, ~, -
- State is mutable within lesson context
- Can be initialized from lesson data

## Acceptance Criteria
- [ ] AC1: Filesystem supports directories and files with type property
- [ ] AC2: Path resolution handles absolute and relative paths
- [ ] AC3: Operations like mkdir, touch, ls, rm work correctly
- [ ] AC4: Filesystem state persists within a lesson step
- [ ] AC5: Filesystem resets between lesson steps

## Edge Cases
- Empty path defaults to current directory
- Parent of root (cd /) returns root
- Non-existent paths return clear error messages
```

The spec is the contract. The AI implements it. You verify it. Done.

## The Workflow

I created five [Cursor skills](https://docs.cursor.com/context/rules-for-ai#skills) to power the workflow. Skills are markdown files that teach the AI how to perform specific tasks - think of them as reusable instructions the AI follows consistently.

1. **`/spec-create`** - Creates spec from user story, checks for duplicates
2. **`/spec-review`** - Validates spec structure and completeness
3. **`/spec-update`** - Fixes issues, updates status (Draft → Approved)
4. **`/spec-exec`** - Implements code on feature branch, applies Clean Code principles
5. **`/spec-verify`** - Traces each AC through code, confirms edge cases handled

Each feature goes through: Create → Review → Update → Execute → Verify → Git Commit/Merge.

##  Cursor Command: `/auto-sdd`

I created a custom [Cursor Command](https://docs.cursor.com/context/rules-for-ai#commands) that runs the entire Spec-Driven Development Cycle autonomously from start to finish. While Skills teach the AI how to perform individual tasks, Commands orchestrate multiple skills together into a single instruction to be executed by the AI.

The `/auto-sdd` command in action:

```bash
/auto-sdd As a user, I want a "Complete" button with a congratulatory modal so that users can see a message when they complete a lesson.
```

The AI checks for existing specs, creates/updates the spec, reviews it, implements the code, verifies all acceptance criteria, and hands it over to me to me for final review and merge.

From user story to verified implementation in one command. Time: 5 minutes. Zero back-and-forth.


## Living Documentation: Specs

The specs are living documents. They should be reviewed and updated as the codebase evolves. Bake these "spec checks" into your agents using [Cursor Rules](https://cursor.com/docs/rules). Rules provide persistent, reusable context at the prompt level. These are also referred to as "Steering", think of it as influencing the models to a specific outcome.


## When to Use Each Approach

**Use Vibe Coding for:**
- Exploration and prototyping
- Quick styling tweaks
- Simple one-line changes

**Use Spec Coding for:**
- Production features
- Anything with edge cases
- Multi-step implementations
- When you want autonomous AI work

**Use Both:**
- Vibe code to explore
- Spec code to implement

## The Key Insight

Specs bring determinism to non-deterministic LLMs. They provide a clear, unambiguous set of requirements that the AI must follow. Without specs, each AI response is a fresh roll of the dice.

- **Vibe coding**: "Make it work" (infinite interpretations)
- **Spec coding**: "Make AC1-AC5 pass" (finite, testable conditions)

If you're vibe coding and hitting the patch-upon-patch cycle, try specs. Create one spec. Implement it. Verify it. See if it changes how you work with AI.

---
