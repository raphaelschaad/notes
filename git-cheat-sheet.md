GIT CHEAT SHEET

A collection of useful Git commands and good Git practices.

Also see my dotfiles [.gitconfig](https://github.com/raphaelschaad/dotfiles/blob/master/.gitconfig) and [.gitignore_global_macos](https://github.com/raphaelschaad/dotfiles/blob/master/.gitignore_global_macos).

# Links
- [Understanding the Git Workflow](https://sandofsky.com/blog/git-workflow.html)

# Commit message convention
Blog post [My favourite Git commit](https://fatbusinessman.com/2019/my-favourite-git-commit) and accompanying [HN thread](https://news.ycombinator.com/item?id=21289827).

# Branch naming convention
Use hierarchical branch names to group branches: `<type>/<name>`. Examples for `<type>`:
1. `feature` Add a new feature.
2. `bugfix` Address a known issue.
3. `style` Format (e.g. whitespace), add missing semicolons, etc.
4. `content` Change content (text, image, etc.).
5. `documentation` Add comments or documentation.
6. `test` Add tests.
7. `performance` Improve performance without changing the function.
8. `refactor` Restructure or rename code without changing the function.
9. `chore` Maintain (new syntax, auxiliary tools, etc.)
10. `experiment` Do not merge.

For `<name>` be descriptive but keep it short. Use dashed-lowercase-multi-words (kebab case).

# Pull Requests
Open pull requests with the GitHub web interface. There is the CLI tool `hub` but I think it's rare enough of an action and the web interface is clearer than it would justify to do it in any other way.

# General commands
    $ git fetch origin # update origin remote branches
    $ git rebase -p origin/master # (on master) Pull in changes and preserve feature branch on master, when it could fast-forward
    $ git rebase origin/master # (on feature branch) Prevent feature branch from diverging from master significantly
    $ git log master..origin/master # changes on the master branch (commit-level)
    $ git log -p master..origin/master # changes on the master branch (code-level, "patch")
    $ git merge origin/master # merge changes of master branch
    $ git pull # update remote and merge changes in current branch
    $ git pull --rebase # avoid noisy/unhelpful merge commits on master when local changes were not made on a feature branch
    $ git checkout -b bug-toc-tile # create local branch
    $ git diff master -- Classes/FLStatusUpdateView.m # changes limited to file
    $ git add -i # interactively add parts
    $ git add -p # interactively add parts (bypassing initial menu of -i and jump directly to patching)
    $ git diff # unstaged changes vs. current HEAD
    $ git diff --staged # staged changes vs. current HEAD
    $ git reset HEAD # unstage
    $ git reset --hard # discard uncommitted changes; use with caution
    $ git commit
    $ git diff master > bug-toc-file.patch
    $ git checkout master
    $ git merge bug-toc-tile
    $ git push origin master # push current local branch to remote branch named `master` (creates if doesn't exist)
    $ git branch -d bug-toc-tile # delete a local branch
    $ git branch -D bug-toc-tile # force delete an unmerged local branch
    $ git push --force origin ui-fixes # force push to a remote branch (e.g. after rebasing from master)
    $ git push origin :ui-fixes # delete a remote branch
    $ git cherry-pick 5c294197597a59d63e00b32835487d289aed1afa # on branch to cherry-pick to the commit, creates new commit with new hash
    $ git checkout path/to/file/to/revert # discard changes in specific file; don't use reset --hard for this
    $ git checkout . # discard changes in all unstaged files
    $ git branch -m new-branch-name # rename current branch
    $ git log -Stext # search for string "text" in entire repo history
    $ git checkout <deleting commit>^ -- path/to/deleted/file/to/restore
    $ git commit --amend # Update latest commit with what's staged and/or reword its commit message
    $ git rebase -i <commit>^1 # Edit, squash, reword commit message, etc. back up to <commit>
    $ git checkout -t origin/iphone # track remote iphone branch on local iphone branch (automatically created)
    $ git checkout curator Classes/FLSheetViewController.h # copy file from other branch, "curator" could also be a specific commit hash
    $ git remote add upstream https://github.com/octocat/Spoon-Knife.git # keep track of original repo in fork
    $ git fetch upstream # pull in new changes of original repo in fork without updating it
    $ git merge upstream/master # update fork with changes of original repo
    $ git ls-files # At root of repo, shows all files tracked by git
    $ git clean -xfd # Delete untracked files
    $ git log --all --author="Raphael" # Show commits by author (across all branches)

# Moving local commits from master to feature branch
    ... local commits on master ...
    $ git checkout -b feature-branch
    $ git checkout master
    $ git reset --hard origin/master
    $ git checkout feature-branch

# Stashing
    $ git stash
    $ git stash save "Content Guide hint animation"
    $ git stash -p|--patch # stash only specific files; 'd' for skipping 'a' for stashing
    $ git stash -u|--include-untracked # also stash untracked files
    $ git stash list
    $ git stash show stash@{0}
    $ git stash show -p|--patch stash@{0}
    $ git stash pop
    $ git stash pop stash@{1}
    $ git stash drop # drops stash@{0}
    $ git stash drop stash@{1}
    $ git stash clear # drop all

# Tagging
    $ git tag -a 1.0 -m "1.0"
    $ git tag # list tags
    $ git show-ref --tags # list tags with commit hash (lightweight) or tag hash (annotated)
    $ git tag -ln tagname # show tag with commit message (lightweight) or tag message (annotated)
    $ git push origin --tags # push tags to remote
    $ git tag -d tagname # delete local tag
    $ git push origin :refs/tags/tagname # delete remote tag

# Visual merge conflict resolving (ARCHIVE)
    $ git mergetool # hit enter to start; on OS X defaults to FileMerge.app (pretty good); resolve conflict, save and exit

# Submodules (ARCHIVE)
## Add submodule
    $ git submodule add https://github.com/Flipboard/JSONKit.git 3rdPartyUtils/JSONKit
    $ git remote add upstream git://github.com/johnezang/JSONKit.git # in submodule
    $ git commit -m "Added submodule 3rdPartyUtils/JSONKit"
    $ git checkout v1.4
    $ git commit -m "Checkout JSONKit at v1.4"

## Fix/customize submodule
    $ git checkout -b fix # always on separate branch or master, some submodules are in detached HEAD mode (checked out at last stable release)
    ... fix ...
    $ git commit -a -m "fix"
    $ git push # to GitHub
    $ git add <submodule> # back in superproject
    $ git commit -m "Fixed submodule."
    $ git push origin master # to GitHub

## Update submodule (watch original repos on GitHub)
    $ git fetch upstream # fetch changes from original to our fork
    $ git merge upstream/master # merge changes in
    $ git push origin master # to GitHub
    $ git add <submodule> # back in superproject
    $ git commit -m "Updated submodule."
    $ git push origin master # to GitHub

    $ git submodule update --init # usually you just want to register all submodules found in .gitmodules into .git/config and clone repos (and automatically check out in superproject specified commits) in this one step
    $ git submodule update # somebody changed something within a submodule and you want the update (shows up modified:   3rdPartyUtils/MTStatusBarOverlay (new commits))

# Git Subtree (ARCHIVE)
[Background on subtrees](http://log.pardus.de/2012/08/modular-git-with-git-subtree.html)

## Add subtree
    $ git subtree add --prefix=3rdParty/JSONKit --squash https://github.com/johnezang/JSONKit.git stable

## Update subtree
    $ git subtree pull --prefix=3rdParty/JSONKit --squash https://github.com/johnezang/JSONKit.git <some-newer-version>

## Update subtree up to a specific commit
    $ git subtree merge --prefix=Momo/3rdParty/GPUImage <commit> --squash

I think git subtree merge doesn't automatically pull the latest, so I did a git subtree pull and cancelled the following merge first.
