# Interactive Skills and Exercises

## Overview

Hands-on practice is the fastest way to build GitHub proficiency. This document covers interactive learning tools — from GitHub's official Skills courses to the new Exercise Builder plugin for creating custom training content.

---

## GitHub Skills

**Link:** [skills.github.com](https://skills.github.com)

GitHub Skills provides free, interactive courses that run directly in GitHub repositories.

### How It Works

1. Choose a course from the catalog
2. Click "Start" to create a personal copy of the course repo
3. Follow step-by-step instructions in Issues and Pull Requests
4. GitHub Actions validates your work and advances you through the course

### Popular Courses

| Course | Topic | Duration |
|--------|-------|----------|
| Introduction to GitHub | Repos, branches, commits, PRs | ~1 hour |
| Communicate using Markdown | Formatting, tables, task lists | ~1 hour |
| GitHub Pages | Static site deployment | ~1 hour |
| Review pull requests | Code review best practices | ~30 min |
| Reusable workflows | GitHub Actions patterns | ~1 hour |
| Code with Copilot | AI-assisted coding basics | ~1 hour |

---

## GitHub Skills Exercise Builder

**Link:** [github.com/arilivigni/gh-skills-builder](https://github.com/arilivigni/gh-skills-builder)

The GitHub Skills Exercise Builder is a Copilot plugin that helps educators and enablement leads create custom interactive learning exercises.

### What It Does

- Generates exercise templates in the GitHub Skills format
- Creates step-by-step learning paths with automated validation
- Integrates with GitHub Actions for progress tracking
- Supports custom scenarios tailored to your organization's workflows

### Use Cases

- **Internal onboarding** — Build exercises that teach your team's specific GitHub workflows
- **Bootcamps** — Create structured multi-day training programs
- **Workshops** — Generate hands-on labs for live enablement sessions
- **Certification prep** — Build practice exercises aligned to exam objectives

### Getting Started

1. Install the plugin from the repository
2. Use Copilot to describe the exercise you want to create
3. The builder generates the repository structure, instructions, and validation workflows
4. Share the exercise repo with learners

---

## Building Custom Exercises for Your Org

### Exercise Design Principles

1. **Start simple** — Each exercise should teach one concept
2. **Validate automatically** — Use GitHub Actions to check learner progress
3. **Provide hints** — Include guidance without giving away the answer
4. **Build progressively** — Later exercises should build on earlier ones
5. **Use real scenarios** — Base exercises on actual team workflows

### Example Exercise Structure

```
my-exercise/
├── .github/
│   ├── workflows/
│   │   └── validate.yml       # Auto-checks learner's work
│   └── steps/
│       ├── 01-intro.md        # Step 1 instructions
│       ├── 02-branch.md       # Step 2 instructions
│       └── 03-pr.md           # Step 3 instructions
├── README.md                  # Course overview
└── src/                       # Starter code for the exercise
```

---

## Tips for Enablement Leads

- Pair GitHub Skills courses with your certification prep program
- Use the Exercise Builder to create org-specific onboarding labs
- Track completion rates to identify where teams need more support
- Run "Skills Sprints" — focused 1-week challenges around a single topic
- Recognize completions publicly to drive engagement
