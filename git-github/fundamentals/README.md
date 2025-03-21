# Git Fundamentals

This section covers the fundamental concepts and commands of Git, from basic setup to understanding its internal structure.

## Quick Reference

| Command                                              | Description                     |
| ---------------------------------------------------- | ------------------------------- |
| `git config --global user.name "Your Name"`          | Set global user name            |
| `git config --global user.email "email@example.com"` | Set global email                |
| `git init`                                           | Initialize a new Git repository |
| `git clone <url>`                                    | Clone a repository              |
| `git status`                                         | Check repository status         |
| `git add <file>`                                     | Add file to staging area        |
| `git commit -m "message"`                            | Commit staged changes           |
| `git log`                                            | View commit history             |
| `git remote add origin <url>`                        | Add remote repository           |
| `git push -u origin main`                            | Push to remote repository       |
| `git pull`                                           | Pull changes from remote        |

## Topics Covered

- [Installation and Configuration](./installation-configuration.md)
- [Git Workflow](./git-workflow.md)
- [Local vs Remote Repositories](./local-remote-repositories.md)
- [File States in Git](./file-states.md)
- [.gitignore and File Exclusion](./gitignore-exclusion.md)
- [Git Internal Structure](./internal-structure.md)
- [Offline Git Usage](./offline-usage.md)

## Key Concepts

Git is a distributed version control system designed to handle everything from small to very large projects with speed and efficiency. Understanding its fundamentals is essential for effective collaboration and source code management.

In this section, you'll learn how Git tracks changes, manages different versions of your codebase, and facilitates collaboration between team members through a decentralized approach to version control. 