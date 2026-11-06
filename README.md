# AI agent skills

## Purpose
This repository provides modular AI agent skills, prompt templates, and agentic tools for OpenCode, OpenChamber, and autonomous CLI environments. Agents load these skills to execute multi-agent orchestration, automated security audits, concurrency scaling, and terminal delegation.

## Architecture
Each skill lives in a directory under `skills/` containing a `SKILL.md` specification and optional reference guides. Agent runtimes parse YAML frontmatter name and description fields to select matching skills for incoming tasks.

The repository contains four primary skills:

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
npx skills add devsanthoshmk/skills/skills/teamwork
npx skills add devsanthoshmk/skills/skills/boost
npx skills add devsanthoshmk/skills/skills/securecoder
npx skills add devsanthoshmk/skills/skills/agy-terminal-subagent
```

## Usage
Autonomous agents and LLM workflow patterns execute these skills during task runs.

### Teamwork orchestration
The teamwork skill coordinates analytical workflows across five execution phases:

1. **Strategy exploration.** Dispatches 32 parallel exploration workers with adversarial falsification and tournament aggregation.
2. **Problem decomposition.** Splits approved strategies into master skeletons with up to 10 subproblems.
3. **Parallel solving.** Solves subproblems concurrently with solver agents, falser audits, and solve mergers.
4. **Global verification.** Assembles proofs and runs referee review waves with verification tree aggregation.
5. **Surgical revision.** Re-plans outlines and applies targeted patches to failing sections.

Agents select this skill for theorem proving, complex algorithmic problems, and multi-candidate architecture evaluation.

### Boost concurrency engine
The boost skill removes serialization bottlenecks across parallel subagent tasks:

- **Concurrency ceiling.** Dispatches up to 128 parallel subagents.
- **Dynamic refill.** Fills open worker slots immediately as child tasks complete, preventing head-of-line blocking.
- **Partitioning.** Partitions workloads exceeding 32 workers into slices managed by sub-orchestrators.
- **Temperature asymmetry.** Sets high temperature T=1.0 for creative exploration, and low temperature T=0.2 for audits, mergers, and verification.
- **Quorum advancement.** Starts downstream aggregation when 80 percent of workers finish, and collects stragglers asynchronously.

### SecureCoder security engineering
The securecoder skill directs vulnerability remediation workflows:

1. **Threat modeling.** Maps entry points, trust boundaries, and sensitive data flows in implementation plans.
2. **Planning.** Defines automated scanner checks and verification requirements before editing code.
3. **Remediation.** Applies defensive fixes following web security standards.
4. **Scanner verification.** Runs scanners for common flaws, including cross-site scripting, SQL injection, CSRF, path traversal, and secret exposure.
5. **Proof of concept verification.** Confirms vulnerabilities no longer reproduce and documents attack scenarios in walkthrough reports.
6. **Audit reporting.** Writes formal findings and remediation logs into walkthrough reports.

### Terminal subagent delegation
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
