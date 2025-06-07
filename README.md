# Development Environment Tools

A streamlined collection of scripts for managing git worktrees, tmux sessions, and development environments across multiple branches.

## What this does

These tools enable **parallel development workflows** by creating isolated environments for different git branches. Perfect for Claude Code users who want to work on multiple features simultaneously while waiting for long-running tasks to complete.

Key benefits:

- **Parallel task execution**: Work on multiple features in the same repo with separate Claude Code instances
- **No context switching overhead**: Each branch maintains its own development server, editor state, and git context
- **Isolated workspaces**: Each branch gets its own git worktree and tmux session
- **Automatic setup**: One command creates everything you need
- **Smart project detection**: Automatically detects and suggests dev commands (npm/yarn/cargo/make/docker)
- **Editor integration**: Easy commands to launch Cursor, Claude Code, or VS Code
- **Clean workflows**: No more stashing, conflicts, or lost work when switching branches

## Features

### 🚀 Quick Environment Creation

```bash
multi start feature/new-dashboard
```

Creates:

- Git worktree from main branch
- **3-window tmux session for optimal parallel workflow:**
  - `main` window: Editor launch (Claude Code, Cursor, VS Code)
  - `dev` window: Development servers and build processes
  - `git` window: Git operations and repository management
- Automatic session switching
- Development command suggestions

### 🔄 Easy Branch Switching

```bash
multi switch main
multi list  # See all environments
```

### 🧹 Clean Workspace Management

```bash
multi cleanup feature/old-branch    # Remove specific environment
multi cleanup-all                   # Clean everything
```

### 🛠 Automatic Project Detection

Detects and suggests commands for:

- Node.js projects (npm/yarn dev/start)
- Rust projects (cargo run)
- Docker projects (docker-compose up)
- Make-based projects

## Installation

1. **Clone and make executable:**

```bash
git clone git@github.com:jeremyletran/multi.git ~/.local/bin/multi
cd ~/.local/bin/multi
chmod +x multi setup-multi
```

2. **Run setup:**

```bash
./setup-multi
```

This will:

- Check dependencies (tmux, git)
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

1. **Git Worktrees**: Creates isolated checkouts in `.dev-worktrees/` directory
2. **3-Window Tmux Sessions**: Each environment gets a dedicated session optimized for parallel development:
   - `main` window: Editor launch (claude ., cursor ., code .) - your primary coding interface
   - `dev` window: Development servers, build processes, and long-running tasks
   - `git` window: Git operations, status checks, and repository management
3. **Smart Naming**: Session names use repo + branch for organization
4. **Automatic Cleanup**: Worktrees are properly managed and cleaned up

**Why 3 windows?** This setup maximizes productivity when using Claude Code by allowing you to:

- Keep your AI assistant active in the main window while
- Running development servers/builds in the dev window and
- Managing git operations in a separate context
- Work on multiple features simultaneously without interference

## Example Workflow

```bash
# Start working on a feature
cd my-project
multi start feature/user-auth

# Tmux session opens automatically with 3 windows
# In the main window:
cursor .  # or claude . or code .

# In the dev window (if detected):
npm run dev

# Switch to different branch
multi switch feature/login-error

# List all your environments
multi list

# Clean up when done
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

- Make sure ~/.local/bin is in your PATH
- Run `source ~/.zshrc` or restart terminal

**Tmux not found:**

- Install with `brew install tmux` (macOS) or `apt install tmux` (Ubuntu)

**Permission denied:**

- Make scripts executable: `chmod +x ~/.local/bin/multi`

**Worktree conflicts:**

- Clean up with `multi cleanup-all --force`
- Check for uncommitted changes in worktrees

## Why these tools?

**Traditional workflow problems:**

- Waiting for Claude Code tasks while context switching kills productivity
- Git branch switching requires stashing changes and stopping dev servers
- Single-threaded development when you could be working on multiple features
- Lost context when switching between different parts of a project

**Solution: Parallel development environments**
These tools solve this by giving each branch its own complete workspace with dedicated tmux sessions. Perfect for Claude Code users who want to:

- Start a build/test in one environment while coding in another
- Work on multiple features simultaneously with separate AI assistant instances
- Switch instantly between different features without losing context
- Maintain separate development servers for different branches
- Keep git operations isolated per feature branch
