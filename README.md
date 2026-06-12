# Expert Project — Ship your workflow as a plugin

Across the course you've built scoped subagents, orchestrated them into workflows, written skills, commands, and hooks, and learned how a plugin packages all of it. Now you'll put it together into one real, shareable thing: a plugin that carries a multi-agent workflow, tested against a live codebase and published so anyone can install it with a single command.

Lean on Claude as you go: ask it to scaffold files, look up current syntax, and review your structure. That's how you'd really build one.

---

## The repository

We've provided a repo for your plugin — this is where you'll assemble and publish it. Build and test it against the course API project you've worked in since the Beginner projects, so your subagents and workflow operate on real code rather than a toy example.

By the end, the provided repo will be two things at once: the plugin itself, and the marketplace that offers it.

---

## What you'll build

A single plugin that bundles:

- Two scoped subagents — purpose-built workers, each limited to the tools and model its job needs.
- A multi-agent workflow command — one command that runs those subagents as a workflow, with at least one parallel step and one dependent step.
- A skill and a hook — to round the plugin into a complete setup, not just agents.
- A manifest, a README, and a marketplace catalog — so the plugin installs by name and you can version it.

---

## Before you start

- Have the provided repo cloned, and the course API project handy to test against.
- Skim back over Unit 8 (creating, scoping, and orchestrating subagents) and Unit 9 (plugin structure, `${CLAUDE_PLUGIN_ROOT}`, marketplaces, versioning) — you'll use all of it.
- Decide on a theme for your plugin so the pieces belong together. A 'code quality' plugin (review + tests + a formatting hook) or a 'release' plugin (changelog + version bump + checks) both work well.

---

## Tasks

**Task 1 — Scaffold the plugin.**
In the provided repo, create the plugin structure: `.claude-plugin/plugin.json` with a `name` and a starting `version`, the component folders you'll need at the root, and a `README.md`. Confirm the layout is right — only `plugin.json` lives inside `.claude-plugin/`, everything else sits at the root.

**Task 2 — Build two scoped subagents.**
Add at least two custom subagents in `agents/`. For each one:
- a `description` that names when to use it, in the words a real situation would use;
- a body that says what to do and what to return;
- a `tools` line limited to what the job actually needs (read-only workers get read-and-search tools only);
- a `model` matched to the difficulty of the job.

Make them genuinely different — for example, a read-only reviewer and an agent that writes or edits.

**Task 3 — Orchestrate them into a workflow command.**
Add a command in `commands/` whose body runs your subagents as a workflow. It must include at least one parallel step and one dependent (sequential) step — independent work running together, and a step that waits for earlier results. Write the orchestration in plain language, the command is the one-word trigger for the whole flow.

**Task 4 — Round out the bundle: a skill and a hook.**
Add one skill (`skills/<name>/SKILL.md`) and one hook (`hooks/hooks.json`) that fit your plugin's theme. If the hook runs a bundled script, reference it through `${CLAUDE_PLUGIN_ROOT}` so it resolves wherever the plugin installs — don't hardcode a path.

**Task 5 — Test it locally.**
Load the plugin with `claude --plugin-dir .` from the repo root and run it against the course API project. Confirm each piece fires by its namespaced name, and that your workflow command orchestrates the subagents in the right order. Use `/reload-plugins` to pick up edits as you refine. Fix anything that doesn't load — start by checking folder placement.

**Task 6 — Publish it as a marketplace.**
Add `.claude-plugin/marketplace.json` listing your plugin (its `name` matching `plugin.json`, `source: "./"`, a one-line description). Commit and push. Then, as your own first user, run `/plugin marketplace add <your repo>` and `/plugin install <your-plugin>@<your-marketplace>` in a fresh session, and confirm everything installs and runs cleanly.

**Task 7 — Document it in `NOTES.md`.**
Add a `NOTES.md` to the repo covering:
- what the plugin does and how to install it;
- one scoping decision and why you made it (why a subagent got the tools or model it did);
- why your workflow command runs the steps it does in parallel versus in sequence.

---

## Definition of Done

- [ ] The repo has a valid plugin: `.claude-plugin/plugin.json` with `name` and `version`, component folders at the root, and a `README.md`.
- [ ] At least two subagents in `agents/`, each with a sharp description, a body that states what to return, a scoped `tools` line, and a fitting `model`.
- [ ] A workflow command in `commands/` that orchestrates the subagents with at least one parallel step and one dependent step.
- [ ] A skill in `skills/<name>/SKILL.md` and a hook in `hooks/hooks.json`, both fitting the plugin's theme; any bundled script path uses `${CLAUDE_PLUGIN_ROOT}`.
- [ ] The plugin loads and runs locally via `--plugin-dir` against the course API project.
- [ ] `.claude-plugin/marketplace.json` is present and correct, and the plugin installs cleanly via `/plugin marketplace add` + `/plugin install`.
- [ ] `NOTES.md` covers install steps, one scoping decision, and one orchestration decision.
- [ ] Everything is committed and pushed.

---

## How to submit

Push your plugin repo and share its link. Make sure `marketplace.json`, `plugin.json`, all component folders, the `README.md`, and `NOTES.md` are committed, and that the repo installs as a marketplace from a clean checkout.

---

## How it's checked

A reviewer will:
- read your `marketplace.json` and `plugin.json`, then add and install your plugin from the repo exactly as a teammate would;
- run your workflow command against the course API project and watch it spawn the right subagents in the right order — parallel where the work is independent, sequential where a step depends on an earlier result;
- check that each subagent is scoped sensibly (a read-only worker has no power to write, models match the work) and that the hook's bundled paths use `${CLAUDE_PLUGIN_ROOT}`;
- read `NOTES.md` for your scoping and orchestration reasoning.
