# Agent Skills Collection

A curated collection of skills and workflow extensions for AI coding agents and autonomous CLI tools.

---

## Installation

Add this skills repository to your agent environment:

```bash
npx skills add devsanthoshmk/skills
```

To update installed skills:

```bash
npx skills update devsanthoshmk/skills
```

---

## Included Skills

### `agy-terminal-subagent`

Enables headless terminal subagent delegation with custom multi-model routing and reasoning tiers.

#### Multi-Account Configuration

To isolate token storage and operate multiple profiles concurrently:

##### Linux (Bash / Zsh)

```bash
mkdir -p ~/.agy-alt-profile

cat << 'EOF' >> ~/.bashrc
# Multi-Account Aliases
alias agy='HOME="$HOME/.agy-alt-profile" SSH_TTY="/dev/pts/99" command agy'
alias agy-og='command agy'
EOF

source ~/.bashrc
```

##### macOS (Zsh)

```zsh
mkdir -p ~/.agy-alt-profile

cat << 'EOF' >> ~/.zshrc
# Multi-Account Aliases
alias agy='HOME="$HOME/.agy-alt-profile" SSH_TTY="/dev/ttys000" command agy'
alias agy-og='command agy'
EOF

source ~/.zshrc
```

##### Windows (PowerShell)

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

---

## License

MIT
