# Multi-Dev: Git Worktree Automation

**Save 2-3 minutes per branch** with automated git worktree setup, configuration management, and safety features.

## What this does

Automates the tedious manual steps of creating isolated development environments for different git branches. One command replaces 5-6 manual steps and eliminates common mistakes.

**Time-saving automation:**
- **One-command setup**: `multi start feature-branch` creates worktree, copies configs, installs dependencies
- **Smart configuration**: Automatically copies `.env*`, `*.pem`, certificates to each worktree  
- **Dependency management**: Detects yarn/npm and runs clean installs with proper lockfiles
- **Organized structure**: Creates consistent `.dev-worktrees/` organization you can rely on

**Safety features:**
- **Data loss prevention**: Detects uncommitted changes and unpushed commits before cleanup
- **Smart cleanup**: Safe removal with confirmation prompts for risky operations
- **Structured workspace**: Organized file management prevents lost work

**Workflow enhancements:**
- **Multiple environments**: Work on different branches simultaneously without conflicts
- **Tmux integration**: Optional persistent sessions with structured 3-window layout
- **Easy switching**: Quick environment switching with interactive menus
- **Project detection**: Auto-detects and suggests dev commands (npm/yarn/cargo/docker)

## Features

### 🚀 Automated Environment Setup

```bash
multi start feature/new-dashboard
```

**Automatically handles:**
- Creates git worktree from main branch in organized `.dev-worktrees/` folder
- Copies essential config files (`.env*`, `*.pem`, certificates) to new worktree
- Detects project type and runs appropriate dependency installation:
  - `yarn install --frozen-lockfile` (if yarn.lock exists)
  - `npm ci` (if package-lock.json exists) 
  - `npm install` (fallback)
- Sets up tmux session with 3 organized windows:
  - `main`: Editor launch commands ready
  - `dev`: Development server workspace 
  - `git`: Git operations and status
- Suggests project-specific dev commands

### 🔄 Easy Environment Management

```bash
multi switch main        # Switch to specific environment
multi switch            # Interactive menu of all environments  
multi list              # See all environments with status
```

### 🛡️ Safe Cleanup with Data Protection

```bash
multi cleanup feature/old-branch    # Safe removal with checks
multi cleanup feature/old --force   # Override safety checks
multi cleanup-all                   # Clean all environments safely
```

**Safety features prevent data loss:**
- Detects uncommitted changes before removal
- Counts and warns about unpushed commits
- Shows recent commits that would be lost
- Requires explicit confirmation or `--force` flag

### 🛠 Smart Project Detection

Auto-detects and suggests commands for:
- Node.js projects (npm/yarn dev/start)
- Rust projects (cargo run) 
- Docker projects (docker-compose up)
- Make-based projects

## Installation

1. **Clone and make executable:**

```bash
git clone git@github.com:jeremyletran/multi-dev.git ~/.local/bin/multi-dev
cd ~/.local/bin/multi-dev
chmod +x multi setup-multi
```

2. **Run setup:**

```bash
./setup-multi
```

This will:

- Check dependencies (tmux, git)
- Install the `multi` script to ~/.local/bin
- Add ~/.local/bin to your PATH
- Test the installation

3. **Restart your terminal** or run:

```bash
source ~/.zshrc  # or ~/.bashrc
```

## Commands

### Main Commands

- `multi start <branch>` - Create and switch to development environment
- `multi switch <branch>` - Switch to existing environment
- `multi list` - List all environments for current repo
- `multi stop <branch>` - Stop tmux session
- `multi cleanup <branch>` - Remove environment (stop + delete worktree)
- `multi cleanup-all` - Remove all environments for current repo

## How it works

1. **Automated Git Worktrees**: Creates isolated checkouts in organized `.dev-worktrees/` directory
2. **Smart Configuration Management**: Automatically finds and copies essential files (`.env*`, certificates, keys)
3. **Intelligent Dependency Installation**: Detects lockfiles and runs appropriate package manager commands
4. **Structured Workspace Organization**: Consistent naming and directory structure across all environments
5. **Safety-First Cleanup**: Comprehensive checks for uncommitted changes and unpushed commits before removal
6. **Optional Tmux Integration**: Each environment gets a dedicated session with organized windows:
   - `main` window: Editor launch commands ready to use
   - `dev` window: Development servers and build processes  
   - `git` window: Git operations and repository management

**The automation eliminates these manual steps every time:**
- Creating worktree directory with proper naming
- Copying configuration files individually  
- Running correct package manager with right flags
- Setting up development environment structure
- Remembering safe cleanup procedures

## Example Workflow

```bash
# Traditional manual setup (5-6 steps every time):
git worktree add .dev-worktrees/feature-user-auth feature/user-auth
cd .dev-worktrees/feature-user-auth  
cp ../../.env* .
cp ../../*.pem .
npm install
npm run dev

# vs Multi-dev automated setup (1 command):
cd my-project
multi start feature/user-auth
# Done! Worktree created, configs copied, dependencies installed, tmux session ready

# Work in your preferred editor:
cursor .  # or claude . or code .

# Switch between environments instantly:
multi switch feature/login-error

# See all your environments:
multi list

# Safe cleanup when done:
multi cleanup feature/user-auth
```

## Requirements

- **tmux** - Session management
- **git** - Repository operations
- **bash** - Script execution

## File Structure

```
~/.local/bin/
├── multi           # Main development environment manager
├── setup-multi     # Installation and setup script
└── README.md       # This file

# Created by the tool:
your-project/
└── .dev-worktrees/     # Isolated branch checkouts
    ├── main/
    ├── feature-auth/
    └── bugfix-login/
```

## Tips

- **Branch naming**: Use descriptive names like `feature/user-auth` or `bugfix/login-error`
- **Cleanup regularly**: Use `multi cleanup-all` to remove old environments
- **Session persistence**: Tmux sessions survive terminal restarts
- **Multiple repos**: Each git repository gets its own set of environments

## Troubleshooting

**Command not found:**

- Run the setup script: `./setup-multi`
- Make sure ~/.local/bin is in your PATH
- Run `source ~/.zshrc` or restart terminal

**Tmux not found:**

- Install with `brew install tmux` (macOS) or `apt install tmux` (Ubuntu)

**Permission denied:**

- Make scripts executable: `chmod +x ~/.local/bin/multi`

**Worktree conflicts:**

- Clean up with `multi cleanup-all --force`
- Check for uncommitted changes in worktrees

## Why Multi-Dev vs Manual Git Worktrees?

### Manual Git Worktree Setup
```bash
# Every single time for each new branch:
git worktree add .dev-worktrees/feature-auth feature/auth
cd .dev-worktrees/feature-auth
# Find and copy config files manually:
cp ../../.env* .
cp ../../*.pem .  
cp ../../*.key .
# Figure out package manager and run install:
npm install  # or yarn install? npm ci?
# Set up development workspace manually
# Remember paths for later switching
```

**Time-consuming manual process:**
- 5-6 separate commands every time
- Manual file hunting and copying
- Guessing correct package manager commands
- No organized structure or naming consistency
- No safety checks during cleanup
- Easy to forget steps or make mistakes

### Multi-Dev Automated Solution
```bash
# One command does everything:
multi start feature/auth
# Automatically: creates worktree, copies configs, installs deps, sets up workspace
```

**Key automation benefits:**
- **Time savings** - 2-3 minutes saved per branch setup
- **Consistency** - Same organized structure every time  
- **Smart config management** - Automatically finds and copies `.env*`, certificates, keys
- **Intelligent dependencies** - Detects yarn.lock vs package-lock.json, runs appropriate commands
- **Safety features** - Comprehensive data loss prevention during cleanup
- **Easy management** - Simple commands for switching, listing, and cleanup
- **Optional workflow enhancement** - Tmux integration for persistent sessions

### The Real Benefits

**For everyone:**
- Eliminates repetitive manual setup steps
- Prevents configuration mistakes and missing files
- Provides safe cleanup with uncommitted change detection
- Creates organized, consistent workspace structure

**Bonus workflow features:**
- Tmux sessions for persistent development environments
- Interactive environment switching
- Visual status indicators
- Parallel development support

Whether you use tmux or prefer your own terminal setup, the automation and safety features save time and prevent mistakes on every branch.
