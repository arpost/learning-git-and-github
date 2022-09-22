# Learning Git and GitHub

All of the instructions below assume you have administrator access to your computer.

## Installing Git on MacOS
On Mavericks (10.9) or newer, try to run git from the Terminal:

```$ git --version```

If you don’t have it installed already, Terminal will prompt you to install it.

## Installing Git on Windows
Go to https://git-scm.com/download/win and the download will start automatically. For more information, go to https://gitforwindows.org. 

## Installing Windows Terminal
Go to the Microsoft Store, search for Windows Terminal, and install it. It is free of charge.

## Installing OpenSSH Client on Windows 10
In an administrator account, go to Settings, and go to Apps & Features. Click on Optional Features. If OpenSSH Client is not listed under Installed Features, click Add a feature and install it.

## Activate SSH Agent on Windows 10
Open Services from the Start Menu as an administrator. Scroll down to OpenSSH Authentication Agent > right click > Properties. Change the Startup type from Disabled to Automatic. Reboot. Open Terminal, and type `where ssh` in Cmd or `get-command ssh` in Powershell to get the path to your ssh command. Then open Edit environment variables for your account and create a `GIT_SSH` variable set to the path to your ssh command. Reboot again.

## Create a ssh key and add it to GitHub (all platforms)
