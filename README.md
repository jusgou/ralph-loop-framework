```
╔══════════════════════════════════════════════════════════╗
║                                                          ║
║   ██╗  ██╗ █████╗       ██╗  ██╗ █████╗     ██╗        ║
║   ██║  ██║██╔══██╗      ██║  ██║██╔══██╗    ██║        ║
║   ███████║███████║█████╗███████║███████║    ██║        ║
║   ██╔══██║██╔══██║╚════╝██╔══██║██╔══██║    ╚═╝        ║
║   ██║  ██║██║  ██║      ██║  ██║██║  ██║    ██╗        ║
║   ╚═╝  ╚═╝╚═╝  ╚═╝      ╚═╝  ╚═╝╚═╝  ╚═╝    ╚═╝        ║
║                                                          ║
╚══════════════════════════════════════════════════════════╝
```

# Nelson Loop Framework

A hybrid framework supporting two Nelson Loop methodologies for autonomous AI development.

## Quick Start

### Toro Mode (PRD-based)
```bash
# Scaffold a new project
cd /path/to/project
nelson-scaffold both .

# Generate PRD interactively
nelson-prd-generator

# Run Nelson
.nelson/nelson.sh --tool claude 15

# Monitor progress (in another terminal)
nelson-status --watch
```

### Plan/Build Mode
```bash
# Scaffold a new project
cd /path/to/project
nelson-scaffold both .

# Generate specifications interactively
nelson-specs-generator

# Run planning phase
.nelson/loop.sh plan

# Run build phase
.nelson/loop.sh 20

# Monitor progress (in another terminal)
nelson-status --watch
```

## Core Commands

### nelson-scaffold
Scaffold a new Nelson project with template files in `.nelson/` directory.

```bash
nelson-scaffold both .          # Both modes
nelson-scaffold toro .     # PRD-based only
nelson-scaffold plan-build .    # Plan/build only
```

### nelson-prd-generator (THE EASY BUTTON!)
Interactively generate comprehensive prd.json files.

```bash
nelson-prd-generator           # Auto-creates .nelson/prd.json
nelson-prd-generator my.json   # Custom path
```

Asks you questions and generates well-structured user stories with testable acceptance criteria.

### nelson-specs-generator (THE EASY BUTTON FOR PLAN/BUILD!)
Interactively generate comprehensive specification documents.

```bash
nelson-specs-generator         # Auto-creates specs/*.md
```

Asks you questions and generates detailed specifications for the plan/build workflow. Creates structured markdown files covering:
- Overview and goals
- Technical requirements
- Data models and API specs
- UI/UX specifications
- Edge cases and testing strategy

### nelson-punch-ralph (QUALITY REVIEW AGENT)
Aggressive QA agent that reviews completed work and prevents compounding errors.

```bash
nelson-punch-ralph           # Review if 3+ stories complete
nelson-punch-ralph --force   # Force review any time
```

**What it does:**
- Reviews completed stories with extreme scrutiny
- Catches compounding errors before they spread
- Demands documentation rigor
- Creates timestamped review logs in `.nelson/nelson-logs/`
- Identifies critical, major, and minor issues
- Avoids perfectionism loops (only flags meaningful issues)

**When it runs:**
- Automatically at stories 3, 7, 11, 15, etc. (every 4 stories starting at 3)
- Final review when all stories complete
- On-demand with `nelson-punch-ralph --force`

**Review outputs:**
- `.nelson/nelson-logs/YYYY-MM-DD-HH-MM-SS-review.md` - Detailed review
- `.nelson/COMPLETION_REPORT.md` - Final project completion report

### nelson-status (MONITORING DASHBOARD)
Monitor Nelson's progress in real-time.

```bash
nelson-status                  # One-shot view
nelson-status --watch          # Auto-refresh every 2s
nelson-status --watch 5        # Custom interval
```

Shows:
- Current task being worked on
- Progress bar and completion percentage
- Full task list with status indicators
- Activity monitoring (detects if stuck)
- Process status (Nelson & Claude)

### Running Nelson

```bash
# Toro mode (PRD-based)
.nelson/nelson.sh --tool claude 10

# Plan/Build mode
.nelson/loop.sh plan           # Planning phase
.nelson/loop.sh 20             # Build phase
```

## Two Approaches

### 1. Toro Mode (PRD-Based)

**Best for**: Well-defined features with clear user stories

**Workflow**:
```bash
# 1. Generate prd.json interactively (RECOMMENDED)
nelson-prd-generator    # Auto-creates .nelson/prd.json

# OR manually edit .nelson/prd.json with your user stories

# 2. Run the loop
.nelson/nelson.sh --tool claude 10

# 3. Monitor progress (in another terminal)
nelson-status --watch

# Nelson will:
# - Read .nelson/prd.json
# - Implement each story
# - Verify acceptance criteria
# - Update .nelson/prd.json
# - Commit and push
# - Output <promise>COMPLETE</promise> when done
```

### 2. Plan/Build Mode (Implementation Plan)

**Best for**: Complex projects needing planning phase

**Workflow**:
```bash
# 1. Generate specifications interactively (RECOMMENDED)
nelson-specs-generator    # Auto-creates specs/*.md

# OR manually create specs/ directory with specifications
# mkdir -p specs
# echo "# Feature Spec" > specs/feature.md

# 2. Run planning mode
.nelson/loop.sh plan

# 3. Review @IMPLEMENTATION_PLAN.md
cat .nelson/@IMPLEMENTATION_PLAN.md

# 4. Run build mode
.nelson/loop.sh 20

# 5. Monitor progress (in another terminal)
nelson-status --watch

# Nelson will:
# - Read implementation plan
# - Implement highest priority task
# - Run tests
# - Update plan
# - Commit and push
# - Output <promise>COMPLETE</promise> when done
```

## Monitoring Nelson

**IMPORTANT**: Nelson runs silently in `--print` mode (by design). Use `nelson-status --watch` to monitor progress!

**What nelson-status shows:**
```
╔════════════════════════════════════════════════════════╗
║  Nelson Status Dashboard                                ║
╚════════════════════════════════════════════════════════╝

Mode:    toro
Project: MyApp
Branch:  nelson/dark-mode

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
  ✓ Nelson running (PID: 12345)
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
nelson-prd-generator
```

You'll be asked:
1. Project name and description
2. Branch name (e.g., nelson/feature-name)
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
  "branchName": "nelson/dark-mode",
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

All Nelson files are organized in `.nelson/` directory to keep project root clean!

### After Scaffolding (Both Modes)

```
project/
├── .gitignore            # With Nelson ignore entries
├── .nelson/               # All Nelson files here
│   ├── nelson.sh          # Toro loop
│   ├── CLAUDE.md         # Claude instructions
│   ├── prd.json          # User stories
│   ├── loop.sh           # Plan/build loop
│   ├── PROMPT_plan.md    # Planning instructions
│   ├── PROMPT_build.md   # Build instructions
│   ├── @IMPLEMENTATION_PLAN.md  # Task tracking
│   ├── AGENTS.md         # Project guide
│   ├── nelson-status.sh   # Monitoring dashboard
│   ├── progress.txt      # Iteration log (auto-created)
│   └── archive/          # Previous runs (auto-created)
├── specs/                # Specifications (create this)
└── src/                  # Your actual project code
```

## Best Practices

### 1. Always Monitor Nelson

Run `nelson-status --watch` in a separate terminal to:
- See what task Nelson is working on
- Detect if Nelson is stuck
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

When Nelson discovers how to:
- Run tests
- Start dev server
- Fix common errors

Add it to `.nelson/AGENTS.md`!

### 4. Use Meaningful Commits

Nelson auto-commits. Review commit messages in your prompts.

### 5. Monitor Progress

- Toro: Check `.nelson/progress.txt`
- Plan/Build: Check `.nelson/@IMPLEMENTATION_PLAN.md`
- Both: Use `nelson-status --watch`!

## Troubleshooting

### Nelson keeps failing the same task

1. Check acceptance criteria - are they specific enough?
2. Review `.nelson/progress.txt` or `.nelson/@IMPLEMENTATION_PLAN.md`
3. Add learnings to `.nelson/AGENTS.md`
4. Make criteria more explicit

### Nelson implements placeholders

Add to acceptance criteria:
```
"No TODOs or placeholders"
"All functionality fully implemented"
```

### Tests keep failing

1. Ensure `.nelson/AGENTS.md` has correct test command
2. Add to acceptance criteria: "All tests pass"
3. Consider breaking into smaller stories

### Nelson doesn't commit

1. Check git is initialized
2. Verify branch exists
3. Check git credentials

### Can't see what Nelson is doing

**This is normal!** Nelson runs in `--print` mode which buffers output.

**Solution**: Use `nelson-status --watch` to monitor progress in real-time.

### Nelson appears stuck

Check `nelson-status` for:
- "⚠ No commits in X minutes" warning
- Last file modification time
- Claude CPU usage (should be >0%)

If truly stuck:
1. Kill Nelson (Ctrl+C)
2. Review `.nelson/progress.txt` for errors
3. Update `.nelson/AGENTS.md` or `.nelson/CLAUDE.md` with fixes
4. Restart

## Integration with nelson-loop Plugin

The `/nelson-loop` command runs in your current session:

```bash
# In current session (interactive)
/nelson-loop "Implement feature X" --max-iterations 20

# External loop (spawns fresh instances)
.nelson/nelson.sh --tool claude 10
.nelson/loop.sh 20
```

Both approaches are valid! Choose based on your needs:
- Current session: Interactive, can ask questions
- External loop: Autonomous, runs independently

## Complete Command Reference

```bash
# Setup
nelson-scaffold both .                  # Scaffold project

# PRD Generation
nelson-prd-generator                    # Interactive PRD creation
nelson-prd-agent                        # Conversational PRD creation

# Monitoring
nelson-status                           # One-shot view
nelson-status --watch                   # Auto-refresh (2s)
nelson-status --watch 10                # Custom interval

# Running Nelson
.nelson/nelson.sh --tool claude 10       # Toro mode
.nelson/loop.sh plan                    # Planning phase
.nelson/loop.sh 20                      # Build phase

# In-session
/nelson-loop "Task" --max-iterations 15
```

## Documentation Files

```bash
# Quick reference
cat ~/.nelson-templates/QUICK_START.md

# Full documentation
cat ~/.nelson-templates/README.md

# All commands
cat ~/.nelson-templates/COMMANDS.md

# PRD generator guide
cat ~/.nelson-templates/PRD_GENERATOR_PROMPT.md
```

## Credits

Nelson Loop Framework is built upon and inspired by the original Ralph Wiggum technique and implementations.

### Original Authors

- **Geoffrey Huntley** ([@ghuntley](https://github.com/ghuntley))
  - Original Ralph Wiggum technique
  - Repository: https://github.com/ghuntley/how-to-ralph-wiggum
  - Blog: https://ghuntley.com/ralph/

- **Snarktank Ralph** ([@snarktank](https://github.com/snarktank))
  - PRD-based implementation
  - Repository: https://github.com/snarktank/ralph

- **Mike O'Brien** ([@mikeyobrien](https://github.com/mikeyobrien))
  - Ralph Orchestrator
  - Repository: https://github.com/mikeyobrien/ralph-orchestrator

### What Nelson Adds

- **Quality Assurance Layer** - "Nelson Punch Ralph" aggressive review agent
- **Dual Modes** - Toro (PRD-based) + Plan/Build modes
- **Real-time Monitoring** - nelson-status dashboard
- **Interactive Generators** - PRD and specs generators
- **Compacting** - Context management after each story
- **Organized Structure** - All files in `.nelson/` directory

Special thanks to the Ralph Loop community for pioneering autonomous AI development patterns.

---

Now go throw punches. 🥊

Remember: Use `nelson-status --watch` to monitor progress.
