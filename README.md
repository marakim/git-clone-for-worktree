# git-clone for git-worktree

Clone a git repository for use with
[git-worktree](https://www.git-scm.com/docs/git-worktree)


## Usage

```powershell
git-clone-for-worktree [-Repository] <string> [[-Directory] <string>] [-SingleBranch]
```

### Arguments

* **Repository**: The repository to clone from.
* **Directory**: The directory to clone into. By default the basename of the repository without extension.
* **SingleBranch**: Only fetch the main branch.


## Using Worktrees

You can clone a repository similar to using `git-clone` with:

```powershell
git-clone-for-worktree git@github.com:my-project/my-repo.git
```

This creates a git repository in `my-repo` with a folder
containing one worktree tracking the primary branch (usually `main`).

Once you enter this worktree, you can use normal commands like `git checkout`
to checkout new and existing branches.

### Creating New Worktrees

If a pressing change requires a hotfix, you can create a new `hotfix` worktree
by going to the repository root and running:

```powershell
git worktree add -b urgent-bugfix hotfix main
```

This creates a new branch `urgent-bugfix` and a new `hotfix` worktree for that branch
based off `main` without disturbing your other worktrees.

For more information on worktrees, see the documentation for [git-worktree](https://git-scm.com/docs/git-worktree).


## Single Branch with Topics

Projects with many contributors can have large numbers of branches.
In these cases, it is common to prefix branch names with your username.
Syncing every branch on these repositories takes a long time,
so it is useful to clone in single branch mode and specify explicit topic branches to sync.

To setup a repository to have `git pull` sync only the main branch and branches with the prefix `USERNAME/`:

```powershell
git-clone-for-worktree -SingleBranch git@github.com:my-project/my-repo.git
cd my-repo
git config --add remote.origin.fetch '+refs/heads/USERNAME/*:refs/remotes/origin/USERNAME/*'
```

You can still fetch individual branches, for example `OTHERUSER/their-branch`, with:

```powershell
git fetch origin OTHERUSER/their-branch:refs/remotes/origin/OTHERUSER/their-branch
```