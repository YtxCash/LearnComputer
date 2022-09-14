# Git

Failed to connect to raw.githubusercontent.com输入访问不了的域名,查询之后可以获得正确的 IP 地址,本机
的 host 文件中添加

```git
https://www.ipaddress.com
sudo vim /etc/hosts
```

## SSH Key

[Authentication](https://docs.github.com/en/authentication/connecting-to-github-with-ssh)

## Config

```git
git config --global user.email ""
git config --global user.name ""
git config --global core.editor "nvim"
```

## Command

```git
git init
git status
git add
git commit
git log
git reflog
git log --pretty=short
git log --graph
git log xx.md
git log -p xx.md
git diff
git diff HEAD
```

## Branch

```git
- List all branches (local and remote; the current branch is highlighted by `*`):
    git branch --all

- List which branches include a specific Git commit in their history:
    git branch --all --contains commit_hash

- Show the name of the current branch:
    git branch --show-current

- Create new branch based on the current commit:
    git branch branch_name

- Create new branch based on a specific commit:
    git branch branch_name commit_hash

- Rename a branch (must not have it checked out to do this):
    git branch -m old_branch_name new_branch_name

- Delete a local branch (must not have it checked out to do this):
    git branch -d branch_name

- Delete a remote branch:
    git push remote_name --delete remote_branch_name
```

## Commit

```git
- Commit staged files to the repository with a message:
    git commit --message "message"

- Commit staged files with a message read from a file:
    git commit --file path/to/commit_message_file

- Auto stage all modified and deleted files and commit with a message:
    git commit --all --message "message"

- Commit staged files and sign them with the specified GPG key (or the one defined in the config file if no argument is specified):
    git commit --gpg-sign key_id --message "message"

- Update the last commit by adding the currently staged changes, changing the commit's hash:
    git commit --amend

- Commit only specific (already staged) files:
    git commit path/to/file1 path/to/file2

- Create a commit, even if there are no staged files:
    git commit --message "message" --allow-empty
```

## Remote

```git
- Show a list of existing remotes, their names and URL:
    git remote -v

- Show information about a remote:
    git remote show remote_name

- Add a remote:
    git remote add remote_name remote_url

- Change the URL of a remote (use `--add` to keep the existing URL):
    git remote set-url remote_name new_url

- Show the URL of a remote:
    git remote get-url remote_name

- Remove a remote:
    git remote remove remote_name

- Rename a remote:
    git remote rename old_name new_name
```

## Clone

```git
- Clone an existing repository into a new directory (the default directory is the repository name):
    git clone remote_repository_location path/to/directory

- Clone an existing repository and its submodules:
    git clone --recursive remote_repository_location

- Clone only the `.git` directory of an existing repository:
    git clone --no-checkout remote_repository_location

- Clone a local repository:
    git clone --local path/to/local/repository

- Clone quietly:
    git clone --quiet remote_repository_location

- Clone an existing repository only fetching the 10 most recent commits on the default branch (useful to save time):
    git clone --depth 10 remote_repository_location

- Clone an existing repository only fetching a specific branch:
    git clone --branch name --single-branch remote_repository_location

- Clone an existing repository using a specific SSH command:
    git clone --config core.sshCommand="ssh -i path/to/private_ssh_key" remote_repository_location

```

## Rebase

```git
- Rebase the current branch on top of another specified branch:
    git rebase new_base_branch

- Start an interactive rebase, which allows the commits to be reordered, omitted, combined or modified:
    git rebase -i target_base_branch_or_commit_hash

- Continue a rebase that was interrupted by a merge failure, after editing conflicting files:
    git rebase --continue

- Continue a rebase that was paused due to merge conflicts, by skipping the conflicted commit:
    git rebase --skip

- Abort a rebase in progress (e.g. if it is interrupted by a merge conflict):
    git rebase --abort

- Move part of the current branch onto a new base, providing the old base to start from:
    git rebase --onto new_base old_base

- Reapply the last 5 commits in-place, stopping to allow them to be reordered, omitted, combined or modified:
    git rebase -i HEAD~5

- Auto-resolve any conflicts by favoring the working branch version (`theirs` keyword has reversed meaning in this case):
    git rebase -X theirs branch_name

```

## Reset

```git
- Unstage everything:
    git reset

- Unstage specific file(s):
    git reset path/to/file1 path/to/file2 ...

- Interactively unstage portions of a file:
    git reset --patch path/to/file

- Undo the last commit, keeping its changes (and any further uncommitted changes) in the filesystem:
    git reset HEAD~

- Undo the last two commits, adding their changes to the index, i.e. staged for commit:
    git reset --soft HEAD~2

- Discard any uncommitted changes, staged or not (for only unstaged changes, use `git checkout`):
    git reset --hard

- Reset the repository to a given commit, discarding committed, staged and uncommitted changes since then:
    git reset --hard commit
```

## Clean

```git
git reflog expire --expire=7.days.ago --expire-unreachable=now --all
git reflog expire --expire=now --all
git gc --prune=now
git gc --aggressive --prune=now
```
