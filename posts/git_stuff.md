# More "Complicated" Git Stuff for Reference

## Important refresher background information

- `HEAD` basically means the latest commit in the repository
    - Checking out to different commits will change the `HEAD` which causes that *detached `HEAD` state*
    - to get back to the original `HEAD`, do `git checkout main`
- You can do `HEAD~#` where `#` is an actual number to specified how many commmits before the current `HEAD`
    - so `HEAD~1` would mean the previous commit
---

## What's the difference between `git checkout` VS `git revert` VS `git reset`?

All of them can be use to **rollback to a previous commit**, the difference is how much "commitment" in rolling back.

### 1) `git checkout`

This command is use for:
- checking out to another branch
- checking out to a commit (using commit hash from `git log`)

It changes the working area, but still keeps the staged changes.Here's an example:

```bash

# Currently there are two files in the directory
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ ls
file1  file2

# Created third file and added it to the staging area
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ touch file3

codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git add .

codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git status
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   file3

# Now we will checkout to a different commit and see
# if the staged changes will still be in effect
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git log
commit ff755636614ca0816ec2095f4d5d616e6ba851d5 (HEAD -> main)
Author: brainuser5705 <codeuser5705@outlook.com>
Date:   Tue Jun 7 19:24:31 2022 -0400

    added file2

commit 0344762c466848912b4fc7f63555b1eeb53943c4
Author: brainuser5705 <codeuser5705@outlook.com>
Date:   Tue Jun 7 19:24:18 2022 -0400

    added file1

codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git checkout 0344762c466848912b4fc7f63555b1eeb53943c4
Note: switching to '0344762c466848912b4fc7f63555b1eeb53943c4'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 0344762 added file1
A       file3

# Yep, it's still in git status
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test ((0344762...))
$ git status
HEAD detached at 0344762
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   file3

# Now going back to the original HEAD
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test ((0344762...))
$ git checkout main
Previous HEAD position was 0344762 added file1
Switched to branch 'main'
A       file3

codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git status
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   file3
```

### 2) `git revert`

This command will:
- create a commit to revert a specified commit (only one)

```bash
# We start off with three files
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ ls
file1  file2  file3

# Each one was commited separately
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git log
commit b5adbfed38e905173f17e704b02f181e0a152a6d (HEAD -> main)
Author: brainuser5705 <codeuser5705@outlook.com>
Date:   Tue Jun 7 19:33:08 2022 -0400

    file3

commit 3058fcd9e42c24e3bedb717d31c120cd5de73619
Author: brainuser5705 <codeuser5705@outlook.com>
Date:   Tue Jun 7 19:32:57 2022 -0400

    file2

commit 4b8883f2343b566d8efe802b4d687e402727b443
Author: brainuser5705 <codeuser5705@outlook.com>
Date:   Tue Jun 7 19:32:44 2022 -0400

    file1

# We revert the second commit which was the one that
# added file2
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git revert 3058fcd
[main b53b4d1] Revert "file2"
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 file2

# If we do git log, we see that it adds a commit
# to revert the second commit
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git log
commit b53b4d1a3a20cd6acd362eb74cb39e21d1782d4e (HEAD -> main)
Author: brainuser5705 <codeuser5705@outlook.com>
Date:   Tue Jun 7 19:33:34 2022 -0400

    Revert "file2"

    This reverts commit 3058fcd9e42c24e3bedb717d31c120cd5de73619.

commit b5adbfed38e905173f17e704b02f181e0a152a6d
Author: brainuser5705 <codeuser5705@outlook.com>
Date:   Tue Jun 7 19:33:08 2022 -0400

    file3

commit 3058fcd9e42c24e3bedb717d31c120cd5de73619
Author: brainuser5705 <codeuser5705@outlook.com>
Date:   Tue Jun 7 19:32:57 2022 -0400

    file2

commit 4b8883f2343b566d8efe802b4d687e402727b443
Author: brainuser5705 <codeuser5705@outlook.com>
Date:   Tue Jun 7 19:32:44 2022 -0400

    file1

# Now we only have file1 and file3
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ ls
file1  file3
```

### 3) `git reset`

The *default* use of the command (with no extra flags) will:
- reset back to a specified commit and make it `HEAD`
- change the git history by removing any commit after the specified commit 

#### `git reset --mixed` (default)

`--mixed` tells it to:
- unstage changes, moving them back to working area

```bash
# We start with three files
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ ls
file1  file2  file3

# each one was commited separately
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git log
commit 07c367788f15ce488da447e11716e5b8c97d73cf (HEAD -> main)
Author: brainuser5705 <codeuser5705@outlook.com>
Date:   Tue Jun 7 20:27:35 2022 -0400

    3

commit 8199a79db1f9d7ee4cc7c076ef5df236f5bc93a8
Author: brainuser5705 <codeuser5705@outlook.com>
Date:   Tue Jun 7 20:27:29 2022 -0400

    2

commit bd1b22ec708c4313af6cf252610fa5c875c06137
Author: brainuser5705 <codeuser5705@outlook.com>
Date:   Tue Jun 7 20:08:41 2022 -0400

    1

# We create and stage a fourth file
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ touch file4

codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git add file4

codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git status
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   file4

# do a mix reset
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git reset bd1b22e

# all the changes are unstaged
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git status
On branch main
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        file2
        file3
        file4

nothing added to commit but untracked files present (use "git add" to track)
```

#### `git reset --soft`

`--soft` just reset back to the specified commit

```bash
# We start with three files
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ ls
file1  file2  file3

# Each one was commited separately
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git log
commit afc65d880b760b10d4474ac1cf747c84974482ba (HEAD -> main)
Author: brainuser5705 <codeuser5705@outlook.com>
Date:   Tue Jun 7 20:17:37 2022 -0400

    3

commit 4673c5a386e9f86d4e5b13993cd1331d960c6dcc
Author: brainuser5705 <codeuser5705@outlook.com>
Date:   Tue Jun 7 20:17:30 2022 -0400

    2

commit bd1b22ec708c4313af6cf252610fa5c875c06137
Author: brainuser5705 <codeuser5705@outlook.com>
Date:   Tue Jun 7 20:08:41 2022 -0400

    1

# We create a fourth file and stage it
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ touch file4

codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git add file4

codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git status
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   file4

# Now we reset back to the first commit
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git reset --soft bd1b22e

codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git log
commit bd1b22ec708c4313af6cf252610fa5c875c06137 (HEAD -> main)
Author: brainuser5705 <codeuser5705@outlook.com>
Date:   Tue Jun 7 20:08:41 2022 -0400

    1

# The working area remains the same
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ ls
file1  file2  file3  file4

# the first commit is still the same
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git show bd1b22e
commit bd1b22ec708c4313af6cf252610fa5c875c06137 (HEAD -> main)
Author: brainuser5705 <codeuser5705@outlook.com>
Date:   Tue Jun 7 20:08:41 2022 -0400

    1

diff --git a/file1 b/file1
new file mode 100644
index 0000000..e69de29

# The staging area changes with the fourth
# along with the other files added after the first commit
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git status
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   file2
        new file:   file3
        new file:   file4
```

> So really the main difference between `--mixed` and `--soft` is that soft keeps the changes in the staging area and mixed does not.

#### `git reset --hard`

`--hard` tells it to:
- clear staging area and deletes any changes
- reset the working area to specified commit

```bash
# starting with three files
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ ls
file1  file2  file3

# each file was commited separately
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git log
commit d62942dbc061ce1e84567633e93ef248d51c62ff (HEAD -> main)
Author: brainuser5705 <codeuser5705@outlook.com>
Date:   Tue Jun 7 20:08:54 2022 -0400

    3

commit e88c0af1ec31bde8379b004bc594b941ba4d1ab5
Author: brainuser5705 <codeuser5705@outlook.com>
Date:   Tue Jun 7 20:08:48 2022 -0400

    2

commit bd1b22ec708c4313af6cf252610fa5c875c06137
Author: brainuser5705 <codeuser5705@outlook.com>
Date:   Tue Jun 7 20:08:41 2022 -0400

    1

# Create and stage the fourth file
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ touch file4

codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git add file4

codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git status
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   file4

# Do a hard reset to the first commit
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git reset --hard bd1b22e
HEAD is now at bd1b22e 1

# git history is changed
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git log
commit bd1b22ec708c4313af6cf252610fa5c875c06137 (HEAD -> main)
Author: brainuser5705 <codeuser5705@outlook.com>
Date:   Tue Jun 7 20:08:41 2022 -0400

    1

# working area is also changed
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ ls
file1

# all other files are gone!
codeu@DESKTOP-UVAMUBG MINGW64 ~/Desktop/test (main)
$ git status
On branch main
nothing to commit, working tree clean
```



*References*
- https://stackoverflow.com/questions/8358035/whats-the-difference-between-git-revert-checkout-and-reset
- https://www.geeksforgeeks.org/git-difference-between-git-revert-checkout-and-reset/
- https://www.initialcommit.com/blog/git-reset#what-is-git-reset