# Getting Started with GitHub and Git:

GitHub is a web-based repository hosting service, that is, a place for storing code where you can track changes, and where others can also make changes. GitHub is a repository for Git, which is a  "distributed version control system," that is, software for tracking changes in your code. It is designed for coordinating work among programmers, but it can be used to track changes in any set of files.

Unfortunately there's a bit of a learning curve with Github and Git, but once you get the hang of it, it is a powerful too. The main documentation for GitHub is [here](https://docs.github.com/en). These instructions try to synthesize the important things I needed to actually get Git and GitHub up, running, and working together. 

Note that there are many useful tutorials online, just none with exactly the info I needed. So if things aren't making sense on this site, here are some other pages to try:
[Mark's Blog](http://longair.net/blog/2009/04/16/git-fetch-and-merge/)

## 0. A brief introduction to how git works
![Git diagram from Oliver Steel's Blog](https://images.osteele.com/2008/git-transport.png)

One of the tricky things about Git is keeping track of all the different places information gets stored. Above is a couple of helpful diagrams for keeping things straight. There are three main places/containers that your work is stored:
- Your local workspace is the folder where you have your code on your computer, and where it all gets stored.
- Your local repository is the container where all your versions get stored by git.
- The remote repository is a place in the cloud/ online (typically on Github) where all the code that everyone can access is stored. This is also useful for working between devices, and if your local computer fails.
There also is an "index" container that buffers between your local repository and your workspace, another place where changes are stored.
- The index is a container designed to be a buffer between your local repository and your workspace. Changes from your workspace will get "staged" to be added to your local repository using the `add` command discussed below. You can bypass this container using `commit -a` but it can be quite helpful, as we'll see.


## 1. Make sure you have git installed locally. 
[Here is a link](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) with installation instructions, and how to check if it is already installed.
For me, on a mac I typed:

`git –version`

into a terminal, and it was already pre-installed.

## 2. Set up your user and passkey
If you haven’t done it before, you need to **tell git who you are**:

`git config --global user.email "myemail@gmail.com"`

`git config --global user.name "Luke L."`

Note that git no longer supports password authentication. So you need to **set up a key**. 
### Setting up an ssh key:
There are more detailed instructions for doing this on [this page.](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) 
In brief: 
1. first set up a key on your computer by typing (replace myemail@gmail.com with your email):
```
ssh-keygen -t ed25519 -C "myemail@gmail.com"
Enter file in which to save the key (/Users/lleisman/.ssh/id_ed25519):
```
  Hit enter to store the key in the default location. If you do enter a new location be sure to include the FULL path, e.g. /Users/lleisman/.ssh/mynewfilename.

```
Enter passphrase (empty for no passphrase): 
Enter same passphrase again:
```
  I elected to not bother with a passphrase, but in theory this protects the key if someone steals/hacks your computer. If you do include a passphrase to protect the key, then you can store it in your mac chain, following the instructions in [the detailed instructions](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent).
```
Your identification has been saved in /Users/lleisman/.ssh/id_ed25519
Your public key has been saved in /Users/lleisman/.ssh/id_ed25519.pub
```
2. Next **start the ssh agent** in the background by typing the following command:
```
eval "$(ssh-agent -s)"
```

3. **Update the ssh config file.**
To do this check and see if your ssh config file exists: 
```
open ~/.ssh/config
```

If it doesn’t exist, create a blank file with the touch command (if this command doesn’t work it’s likely because the .ssh directory doesn’t exist because you didn’t use the default location).

```
touch ~/.ssh/config
```

   Now, open the file 
```open ~/.ssh/config```
  and add the following:
<pre>
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
</pre>
  - NOTE: don’t include the UseKeychain line if you didn’t include a passphrase, and if you chose a different file name, put that filename in the IdentifyFile line.

4. Next, **add your SSH private key to the ssh-agent** and, if you had a passphrase, store your passphrase in the keychain.
- With passphrase:
`ssh-add --apple-use-keychain ~/.ssh/id_ed25519`
- Without passphrase:
`ssh-add ~/.ssh/id_ed25519`
5. Finally, **add your key to your GitHub account** following [these instructions](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) 
Basically copy the key using, e.g.:
`pbcopy < ~/.ssh/id_ed25519.pub`
And then on the website navigate to Settings > SSH and GPG keys > New SSH Key, and past in the key, adding a description of the device the key is stored on.

## 3. Link your local directory with the github repository

Navigate to the directory you want to story your files in, and then download a copy of any existing code online using the clone command:
```
git clone git@github.com:INMAS-Math/Workshop-01.git
```
Now initialize the local directory:  

```
git init
```

If you already have a local directory with some code, you can initialize it, and then link it to the remote repository. You can find the address of the repository you are interested in by going to the relevant github page, and clicking the down arrow by the big Code button. Since you have an ssh key set up, your remote url now needs to be the ssh url. The command we use to do this is [git remote](https://articles.assembla.com/en/articles/1136998-how-to-add-a-new-remote-to-your-git-repo) with relevant parameters (I also first did this following a summary of commands posted [here](https://gist.github.com/alexpchin/102854243cd066f8b88e)). For the INMAS workshop #1 repository I used the following (INMASWk1 is just the name I'm telling Git to associate with that link; note that typically git documentation calls the remote repository `origin` since it's the origin of anything we download).

```
git remote add INMASWk1 git@github.com:INMAS-Math/Workshop-01.git
```

Note that if you need to change the url in the future, you can use the set-url option:

`git remote set-url INMASWk1 git@github.com:INMAS-Math/Workshop-01.git`

Check that it worked by typing `git remote -v` which gives verbose output of the current links:
```
git remote -v                                                        
INMASWk1	git@github.com:INMAS-Math/Workshop-01.git (fetch)
INMASWk1	git@github.com:INMAS-Math/Workshop-01.git (push)
```

Note that there are other alternative approaches to `git clone` using, e.g. `git pull` or `git fetch`. Read more [here](https://stackoverflow.com/questions/3620633/what-is-the-difference-between-pull-and-clone-in-git).


## 4. Editing files and uploading your changes.

Now you're ready to make changes to files. After you've made an edit, you [need to add](https://softwareengineering.stackexchange.com/questions/283182/why-do-i-have-to-run-git-add-after-making-changes) or "stage" your edited files to your local git repository so that it knows to track your changes:

```
git add .
```

This adds all the files. To only add a specific file, state it by name:

```
git add Notebook_06.ipynb
```

You then commit your changes. Each commit creates a local check point of file changes; you can think of a a commit as like a save or snapshot of everything that is currently staged by the git add command. "Committed snapshots can be thought of as “safe” versions of a project - Git will never change them unless you explicitly ask it to." (Quoted from [this helpful page](https://www.atlassian.com/git/tutorials/saving-changes/git-commit)) 

```
git commit -m "typo fix"
```

The `-m` option lets you add a message, in this case that a typo was fixed, describing whatever changed in this update. You can read more about commits [here](https://stackoverflow.com/questions/43970559/what-is-exactly-meaning-of-commit-command-in-git)

Finally, you can push your changes to the remote repository (main specifies the main branch):

```
git push INMASWk1 main
```

This pushes your main branch to INMASWk1, which was what we named the remote repo above. Generally if this is stored as the default, you can also just run:

```
git push
```

## 5. Downloading changes to the code from others

There are a couple of ways to update your code with changes from the remote server. One way is to use a pull command:
```
git pull INMASWk1 main
```

A better way, advocated for [here](http://longair.net/blog/2009/04/16/git-fetch-and-merge/), among other places, is to use git fetch and git merge:
```
git fetch origin master
git merge origin/master
```
[Here](https://stackoverflow.com/questions/292357/what-is-the-difference-between-git-pull-and-git-fetch) is a really helpful discussion of the difference.

## 6. On Merging: what happens when both you and others have done some work?

[One possibly helpful page](https://happygitwithr.com/pull-tricky)

## 7. On fixing problems
If you need to overwrite changes you've been making locally, you can follow [this procedure](https://stackoverflow.com/questions/1125968/how-do-i-force-git-pull-to-overwrite-local-files)

As long as things are backed up in the remote repository, you can just completely delete the .git folder that contains your local repository (all your local backups). For example, you might do this if you ran a `git init` command in the wrong folder and want to undo the init. You can do this using `rm -rf .git` as described [here](https://stackoverflow.com/questions/3212459/is-there-a-command-to-undo-git-init)



