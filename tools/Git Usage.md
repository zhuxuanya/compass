# Git Usage

## Configuration
To set up user information with github:
```bash
git config --global user.name <username>
git config --global user.email <email>
git config --list
```

## Common operations

### Status check
To check the current status of the repository:
```bash
git status
```
Use `git status uno` to ignore untracked files.

### Branch management
To create and switch to a new branch:
```bash
git clone git@github.com:xxx.git -b base_branch
git checkout -b new_branch
git push -u origin new_branch
```

### Commit changes
Add changes and commit:
```bash
git add <file>
git commit -m "commit message"
git push
```
Use `git add .` to add all changes in the directory.

## Advanced operations

### Temporary storage
To stash changes temporarily:
```bash
git stash
git stash pop
```

### Roll back changes
To rollback the last commit:
```bash
git revert HEAD
git push
```
To rollback to a specific commit and discard all changes after it:
```bash
git log
git reset --hard <commit hash>
git push --force
```
Modes of `git reset`:
- `--soft`: Keeps the changes in the working directory.
- `--mixed` (default): Retains changes in the working directory but not in the staging area.

### Submodule management

Git submodules keep a git repository as a subdirectory of another git repository.  To add a submodule:

```bash
git submodule add <repository-url>
git submodule init
```

To initialize and update the submodule:

```bash
git submodule init
git submodule update
```

If the repository is with nested submodules:

```bash
git submodule update --init --recursive
```
