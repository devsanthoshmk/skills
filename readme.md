# Instructiions to install the skill

```bash
npx skills add devsanthoshmk/skills
```

# Instructiions to install the skill

```bash
npx skills update devsanthoshmk/skills
```

# Instructiions to use the skill

## agy-terminal-subagent

if you want higher usage you can use different accounts in antigravity cli and antigravity desktop using the below scripts

### 🐧 1. Linux (Bash / Zsh)

```bash
# 1. Create the isolated profile folder
mkdir -p ~/.agy-alt-profile

# 2. Add aliases to your shell config (~/.bashrc or ~/.zshrc)
cat << 'EOF' >> ~/.bashrc

# Antigravity Multi-Account Aliases
alias agy='HOME="$HOME/.agy-alt-profile" SSH_TTY="/dev/pts/99" command agy'
alias agy-og='command agy'
EOF

# 3. Reload shell
source ~/.bashrc
```
*(If using Zsh, replace `~/.bashrc` with `~/.zshrc`)*

---

### 🍎 2. macOS (Zsh)

```zsh
# 1. Create the isolated profile folder
mkdir -p ~/.agy-alt-profile

# 2. Add aliases to ~/.zshrc
cat << 'EOF' >> ~/.zshrc

# Antigravity Multi-Account Aliases
alias agy='HOME="$HOME/.agy-alt-profile" SSH_TTY="/dev/ttys000" command agy'
alias agy-og='command agy'
EOF

# 3. Reload shell
source ~/.zshrc
```

---

### 🪟 3. Windows (PowerShell)

```powershell
# 1. Create the isolated profile folder
New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.agy-alt-profile" | Out-Null

# 2. Ensure PowerShell profile file exists
if (-not (Test-Path $PROFILE)) {
    New-Item -ItemType File -Force -Path $PROFILE | Out-Null
}

# 3. Add wrapper functions to your PowerShell Profile
@'
# Antigravity Multi-Account Wrappers
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

# 4. Reload PowerShell Profile
. $PROFILE
```

---

### How It Works:
* **`agy`** &rarr; Launches with the isolated profile directory and forces file-based token storage (ignoring system keyring), allowing a distinct Google account login.
* **`agy-og`** &rarr; Launches with your default environment and shared Antigravity 2.0 / system keyring account.

note: you can link the global skills, mcp and other configuration directories to that custom config folder if needed
