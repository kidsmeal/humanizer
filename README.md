# Humanizer

A [Claude Code](https://claude.com/claude-code) skill that makes Claude write
prose like an engineer instead of an LLM.

LLM-generated docs have a recognizable cadence: em-dashes everywhere, bolded
phrases mid-sentence, rhetorical contrasts ("it's a tool, not a framework"),
short punchy closers ("That is it."), section titles that only make sense
after you've read the section. Each habit on its own is survivable. Together
they make a README read like a pitch deck, and readers have learned to spot
them.

This skill teaches Claude to drop those habits when it writes or edits
READMEs, docs, release notes, landing copy, PR descriptions, and commit
bodies. Grammar stays correct and professional. The doc stops sounding
generated.

## Install

```
/plugin marketplace add kidsmeal/humanizer
/plugin install humanizer@humanizer
```

If you install during an existing session, run `/reload-plugins` to activate
it.

## What it does

Once installed, the skill activates whenever Claude writes or edits
human-facing prose, or when you say things like "humanize this" or "make this
sound less AI". It applies two layers:

- Seven principles that explain why the AI cadence fails (plainness signals
  confidence, headings are navigation, emphasis is a budget, and so on).
- Nine concrete tells with before/after examples: em-dashes, inline emphasis
  bolding, "X, not Y" antithesis, fragment closers, fake-profound restatement,
  cryptic headings, throat-clearing and inflated changelog verbs, emoji and
  uniform structure, and filler habits.

It ends with a mechanical revision checklist (grep for the dash, read every
heading cold, check every paragraph's last sentence) so the cleanup pass
doesn't rely on judgment alone.

## What it doesn't do

It doesn't make writing casual or sloppy. The target is plain technical
writing with full grammar, the kind a careful engineer produces. It also
doesn't touch code, comments, or commit subject-line conventions; it's about
prose.

## Origin

The before/after examples come from a real README that one of my own projects
shipped with. A reader called out the AI cadence line by line, and this skill
is that critique turned into rules Claude can follow.

## License

MIT. See [LICENSE](LICENSE).
