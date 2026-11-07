# AI agent skills

## Purpose
This repository provides modular agent skills, prompt templates, and CLI tools for OpenCode and OpenChamber. Agents load these skills to coordinate subagents, audit security, scale concurrency, and delegate CLI tasks.

## Architecture
Each skill lives in a directory under `skills/` with a `SKILL.md` specification and optional reference guides. Runtimes match skills to tasks using the name and description in each skill's YAML frontmatter.

The repository contains five primary skills:

- **unity-lite.** Lightweight multi-agent coordination workflow with execution group ordering, isolated delegation, and verification gates.
- **teamwork.** Orchestrates multi-phase proof exploration, problem decomposition, parallel solving, adversarial falsification, tournament aggregation trees, and referee verification waves.
- **boost.** Scales parallel subagent execution up to 128 workers using sliding-window dispatch, dynamic refill, temperature asymmetry routing, and quorum advancement.
- **securecoder.** Scans codebases for web vulnerabilities, runs automated security checks, verifies exploit scenarios, and writes remediation audit reports.
- **agy-terminal-subagent.** Dispatches headless subagents through the Antigravity CLI with model routing, reasoning tiers, session continuation, and multi-profile isolation.

## Setup
Add the repository to your environment with the skills CLI:

```bash
npx skills add devsanthoshmk/skills
```

Update installed skills:

```bash
npx skills update devsanthoshmk/skills
```

Install individual skills directly:

```bash
npx skills add devsanthoshmk/skills/skills/unity-lite
npx skills add devsanthoshmk/skills/skills/teamwork
npx skills add devsanthoshmk/skills/skills/boost
npx skills add devsanthoshmk/skills/skills/securecoder
npx skills add devsanthoshmk/skills/skills/agy-terminal-subagent
```

## Usage
Autonomous agents and LLM workflow patterns execute these skills during task runs.

### unity-lite orchestration
The `unity-lite` skill provides structured multi-agent coordination within a self-contained workflow:

- **Direct Main Thread execution.** The Level-0 agent executes single-step tasks directly instead of spawning subagents.
- **Decomposition and planning.** Spawns a Level-1 Planning Agent to structure complex tasks into numbered execution groups:
  ```text
  [Group 1] ──┬── <Task A>
              ├── <Task B>
              └── <Task C>
                   │
                   ▼
  [Group 2] ──┬── <Task D>
              └── <Task E>
                   │
                   ▼
  [Group 3] ──── <Task F>
  ```
- **Self-contained delegation.** Passes complete context, constraints, and file paths to workers rather than raw transcript dumps.
- **Two-stage verification.** A Level-1 Verification Agent inspects intent fulfillment and can spawn Level-2 agents for decision extraction and verification.
- **Bounded correction loop.** Feeds verification failures back to the producing agent with concrete evidence and caps automatic retries at 3 iterations.
- **Terminal contract.** Returns strictly `Completed`, `Incomplete`, `Blocked`, `Failed`, or `Requires User Input`.

### teamwork orchestration
The teamwork skill coordinates analytical workflows across five execution phases:

1. **Strategy exploration.** Dispatches 32 parallel exploration workers with adversarial falsification and tournament aggregation.
2. **Problem decomposition.** Splits approved strategies into master skeletons with up to 10 subproblems.
3. **Parallel solving.** Solves subproblems concurrently with solver agents, falser audits, and solve mergers.
4. **Global verification.** Assembles proofs and runs referee review waves with verification tree aggregation.
5. **Surgical revision.** Re-plans outlines and applies targeted patches to failing sections.

Agents select this skill for theorem proving, complex algorithmic problems, and multi-candidate architecture evaluation.

### boost concurrency engine
The boost skill removes serialization bottlenecks across parallel subagent tasks:

- **Concurrency ceiling.** Dispatches up to 128 parallel subagents.
- **Dynamic refill.** Fills open worker slots immediately as child tasks complete to avoid head-of-line blocking.
- **Partitioning.** Partitions workloads over 32 workers into slices managed by sub-orchestrators.
- **Temperature asymmetry.** Sets high temperature T=1.0 for creative exploration, and low temperature T=0.2 for audits, mergers, and verification.
- **Quorum advancement.** Starts downstream aggregation when 80 percent of workers finish, and collects stragglers asynchronously.

### securecoder security engineering
The securecoder skill directs vulnerability remediation workflows:

1. **Threat modeling.** Maps entry points, trust boundaries, and sensitive data flows in implementation plans.
2. **Planning.** Defines automated scanner checks and verification requirements before editing code.
3. **Remediation.** Applies defensive fixes following web security standards.
4. **Scanner verification.** Runs scanners for common flaws, including cross-site scripting, SQL injection, CSRF, path traversal, and secret exposure.
5. **Proof of concept verification.** Confirms vulnerabilities no longer reproduce and documents attack scenarios in walkthrough reports.
6. **Audit reporting.** Writes formal findings and remediation logs into walkthrough reports.

### agy-terminal-subagent delegation
The `agy-terminal-subagent` skill delegates tasks to background CLI processes.

Start a new headless session:

```bash
agy -p "Inspect the repository and fix auth validation" \
  --dangerously-skip-permissions \
  --output-format json \
  --model "claude-sonnet-4-6" \
  --print-timeout 24h \
  | jq .
```

Continue an existing session:

```bash
agy --conversation "<CONVERSATION_ID>" \
  -p "Run tests and report changed files" \
  --dangerously-skip-permissions \
  --output-format json \
  --model "claude-sonnet-4-6" \
  --print-timeout 24h \
  | jq .
```

Select models based on task requirements:

- **gemini-3.7-flash-low.** Simple edits, data extraction, and formatting.
- **gemini-3.7-flash-high.** Implementation tasks with clear execution plans.
- **claude-sonnet-4-6.** Complex debugging, refactoring, and multi-file code modifications.
- **claude-opus-4-6-thinking.** Architectural decisions, trade-off evaluations, and deep reasoning.

## Configuration
Configure environment settings and multi-account profiles for terminal subagents.

### Multi-account profile setup
To isolate authentication tokens and run multiple CLI profiles concurrently, redirect the home path and terminal device in your shell.

#### Linux with Bash or Zsh
Add profile aliases to `~/.bashrc` or `~/.zshrc`:

```bash
mkdir -p ~/.agy-alt-profile

cat << 'EOF' >> ~/.bashrc
alias agy='HOME="$HOME/.agy-alt-profile" SSH_TTY="/dev/pts/99" command agy'
alias agy-og='command agy'
EOF

source ~/.bashrc
```

#### macOS with Zsh
Add profile aliases to `~/.zshrc`:

```zsh
mkdir -p ~/.agy-alt-profile

cat << 'EOF' >> ~/.zshrc
alias agy='HOME="$HOME/.agy-alt-profile" SSH_TTY="/dev/ttys000" command agy'
alias agy-og='command agy'
EOF

source ~/.zshrc
```

#### Windows with PowerShell
Add profile functions to `$PROFILE`:

```powershell
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.agy-alt-profile" | Out-Null

if (-not (Test-Path $PROFILE)) {
    New-Item -ItemType File -Force -Path $PROFILE | Out-Null
}

@'
function agy {
    $origUser = $env:USERPROFILE
    $origHome = $env:HOME
    $env:USERPROFILE = "$origUser\.agy-alt-profile"
    $env:HOME = $env:USERPROFILE
    $env:SSH_TTY = "pty0"
    try {
        & (Get-Command agy.exe -CommandType Application) @args
    }
    finally {
        $env:USERPROFILE = $origUser
        $env:HOME = $origHome
        Remove-Item env:SSH_TTY -ErrorAction SilentlyContinue
    }
}

function agy-og {
    & (Get-Command agy.exe -CommandType Application) @args
}
'@ | Add-Content -Path $PROFILE

. $PROFILE
```

### Transcript inspection
The CLI saves session logs to local JSONL files:

```text
~/.gemini/antigravity-cli/brain/<CONVERSATION_ID>/.system_generated/logs/
```

To extract messages from the compact log:

```bash
jq -r 'select(.type=="USER_INPUT" or .type=="PLANNER_RESPONSE") | "[\(.source)]: \(.content)"' transcript.jsonl
```

To extract tool calls:

```bash
jq -c 'select(.tool_calls != null) | {thinking, tools: .tool_calls}' transcript.jsonl
```

## License
MIT
