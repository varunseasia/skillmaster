# SkillMaster Release Notes

---

## v7.0.1 (2026-10-21)

This release refines brainstorming questions following the v7.0.0 rebuild, trims skills to stay under Claude Code's compaction threshold, and introduces human-authored summary requirements for contributions.

### Brainstorming
- **Context-First Exploration**: Before asking questions, brainstorming inspects existing project files, plans, and specifications to avoid asking for details already provided (#2560).
- **Targeted Questioning**: Replaced generic opening prompts with tailored questions based on discovered context, explaining the design impact of each answer (#2560, #2571).
- **Conditional Skip**: Only offers to skip questions if brainstorming was automatically triggered, preserving intent when explicitly invoked via `/brainstorming` (#2560).
- **Enforced Design Review**: Requires presenting alternative approaches and securing design document approval before allowing implementation to start (#2571).

### Compaction Resilience
- **`subagent-driven-development`**: Moved fix loop, final review, report handling, resuming, and preflight guidance to `references/` files loaded on-demand (#2565).
- **`writing-skills`**: Moved templates, discovery, and bulletproofing guidance to `references/` (#2564).
- **`executing-plans`**: Streamlined instructions and removed redundant workflow repetitions (#2563).
- **Size Enforcement**: Added CI check (`tests/skill-size/test-skill-size.sh`) ensuring skills stay safely under 20,000 characters.

### Contributing & Diagnostics
- **Human-Authored Summaries**: Pull request and issue templates now require a human-written summary (<200 words) (#2569).
- **Policy on Third-Party Issues**: Prohibits agent-generated PRs addressing issues not reported by your partner unless explicitly requested by maintainers (#2569).
- **`diagnosing-skillmaster`**: Updated to draft bug reports complying with new template requirements (#2569).

---

## v7.0.0 (2026-10-09)

Major rebuild of the brainstorming experience, native plugin support for the Antigravity Marketplace, and extensive Windows/hook reliability improvements.

### Brainstorming
- **Interactive Intent Extraction**: Draws out core objectives through structured questions before proposing architectural solutions (#2463).
- **Dynamic Task Sizing**: Automatically distinguishes between quick changes (resolved in chat) and complex projects requiring formal design documents (#2463).
- **Builder Verification**: Dispatches an independent builder subagent to review specs for ambiguities and implementation gaps before user sign-off (#2463).
- **Implementation Ownership**: Clarifies language, platform, and framework decisions explicitly (#2482, #2483).
- **Visual Companion Security**: Fixed string substitution bugs to ensure special characters (e.g. `$'`, `$&`) render literally (#2362, #2364).

### Antigravity Integration
- **Native Plugin Manifest**: Added `.antigravity-plugin/plugin.json` for seamless installation via `agy plugin install` (#2479).
- **Skill Discovery Bootstrap**: Antigravity agents automatically load the `using-skillmaster` bootstrap at session startup through description indexing (#2479).

### Hooks & Windows Reliability
- **Resilient SessionStart**: Hook execution no longer depends on external binaries (`cat`, `dirname`, `bash`) on startup `PATH` (#2310, #2349).
- **Git for Windows Detection**: `run-hook.cmd` auto-discovers per-user Git installs and avoids non-functional WSL bash launchers on Windows (#1863, #2393).
- **Security & Error Codes**: `run-hook.cmd` ignores current-directory executables and reliably forwards hook exit codes (#2393).

### SDD & Execution
- **`sdd-workspace`**: Preserves existing `.skillmaster/sdd/.gitignore` files (#2161, #2399).
- **Plan Paths on Windows**: Standardized repo-relative plan marker paths across Git Bash and native Windows (#2149).
- **Executing Plans**: `task-done` reliably records tasks whose test commands produce no stdout (#2385, #2388).
- **Systematic Debugging**: Diagnostic examples updated to prevent sensitive token/environment leakage in logs (#2375, #2380).
- **Hermes Agent**: Updated plugin registration to export full skill descriptions (#2284).

---

## v6.4.2 (2026-09-25)

Introduces lean, decision-focused implementation planning to reduce model token consumption and prevent premature code scaffolding during planning.

### Writing Plans
- **Decision-Focused Plans**: Plans record interface contracts, function signatures, test assertions, and expected outputs instead of full code transcriptions (#2333).
- **Token Efficiency**: Reduces planning token usage by ~70% and cuts planning time significantly while maintaining full execution accuracy (#2333).
- **Self-Review Proportions**: Enforces self-review to ensure plan length remains proportionate to the underlying design specification (#2333).
- **Step Sizing**: Standardizes tasks around verifiable outcomes rather than arbitrary time estimates (#2333).

### Documentation
- **Canonical Guidelines**: Retired `CLAUDE.md` in favor of `AGENTS.md` as the authoritative contributor standard.

---

## v6.4.1 (2026-09-18)

Introduces session forensics with `diagnosing-skillmaster`, native inline plan execution, and expanded platform integrations.

### New Skills
- **`diagnosing-skillmaster`**: Forensic analysis skill that diagnoses failed sessions (missed skills, plan deviations, runaway loops) using transcripts and provides line-level evidence (#2236, #2287).

### Executing Plans & Review
- **Native Inline Execution**: Rebuilt `executing-plans` to execute tasks inline under shared workspace rules with a single whole-branch review upon completion, offering a fast, cost-effective alternative to multi-agent dispatch (#2318).
- **Review Focus Section**: Implementation plans include targeted failure modes and boundary conditions for reviewers to inspect (#2319).
- **Project-Wide TDD Scope**: TDD requires executing full project test suites to detect regressions in neighboring files (#2110).

### Subagent-Driven Development
- **Plan-Scoped Workspaces**: Workspaces use dedicated directories (`.skillmaster/sdd/<plan-basename>/`) to eliminate cross-plan collision and state pollution (#2138, #2045).
- **Review Package Validation**: `review-package` validates `BASE..HEAD` git commit ranges before initiating reviews (#2136, #2050).

### Harness Expansions
- **OpenCode 2.0.4+**: Added native V2 skill registration and session lifecycle context hooks (#2106, #2306).
- **Muse**: Added native plugin manifest and SessionStart support (#2317).
- **Qwen Code**: Added marketplace installation documentation (#2132).
- **Permission Resilience**: Script execution across skills invokes explicit interpreters (`bash`, `node`) when package managers strip execution bits (#2301).

---

## v6.3.0 (2026-08-12)

Broadens harness compatibility, scales brainstorming ceremony by task complexity, and improves SDD controller resilience.

### Harness Support
- **Devin CLI**: Added native plugin support with auto-triggering skills (#1995).
- **Hermes Agent**: Added native registration and session startup bootstrap (#1922, #2025).
- **Grok Build CLI**: Added installation and configuration guides (#1919).

### Workflows & Execution
- **Dynamic Brainstorming**: Ceremonies dynamically scale across spike, bounded, and architectural scopes (#2063).
- **SDD Non-Blocking Conflicts**: Controllers resolve minor plan ambiguities without blocking user sessions (#2077).
- **Subagent Batching**: Micro-tasks batch into single dispatches to reduce agent overhead (#2078).
- **Worktree Safety**: `finishing-a-development-branch` prompts before deleting worktrees with untracked files (#2016, #2024).
- **Codex Optimization**: Converted subagent polling into event-driven waits (#2060, #2061).

---

## v6.2.0 (2026-07-23)

Major compression of core skills, implementer resume loops, and Windows hook fixes.

### Subagent-Driven Development
- **Persistent Plan Scoping**: Strict workspace isolation under `.skillmaster/sdd/<plan-basename>/` with automatic ledger cleanup after final review (#2045).
- **Implementer Resume Loop**: Code review fix cycles resume the active implementer subagent rather than spawning new sessions, guarded by a 5-round circuit breaker (#2050).

### Skills Optimization & TDD
- **House Style Compression**: Removed promotional text, folding core arguments into Excuse/Reality rationalization tables across all skills.
- **`writing-good-tests.md`**: Rebuilt TDD testing guidance around falsifiability and mutation checks, replacing testing anti-patterns documentation.
- **Finishing Branches**: Forge-agnostic PR creation; discard option restricted to explicit user confirmations.

### Windows & Environment
- **Git Bash Hook Dispatch**: Claude Code SessionStart hooks explicitly declare `shell: "bash"` to resolve Windows PowerShell quote parsing issues (#1751, #1918).
- **Restored Gemini CLI**: Re-enabled Gemini CLI extension support and tool references (#1959).

---

## v6.1.0 & v6.1.1 (June – July 2026)

Bootstrap token optimizations, Codex marketplace integration, and packaging automation.

- **Token Cost Reduction**: Compressed `using-skillmaster` bootstrap and pruned per-harness tool mappings, removing unnecessary walkthrough text.
- **Codex Manifest**: Added `.agents/plugins/marketplace.json` root manifest for Codex marketplace installs; added explicit `hooks: {}` to prevent false SessionStart auto-discovery (#234).
- **Deterministic Packaging**: Added `package-codex-plugin.sh` for verifiable, reproducible plugin archive builds.

---

## v6.0.0 – v6.0.3 (June 2026)

Major architectural overhaul of task reviews, visual companion security hardening, and new platform support.

### Subagent Review Architecture
- **Unified Task Reviewer**: Replaced dual reviewers with a single `task-reviewer-prompt.md` covering both specification compliance and code quality (#1538, #1543).
- **File-Based Diff Hand-off**: `task-brief` and `review-package` write context directly to disk, avoiding expensive text pasting in parent agent contexts (#994).
- **Final Whole-Branch Review**: Completed runs conclude with a comprehensive whole-branch review on top-tier models.
- **Local Worktrees**: Worktrees default to project-local `.worktrees/` instead of global home directories.
- **Scratch Workspace Relocation**: SDD scratch files moved out of `.git/` into `.skillmaster/sdd/` to satisfy harness file permissions (#1780).

### Brainstorming Visual Companion
- **Tab-Scoped Security**: Per-session authentication key stored in cookies guards all HTTP and WebSocket connections (#1014).
- **Sandboxed Server**: File server strictly restricts access to project content directories (#755).
- **Persistent Reconnection**: Visual companion server maintains port and session keys across restarts (#1703).

### Platform Expansions
- Added native support for **Kimi Code**, **Pi**, and **Antigravity**.
- Platform-neutral refactoring across all skill instructions ("dispatch a subagent", "instructions file").

---

## v5.1.0 (2026-04-30)

Removed legacy slash commands, consolidated code review templates, and established AI contributor standards.

- **Removed Legacy Slash Commands**: Deprecated `/brainstorm`, `/write-plan`, and `/execute-plan` stubs in favor of direct skill invocations (#1188).
- **Consolidated Code Review**: Merged persona and checklist into `skills/requesting-code-review/code-reviewer.md`, dispatching general-purpose subagents (#1299).
- **Automatic Worktree Detection**: Detects `GIT_DIR != GIT_COMMON` to prevent redundant nested worktrees (#1121).
- **AI Contributor Guidelines**: Established pre-submission checklists and quality filters in `AGENTS.md` to prevent low-quality agent PRs.
- **OpenCode Bootstrap Caching**: In-memory caching for `getBootstrapContent()` to avoid repetitive file reads on every agent turn (#1202).
