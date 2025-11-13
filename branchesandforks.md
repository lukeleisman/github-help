This file goes through helpful things on github branches and forks.

## Forking
- Forking is useful when you want to work on a project but don't have write access. 
- A fork is essentially a clone of someone elses repository, associated with your user.
- The idea is that you will create a fork which creates your own local repository where you make changes. 
Then you take out a pull request, which requests to merge your forked branch back in with the main.
Here's a somewhat helpful page:
https://stackoverflow.com/questions/3611256/forking-vs-branching-in-github

## Branching
- A branch allows you to work on some part of the code without modifying the main code until your fix is ready to go.
- To create a branch:
  ```
  git checkout -b newbranchname
  git push origin branchname
  ```
Note that checkout -b both creates the branch and switches you to be modifying that branch. To switch between branches use checkout without the -b.
https://stackoverflow.com/questions/40307960/how-to-create-a-branch-in-github
- to view all branches:
  ``` git branch -a```

Suppose we've just done some work but then realize we aren't on the branch we want to be on (we almost committed the work to the wrong place!).
If you haven't committed yet, you can stash your work, move to the branch you want to be on (or create a new branch), and then move the work over to the branch. 
```
git stash
git checkout -b newbranchname
git stash pop
```

Generally speaking, best practice is to always work in branches, as we describe on the <a href="https://github.com/lukeleisman/github-help/blob/main/commandsandworkflow.md"> Commands and Workflow </a> page.
In brief:
- Use these commands:
  ```
git checkout -b mybugbranch
git status
git add --all
git status
git commit -m "added a change to fix mybug"
git push origin mybugbranch
```
Repeat add, commit, and push commands until the project is done/ issue is fixed.
Then on the github repo take out a pull request (a proposal to merge with the main branch), merge the branch in, and then delete the working branch.

Branching can get fairly complicated, here's a rundown on how to handle some of these situations:
https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging
