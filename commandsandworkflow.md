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


git branch -d addraweb
