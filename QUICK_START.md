# Ralph Loop - Quick Start Guide

## Installation Complete ✓

The Ralph Loop framework is installed at `~/.ralph-templates/`

The `ralph-scaffold` command is in your PATH.

## 30-Second Start

```bash
# 1. Go to your project
cd /path/to/project

# 2. Scaffold Ralph files (creates .ralph/ directory)
ralph-scaffold both .

# 3. Generate PRD interactively (RECOMMENDED for snarktank mode)
ralph-prd-generator    # Auto-detects .ralph/ and creates .ralph/prd.json

# 4a. For snarktank mode: Run Ralph
.ralph/ralph.sh --tool claude 10

# OR

# 4b. For plan/build mode: Create specs/, then run
.ralph/loop.sh plan       # Planning phase
.ralph/loop.sh 20         # Build phase
```

## Which Mode Should I Use?

### Use Snarktank Mode When:
- ✓ You have clear user stories
- ✓ Well-defined acceptance criteria
- ✓ Feature-based development
- ✓ Want PRD-driven workflow

### Use Plan/Build Mode When:
- ✓ Complex project needing planning
- ✓ Greenfield projects
- ✓ Need architectural planning first
- ✓ Want specification-driven workflow

### Use Both When:
- ✓ You're not sure yet
- ✓ Want maximum flexibility
- ✓ Different projects need different approaches

## Command Reference

```bash
# Scaffold a project (creates .ralph/ directory)
ralph-scaffold both /path/to/project
ralph-scaffold snarktank /path/to/project
ralph-scaffold plan-build /path/to/project

# Generate PRD interactively (NEW!)
ralph-prd-generator                # Auto-detects .ralph/, creates .ralph/prd.json
ralph-prd-generator my-prd.json    # Custom output file

# Monitor Ralph progress (NEW!)
ralph-status                       # One-shot status view
ralph-status --watch               # Auto-refresh every 2s
ralph-status --watch 5             # Auto-refresh every 5s

# Run snarktank mode
.ralph/ralph.sh --tool claude 10   # 10 iterations max

# Run plan/build mode
.ralph/loop.sh plan                # Planning mode
.ralph/loop.sh 20                  # Build mode, 20 iterations

# Use ralph-loop plugin (in-session)
/ralph-loop "Task description" --max-iterations 20
```

## File Cheat Sheet

All Ralph files live in `.ralph/` directory to keep your project root clean!

### Snarktank Mode Files
- `.ralph/ralph.sh` - The loop script (executable)
- `.ralph/CLAUDE.md` - Instructions for Claude
- `.ralph/prd.json` - User stories (EDIT THIS)
- `.ralph/progress.txt` - Iteration log (auto-generated)
- `.ralph/AGENTS.md` - Project guide (UPDATE THIS)

### Plan/Build Mode Files
- `.ralph/loop.sh` - The loop script (executable)
- `.ralph/PROMPT_plan.md` - Planning instructions (customize goal)
- `.ralph/PROMPT_build.md` - Build instructions
- `.ralph/@IMPLEMENTATION_PLAN.md` - Task list (auto-generated)
- `.ralph/AGENTS.md` - Project guide (UPDATE THIS)
- `specs/` - Your specifications (CREATE THIS in project root)

## Completion Promises

Ralph stops when it outputs:
```
<promise>COMPLETE</promise>
```

If not output, runs until max iterations.

## PRD Generator (The Easy Button!)

The hardest part of Ralph is writing good user stories with testable acceptance criteria. The PRD generator does this for you!

### Interactive Mode (Recommended)
```bash
ralph-prd-generator
```

You'll be asked:
- Project name and description
- Branch name
- Main goal/problem
- Tech stack
- List of features
- Any constraints

Claude will then generate a complete `prd.json` with:
- Well-formed user stories
- Specific, testable acceptance criteria
- Proper priority ordering
- Technical validation steps

### What You Get

```json
{
  "project": "TodoApp",
  "branchName": "ralph/dark-mode",
  "description": "Add dark mode toggle to existing todo application",
  "userStories": [
    {
      "id": "US-001",
      "title": "Add dark mode state management",
      "description": "As a developer, I need theme state management so that components can access and update the theme.",
      "acceptanceCriteria": [
        "Create ThemeContext with light/dark state",
        "ThemeProvider wraps app root",
        "useTheme hook returns current theme and toggle function",
        "Typecheck passes"
      ],
      "priority": 1,
      "passes": false,
      "notes": ""
    }
  ]
}
```

### Tips for Good PRD Generation

- **Be specific about features**: "User authentication with JWT" not "auth"
- **Mention your tech stack**: Helps generate relevant acceptance criteria
- **List constraints**: "Must work with existing Tailwind config"
- **Think about the user**: What are you building and why?

## Common Workflows

### Workflow 1: New Feature (Snarktank with PRD Generator)
```bash
ralph-scaffold snarktank .
ralph-prd-generator          # Interactive PRD creation (creates .ralph/prd.json)
# Edit .ralph/AGENTS.md with project info

# Run Ralph in one terminal
.ralph/ralph.sh --tool claude 15

# Monitor in another terminal (recommended!)
ralph-status --watch
```

### Workflow 1b: New Feature (Manual PRD)
```bash
ralph-scaffold snarktank .
# Manually edit .ralph/prd.json with user stories
# Edit .ralph/AGENTS.md with project info
.ralph/ralph.sh --tool claude 15
```

### Workflow 2: Complex Project (Plan/Build)
```bash
ralph-scaffold plan-build .
mkdir specs
# Write specs/*.md files
# Update .ralph/PROMPT_plan.md with goal
.ralph/loop.sh plan 5
# Review .ralph/@IMPLEMENTATION_PLAN.md
.ralph/loop.sh 30
```

### Workflow 3: Quick Task (Plugin)
```bash
# In Claude Code session
/ralph-loop "Add dark mode toggle" --max-iterations 10
```

## Customization Tips

### Make Acceptance Criteria Specific
```json
"acceptanceCriteria": [
  "Priority dropdown in edit modal",
  "Current selection is highlighted",
  "Saves on change",
  "No placeholders or TODOs",
  "All tests pass",
  "Typecheck passes"
]
```

### Update AGENTS.md As You Go
```markdown
## Gotchas
### Issue: Tests fail with ECONNREFUSED
- Problem: Database not running
- Solution: Run `docker-compose up -d db` first
```

### Add Project Goal to Planning
```markdown
GOAL: Build a task management app with priority levels,
filtering, and real-time updates using React, TypeScript,
and PostgreSQL.
```

## Troubleshooting

### "command not found: ralph-scaffold"
```bash
# Reload your shell
source ~/.bashrc
# OR start a new terminal
```

### Ralph keeps repeating the same task
- Check acceptance criteria are specific
- Review progress.txt or @IMPLEMENTATION_PLAN.md
- Add learnings to AGENTS.md
- Make criteria more testable

### No completion promise
- Add to acceptance criteria: "Output <promise>COMPLETE</promise> when all tasks done"
- Or set reasonable --max-iterations

## Full Documentation

Read the complete guide:
```bash
cat ~/.ralph-templates/README.md
```

## Examples

See working examples in the README.md file.

---

Need help? Check the README.md or visit:
- https://ghuntley.com/ralph/
- https://github.com/snarktank/ralph
