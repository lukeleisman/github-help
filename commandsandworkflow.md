## General procedure for making updates
If you are just working away, typically add your changes and stay up to date using the following sequence:
```
git status
git add --all
git status
git commit -m "descriptive message"
git pull origin main
git push origin main
git status
```

## Good practice: working in a branch
Generally it is good procedure to make changes in a branch, named for the specific change or fix you are trying to do.
In the example here suppose I want to fix a bug called mybug and thus want to do work on a branch I'll call mybugbranch:
```
git checkout -b mybugbranch
git status
git add --all
git status
git commit -m "added a change to fix mybug"
git push origin mybugbranch
```
You can continue making commits to your branch until you are happy with it. 

Now, we want to merge back into the main branch. This is perhaps easiest to do in the github online interface.
- on the repository page for your branch there will be a link to "compare and pull request" - click it. This will open a pull request. (You can also just open a pull request under pull request; note: a pull request is just a proposal to merge different branches of code).
- write up a description of the changes being made.
- you can assign reviewers to look over the merge, and also assign people to be responsible for it
- click "Create pull request"
- Assuming there are no conflicts, you should be able to then confirm the merge, or a reviewer may need to confirm it.
- The pull request will then say it is successfully merged and closed, and that you can delete the branch.
- So delete the remote branch by clicking Delete branch.

Now we want our local configuration to match the merged remote situation. So pull down the merged main branch, and delete the extra working branch:
```
git checkout main     # switches to the main branch
git branch            # views your branches; confirms you've switched
git pull origin main  # pulls down the merged main branch
git branch -d <working branch> # deletes the working branch that is no longer needed
```

### Even better practice: checking out issues
Before undertaking a specific fix or upgrade to the code or project, it's best to submit it as an issue, and then create a branch based on that issue. This way everyone knows exactly what that branch is about and what you are working on.

To do this:
- on the github repository page, select the "Issues" tab and then click New Issue in the upper right corner.
- describe the issue that needs improving, and the click create. This will assign an issue number and create a page where the issue can be discussed in more detail.
- In the lower right on the page for the newly created issue there's an option to "create a branch" based on this issue. The branch name will start with the issue number, which is very helpful for referencing what is going on.
- When you create a branch it will then prompt you to run fetch and checkout commands to get to work on that branch. For example, I had a page missing a google drive link, and created an issue, #4, called add google drive link. Here were the commands:
```
git fetch origin
git checkout 4-add-google-drive-link
```

When you merge the branch back into the main branch with whatever fixes you've come up with it will automatically close the issue!

## Other situations that may come up
Suppose you need to undo a local commit. You can do this using a reset command:
```
git commit -m "Something terribly misguided" # (0: Your Accident)
git reset HEAD~
# === If you just want to undo the commit, stop here! ===
[ edit files as necessary ]                   
git add .                                    
git commit -c ORIG_HEAD
```                    
For more info, see this helpful page: https://stackoverflow.com/questions/927358/how-do-i-undo-the-most-recent-local-commits-in-git 
