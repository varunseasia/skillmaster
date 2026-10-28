# SkillMaster


SkillMaster is a complete software development methodology for your coding agents, built on top of a set of composable skills and some initial instructions that make sure your agent uses them.


## Table of Contents


- [How it works](#how-it-works)

- [Commercial Services](#commercial-services)

- [Installation](#installation)

- [The Basic Workflow](#the-basic-workflow)

- [When Something Goes Wrong](#when-something-goes-wrong)

- [Community](#community)

- [What's Inside](#whats-inside)

- [Philosophy](#philosophy)

- [Contributing](#contributing)

- [Updating](#updating)

- [License](#license)

- [Visual companion telemetry](#visual-companion-telemetry)


## How it works


It starts from the moment you fire up your coding agent. As soon as it sees that you're building something, it **doesn't** just jump into trying to write code. Instead, it steps back and asks you what you're really trying to do. 


Once it's teased a spec out of the conversation, it shows it to you in chunks short enough to actually read and digest. 


After you've signed off on the design, your agent puts together an implementation plan that's clear enough for an enthusiastic junior engineer with poor taste, no judgement, no project context, and an aversion to testing to follow. It emphasizes true red/green TDD, YAGNI (You Aren't Gonna Need It), and DRY. 


Next up, once you say "go", it launches a **subagent-driven-development** process, having agents work through each engineering task, inspecting and reviewing their work, and continuing forward. It's not uncommon for your agent to work autonomously for a couple hours at a time without deviating from the plan you put together.


There's a bunch more to it, but that's the core of the system. And because the skills trigger automatically, you don't need to do anything special. Your coding agent just has SkillMaster.


## Commercial Services


If you're using SkillMaster in enterprise and could benefit from commercial support, additional tooling, or managed spending, please don't hesitate to drop us a line at sales@primeradiant.com.


## Installation

Use the command for your coding agent below.

 Coding Agent | Repository Installation Instructions |
|---|---|
| Google Antigravity | `agy plugin install https://github.com/varunseasia/skillmaster` |
| Devin CLI | `devin plugins install varunseasia/skillmaster` |
| Gemini CLI | `gemini extensions install https://github.com/varunseasia/skillmaster` |
| Kimi Code | `/plugins install https://github.com/varunseasia/skillmaster` |
| Pi | `pi install git:github.com/varunseasia/skillmaster` |
| Qwen Code | `qwen extensions install varunseasia/skillmaster` |
| Hermes Agent | `hermes plugins install varunseasia/skillmaster --enable` |
| OpenCode | Follow the [installation instructions](https://raw.githubusercontent.com/varunseasia/skillmaster/main/.opencode/INSTALL.md). |
| Muse | Clone the repository, then install and approve the plugin as shown below. |

### Muse installation

```bash
git clone https://github.com/varunseasia/skillmaster.git
muse plugins install ./skillmaster
muse plugins approve skillmaster
```

After installing, restart any active Muse sessions so the session-start hook can take effect.

## The Basic Workflow


1. **brainstorming** - Activates before building anything. Draws out what you actually want, and why, through questions that get you describing it, then plays its understanding back for correction. Small changes get agreed in chat; projects get a written design document a builder could plan from.


2. **using-git-worktrees** - Activates after design approval. Creates isolated workspace on new branch, runs project setup, verifies clean test baseline.


3. **writing-plans** - Activates with approved design. Breaks work into bite-sized tasks (2-5 minutes each). Every task has exact file paths, complete code, verification steps.


4. **subagent-driven-development** or **executing-plans** - Activates with plan. Either dispatches a fresh subagent per task with a review after each (most thorough), or implements every task inline in the current session with one fresh review of the whole branch at the end (cheapest).


5. **test-driven-development** - Activates during implementation. Enforces RED-GREEN-REFACTOR: write failing test, watch it fail, write minimal code, watch it pass, commit. Deletes code written before tests.


6. **requesting-code-review** - Activates between tasks. Reviews against plan, reports issues by severity. Critical issues block progress.


7. **finishing-a-development-branch** - Activates when tasks complete. Verifies tests, presents options (merge/PR/keep/discard), cleans up worktree.


**The agent checks for relevant skills before any task.** Mandatory workflows, not suggestions.


## When Something Goes Wrong


Sometimes a session misbehaves: a skill fires when it shouldn't, stays silent when it should, or the agent ignores its plan, repeats work, or burns more tokens than you'd expect. Ask your coding agent to "figure out what went wrong with skillmaster in this session" and it will invoke the **diagnosing-skillmaster** skill. To examine an earlier session, name it: "figure out what went wrong with skillmaster in session `\<id>`".


The skill reads the session transcript, reports what happened with line-level evidence, and, if you want, packages a scrubbed bundle for a bug report.


## Community


SkillMaster is built by [Jesse Vincent](https://blog.fsck.com) and the rest of the folks at [Prime Radiant](https://primeradiant.com).


- **Discord**: [Join us](https://discord.gg/35wsABTejz) for community support, questions, and sharing what you're building with SkillMaster

- **Issues**: https://github.com/skillmaster/skillmaster/issues

- **Release announcements**: [Sign up](https://primeradiant.com/skillmaster/) to get notified about new versions


## What's Inside


### Skills Library


**Testing**

- **test-driven-development** - RED-GREEN-REFACTOR cycle (includes testing anti-patterns reference)


**Debugging**

- **systematic-debugging** - 4-phase root cause process (includes root-cause-tracing, defense-in-depth, condition-based-waiting techniques)

- **verification-before-completion** - Ensure it's actually fixed

- **diagnosing-skillmaster** - Work out what went wrong in a session, with evidence; export a scrubbed bundle or file an issue


**Collaboration** 

- **brainstorming** - Drawing out intent before building

- **writing-plans** - Detailed implementation plans

- **executing-plans** - Inline plan execution: one context, one final review

- **dispatching-parallel-agents** - Concurrent subagent workflows

- **requesting-code-review** - Pre-review checklist

- **receiving-code-review** - Responding to feedback

- **using-git-worktrees** - Parallel development branches

- **finishing-a-development-branch** - Merge/PR decision workflow

- **subagent-driven-development** - Fast iteration with two-stage review (spec compliance, then code quality)


**Meta**

- **writing-skills** - Create new skills following best practices (includes testing methodology)

- **using-skillmaster** - Introduction to the skills system


## Philosophy


- **Test-Driven Development** - Write tests first, always

- **Systematic over ad-hoc** - Process over guessing

- **Complexity reduction** - Simplicity as primary goal

- **Evidence over claims** - Verify before declaring success


Read [the original release announcement](https://blog.fsck.com/2025/10/09/skillmaster/).


## Contributing


The general contribution process for SkillMaster is below. Keep in mind that we don't generally accept contributions of new skills and that any updates to skills must work across all of the coding agents we support. We also don't accept agent-written PRs that fix issues filed by someone else unless a maintainer asked for one, even when you hit the same problem; add your report to the existing issue instead.


1. Fork the repository

2. Switch to the 'dev' branch

3. Create a branch for your work

4. Follow the `writing-skills` skill for creating and testing new and modified skills

5. Submit a PR, filling in every section of the pull request template, including the summary a human writes in their own words.


Skill-behavior tests use the drill eval harness from [skillmaster-evals](https://github.com/prime-radiant-inc/skillmaster-evals/), cloned into `evals/` — see `evals/README.md` for setup. Plugin-infrastructure tests live at `tests/` and run via the relevant `run-*.sh` or `npm test`.


See `skills/writing-skills/SKILL.md` for the complete guide.


## Updating


SkillMaster updates are somewhat coding-agent dependent, but are often automatic.


## License


MIT License - see LICENSE file for details


## Visual companion telemetry


Because skills and plugins don't provide any feedback to creators, we have no idea how many of you are using SkillMaster. By default, the Prime Radiant logo on brainstorming's optional visual companion feature is loaded from our website. It includes the version of SkillMaster in use. It does not include any details about your project, prompt, or coding agent. We don't see your clicks or anything about what you're building. This helps us have a rough idea of how many folks are using SkillMaster and which version of SkillMaster they're using. It's 100% optional. To disable this, set the environment variable `SKILLMASTER_DISABLE_TELEMETRY` to any true value. SkillMaster also honors Claude Code's `DISABLE_TELEMETRY` and `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` opt-outs.
