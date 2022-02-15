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
