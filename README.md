# Learning Git and GitHub

These instructions will guide you through installing and configuring a source code version control system called git on your computer, setting up your access to the GitHub code repository at https://github.com, creating a project, and making some changes to it.

Version control systems maintain a history of all changes made to your software development project. The biggest benefit of using one is that you can easily back out changes to your code if you really mess things up (everyone does at some point). They also allow teams of programmers to make changes to the code at the same time and provide support for merging the changes together. Furthermore, modern version control systems can synchronize changes to your project between multiple copies stored on different servers. Git is probably the most popular version control system, and GitHub wraps git in an easy-to-use website. GitHub makes it easy to create a remotely hosted copy of a project, free of charge in many cases, and share it with others. As a result, GitHub has become the defacto standard for hosting open source software projects.

All of the instructions below assume you have administrator access to your computer. After you are done, you will be able to access git repositories on GitHub from your computer without ever needing to enter a password!

NOTE: the Windows instructions are for Windows 10. If you have Windows 11, the instructions likely still work with minor modifications, but I don't yet have access to a Windows 11 machine to verify that.

## Create a GitHub account
The first step is to create a GitHub account at https://github.com.

## Install Git on MacOS
On Mavericks (10.9) or newer, try to run git from the Terminal:

```$ git --version```

If you don’t have git already, Terminal will prompt you to install it. The rest of what you need is installed by default or will be installed along with git. You can skip down to Create a ssh key.

If you have an older version of MacOS, you will need to install Xcode from the App Store, launch Xcode, and install the command line tools when prompted.

## Install Git on Windows
Go to https://git-scm.com/download/win and the download will start automatically. For more information, go to https://gitforwindows.org. 

## Install Windows Terminal
Go to the Microsoft Store, search for Windows Terminal, and install it. It is free of charge from Microsoft. You will probably want to set Terminal's default shell to PowerShell, which is far superior to the regular command prompt. There is also a built in PowerShell app in Windows that you can use, but Terminal is much nicer. For example, Terminal supports normal cut, copy, and paste keyboard shortcuts, not to mention theming.

## Install OpenSSH Client on Windows
OpenSSH will give you the ssh command, which git uses behind the scenes to communicate securely with GitHub's servers. MacOS has ssh installed by default.

In an administrator account, go to Settings, and go to Apps & Features. Click on Optional Features. If OpenSSH Client is not listed under Installed Features, click Add a feature and install it. If you don't see the OpenSSH Client listed, you probably are not in an administrator account. 

To make sure this worked, in your regular user account, open Terminal, and from Powershell run `get-command ssh`. The response should look something like `C:\Windows\System32\OpenSSH\ssh.exe`. If not, reboot your computer and try again.

## Configure git to use the correct ssh (Windows)
Open a Terminal, and run the following command to configure git to use OpenSSH's ssh command, substituting `<the git command from above>` with what `get-command ssh` just told you:

```
git config --global core.sshCommand <the git command from above>
```

On MacOS, git works with ssh out of the box.

## Create a ssh key (all platforms)
SSH keys are an alternative to entering a username and password to connect to a server using SSH. They make use of public key cryptography to ensure a secure connection.

Run the following in Terminal to create a key, substituting your_email@example.com with your own email address:

```
ssh-keygen -t ed25519 -C "your_email@example.com"
```

If you get an error about the ed25519 encryption algorithm being unsupported, run this instead:

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

The email address just labels the key for your personal reference. When prompted to save your new key, press `<Enter>` to use the default location. When prompted for a passphrase (a password), do not skip this step! Read below for instructions on how to use SSH agent to avoid needing to type your passphrase all the time.

These commands will create two files in your `.ssh` directory located at `$HOME/.ssh` (Mac) or `$Env:USERPROFILE/.ssh` (Windows Powershell): `id_ed25519` and `id_ed25519.pub`, or `id_rsa` and `id_rsa.pub` if you ran the second command. `$HOME` and `$Env:USERPROFILE` are environment variables that refer to `/Users/<username>` on Mac and `C:\Users\<username>` on Windows, respectively, where `<username>` is your account name.

The file ending in `.pub` is your public key and is meant to be copied to servers that you want to access. The other file is your private key and should *never* be shared with anyone.

## Add your public key to GitHub (all platforms)
Login to GitHub. Click your avatar in the upper right corner of the window, and select Settings. Then select SSH and GPG keys on the left. Click the New SSH key button. Give the key a name (any name will do), leave Key type as-is, and copy and paste the contents of your public key file into the text box. You can open the file in a program like Notepad or TextEdit. Or you can print its contents in the Terminal with `type id_ed25519.pub` (Windows) or `cat id_ed25519.pub` (Mac).

## Make sure everything works so far
To check whether you are on the right track, in Terminal, run `ssh -T git@github.com`. If all is well, you will be prompted for your key's passphrase, and then you will get a successful connection message.

## Activate SSH Agent on Windows
If you try using your key to connect to GitHub now, you will be prompted to enter your passphrase every time you connect. SSH Agent will enable you to use your key without needing to enter the passphrase all the time.

Open Services as an administrator (from any user account, search for Services in the search bar and look for Run as administrator). Scroll down to OpenSSH Authentication Agent > right click > Properties. Change the Startup type from Disabled to Automatic. Then open PowerShell as an administrator the same way, and type `Start-Service ssh-agent` to start the agent. You only have to do this once - ssh-agent will start automatically on boot from now on.

## Add your public key to SSH Agent (all platforms)
As promised, if you do this step, you will not have to enter your key's passphrase every time it is used. In Terminal, from your `.ssh` directory run `ssh-add id_ed25519.pub` or `ssh-add id_rsa.pub` depending on which file you generated above. Enter the passphrase when prompted, and you should see an Identity added message.

## Make sure it worked
To check whether all this worked, in Terminal, run `ssh -T git@github.com` again. If all is well, you should see the same connection successful message without needing to enter a passphrase.

## Configure git
Run the following two commands in your Terminal to set your name and your email address. Doing this will ensure that your changes to your project are easily recognizable as coming from you.
```
git config --global user.name "<Your name>"
git config --global user.email "<Your email address>"
```

## Create a new repository
In your GitHub account, click Repositories, and then click the New button. Give your repository a name and description, and select whether you want it to be public or private. Check Add a README file, and then click the Create Repository button at the bottom.

## Copy your repository on to your computer
On your repository's main page, click the Code button, and click the Copy button to the right of the string starting with `git@github.com...`. In your Terminal, cd to a directory in which to put your repository, and type `git clone <repository url>`, substituting `<repository url>` with the string you just copied. Your repository, containing the README file, will now appear on your computer.

## Edit your README
Open the README file in a text editor, and add some text about your project. Save and close it.

## Add your changes to your repository
Back in Terminal, `cd` into the repository, and type `git add .` to stage the changes you just made to your README file. Git will do nothing with your changes and additions until you stage them. Warning, the `git add .` command will suck up *any* changes you make to any files in that directory, including any new files that you have added to the directory, so be careful. To help you out, git will list the files it staged so you can check whether any files were staged in error. You can replace `.` with the path to a file if you want to stage a specific file and ignore the rest. After you have confirmed that git staged the correct file(s), place any staged files into the repository by typing `git commit -m "<a message>"`, substituting `<a message>` with a brief description of what you changed.

Now your changes are stored locally on your hard drive. You can continue to stage and commit changes locally. However, you should frequently (ideally daily) share your changes with your team. You do that with the `push` command. Type `git push`, and then refresh your browser to see the changes to your README! If `git push` resulted in an error, try the full version of this command, `git push origin main` (`origin` refers to GitHub, and `main` refers to the default branch of your code -- see below for more about branches).

## Making changes to a team project
So, you've decided to work with a team on a new project. One way to divide up work is for each of you to create a branch for your changes, called a feature branch. You work on your changes, committing and pushing your changes as you go. When your team decides that your changes are ready, you merge them into the main branch, typically called `main`. Note that older GitHub projects may use `master` as the default main branch, but anything new will use `main` by default. Some projects have more complicated branching schemes, but this one is useful as a starting point.

To create a feature branch, type `git checkout -b <branch_name>`, substituting `<branch_name>` with the name of your branch. It's a good idea to include in the branch name your name and a two or three word description of what you are doing.

## Next steps
This concludes the introduction to git and GitHub. There are lots of tutorials and documentation about git and GitHub online, and there is much more to learn. If you are using a code editor like Visual Studio Code or an integrated development environment like IntelliJ, it will have built in git support that hides all of git's complexity behind menus and windows. Some even have GitHub support, which will make using git even easier. However, after you get started with the basics, it is important to learn what is happening under the hood. One of the better sources of information about git is the Git Book, found at https://git-scm.com/book/en/v2.

Happy coding!
