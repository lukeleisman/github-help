# Getting Started with GITHUB:

GitHub is a web-based repository hosting service, that is, a place for storing code where you can track changes, and where others can also make changes. GitHub is a repository for Git, which is a  "distributed version control system," that is, software for tracking changes in your code. It is designed for coordinating work among programmers, but it can be used to track changes in any set of files. 

The main documentation for GitHub is [here](https://docs.github.com/en). These instructions try to synthesize the important things I needed to actually get Git and GitHub up, running, and working together. 

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
```eval "$(ssh-agent -s)"```

3. **Update the ssh config file.**
    To do this check and see if your ssh config file exists: 
```open ~/.ssh/config```

    If it doesn’t exist, create a blank file with the touch command (if this command doesn’t work it’s likely because the .ssh directory doesn’t exist because you didn’t use the default location).
```touch ~/.ssh/config```

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
Alternatively, initialize the local directory ([here's](https://stackoverflow.com/questions/3620633/what-is-the-difference-between-pull-and-clone-in-git) a helpful page on different approaches): 

`git init`

Now link this to a remote repository. You can find the address of the repository you are interested in by going to the relevant github page, and clicking the down arrow by the big Code button. Since you have an ssh key set up, your remote url now needs to be the ssh url. The command we use to do this is [git remote](https://articles.assembla.com/en/articles/1136998-how-to-add-a-new-remote-to-your-git-repo) with relevant parameters. For the INMAS workshop #1 repository I used the following (INMASWk1 is just the name I'm telling Git to associate with that link).

`git remote add INMASWk1 git@github.com:INMAS-Math/Workshop-01.git`

Note that if you need to change the url in the future, you can use the set-url option:

`git remote set-url INMASWk1 git@github.com:INMAS-Math/Workshop-01.git`

Check that it worked by typing `git remote -v` which gives verbose output of the current links:
```
git remote -v                                                        
INMASWk1	git@github.com:INMAS-Math/Workshop-01.git (fetch)
INMASWk1	git@github.com:INMAS-Math/Workshop-01.git (push)
```

## 4. Your first download and upload!

Instructions coming soon.
