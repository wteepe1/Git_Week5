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

## Global Configuration
  - When you perform operations with git, it will keep track of who you are. It does this so that things stay organized in large projects.
  - To provide git with your username, use `git config --global user.name "<YOUR_NAME>"`
    - In general, whenever I write \<SOMETHING\> I intend for you to substitute that text
  - To provide git with your email address (optional), use `git config --global user.email <YOUR_EMAIL_ADDRESS>`


## Creating a repository
  - `mkdir myProject` - Creates the directory for a project
  - `cd myProject` - Change into project directory
  - `echo "<PROJECT_NAME>" >> outline.txt` - Add an outline file to your project containing a name for your project
  - `ls -a` (or `ll` on Ubuntu) - Take a look at what's initially in the project directory
  - `git init` - Initializes the repository
  - Using `ls -a`, take another look at the project directory (now also a repository)
    - What's different?
    - Explore a bit with `cd` and `cat`

## 'Git'ting started
  - Git projects operate on a branching model. Branches keep track of different versions of a project, without having to store completely independent copies.
  - The most basic way to work with git is to use only a single branch, usually called "Main". As changes are made, they are all committed (sort of like git's version of saving) to the Main branch.
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
