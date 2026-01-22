# Ralph Loop Framework - Command Reference

All commands are available after reloading your shell: `source ~/.bashrc`

## Core Commands

### ralph-scaffold
Scaffold a new Ralph project with template files.

```bash
# Scaffold both approaches
ralph-scaffold both /path/to/project

# Scaffold snarktank mode only (PRD-based)
ralph-scaffold snarktank /path/to/project

# Scaffold plan/build mode only (IMPLEMENTATION_PLAN-based)
ralph-scaffold plan-build /path/to/project

# Scaffold in current directory
ralph-scaffold both .
```

**What it does:**
- Creates loop orchestrator scripts (ralph.sh, loop.sh)
- Creates prompt files (CLAUDE.md, PROMPT_*.md)
- Creates template files (prd.json, @IMPLEMENTATION_PLAN.md)
- Creates AGENTS.md project guide
- Sets up .gitignore entries

---

### ralph-prd-generator (NEW!)
Interactively generate a comprehensive prd.json file.

```bash
# Generate prd.json in current directory
ralph-prd-generator

# Generate with custom filename
ralph-prd-generator my-feature.json
```

**What it does:**
- Asks questions about your project
- Collects feature requirements
- Generates well-structured user stories
- Creates specific, testable acceptance criteria
- Outputs valid prd.json ready for Ralph

**Questions asked:**
1. Project name
2. Branch name (e.g., ralph/feature-name)
3. Feature description
4. Main goal/problem
5. Tech stack
6. List of features
7. Constraints/requirements

**Output:**
- Complete prd.json with 3-8 user stories
- Specific acceptance criteria
- Proper priority ordering
- Technical validation steps included

---

### ralph-specs-generator (NEW!)
Interactively generate comprehensive specification documents for plan/build mode.

```bash
# Generate specs/*.md in current directory
ralph-specs-generator
```

**What it does:**
- Asks questions about your project/feature
- Collects requirements and technical constraints
- Generates comprehensive specification documents
- Creates multiple .md files in specs/ directory
- Ready for use with plan/build workflow

**Questions asked:**
1. Project name and feature/module name
2. Problem statement
3. Desired outcome
4. Tech stack
5. List of components/features
6. Technical requirements/constraints
7. Edge cases and special scenarios

**Output:**
- Multiple markdown files in `specs/` directory:
  - `overview.md` - High-level architecture and goals
  - `api.md` - API specifications (if applicable)
  - `data-models.md` - Database schema (if applicable)
  - `ui.md` - Frontend specifications (if applicable)
- Or single comprehensive spec file for smaller features
- Covers: goals, user stories, technical requirements, data models, API endpoints, edge cases, testing strategy

---

### ralph-status (NEW!)
Monitor Ralph's progress in real-time with a comprehensive dashboard.

```bash
# One-shot status view
ralph-status

# Auto-refresh every 2 seconds (recommended)
ralph-status --watch

# Auto-refresh with custom interval (5 seconds)
ralph-status --watch 5
```

**What it shows:**
- Current task being worked on
- Progress bar with completion percentage
- Full task list with status indicators (✓ completed, ▶ in progress, ○ pending)
- Activity monitoring (last commit, last file modified)
- Process status (Ralph and Claude PIDs, CPU, memory)
- Warning if no activity detected (stuck detection)

**Works with both modes:**
- Auto-detects snarktank mode (reads .ralph/prd.json)
- Auto-detects plan/build mode (reads .ralph/@IMPLEMENTATION_PLAN.md)

**Use when:**
- Ralph runs in `--print` mode (which is silent by design)
- You want to see what Ralph is working on
- You need to detect if Ralph is stuck
- You want real-time progress updates

---

### ralph-prd-agent
Launch an interactive Claude session for PRD generation.

```bash
ralph-prd-agent
```

**What it does:**
- Opens an interactive Claude Code session
- Loads PRD generator agent prompt
- Allows conversational PRD creation
- Helpful for complex projects needing discussion

**Use when:**
- You want to discuss the PRD interactively
- Requirements are complex or uncertain
- You need to iterate on user stories
- You prefer conversational interface

---

## Ralph Loop Scripts

These are created by `ralph-scaffold` in your project directory.

### ./ralph.sh (Snarktank Mode)
Run the snarktank/ralph loop with PRD-based user stories.

```bash
# Run with Claude Code for 10 iterations
./ralph.sh --tool claude 10

# Run with Amp
./ralph.sh --tool amp 15

# Default (Claude, 10 iterations)
./ralph.sh
```

**What it does:**
- Reads prd.json
- Implements each user story
- Verifies acceptance criteria
- Updates prd.json (sets passes: true)
- Commits and pushes
- Archives previous runs when branch changes
- Stops when <promise>COMPLETE</promise> is output

---

### ./loop.sh (Plan/Build Mode)
Run the plan/build loop with implementation plan.

```bash
# Planning mode (5 iterations)
./loop.sh plan
./loop.sh plan 3

# Build mode (20 iterations)
./loop.sh 20
./loop.sh

# Build mode with custom iterations
./loop.sh 50
```

**What it does:**
- **Plan mode**: Analyzes codebase, creates @IMPLEMENTATION_PLAN.md
- **Build mode**: Implements tasks from plan, runs tests, commits
- Auto-pushes after each iteration in build mode
- Stops when <promise>COMPLETE</promise> is output

---

## Plugin Commands

### /ralph-loop
Run Ralph Loop in current Claude Code session.

```bash
/ralph-loop "Implement dark mode" --max-iterations 10
/ralph-loop "Add authentication" --completion-promise "AUTH COMPLETE"
```

**What it does:**
- Runs Ralph iterations in your current session
- Interactive (can ask questions)
- Uses stop hooks to loop
- Good for supervised work

**Difference from scripts:**
- `/ralph-loop`: Same session, interactive, supervised
- `./ralph.sh` / `./loop.sh`: Fresh instances, autonomous, unsupervised

---

## Template Files

### Project Templates

Located at `~/.ralph-templates/`

**Snarktank mode:**
- `snarktank/ralph.sh` - Loop orchestrator
- `snarktank/CLAUDE.md` - Instructions for Claude
- `snarktank/prd.json.example` - PRD template

**Plan/Build mode:**
- `plan-build/loop.sh` - Loop orchestrator
- `plan-build/PROMPT_plan.md` - Planning instructions
- `plan-build/PROMPT_build.md` - Build instructions

**Shared:**
- `shared/AGENTS.md` - Project guide template

### Helper Scripts

- `ralph-scaffold` - Project scaffolder
- `ralph-prd-generator` - Interactive PRD generator
- `ralph-specs-generator` - Interactive specs generator
- `ralph-prd-agent` - Conversational PRD generator
- `ralph-status` - Monitoring dashboard
- `PRD_GENERATOR_PROMPT.md` - Agent prompt for PRD generation

### Documentation

- `README.md` - Complete documentation
- `QUICK_START.md` - Quick reference guide
- `COMMANDS.md` - This file

---

## Common Workflows

### Workflow 1: New Feature with Snarktank

```bash
# 1. Scaffold the project
cd /path/to/project
ralph-scaffold snarktank .

# 2. Generate PRD interactively (RECOMMENDED)
ralph-prd-generator

# 3. Update project guide
vim .ralph/AGENTS.md  # Add how to run tests, build, etc.

# 4. Run Ralph (in one terminal)
.ralph/ralph.sh --tool claude 15

# 5. Monitor progress (in another terminal)
ralph-status --watch
```

### Workflow 2: Complex Project with Plan/Build

```bash
# 1. Scaffold the project
cd /path/to/project
ralph-scaffold plan-build .

# 2. Generate specifications interactively (RECOMMENDED)
ralph-specs-generator

# OR manually create specifications
# mkdir specs
# cat > specs/feature.md <<EOF
# # Feature Specification
# [Description of what you're building]
# EOF

# 3. Update project guide
vim .ralph/AGENTS.md  # Add how to run tests, build, etc.

# 4. Run planning
.ralph/loop.sh plan

# 5. Review plan
cat .ralph/@IMPLEMENTATION_PLAN.md

# 6. Run build
.ralph/loop.sh 30

# 7. Monitor progress (in another terminal)
ralph-status --watch
```

### Workflow 3: Hybrid Approach

```bash
# 1. Use both modes
ralph-scaffold both .

# 2. Start with planning
ralph-specs-generator
.ralph/loop.sh plan
cat .ralph/@IMPLEMENTATION_PLAN.md

# 3. Switch to snarktank for implementation
ralph-prd-generator  # Convert plan to user stories
.ralph/ralph.sh --tool claude 20

# 4. Monitor progress (in another terminal)
ralph-status --watch
```

### Workflow 4: In-Session with Plugin

```bash
# In a Claude Code session
/ralph-loop "Add user profile page with avatar upload" --max-iterations 15
```

---

## File Outputs

### Created by ralph-scaffold

Snarktank mode:
- `ralph.sh` (executable)
- `CLAUDE.md`
- `prd.json`
- `prd.json.example`
- `progress.txt`
- `AGENTS.md`

Plan/Build mode:
- `loop.sh` (executable)
- `PROMPT_plan.md`
- `PROMPT_build.md`
- `@IMPLEMENTATION_PLAN.md`
- `AGENTS.md`

### Created during Ralph runs

Snarktank:
- `progress.txt` - Learnings from each iteration
- `archive/YYYY-MM-DD-branch-name/` - Previous runs

Plan/Build:
- `@IMPLEMENTATION_PLAN.md` - Updated with progress

---

## Environment Setup

### Add to PATH (done automatically)

```bash
# Added to ~/.bashrc
export PATH="$HOME/.ralph-templates:$PATH"
```

### Reload shell

```bash
source ~/.bashrc
# OR
exec bash
# OR
# Open new terminal
```

---

## Dependencies

Required:
- `claude` - Claude Code CLI
- `jq` - JSON processor (for ralph-prd-generator)
- `git` - Version control

Optional:
- `amp` - Anthropic Amp (for ./ralph.sh --tool amp)

Install jq:
```bash
# Fedora
sudo dnf install jq

# Ubuntu/Debian
sudo apt install jq

# macOS
brew install jq
```

---

## Help & Documentation

```bash
# Quick reference
cat ~/.ralph-templates/QUICK_START.md

# Full documentation
cat ~/.ralph-templates/README.md

# Command reference
cat ~/.ralph-templates/COMMANDS.md

# PRD generator guide
cat ~/.ralph-templates/PRD_GENERATOR_PROMPT.md

# List all templates
ls -la ~/.ralph-templates/
```

---

## Tips

1. **Always update .ralph/AGENTS.md** - Future iterations depend on it
2. **Use ralph-prd-generator** - Saves hours of PRD writing (snarktank mode)
3. **Use ralph-specs-generator** - Saves hours of spec writing (plan/build mode)
4. **Use ralph-status --watch** - Monitor progress in real-time
5. **Start small** - Test with 5-10 iterations first
6. **Review commits** - Ralph auto-commits, check git log
7. **Tune prompts** - Edit .ralph/CLAUDE.md or .ralph/PROMPT_*.md if Ralph goes wrong
8. **Set completion promises** - Prevent infinite loops

---

## Learn More

- Original technique: https://ghuntley.com/ralph/
- Snarktank repo: https://github.com/snarktank/ralph
- Ralph Orchestrator: https://github.com/mikeyobrien/ralph-orchestrator
