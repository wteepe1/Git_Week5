# Getting started with git

## Updating your version of git on smic

To run a more recent version of git (2.18.0) on smic, please run the following lines in your Terminal after logging in to OnDemand:

```
cd ~
echo "source /usr/local/packages/Modules/3.2.10/init/bash" >> .bashrc
echo "export MODULEPATH=/usr/local/packages/Modules/3.2.10/modulefiles/admin:/usr/local/packages/Modules/3.2.10/modulefiles/apps" >> .bashrc
echo "export PATH=/usr/local/packages/Modules/3.2.10/bin:$PATH" >> .bashrc
echo "source ~/.modules" >> .bashrc
echo "module load git" >> .modules
```

After running these commands, please log out of OnDemand, and then log back in. Once you're logged back in, open Terminal and run

`git --version`

The version number should now be 2.18.0.

## Personal Access Tokens

Learn how to [create a personal access token for GitHub](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)

## Global Configuration
  - When you perform operations with git, it will keep track of who you are. It does this so that things stay organized in large projects.
  - To provide git with your username, use `git config --global user.name "<YOUR_NAME>"`
    - In general, whenever I write \<SOMETHING\> I intend for you to substitute that text
  - To provide git with your email address, use `git config --global user.email <YOUR_EMAIL_ADDRESS>`

## Creating a repository
  - `mkdir myProject` - Creates the directory for a project
  - `cd myProject` - Change into project directory
  - `echo "<PROJECT_NAME>" >> outline.txt` - Add an outline file to your project containing a name for your project
  - `ls -a` (or `ll` on Ubuntu) - Take a look at what's initially in the project directory
  - `git init` - Initializes the repository
  - Using `ls -a`, take another look at the project directory (now also a repository)
    - What's different?
     	- added a .git folder
     	- all information on what you have done is in .git
     	- ls -a shows .git
     	- ls just shows outline.txt
    - Explore a bit with `cd` and `cat`

## 'Git'ting started
  - Git projects operate on a branching model. Branches keep track of different versions of a project, without having to store completely independent copies.
  - The most basic way to work with git is to use only a single branch, usually called "Main or Master". As changes are made, they are all committed (sort of like git's version of saving) to the Main branch.
  - `git` is the actual program that handles everything behind the scenes for us
    - To accomplish different things, we will pass options to git
  - `git status` - How do things look?
    - Note the branch at the top
    - What is git keeping track of?
  - `git log` - See what commits have been made to this project
    - Since we haven't yet done anything with our repository, there is nothing yet to show in the log. First, we need to commit something.

## Git Stages
  - In order to keep track of the files associated with your project, you need to tell git to include them. Files that you associated with your repository are 'tracked'. In order to do this, use `git add`.
    - `git add outline.txt`
    - One advantage of this setup is that you can have files in your folder that are not tracked by git, if you're not ready to include them in your project.
  - If you change a tracked file, git will automatically note the change. Try editing the outline file and then run `git status`. Note that staging the _addition_ of a new file and _changes_ to the file itself are different.
    - To see what changes have been made, use `git diff`.
  - Now to actually include our outline file (and any edits you've made to it) in the repository, we need to commit them. To commit everything in the staging area, we use `git commit -m <COMMIT_MESSAGE>`. The commit message should be short, but informative.
    - What information is displayed after you commit?
  - Now run `git log` again. What do you see?

## Rolling back changes
  - Now let's make another commit, where we simulate adding something very important to our repository.
    - `echo "SUPER IMPORTANT TEXT" > important.txt`
    - `git status`
    - `git add important.txt`
    - `git status`
    - `git commit -m "Adding important.txt"`
    - `git log`
      - `git log --pretty=oneline`
  - Now let's say that we make a silly mistake and delete our important file.
    - `rm important.txt`
    - `git add important.txt` - (We use add even though we've deleted the file. We're telling git to add the change.)
    - `git commit -m "Accidentally deleting important thing"`
  - Oh no! Now we've remembered that the thing was really important. Can we retrieve it?
    - First, we take a look at our log - `git log --pretty=oneline --graph`
      - Compare this output to workflow schematic above
    - Find the last commit _before_ the important thing was deleted and copy the beginning of the unique hash.
    - Use the git checkout command to go back in time and recover the 'deleted' file. `git checkout <PREVIOUS_COMMIT_HASH> important.txt`.
    - Now take a look at the status of our files:
      - Run `ls` in the working directory.
      - `git status`
    - Now we add our file back to the repository - `git commit -m "Adding important stuff back again"`
  - There's one other option for fixing our mistake, if we hadn't already used checkout. Instead, we could directly undo the entire commit where we mistakenly deleted the important file. Note that this is different than using checkout, because it automatically creates a new commit that undoes the previous one and it applies to the entire commit.
    - `git revert <BAD_COMMIT_HASH>`

## Branching
  - So far, we have only been discussing git commands in the context of a _single version_ of a repository. However, one of the most powerful features of git is its ability to handle branches, or different versions of a project.
  - By creating different branches, you can try changes to a project that don't affect the primary/main version. If the changes look good, you can then merge them back into the main version (Main branch).
  - `git branch featureOne` will create a new branch for your project.
  - `git branch` - Running this command without a name for a new branch just gives you a list of the branches that exist for your project. Note that you have a new branch, but you are still focused on Main.
  - `git status` - Looking at your status will also show the current branch that you're on.
  - To switch branches, and make changes that are specific to your feature branch, use `git checkout featureOne`.
  - Now verify that you've actually switched using either `git status` or `git branch`.
  - Now let's make a change on our new branch. Use `touch featOneFile.txt` to create a new file that's related to feature one.
  - Now we'll add it to the new branch by running `git add featOneFile.txt` and `git commit -m "Adding new file for feature one."`
  - Now, take a look at the project using `git status` and `git log`.
    - What do you notice about the commits we've made?
  - Let's say our new file has successfully implemented our new feature and we want to merge it back into our Main branch. First, we need to switch back to our Main banch with `git checkout main`.
  - Now, we can pull in the changes from our feature branch with `git merge feature One`.
  - Since main now has all the changes we made in feature One, we can delete that feature branch - `git branch -d featureOne`
  - Note that when you look at `git log --graph` now, it's as if the feature branch never existed. The changes made there are now seamlessly integrated into the main branch.

## Collaborative coding
  - We are going to use git to work together to fix and expand a catalog of animals found in Louisiana.
  - I've started this catalog and posted it here: https://github.com/FoundCompBio-Spr22/LAFauna
  - However, I've made some mistakes and a lot more work needs to be done. To collaborate on this, let's all first make our own copy of the repository so that we can suggest edits and additions. To do this, click the `Fork` button in the upper right of the repository. Remember that forking will make a copy of the entire repository and associate it with your account.
  - Now we have our own copy, but it would be more convenient to work with these files locally (in our HPC accounts). So, next we need to __clone__ the repository to our accounts.
	  - Open up Terminal
	  - Use `cd` to change directories to the location where you want to store this repository (e.g., `~/`)
	  - Now go back to your fork of the repository on GitHub and click the green `Clone or download` button in the upper right.
		- Click the blue `Use HTTPS` link in the upper right of the dropdown menu
		- Copy the URL that GitHub provides, which starts https://github.com/...
		- Now navigate to the place on your computer where you want to store your repository and run `git clone <GITHUB_URL>`
  - You should now have a complete version of the "LAFauna" repository on your home directory. Use `cd` to change directories into your repository folder.
  - Go ahead and divide up into groups of two or three. Each group will then be assigned one of the groups of animals. You will need to do a few things for your group:
	 - First, make a new branch with a name that indicates which group you're working on and checkout that branch.
	 - Now, in the folder for your group, look at the list of scientific names of the 6 species. With some internet research, figure out which of these species has been incorrectly included because it does not actually occur in Louisiana and remove it from the list.
	 - Also, create a corresponding file for the common names of the remaining species, with the same file naming scheme (e.g., LAFish_common.txt). Look up the common names for the remaining 5 species and add them to this file.
	 - Make a commit to your branch after you add or delete each species name.
  - Because you cloned this repository directly from GitHub, git automatically remembers this original repostitory. Try running `git remote -v`. Note the name git uses for this remote repository - `origin`.
  - Now we're going to send our updates back to origin. In git, this is called `pushing`. A couple of things to note: you can push any branch individually, and the changes are sent to _your_ fork of the repo.
	 - `git push origin <YOUR_BRANCH>`
	 - GitHub may require that you manually enter your username and personal access token (see above) before it accepts the push. If you get tired of doing this, you can set up automatic authentication using ssh.
  - Go back to your fork on GitHub and check that the updates have been pushed. Note that you can look at different branches by selecting them from the dropdown menu in the upper left.
  - If all looks good, create a pull request to send your changes back to the main repository on the class page (like you did for last week's assignment).
  - Let me know when you've done this, and I'll merge all the pull requests into the class repo.
  - Once everyone's updates have been merged, you might want to update your fork with all of the changes. The best way to do this is with a "pull". But first, you'll need to add the class repo as a remote:
	 - `git remote add class https://github.com/FoundCompBio-Spr22/LAFauna.git`
	 - I've used the name __class__ to indicate this new remote, but you can name it whatever you want.
	 - `git remote -v`
	 - `git pull class master` (Pulling changes from the master branch of class)
  - Now you've synced your local version of the fork using the class repo, but your fork on GitHub still needs to be updated.
  - Paying attention to which branch you're on locally, run `git push origin <YOUR_BRANCH>`.
  - Now go back to your fork on GitHub and verify that the updates are there.

Who learned something interesting about the group they were assigned?

## Git Resources

- [Pro Git Book](https://git-scm.com/book/en/v2)
- [Git It Tutorial Software](https://github.com/jlord/git-it-electron/releases)
- [Software Carpentry Git Tutorial](http://swcarpentry.github.io/git-novice/)
