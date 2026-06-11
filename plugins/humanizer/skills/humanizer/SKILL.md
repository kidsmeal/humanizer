---
name: humanizer
description: Rewrite or draft prose (READMEs, docs, release notes, landing copy, PR descriptions, commit bodies) so it reads like a working engineer wrote it rather than an LLM. Strips the AI tells: em-dashes, inline emphasis bolding, "X, not Y" antithesis, punchy fragment closers, cryptic section titles, fake-profound restatements. Use when writing or editing any human-facing prose, or when the user says "humanize this", "make this sound less AI", "this reads like marketing", or points at a doc that sounds generated. Grammar stays correct and professional; only the AI cadence goes.
---

# Humanizer

Make written prose read like a competent engineer wrote it for other engineers. The target is plain, direct technical writing with full correct grammar and capitalization (published prose gets full grammar even if the user chats casually). The thing to remove is the LLM *cadence*, the set of habits that make a reader think "a model wrote this."

The tells below are specific and mechanical. All of them appeared in one real LLM-generated project README, and each has a concrete before/after taken from it.

## The core test

Before keeping a sentence, ask: would an engineer write this in a doc, or is it a flourish a model added to sound insightful? If it restates the previous sentence in a more profound-sounding way, adds a rhetorical contrast, or ends a paragraph with a confident fragment, cut it. State the fact and move on.

## Principles (the why behind the tells)

The tells further down are surface patterns. These are the underlying rules that generate them. When a case isn't covered by a tell, decide from these.

1. **Plainness signals confidence.** Dressing a fact in rhetoric implies it can't stand alone. A strong claim stated flat ("It has zero dependencies and runs entirely on local files") outperforms the same claim wrapped in a flourish. If a sentence's only job is to make the previous sentence sound impressive, deleting it strengthens the doc.
2. **Describe the user's chair, never the system's internals.** "Technically accurate" is not the bar; accurate to what the reader will experience is. "Injects a brief into the session" was true and still misleading, because the reader pictures seeing a brief on screen and then doesn't. Say what the person will observe first, then the mechanism.
3. **Headings are navigation.** A heading should let a stranger decide whether to skip the section before reading it. If it only makes sense after reading the section, it failed. Boring headings are a feature.
4. **Emphasis is a budget.** Bold ten phrases and bold means nothing. Spend formatting on structure (labels, definitions, headers) and let word choice carry stress. A sentence that needs bolding to land is built wrong.
5. **Say each thing once, where it belongs.** A fact repeated in two places with different phrasing reads as padding and will drift apart as the doc evolves. One home per fact also keeps claims auditable against the code.
6. **Explain constraints by their reason, then stop.** "No cross-repo cursor, because that would need a sync layer, which is a different tool" persuades. "These are deliberate no's" performs. Readers trust the first kind and smell the second.
7. **The review pass must be mechanical.** You will violate your own style rules while writing about them (this skill shipped with an em-dash in its description). Grep for the dash, read every heading cold, check every paragraph's last sentence. Don't trust vibes.

## The tells, with real fixes

### 1. Em-dashes
Hard rule: no em-dashes (`—`) anywhere. Not in prose, not in lists, not in commit messages. Replace with a period, a comma, parentheses, or a restructured sentence.

- Before: `So however a session ends, the breadcrumb is at most one turn stale — and it follows the branch you were on.`
- After: `The checkpoint is written after every turn, so it stays current no matter how the session ends. It's stored per branch.`

### 2. Inline emphasis bolding
Humans don't bold a random word or phrase in the middle of a running sentence to stress it. The words should carry the emphasis, or the sentence should be reworked.

- Before: `what shipped **on this branch** since you were last here`
- After: `what shipped on this branch since you were last here`

Bold is still fine as a structural device: the lead term of a definition-style bullet (`- **NOW cursor.** One active thread at a time...`), a table header, or a named UI label. The rule is that bold may label a thing where it is defined, and may never stress a phrase inside a running sentence.

### 3. "X, not Y" antithesis
The rhetorical contrast where you assert something by negating its opposite. One in a long doc is fine. As a recurring beat it's the single loudest AI tell.

- Before: `Scope the cursor to the **work you're doing**, not the repo.`
- After: `Point the cursor at one piece of work. If a feature spans several repos, run it in each one so every repo keeps its own slice.`

Related forms to kill: "These are deliberate no's, not unbuilt features." "It's a continuity tool, not a task runner." Just say what it is.

### 4. Punchy fragment closers
The short confident sentence or fragment dropped at the end of a paragraph to land a point. "That is it." "That's the point." "By design." "Simple as that." Delete them. The paragraph already made the point.

- Before: `...Claude reads the repo, proposes your one active thread and its next tiny step, and you confirm or correct it. That is it.`
- After: `...Claude reads the repo, proposes your one active thread and its next step, and you confirm or correct it.`

### 5. Fake-profound restatement
A sentence whose only job is to re-say the previous one in a way that sounds like a takeaway. Often starts with "So" or "Which means" and ends with a flourish. If removing it loses no information, remove it.

- Before: `The single-cursor constraint is the point — that limit is what keeps you finishing things instead of accumulating half-done threads.`
- After: `It holds one active thread per repo so you finish work instead of stacking up half-done threads.`

### 6. Cryptic / cutesy section titles
Section headings should say what the section is about in plain words a reader can scan. Not a clever phrase that only makes sense after you've read the section.

- Before: `## Your cursor follows the branch`
- After: `## Branch switching` (or `## How it works across git branches`)

### 7. Other model habits to drop
- Three-beat lists for rhythm ("fast, reliable, and clean") when two items or a plain sentence would do.
- Filler intensifiers: "incredibly", "seamlessly", "effortlessly", "powerful", "robust", "simply".
- Sentence-fragment list cadence used as a flourish: "Many small features in one repo, each on its own branch, each keeping its own cursor, with no manual thread-tracking." Make it a real sentence.
- Tagline closers that wrap a section like an ad ("Focus, captured.").
- Opening a doc by restating its own title back ("X is a tool that lets you X.").

## How a human technical writer actually sounds

- Leads with the concrete thing the reader wants (what it does, how to run it), not a mission statement.
- Uses normal sentences of varied length. Doesn't manufacture rhythm.
- Lets facts stand without a rhetorical frame around them.
- Bolds and bullets to organize, not to perform.
- Is willing to be plain and even a little flat. Plain writing reads as honest. Flourishes read as generated.

## Revision procedure

When handed a draft (or after writing one), do a dedicated pass:

1. Search the text for `—`. Remove every one.
2. Search for `**` inside running sentences. Keep only structural/label bolding; strip emphasis bolding.
3. Read each paragraph's last sentence. If it's a fragment or a restatement that adds no fact, cut it.
4. Find every "X, not Y" / "not A but B" construction. Keep at most one in the whole document; rewrite the rest as plain statements.
5. Read every section heading cold. If it wouldn't tell a stranger what's in the section, rename it.
6. Strip filler intensifiers and tagline closers.
7. Read it out loud in your head. Anywhere it sounds like it's trying to impress, flatten it.

The output should be something an engineer who hates AI-sounding writing would read without flinching. When unsure between two phrasings, pick the plainer one.
