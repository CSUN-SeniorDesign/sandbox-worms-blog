How to use Git - by Mark Siegmund
(overview)
Some git commands…
(global commands are only needed for first time configuration)
Git config --global user.name “Mark”
Git config --global user.email “siegmund@csun.edu”

Get status - shows differences between desktop and repository
Git add All, index.html, .=all files ,  A- all files
Add -A
Add -
Git checkout -b <branchname>
Git pull origin master  (basically the same as clone)
Git add All, index.html 

Git branch (with no branch name list branches checked out-green color is active branch)
Git branch -a   ---- shows branches on local and in repository
Git branch nameOfNewBranch - creates a new branch
Git branch --merge  ----shows

Git merge branchToMerge    ----  merge with current branch (Master usually)

Git checkout BranchName - switches to a new branch
Or git checkout -b BranchName - this does both create branch and switches to new branch

Git commit -m “message.” - makes changes for local file(s) ready for repository 
Git push origin <branchname> - pushes changes from local files to repository files
Git pull  - pulls down from repository
Git push - actually puts files into repository
-----------------------------------
In a typical session the order will be
Git checkout master
Git pull origin master  (basically the same as clone) giit changes to master (if they are different)
Git branch SomeBranch
Git checkout SomeBranch

(Create files or modify files)

Git status (see changes)
Git add . (for all files that are added)
Git commit -m “message.” (always add a message)
Git push




-------------------
CECS@CECSCEC-69AD6EE MINGW64 ~/repo
$ git pull origin master
fatal: not a git repository (or any of the parent directories): .git

CECS@CECSCEC-69AD6EE MINGW64 ~/repo
$ git clone https://github.com/CSUN-SeniorDesign/sandbox-worms-blog.git
Cloning into 'sandbox-worms-blog'...
remote: Counting objects: 247, done.
remote: Compressing objects: 100% (42/42), done.
remote: Total 247 (delta 38), reused 67 (delta 27), pack-reused 161
Receiving objects: 100% (247/247), 117.19 KiB | 3.35 MiB/s, done.
Resolving deltas: 100% (100/100), done.

CECS@CECSCEC-69AD6EE MINGW64 ~/repo
$ git status
fatal: not a git repository (or any of the parent directories): .git

CECS@CECSCEC-69AD6EE MINGW64 ~/repo
$ ls
sandbox-worms-blog/

CECS@CECSCEC-69AD6EE MINGW64 ~/repo
$ cd sandbox-worms-blog

CECS@CECSCEC-69AD6EE MINGW64 ~/repo/sandbox-worms-blog (master)
$ git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean

CECS@CECSCEC-69AD6EE MINGW64 ~/repo/sandbox-worms-blog (master)
$ git checkout -b markblog2
Switched to a new branch 'markblog2'

CECS@CECSCEC-69AD6EE MINGW64 ~/repo/sandbox-worms-blog (markblog2)
$ git pull origin master
From https://github.com/CSUN-SeniorDesign/sandbox-worms-blog
 * branch            master     -> FETCH_HEAD
Already up to date.

CECS@CECSCEC-69AD6EE MINGW64 ~/repo/sandbox-worms-blog (markblog2)
$ hugo new article/marksblogweek2.md
C:\Users\CECS\repo\sandbox-worms-blog\content\article\marksblogweek2.md created

CECS@CECSCEC-69AD6EE MINGW64 ~/repo/sandbox-worms-blog (markblog2)
$ git add .
warning: LF will be replaced by CRLF in content/article/marksblogweek2.md.
The file will have its original line endings in your working directory.
warning: LF will be replaced by CRLF in content/article/marksblogweek2backup.md.
The file will have its original line endings in your working directory.

CECS@CECSCEC-69AD6EE MINGW64 ~/repo/sandbox-worms-blog (markblog2)
$ git commit -m "my post for the week"
[markblog2 b7840b1] my post for the week
 2 files changed, 42 insertions(+)
 create mode 100644 content/article/marksblogweek2.md
 create mode 100644 content/article/marksblogweek2backup.md

CECS@CECSCEC-69AD6EE MINGW64 ~/repo/sandbox-worms-blog (markblog2)
$ git pust origin markblog2
git: 'pust' is not a git command. See 'git --help'.

The most similar command is
        push

CECS@CECSCEC-69AD6EE MINGW64 ~/repo/sandbox-worms-blog (markblog2)
$ git push origin markblog2
Enumerating objects: 8, done.
Counting objects: 100% (8/8), done.
Delta compression using up to 8 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (5/5), 1.50 KiB | 1.50 MiB/s, done.
Total 5 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To https://github.com/CSUN-SeniorDesign/sandbox-worms-blog.git
 * [new branch]      markblog2 -> markblog2

CECS@CECSCEC-69AD6EE MINGW64 ~/repo/sandbox-worms-blog (markblog2)
$

