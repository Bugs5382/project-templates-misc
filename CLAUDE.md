# Project :: Template MIsc

This repo holds reusable, battle-tested template files that get copied into other projects. It is not itself a buildable project — treat the contents as source material to be adapted, not as code to run from here.

## Layout

The repo is organized by template type, one folder per ecosystem. Current folders:

- `npm/` — Node/npm projects
- `go/` — Go projects

More may be added over time (other languages, runtimes, or project shapes). When new top-level folders appear, treat each as its own self-contained template set with the same "copy and adapt" model described below.

## What's in `npm/`

The `npm/` folder contains the base set of files I expect to have in any Node/npm-based project. When I tell you in the future to "include the base npm template files" (or similar) in a project, copy from this folder as the starting point.

Currently it contains:

- `.github/workflows/` — CI/CD GitHub Actions (test, deploy to npm, version bump, dependabot auto-merge, label checker)
- `.github/dependabot.yml` and `.github/release-drafter.yml` — Dependabot + Release Drafter config
- `.husky/` — Husky git hooks (`commit-msg`, `pre-commit`)

These files are production-grade and battle-tested, but they are **not drop-in**. You must modify them for each target project — package name, script names, Node versions, branch names, secrets, etc. The required edits differ depending on whether the target is a **single-package repo** or a **monorepo** (workspace paths, matrix strategies, and publish targets all change).

## What's in `go/`

Base set of files I expect to have in any Go project. When I tell you to "include the base go template files" (or similar), copy from this folder as the starting point.

Currently it contains:

- `.github/workflows/` — GitHub Actions for Go (lint via golangci-lint, test, release + version bump, license check, YAML lint, label checker)
- `.github/release-drafter.yml` — Release Drafter config

Same caveats as `npm/`: production-grade but **not drop-in**. Module path, Go version, branch names, secrets, and matrix strategy must be edited per project. Mono-repo vs single-module repos require different workflow shapes.

## What's NOT in this repo

Project-scaffolding files (e.g. for npm: `typedoc.json`, `tsdown.config.ts`, `tsconfig.json`, `package.json`, source layout) live in a separate repo: **`project-npm-template`**. Don't try to source those from here. Equivalent Go scaffolding (e.g. `go.mod`, source layout, lint config) similarly does not belong in this repo.

## Per-project responsibilities

When applying these templates to a new project, also:

- **Create an appropriate `.github/ISSUE_TEMPLATE/` folder** for that project (bug report, feature request, etc.). Issue templates are intentionally not stored here because they should be tailored to each project's domain and workflow.
- Verify CODEOWNERS matches the new project's reviewers.
- Reconcile the husky hooks with the project's actual lint/test commands.
- **Ensure a non-git-tracked `plan/` folder exists** at the project root (add `plan/` to `.gitignore` if missing). This folder stores pending Claude issues/notes as individual files. When an issue is resolved, **delete** its file — the file's existence is the signal that work is still open. Treat existing files in `plan/` as open issues worth surfacing when relevant.

## Commit rules

When committing on behalf of the user:

- **Never commit to `main`** (or `master`). Only commit to the user's currently checked-out working/feature branch. If the current branch is `main`, stop and ask the user to switch or create a branch first.
- **Do not include Claude attribution** in commit messages. No `Co-Authored-By: Claude ...` trailer, no "Generated with Claude Code" footer, no mention of Claude/Anthropic anywhere in the message.
