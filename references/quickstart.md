# Quickstart — what an Agent Skill is, and a minimal working example

> Source: agentskills.io/home (Overview) + agentskills.io/skill-creation/quickstart (official, distilled)

This file absorbs the two things the Overview + Quickstart docs contributed that are worth keeping: the *definition* of an Agent Skill and the *Discovery → Activation → Execution* model, followed by the roll-dice minimal example.

## 1. What an Agent Skill is

An Agent Skill is a lightweight, open format for extending AI agent capabilities with specialized knowledge and workflows. At its core, a skill is a **folder containing a SKILL.md file**:

- **Metadata** (`name` and `description` at minimum) — tells the agent what the skill is and when to use it.
- **Instructions** (the Markdown body) — tells the agent how to perform the task.
- Optionally: **scripts/** (executable code), **references/** (on-demand docs), **assets/** (templates, static resources).

A skill packages procedural knowledge and project/team/user-specific context into a portable, version-controlled folder that agents load on demand.

## 2. How Agent Skills work — the three stages

Agents load skills through **progressive disclosure**, in three stages:

1. **Discovery** — at startup, the agent loads only the `name` and `description` of each available skill, just enough to know when it might be relevant.
2. **Activation** — when a task matches a skill's description, the agent reads the full SKILL.md into context.
3. **Execution** — the agent follows the instructions, optionally running bundled code or loading referenced files as needed.

Full instructions load only when a task calls for them, so the agent can keep many skills on hand with a small context footprint. (This is the same concept as the three *content* tiers in references/specification.md — metadata / instructions / resources — but viewed from the agent's runtime behavior rather than from how you structure the files.)

## 3. A minimal working skill: roll-dice

The smallest complete, runnable skill. It gives an agent the ability to roll dice using a random number generator.

### Prerequisites

- VS Code with GitHub Copilot.

The tutorial uses VS Code, but Agent Skills are an open format — the same skill works in any compatible agent (Claude Code, OpenAI Codex, DSH, etc.).

### Create it

A skill is a folder containing a SKILL.md file. VS Code looks in `.agents/skills/` by default. Create `.agents/skills/roll-dice/SKILL.md`:

```markdown
---
name: roll-dice
description: Roll dice using a random number generator. Use when asked to roll a die (d6, d20, etc.), roll dice, or generate a random dice roll.
---

To roll a die, use the following command that generates a random number from 1 to the given number of sides:

```bash
echo $(( RANDOM % <sides> + 1 ))
```

```powershell
Get-Random -Minimum 1 -Maximum (<sides> + 1)
```

Replace `<sides>` with the number of sides on the die (e.g., 6 for a standard die, 20 for a d20).
```

That's one file, under 20 lines. What each part does:

- **`name`** — a short identifier, must match the folder name.
- **`description`** — tells the agent when to use this skill; this is how the agent decides to activate it.
- **the body** — instructions the agent follows on activation (here, generate a random number via a terminal command, substituting the side count from the user's request).

### Try it out

1. Open your project in VS Code.
2. Open the Copilot Chat panel.
3. Select **Agent mode** from the mode dropdown.
4. Type `/skills` and confirm `roll-dice` appears. If not, check the file is at `.agents/skills/roll-dice/SKILL.md` relative to your project root.
5. Ask: "Roll a d20".

The agent should activate `roll-dice`, possibly ask permission to run a terminal command (allow it), run the command, and return a number between 1 and 20.

> Tool-use reliability varies across models. If the agent answers without running a command, try a different model from the dropdown.

### Why this matters

This is the smallest closed loop: name + description + an actually-executable body. It is the concrete answer to "A minimal working skill is a single SKILL.md file." Use it as the shape of any new skill before adding scripts/, references/, or assets/.

## Next steps

From a working skill:

- references/best-practices.md — how to write skills that are well-scoped and effective.
- references/optimizing-descriptions.md — make the description trigger reliably.
- references/evaluating.md — verify the output is actually good.
- references/using-scripts.md — bundle reusable code.
