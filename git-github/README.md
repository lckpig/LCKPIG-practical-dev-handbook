# Git + GitHub Practical Guide

This practical guide provides a comprehensive reference for Git and GitHub, from basic concepts to advanced techniques for professional development teams. The guide is structured to serve both as a quick reference cheat sheet and as a detailed learning resource.

## Detailed Table of Contents

<details>
<summary><strong>1. Fundamentals of Git</strong></summary>

- **Installation and Configuration**
  - Installation in Windows, macOS and Linux
  - Configuration global and per repository (`git config`)
  - User and authentication configuration (`user.name`, `user.email`)
  - Editor and external tools configuration (`core.editor`, `merge.tool`)
  - Git aliases for command optimization (`git config --global alias...`)
- **Git Workflow**
  - Repository initialization (`git init`)
  - Repository cloning (`git clone`)
  - File states (`git status`)
  - Adding files to tracking (`git add`)
  - Committing changes (`git commit`)
  - Viewing history (`git log`)
- **Local vs Remote Repositories**
  - Differences between local and remote repositories
  - Remote repository configuration (`git remote add origin`)
  - Remote visualization and management (`git remote -v`, `git remote rm`)
- **File States in Git**
  - `Untracked`: New files not added
  - `Modified`: Files with changes not staged
  - `Staged`: Files ready for commit
  - `Committed`: Changes saved in history
  - Using `git diff` to see changes between states
- **.gitignore and File Exclusion**
  - `.gitignore` syntax
  - Creating global rules
  - Ignoring versioned files (`git rm --cached`)
  - `.gitkeep` files in empty directories
- **Git Internal Structure**
  - **Blobs:** Store file contents
  - **Trees:** Contain references to blobs and other trees
  - **Commits:** Snapshot of repo state at a specific point
  - **Refs:** Pointers to commits (`HEAD`, `branches`, `tags`)
  - Structure visualization with `git cat-file` and `git ls-tree`
- **Distributed Git Systems**
  - Working without access to remote repositories
  - Using `git bundle` to share changes offline
  - Exporting commits with `git format-patch`
  - Importing commits with `git am`
</details>

<details>
<summary><strong>2. Commit Management</strong></summary>

- **Creating, Modifying and Deleting Commits**
  - `git commit -m "Message"`: Create a commit
  - `git commit -a`: Automatic commit of modified files
  - `git commit --amend`: Modify the last commit
  - `git reset HEAD~1`: Undo the last commit keeping changes
  - `git revert HEAD`: Revert the last commit without altering history
- **Effective Commit Messages**
  - Message convention: Format and best practices
  - Using prefixes (`feat:`, `fix:`, `chore:`, `refactor:`)
  - Relation between commits and issue management (`#123`)
- **Using `git commit --amend`**
  - Correcting the last commit message
  - Adding omitted files without generating a new commit
  - Modifying commit date and author
- **Differences Between `git reset`, `git revert` and `git restore`**
  - **`git reset`**: Modifies history (soft, mixed, hard)
  - **`git revert`**: Creates a new commit inverting changes
  - **`git restore`**: Restores files without modifying history
- **Rebase vs. Merge**
  - **Merge (`git merge`)**
    - Joins branches without altering original commits
    - Creates an additional merge commit
  - **Rebase (`git rebase`)**
    - Reapplies commits on a new base
    - Cleans history, useful in feature branches
  - Recommended use cases for each strategy
- **Commit Signing & Verification**
  - GPG key generation
  - Signing configuration in Git (`git config --global user.signingkey`)
  - Automatic commit signing (`git commit -S`)
  - Commit verification in GitHub (`Verified` badge)
- **Advanced Squashing**
  - Using `git rebase -i HEAD~n` to combine commits
  - `fixup` and `squash`: differences and best practices
  - Squashing in Pull Requests before merging
  - How to avoid conflicts when doing interactive rebase
</details>

<details>
<summary><strong>3. Branches and Workflows</strong></summary>

- **Creating and Deleting Branches**
  - `git branch <name>`: Create a new branch
  - `git checkout -b <name>`: Create and switch to a new branch
  - `git switch -c <name>`: Modern alternative to `checkout -b`
  - `git branch -d <name>`: Delete a local branch
  - `git push origin --delete <name>`: Delete a remote branch
- **Branching Strategies**
  - **Feature Branch**
    - Branches for new features (`feature/<name>`)
    - Use with Pull Requests before merging
  - **GitFlow**
    - Main branches: `main`, `develop`
    - Support branches: `feature/`, `release/`, `hotfix/`
    - Workflow and tools like `git-flow`
  - **Trunk-Based Development**
    - Using `main` as single branch
    - Feature Flags instead of long branches
    - Continuous integration and deployment (CI/CD)
- **Merge vs. Rebase**
  - **Merge (`git merge`)**
    - Branch fusion with additional commit
    - History with bifurcations
    - Recommended strategy for shared repositories
  - **Rebase (`git rebase`)**
    - Reapplication of commits on another branch
    - Linear history without merge commits
    - Use in feature branches before integrating into `main`
  - **When to use each one**
- **Resolving Merge Conflicts**
  - Detecting conflicts with `git status` and `git diff`
  - Manually editing conflicting files
  - Using graphical tools (`git mergetool`)
  - Marking resolved conflicts with `git add`
  - Completing the merge with `git commit`
- **Cherry-picking**
  - `git cherry-pick <commit>`: Apply a specific commit in another branch
  - Use in hotfixes without merging complete branches
  - Multiple cherry-picking with `git cherry-pick A B C`
  - Resolving conflicts in cherry-picking
- **Advanced Trunk-Based Development**
  - Feature Flags to deploy incomplete code
  - Feature Environments for testing before integration
  - Short-duration strategies to avoid stagnant branches
  - Avoiding conflicts with frequent merges from `main`
- **Pull Request Best Practices**
  - Creating PRs from feature branches
  - Assigning reviewers and online discussion
  - Using PR templates to standardize contributions
  - `CODEOWNERS` integration for mandatory approvals
  - Squash & Merge to maintain a clean history
</details>

<details>
<summary><strong>4. Remote Repositories and Collaboration</strong></summary>

- **Cloning and Remote Configuration**
  - `git clone <URL>`: Clone a repository
  - `git clone --depth=1 <URL>`: Shallow cloning for performance optimization
  - `git remote add <name> <URL>`: Add a new remote
  - `git remote remove <name>`: Remove a remote
- **Managing Multiple Remotes**
  - `git remote -v`: View configured remotes
  - Adding additional remotes (`upstream`, `fork`)
  - Working with forks and upstream (`git fetch upstream`, `git rebase upstream/main`)
- **Fetch, Pull and Push**
  - `git fetch`: Download changes without applying them
  - `git pull`: Download and merge changes (`-rebase` to avoid merge commits)
  - `git push`: Send commits to remote
  - Force update (`git push --force-with-lease`)
- **Forks and Pull Requests**
  - Difference between fork and clone
  - Synchronizing a fork with the original repository
  - Creating Pull Requests from forks
  - Keeping forks updated without losing changes
- **GitHub Flow**
  - Flow based on feature branches and PRs
  - Synchronization with `main` before merging
  - PR validations: Linters, automated tests
  - Closing issues from commits (`fixes #123`)
- **Managing Multiple Forks**
  - Cloning and configuring a fork correctly
  - Keeping the fork synchronized with `git rebase upstream/main`
  - Sending effective contributions with well-documented PRs
- **GitHub Codespaces**
  - Configuring a Codespace from a repository
  - Customizing VS Code settings in Codespaces
  - Using remote environments without needing to clone repositories
</details>

<details>
<summary><strong>5. History and Debugging</strong></summary>

- **Advanced git log**
  - `git log --oneline --graph --decorate`: Compact history visualization
  - `git log --author="name"`: Filter commits by author
  - `git log --since="2 weeks ago"`: View commits within a date range
  - `git log -S "string"`: Search for specific changes in history
  - `git shortlog -sn`: View contributions from each author
- **Using git diff**
  - `git diff HEAD`: Compare local changes with the last commit
  - `git diff --staged`: View differences of staged files
  - `git diff <branch1>..<branch2>`: Compare differences between branches
  - `git diff --word-diff`: Compare changes at word level
- **git blame and git bisect**
  - `git blame <file>`: See who modified each line of a file
  - `git bisect start`: Start binary search for errors
  - `git bisect bad` / `git bisect good`: Mark commits in debugging
  - `git bisect reset`: End debugging and restore state
- **Restoring Changes**
  - `git restore <file>`: Discard changes without affecting stage
  - `git checkout -- <file>`: Old alternative to `git restore`
  - `git reset HEAD <file>`: Unstage a file
  - `git reset --hard HEAD~1`: Remove the last commit without leaving traces
  - `git reflog`: Recover accidentally deleted commits
- **Recovering Lost Commits**
  - `git reflog show HEAD`: View history of recent movements
  - `git reset --hard HEAD@{n}`: Restore to a previous state
  - `git checkout HEAD@{n}`: Examine an old version before restoring
- **Advanced git bisect**
  - Automation with `git bisect run <script>`
  - Finding regressions in code with automated tests
  - Strategies to optimize debugging in large teams
- **Git Grep & Fuzzy Search**
  - `git grep "expression"`: Search within code in all versions
  - `git grep -n "expression"`: Show lines where a pattern appears
  - `git grep -c "expression"`: Count occurrences in versioned files
  - `git grep --break --heading`: Group results by file
</details>

<details>
<summary><strong>6. Advanced Version Control</strong></summary>

- **Tags and Semantic Versioning**
  - `git tag <name>`: Create a simple tag
  - `git tag -a <name> -m "message"`: Create an annotated tag
  - `git tag -d <name>`: Delete a tag
  - `git push origin <tag>`: Publish a tag to remote
  - `git push origin --tags`: Push all tags to remote
  - Using tags in semantic versions (`v1.0.0`, `v2.1.3`)
- **Git Stash**
  - `git stash`: Save changes without committing them
  - `git stash list`: View list of saved stashes
  - `git stash pop`: Restore the most recent changes
  - `git stash apply stash@{n}`: Apply a specific stash
  - `git stash drop stash@{n}`: Delete a specific stash
  - `git stash save --include-untracked`: Save changes including unversioned files
- **Submodules**
  - `git submodule add <URL>`: Add a submodule
  - `git submodule init && git submodule update`: Clone a repo with submodules
  - `git submodule foreach git pull origin main`: Update all submodules
  - Strategies for working with modular projects
- **Git Hooks**
  - Configuration of hooks in `.git/hooks/`
  - `pre-commit`: Validations before committing
  - `post-commit`: Automatic actions after a commit
  - `pre-push`: Validations before pushing
  - Using `husky` to manage hooks in large projects
- **Git LFS**
  - `git lfs install`: Configure Git LFS
  - `git lfs track "*.bin"`: Track heavy files
  - `git lfs push origin main`: Upload LFS files to remote
  - Performance optimization in repositories with heavy files
- **Rewriting History**
  - `git filter-repo --path <file>`: Remove specific files from history
  - `git filter-repo --invert-paths`: Remove multiple files keeping the rest
  - `git filter-repo --replace-text <file>`: Mass text replacement in commits
  - Advanced use cases for history cleaning
- **Managing Large Files**
  - Alternatives like `git sparse-checkout` to reduce repo size
  - `.gitattributes` configuration to avoid storing large files
  - Using external repositories to handle heavy assets
- **Working with Worktrees**
  - `git worktree add <path> <branch>`: Create a new copy of the repository on a branch
  - `git worktree list`: View all active instances of the repo
  - `git worktree remove <path>`: Remove a worktree instance
  - Use cases for parallel development and testing in multiple versions
</details>

<details>
<summary><strong>7. GitHub Basics</strong></summary>

- **What is GitHub and What is it Used For?**
  - Difference between Git and GitHub
  - Using GitHub as a collaboration platform
  - Benefits of GitHub compared to other services
- **Creating a GitHub Account**
  - Registration and initial setup
  - Authentication configuration with SSH and PAT (Personal Access Token)
- **First Steps in GitHub Interface**
  - **Creating a repository from the web**
    - Visibility configuration (public/private)
    - Initialize with a `README.md` and `.gitignore`
  - **Cloning a repository from GitHub**
    - Cloning with HTTPS vs. SSH
    - Cloning a fork and keeping it updated
  - **Uploading files to a repository manually**
    - Using the graphical interface to add files
    - Editing files directly from GitHub
- **Connecting a Local Repository with GitHub**
  - `git remote add origin <URL>`: Link local repository with remote
  - `git push -u origin main`: Upload the first commit to remote
  - `git pull origin main`: Get remote changes
- **git push and git pull in Remote Repositories**
  - Differences between `fetch`, `pull` and `push`
  - Solving conflicts when pulling with local changes
- **Issues and Basic Task Management**
  - Creating issues and assigning to team members
  - Labels and issue states (`open` / `closed`)
  - Using `@mentions` and references to commits or PRs
- **Using README.md to Document a Project**
  - Basic Markdown syntax in GitHub
  - Adding images and links in README.md
  - Using tables and sections in README.md
- **Introduction to Forks and Pull Requests**
  - Difference between cloning and forking
  - Creating a PR from a fork
  - Reviewing and accepting a PR in third-party repositories
- **Licenses and Repository Privacy Management**
  - Selecting licenses for open source projects (MIT, GPL, Apache)
  - Configuring private and public repositories
  - Setting up teams and permissions in GitHub
- **Creating and Using GitHub Gists**
  - Difference between repositories and Gists
  - Creating public and private Gists
  - Sharing and modifying Gists from the web interface
</details>

<details>
<summary><strong>8. GitHub Advanced</strong></summary>

- **GitHub Actions: Automation in CI/CD**
  - **YAML workflow configuration**
    - Definition of jobs and steps (`jobs`, `steps`)
    - Using `runs-on` to choose execution environments
  - **Running automated tests**
    - Integration with Jest, Mocha, Cypress and other frameworks
    - Code validation before merges (`pre-merge checks`)
  - **Automated deployments with GitHub Actions**
    - Strategies for deployments on Vercel, Netlify, AWS and GCP
  - **Using matrices for testing in multiple environments**
    - Running tests on different versions of Node.js, Python, Java
  - **Advanced strategies: Parallel and dependent jobs**
    - Using `needs:` to define dependencies between jobs
- **GitHub Packages**
  - **Using GitHub Package Registry (NPM, Docker, Maven, etc.)**
  - **Publishing private and public packages**
  - **Integration with Actions for automated deployments**
- **GitHub Security**
  - **Dependabot:** Automatic scanning and updating of vulnerable dependencies
  - **Code Scanning:** Security analysis in source code
  - **Secret Scanning:** Protection against credential exposure
  - **GitHub Security Advisories:** Vulnerability management in projects
- **Advanced Repository Management**
  - **Permission and access configuration with teams and organizations**
  - **Advanced branch protection (`required reviews`, `required status checks`)**
  - **Using GitHub API to automate tasks with custom scripts**
  - **GitHub Webhooks: Integrations and automation**
- **Pull Requests and Code Reviews**
  - **Conventions and standards for effective PRs in large teams**
  - **Code reviews: Comments, approvals and blocks**
  - **Using PR templates to standardize contributions**
  - **GitHub Discussions: Use in teams and open-source projects**
- **GitHub Copilot & AI in Collaborative Development**
  - **GitHub Copilot configuration and customization**
  - **Limitations and best practices**
  - **Using Copilot in Pair Programming and teams**
- **GitHub CLI**
  - **Creating, managing and merging Pull Requests from terminal**
  - **Managing Issues and Releases with GitHub CLI**
  - **Interacting with GitHub Actions workflows from CLI**
- **Automation and DevOps with GitHub**
  - **Continuous Deployment (CD) strategies with GitHub Actions**
  - **Integration with Docker and Kubernetes**
  - **Implementing GitOps with GitHub and ArgoCD**
  - **Strategies for Infrastructure as Code (IaC) using GitHub**
- **Using GitHub in Open Source Projects**
  - **Strategies for maintaining active repositories**
  - **Issue management and effective labeling**
  - **How to handle external contributions and code reviews**
- **GitHub Enterprise**
  - **Differences with standard GitHub**
  - **Security, audits and compliance in enterprise environments**
  - **Private environment configuration in GitHub Enterprise Cloud and Server**
</details>

<details>
<summary><strong>9. Collaborative Workflows</strong></summary>

- **Pair Programming with Git**
  - Using `git pair` to register commit co-authors
  - Co-authoring configuration in GitHub (`Co-authored-by:` in commits)
  - Pair programming tools: VS Code Live Share, JetBrains Code With Me
- **Effective Code Reviews**
  - Strategies for reviewing code in Pull Requests
  - Using `suggested changes` in GitHub for clear feedback
  - How to structure constructive comments in reviews
  - Using `git diff` and `git blame` to understand previous changes
- **Pre-PR Squashing**
  - Using `git rebase -i` to clean history before merging
  - When to use `squash` vs. `fixup` in commits
  - Strategies to avoid conflicts during rebase
- **Distributed Team Strategies**
  - Using Git in different time zones
  - Integrating PRs asynchronously with automatic approvals
  - Branch synchronization between remote developers
- **Team Conflict Resolution**
  - Strategies to avoid conflicts in shared branches
  - Using `git rerere` to remember previous solutions
  - Integration of graphical merge tools (`kdiff3`, `Beyond Compare`)
- **Feature Flags with Git**
  - Implementing Feature Flags in code (`launchdarkly`, `unleash`)
  - "Dark Launching" strategy for new features
  - Alternatives to feature branches with `trunk-based development`
- **Auditing with git blame**
  - `git blame <file>`: See who is responsible for each line of code
  - `git blame -L 20,30 <file>`: Audit changes in a line range
  - Strategies for debugging and reviewing critical changes
</details>

<details>
<summary><strong>10. Optimization and Best Practices</strong></summary>

- **Best Practices in Commit Structure**
  - Using atomic and descriptive commits
  - Structuring commits following Conventional Commits (`feat:`, `fix:`, `chore:`)
  - Avoiding commits with generic messages (`WIP`, `fix bug`, etc.)
- **Strategies to Avoid Frequent Conflicts**
  - Keeping branches synchronized with `git pull --rebase`
  - Using `git stash` before changing branches
  - Applying `git rerere` to remember previous conflict solutions
- **Git Performance and Optimization in Large Projects**
  - Using `git gc` to optimize object storage
  - Configuring `core.compression` to speed up clones and fetches
  - Activating `git sparse-checkout` to avoid downloading unnecessary files
- **Monorepos vs. Multirepos in Git**
  - Benefits and challenges of monorepos in large teams
  - Using `git subtree` to manage modular projects
  - Monorepo alternatives with `nx` and `Lerna`
- **Credential Security and Repository Access**
  - Using `git credential.helper` to manage authentication
  - SSH and GPG configuration for secure authentication
  - Using personal tokens (`PAT`) instead of passwords in GitHub
- **Repository Performance Optimization**
  - Using `git repack` to reorganize objects
  - `git prune` to remove references to orphaned commits
  - `git gc --aggressive`: When to use it and when to avoid it
- **Efficient Use of .gitattributes**
  - Configuring `text=auto` to normalize line endings
  - Using `export-ignore` to exclude files in distributed packages
  - `diff=word` to improve comparisons in text files
</details>

<details>
<summary><strong>11. Special Cases and Use Cases</strong></summary>

- **Git in Limited Access or Offline Environments**
  - Using `git bundle` to share repositories offline
  - Cloning repositories offline with `git clone --mirror`
  - Exporting and importing patches with `git format-patch` and `git apply`
- **Migration Strategies to Git from Other VCS**
  - Migration from SVN with `git svn`
  - Converting Mercurial (hg) to Git
  - Importing old histories without losing commits
- **Using Git in Open Source Projects**
  - Creating `CONTRIBUTING.md` to guide collaborators
  - Strategies for managing multiple simultaneous Pull Requests
  - Using `good first issue` and `help wanted` in GitHub issues
- **Secrets and Secure Configuration in Public Repositories**
  - Using `.gitignore` to avoid including sensitive files
  - Configuring `pre-commit hooks` to prevent credential leaks
  - Scanning secrets in commits with tools like `gitleaks` and `truffleHog`
- **Signing Commits and Identity Verification**
  - Digital signature configuration with `GPG`
  - Verification of signed commits in GitHub
  - Using SSH instead of HTTPS for secure authentication
- **Migration to GitHub Enterprise and GitLab**
  - Differences between GitHub Enterprise and GitHub Cloud
  - Repository migration strategies with `git push --mirror`
  - Advanced permission configuration in enterprise environments
- **Git in Long Life Cycle Projects**
  - Using `git reflog expire --all --expire=90.days.ago` to clean old references
  - Removing unnecessary files with `git filter-repo`
  - Branch consolidation strategies in long-term projects
</details>

<details>
<summary><strong>12. Visual Reference and Tools</strong></summary>

- **Git Command Cheatsheet**
  - Comprehensive visual reference of common Git commands
  - Command syntax and examples
  - Quick reference diagrams for Git workflows
- **IDE Integration**
  - Git integration with Visual Studio Code
  - Git plugins for JetBrains IDEs (IntelliJ, WebStorm)
  - Eclipse and Visual Studio Git extensions
  - Best practices for using Git within IDEs
- **Visual Git Tools**
  - GitKraken
  - Sourcetree
  - GitHub Desktop
  - GitLens for VS Code
  - Comparing features of different visual Git clients
- **GitHub Pages**
  - Setting up GitHub Pages for projects
  - Jekyll themes and customization
  - Using GitHub Actions for automated deployment
  - Custom domains and SSL with GitHub Pages
- **Repository Statistics**
  - GitHub Insights for contribution analysis
  - Using `git-quick-stats` for local repositories
  - Visualizing commit patterns and team contributions
  - Metrics for evaluating repository health
- **Troubleshooting Guide**
  - Solutions for common Git problems
  - Recovering from detached HEAD states
  - Fixing merge conflicts in complex scenarios
  - Troubleshooting network and authentication issues
- **Context-Specific Git**
  - Git strategies for game development
  - Git with containerized development environments
  - Git in regulated environments (compliance, auditing)
  - Configuration for large binary assets and media projects
</details>

## Quick Reference Sections

- [Fundamentals of Git](./fundamentals/README.md)
- [Commit Management](./commit-management/README.md)
- [Branches and Workflows](./branches-workflows/README.md)
- [Remote Repositories and Collaboration](./remote-collaboration/README.md)
- [History and Debugging](./history-debugging/README.md)
- [Advanced Version Control](./advanced-version-control/README.md)
- [GitHub Basics](./github-basics/README.md)
- [GitHub Advanced](./github-advanced/README.md)
- [Collaborative Workflows](./collaborative-workflows/README.md)
- [Optimization and Best Practices](./optimization-best-practices/README.md)
- [Special Cases and Use Cases](./special-cases/README.md)
- [Visual Reference and Tools](./visual-reference-tools/README.md)

## About This Guide

This Git and GitHub guide is designed for developers at all levels, providing both quick reference commands and detailed explanations of concepts. It focuses on practical applications, best practices, and professional workflows that go beyond official documentation.

Key features of this guide:
- Clear, concise command references organized by function
- Professional-level explanations of Git internals and workflows
- Focus on real-world scenarios and team collaboration strategies
- Performance optimization techniques for large repositories
- Practical examples of common and advanced Git operations

Whether you're looking to quickly reference a Git command, understand proper branching strategies, or implement advanced CI/CD workflows with GitHub Actions, this guide provides the technical knowledge you need without unnecessary filler content.

## How to Use This Guide

- Start with the fundamentals if you're new to Git
- Use the cheat sheets for quick command reference
- Explore advanced sections for professional techniques
- Refer to the best practices section to improve your workflow
- Check the troubleshooting sections when facing challenges

## Contributing

This is a living document. For contributions or corrections, please submit a pull request through the GitHub repository. 