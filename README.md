# Ralph Loop Framework

A hybrid framework supporting two Ralph Loop methodologies for autonomous AI development.

## Quick Start

```bash
# Add to PATH (already done if you installed)
export PATH="$HOME/.ralph-templates:$PATH"

# Scaffold a new project
cd /path/to/project
ralph-scaffold both .

# Generate PRD
ralph-prd-generator

# Run Ralph
.ralph/ralph.sh --tool claude 15

# Monitor progress (in another terminal)
ralph-status --watch
```

## Core Commands

### ralph-scaffold
Scaffold a new Ralph project with template files in `.ralph/` directory.

```bash
ralph-scaffold both .          # Both modes
ralph-scaffold snarktank .     # PRD-based only
ralph-scaffold plan-build .    # Plan/build only
```

### ralph-prd-generator (THE EASY BUTTON!)
Interactively generate comprehensive prd.json files.

```bash
ralph-prd-generator           # Auto-creates .ralph/prd.json
ralph-prd-generator my.json   # Custom path
```

Asks you questions and generates well-structured user stories with testable acceptance criteria.

### ralph-status (MONITORING DASHBOARD)
Monitor Ralph's progress in real-time.

```bash
ralph-status                  # One-shot view
ralph-status --watch          # Auto-refresh every 2s
ralph-status --watch 5        # Custom interval
```

Shows:
- Current task being worked on
- Progress bar and completion percentage
- Full task list with status indicators
- Activity monitoring (detects if stuck)
- Process status (Ralph & Claude)

### Running Ralph

```bash
# Snarktank mode (PRD-based)
.ralph/ralph.sh --tool claude 10

# Plan/Build mode
.ralph/loop.sh plan           # Planning phase
.ralph/loop.sh 20             # Build phase
```

## Two Approaches

### 1. Snarktank Mode (PRD-Based)

**Best for**: Well-defined features with clear user stories

**Workflow**:
```bash
# 1. Generate prd.json interactively (RECOMMENDED)
ralph-prd-generator    # Auto-creates .ralph/prd.json

# OR manually edit .ralph/prd.json with your user stories

# 2. Run the loop
.ralph/ralph.sh --tool claude 10

# 3. Monitor progress (in another terminal)
ralph-status --watch

# Ralph will:
# - Read .ralph/prd.json
# - Implement each story
# - Verify acceptance criteria
# - Update .ralph/prd.json
# - Commit and push
# - Output <promise>COMPLETE</promise> when done
```

### 2. Plan/Build Mode (Implementation Plan)

**Best for**: Complex projects needing planning phase

**Workflow**:
```bash
# 1. Create specs/ directory with specifications
mkdir -p specs
echo "# Feature Spec" > specs/feature.md

# 2. Run planning mode
.ralph/loop.sh plan

# 3. Review @IMPLEMENTATION_PLAN.md
cat .ralph/@IMPLEMENTATION_PLAN.md

# 4. Run build mode
.ralph/loop.sh 20

# 5. Monitor progress (in another terminal)
ralph-status --watch

# Ralph will:
# - Read implementation plan
# - Implement highest priority task
# - Run tests
# - Update plan
# - Commit and push
# - Output <promise>COMPLETE</promise> when done
```

## Monitoring Ralph

**IMPORTANT**: Ralph runs silently in `--print` mode (by design). Use `ralph-status --watch` to monitor progress!

**What ralph-status shows:**
```
╔════════════════════════════════════════════════════════╗
║  Ralph Status Dashboard                                ║
╚════════════════════════════════════════════════════════╝

Mode:    snarktank
Project: MyApp
Branch:  ralph/dark-mode

Current Task:
  ID:     US-002
  Title:  Create dark mode toggle button

Progress:
  Completed: 1 / 3 (33%)
  Remaining: 2
  [████████████████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░]

Task List:

  ✓ US-001 - Add dark mode state management
  ▶ US-002 - Create dark mode toggle button (in progress)
  ○ US-003 - Persist theme preference

Activity:
  Last commit: 2 minutes ago: US-001: Add ThemeContext
  Last modified: src/components/Header.tsx (15s ago)
  ✓ Ralph running (PID: 12345)
  ✓ Claude working (PID: 12346, CPU: 42%, MEM: 2.1%)

Refreshing every 2s...
```

**Status indicators:**
- `✓` = Completed (green)
- `▶` = In progress (yellow, bold)
- `○` = Pending (blue)
- `⚠` = Warning if no activity (red)

## PRD Generator - The Easy Button

Writing good user stories with testable acceptance criteria is the hardest part. The PRD generator solves this!

### Interactive Mode

```bash
ralph-prd-generator
```

You'll be asked:
1. Project name and description
2. Branch name (e.g., ralph/feature-name)
3. Main goal/problem
4. Tech stack
5. List of features
6. Any constraints

Claude will then generate a complete `prd.json` with:
- Well-formed user stories
- Specific, testable acceptance criteria
- Proper priority ordering
- Technical validation steps

### Example Generated PRD

**Input:**
```
Project: TodoApp
Goal: Add dark mode support
Features:
- Dark mode toggle button
- Persistent theme preference
- Smooth transitions
```

**Output:**
```json
{
  "project": "TodoApp",
  "branchName": "ralph/dark-mode",
  "userStories": [
    {
      "id": "US-001",
      "title": "Add dark mode state management",
      "description": "As a developer, I need theme state management...",
      "acceptanceCriteria": [
        "Create ThemeContext with light/dark state",
        "ThemeProvider wraps app root",
        "useTheme hook returns current theme and toggle function",
        "Typecheck passes"
      ],
      "priority": 1,
      "passes": false
    }
  ]
}
```

**Why use it?**
- ✅ 5 minutes vs hours of writing
- ✅ Specific, testable criteria
- ✅ Always includes tests/typecheck
- ✅ Proper dependency ordering
- ✅ Ready to use immediately

## File Structure

All Ralph files are organized in `.ralph/` directory to keep project root clean!

### After Scaffolding (Both Modes)

```
project/
├── .gitignore            # With Ralph ignore entries
├── .ralph/               # All Ralph files here
│   ├── ralph.sh          # Snarktank loop
│   ├── CLAUDE.md         # Claude instructions
│   ├── prd.json          # User stories
│   ├── loop.sh           # Plan/build loop
│   ├── PROMPT_plan.md    # Planning instructions
│   ├── PROMPT_build.md   # Build instructions
│   ├── @IMPLEMENTATION_PLAN.md  # Task tracking
│   ├── AGENTS.md         # Project guide
│   ├── ralph-status.sh   # Monitoring dashboard
│   ├── progress.txt      # Iteration log (auto-created)
│   └── archive/          # Previous runs (auto-created)
├── specs/                # Specifications (create this)
└── src/                  # Your actual project code
```

## Best Practices

### 1. Always Monitor Ralph

Run `ralph-status --watch` in a separate terminal to:
- See what task Ralph is working on
- Detect if Ralph is stuck
- Monitor file changes
- Track progress

### 2. Write Clear Acceptance Criteria

Good:
```
"Add priority dropdown in task edit modal"
"Shows current priority as selected"
"Saves immediately on selection change"
"Typecheck passes"
```

Bad:
```
"Add priority feature"
"Make it work"
```

### 3. Keep AGENTS.md Updated

When Ralph discovers how to:
- Run tests
- Start dev server
- Fix common errors

Add it to `.ralph/AGENTS.md`!

### 4. Use Meaningful Commits

Ralph auto-commits. Review commit messages in your prompts.

### 5. Monitor Progress

- Snarktank: Check `.ralph/progress.txt`
- Plan/Build: Check `.ralph/@IMPLEMENTATION_PLAN.md`
- Both: Use `ralph-status --watch`!

## Troubleshooting

### Ralph keeps failing the same task

1. Check acceptance criteria - are they specific enough?
2. Review `.ralph/progress.txt` or `.ralph/@IMPLEMENTATION_PLAN.md`
3. Add learnings to `.ralph/AGENTS.md`
4. Make criteria more explicit

### Ralph implements placeholders

Add to acceptance criteria:
```
"No TODOs or placeholders"
"All functionality fully implemented"
```

### Tests keep failing

1. Ensure `.ralph/AGENTS.md` has correct test command
2. Add to acceptance criteria: "All tests pass"
3. Consider breaking into smaller stories

### Ralph doesn't commit

1. Check git is initialized
2. Verify branch exists
3. Check git credentials

### Can't see what Ralph is doing

**This is normal!** Ralph runs in `--print` mode which buffers output.

**Solution**: Use `ralph-status --watch` to monitor progress in real-time.

### Ralph appears stuck

Check `ralph-status` for:
- "⚠ No commits in X minutes" warning
- Last file modification time
- Claude CPU usage (should be >0%)

If truly stuck:
1. Kill Ralph (Ctrl+C)
2. Review `.ralph/progress.txt` for errors
3. Update `.ralph/AGENTS.md` or `.ralph/CLAUDE.md` with fixes
4. Restart

## Integration with ralph-loop Plugin

The `/ralph-loop` command runs in your current session:

```bash
# In current session (interactive)
/ralph-loop "Implement feature X" --max-iterations 20

# External loop (spawns fresh instances)
.ralph/ralph.sh --tool claude 10
.ralph/loop.sh 20
```

Both approaches are valid! Choose based on your needs:
- Current session: Interactive, can ask questions
- External loop: Autonomous, runs independently

## Complete Command Reference

```bash
# Setup
ralph-scaffold both .                  # Scaffold project

# PRD Generation
ralph-prd-generator                    # Interactive PRD creation
ralph-prd-agent                        # Conversational PRD creation

# Monitoring
ralph-status                           # One-shot view
ralph-status --watch                   # Auto-refresh (2s)
ralph-status --watch 10                # Custom interval

# Running Ralph
.ralph/ralph.sh --tool claude 10       # Snarktank mode
.ralph/loop.sh plan                    # Planning phase
.ralph/loop.sh 20                      # Build phase

# In-session
/ralph-loop "Task" --max-iterations 15
```

## Documentation Files

```bash
# Quick reference
cat ~/.ralph-templates/QUICK_START.md

# Full documentation
cat ~/.ralph-templates/README.md

# All commands
cat ~/.ralph-templates/COMMANDS.md

# PRD generator guide
cat ~/.ralph-templates/PRD_GENERATOR_PROMPT.md
```

## Learn More

- Original technique: https://ghuntley.com/ralph/
- Snarktank repo: https://github.com/snarktank/ralph
- Ralph Orchestrator: https://github.com/mikeyobrien/ralph-orchestrator

---

**Happy Ralph-ing!** 🎉

Remember: Use `ralph-status --watch` to monitor progress!
