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
