---
layout: post
title: "Git Commands for Everyday Workflows"
date: 2024-09-27
---

Git powers most collaborative software work. Keep this reference handy to cover 99% of your everyday branching, syncing, and clean-up tasks.

## Start Your Day Right

```bash
# Make sure you're on the main branch
git checkout main

# Fetch remote updates without modifying your work
git fetch origin

# Rebase your local main onto origin/main for a linear history
git pull --rebase origin main
```

## Branching and Switching

```bash
# Create a feature branch from the current branch
git checkout -b feature/amazing-improvement

# List local branches and highlight the current one
git branch

# Jump between branches
git checkout feature/amazing-improvement
git checkout main
```

Keep branch names descriptive so teammates understand the purpose at a glance.

## Stage and Commit with Confidence

```bash
# Review what's changed
git status

# Preview modifications before staging
git diff

# Stage specific files or whole directories
git add path/to/file.js
git add src/

# Interactively stage only the hunks you want
git add -p

# Craft a clear commit message
git commit -m "Summarize the change in the imperative mood"
```

Small, focused commits make reviews and rollbacks painless.

## Merge vs. Rebase

```bash
# Merge keeps both branch histories
git checkout main
git merge feature/amazing-improvement

# Rebase replays your commits on top of the latest main
git checkout feature/amazing-improvement
git rebase main

# Abort a rebase if something goes wrong
git rebase --abort
```

Use merges when you want to preserve the exact history, especially for shared feature branches. Prefer rebasing a private branch before opening a pull request to keep the timeline tidy.

## Review Before You Push

```bash
# Inspect staged vs. committed differences
git diff --staged

# Visualize upcoming pushes with a compact graph
git log --oneline --graph --decorate origin/main..HEAD
```

Catch surprises locally so remote builds stay green.

## Publish and Share

```bash
# Push a new branch to origin and track it
git push -u origin feature/amazing-improvement

# Update an existing remote branch
git push
```

Once merged, prune local branches to avoid clutter.

## Park Work in Progress

```bash
# Stash with a useful label
git stash push -m "WIP: responsive header"

# View available stashes
git stash list

# Reapply latest stash and keep it in the list
git stash apply

# Reapply and drop in a single command
git stash pop
```

Stashes keep experiments safe when you need to jump onto a hotfix.

## Undo Mistakes Safely

```bash
# Unstage without discarding edits
git restore --staged path/to/file.js

# Reset a file to the last commit
git restore path/to/file.js

# Roll back the last commit but keep the changes staged
git reset --soft HEAD~1

# Create a new commit that reverts another
git revert <commit-sha>
```

Undo strategies range from subtle to destructive—pick the smallest hammer that solves the issue.

## Clean Up Old Branches

```bash
# Remove a merged local branch
git branch -d feature/amazing-improvement

# Force-delete a branch that isn't merged yet
git branch -D feature/experimental-spike

# Delete the remote branch once it's merged
git push origin --delete feature/amazing-improvement
```

A tidy branch list helps teammates spot work in progress at a glance.

## Video Deep Dive

Level up with this comprehensive Git workflow tutorial:

<iframe width="560" height="315" src="https://www.youtube.com/embed/HVsySz-h9r4" title="Git for Professionals Tutorial" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Happy committing!
