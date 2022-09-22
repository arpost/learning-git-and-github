# Learning Git and GitHub

All of the instructions below assume you have administrator access to your computer.

## Installing Git on MacOS
On Mavericks (10.9) or newer, try to run git from the Terminal:

```$ git --version```

If you don’t have it installed already, Terminal will prompt you to install it.

## Installing Git on Windows
Go to https://git-scm.com/download/win and the download will start automatically. For more information, go to https://gitforwindows.org. 

## Installing Windows Terminal
Go to the Microsoft Store, search for Windows Terminal, and install it. It is free of charge from Microsoft. You will probably want to set the default shell to PowerShell, which is far more capable than the regular command prompt. There is also a built in PowerShell app in Windows that you can use, but Terminal is much nicer.

## Installing OpenSSH Client on Windows 10
In an administrator account, go to Settings, and go to Apps & Features. Click on Optional Features. If OpenSSH Client is not listed under Installed Features, click Add a feature and install it. If you don't see the OpenSSH Client listed, you probably are not in an administrator account. 

## Activate SSH Agent on Windows 10
Open Services as an administrator (from any user account, search for Services in the search bar and look for Run as administrator). Scroll down to OpenSSH Authentication Agent > right click > Properties. Change the Startup type from Disabled to Automatic. Then open PowerShell as an administrator the same way, and type the following to start the agent (this will happen automatically after every reboot):

```
Start-Service ssh-agent
```

Next, in Terminal, open a PowerShell and run `get-command ssh` to get the path to your ssh command. Then in the Start menu, open Edit environment variables for your account and create a `GIT_SSH` variable set to the path to your ssh command. You will need to log out of your account for the setting to take effect.

## Create a ssh key and add it to GitHub (all platforms)
