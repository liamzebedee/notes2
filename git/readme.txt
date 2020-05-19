Alice can peek at what Bob did without merging first, using the "fetch" command; this allows Alice to inspect what Bob did, using a special symbol "FETCH_HEAD", in order to determine if he has anything worth
       pulling, like this:

           alice$ git fetch /home/bob/myrepo master
           alice$ git log -p HEAD..FETCH_HEAD




Merge master into current branch.
`git merge origin/master`

Fetch and merge master.
`git pull origin master`

Show the parent of HEAD
`git show HEAD^`

Search for strings in your project
git grep "hello" v2.5

Show commits since a reference
git log v2.5..                # commits since v2.5

Show commits since a reference, to a specific file
git log v2.5.. Makefile




If you want a good GUI for a project, `gitk` is reasonable



Show a file on a particular branch
git show v2.5:Makefile


See the differences between this branch and master, ignoring package-lock.json
git diff master -- ':!package-lock.json'

See a compact list of changes, eg.
git diff approve-and-call --compact-summary -- :!package-lock.json

 example.js                    |  21 ++++-----------------
 examples/agent/index.js (new) | 280 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
 package.json                  |   7 ++++---
 src/Deposit.js                | 277 ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++-----------------------------------------------------------------------------------------------------------------------------
 src/EthereumHelpers.js        |   3 +--
 5 files changed, 441 insertions(+), 147 deletions(-)
(END)




Find out which branch you were on before
git reflog



Git index.
==========

Git's general flow:

  1. The working tree: the current state of your filesystem
  2. The index: the staging area, where content is cached for committing.
  3. The current branch.
      |
      |--- A branch.
      |--- A commit. (eg. a tag)

 
 The stash is wrapper around tagged commits. Basically any stash commit is tagged as `stash{i}`. Pop'ing from stash is equivalent to merging the commit tagged by the stash.

  

Show the diff between (a) working tree and (b) index
git diff

Show the diff between (a) the index and (b) the HEAD
git diff --cached


Show the diff between the last commit and the index (the staging area)
git diff --cached


Note: a bare `git diff` will show the differences between the working tree and the index.


last commit and the working tree



Anytime git is referring to '--cached', it means the index.

Remove a file from the index / unstage a file
git rm --cached file


Git stashes
===========

A stash is a collection of committed files, not 

git stash



An example: our working tree and index are both dirty.

(base) ➜  git git:(feat-1) ✗ git status
On branch feat-1
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	new file:   README.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	c

The index has tracked a new file, README.md. We've made some additional changes to README that are not yet staged in the index too.
There's also an untracked file, c.

(base) ➜  git git:(feat-1) ✗ git stash
Saved working directory and index state WIP on feat-1: 34c30ba Added heading

(base) ➜  git git:(feat-1) ✗ git status
On branch feat-1
Untracked files:
  (use "git add <file>..." to include in what will be committed)
	c


The `git stash` has saved the working directory and index state. Note that untracked
files are not stashed. If you want to stash them, you can use:

`git stash -u/--include-untracked`


If you want to keep the current state of the index, you use `git stash -k/--keep-index`.
This is useful for testing changes and committing them, when you're just going all-out ham.
eg.

git add -p ./test/DepositLiquidation.js # add some portion of file
git stash -k
truffle test
git commit -m "Add some feature." # the test worked, woot
git stash pop 
# Now continue until you've reduced the working tree down to commits.




Merging and conflicts
=====================

Typically we talk about merges in terms of:

  - common
  - ours: the branch that is receiving the changeset, from a source branch
  - theirs: the source branch of changes, to be merged into our target branch

$ git show :1:hello.rb > hello.common.rb
$ git show :2:hello.rb > hello.ours.rb
$ git show :3:hello.rb > hello.theirs.rb


After performing a merge, you've stuffed up the contents of one file

How to re-perform the merge on one file?

Well, if the changes are committed or staged, you can git stash, re-run git merge, then git stash pop?






Git history
===========


Show the simple one-line history
git log --oneline

253946b (master) Bump to 0.14.0-pre
39d8a8e (tag: v0.13.0-rc, origin/releases/ropsten/v0.13.0-rc) Bump to 0.13.0-rc
669d3bb Bump package and dependencies to 0.13.0-pre
f22ba26 (tag: v0.12.0-rc) Bump package and dependencies to 0.12.0-rc
5039e95 Merge pull request #16 from keep-network/sthompson22/work-against-numerical-sort
0477ee5 (origin/sthompson22/work-against-numerical-sort) Work against a numerically sorted version list
e13202e Merge pull request #15 from keep-network/fixes-shmixes
d6a532a Adjust formatting for safelySend
67756e5 sendSafely handles unexpected exceptions
2f939a7 (origin/fixes-shmixes) Introduce EthereumHelpers.sendSafely


Show history as a graph
git log --graph

