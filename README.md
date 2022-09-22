# Learning Git and GitHub

These instructions will guide you through installing and configuring a source code version control system called git on your computer, and setting up your access to the GitHub code repository at https://github.com. 

All of the instructions below assume you have administrator access to your computer. Once you are done, you will be able to access git repositories on GitHub from your computer without ever needing to enter a password!

## Creating a GitHub account
The first step is to create a GitHub account at https://github.com. You will collaborate with your teammates via this website.

## Installing Git on MacOS
On Mavericks (10.9) or newer, try to run git from the Terminal:

```$ git --version```

If you don’t have it already, Terminal will prompt you to install it. The rest of what you need is installed by default or will be installed along with git.

## Installing Git on Windows
Go to https://git-scm.com/download/win and the download will start automatically. For more information, go to https://gitforwindows.org. 

## Installing Windows Terminal
Go to the Microsoft Store, search for Windows Terminal, and install it. It is free of charge from Microsoft. You will probably want to set Terminal's default shell to PowerShell, which is far superior to the regular command prompt. There is also a built in PowerShell app in Windows that you can use, but Terminal is much nicer. For example, Terminal supports normal cut, copy, and paste keyboard shortcuts, not to mention theming.

## Installing OpenSSH Client on Windows 10
OpenSSH will give you the ssh command, which git uses behind the scenes to communicate securely with GitHub's servers. MacOS has ssh installed by default.

In an administrator account, go to Settings, and go to Apps & Features. Click on Optional Features. If OpenSSH Client is not listed under Installed Features, click Add a feature and install it. If you don't see the OpenSSH Client listed, you probably are not in an administrator account. 

To make sure this worked, open Terminal and from Powershell, run `get-command ssh`. The response should look something like `C:\Windows\System32\OpenSSH\ssh.exe`. If not, reboot your computer and try again.

## Configure git to use the correct ssh
Open a Terminal, and run the following command to configure git to use this ssh command, substituting `<the git command from above>` with what `get-command ssh` just told you:

```
git config --global core.sshCommand <the git command from above>
```

If you are trying this on MacOS, git should work with ssh out of the box.

## Create a ssh key (all platforms)
SSH keys are an alternative to entering a username and password to connect to a server using SSH. They make use of public key cryptography to ensure a secure connection.

Run the following in Terminal to create a key:

```
ssh-keygen -t ed25519 -C "your_email@example.com"
```

If your operating system is too old to support the ed25519 encryption algorithm, run this instead:

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

The email address just labels the key for your personal reference. When prompted to save your new key, press `<Enter>` to use the default location. When prompted for a passphrase (a password), do not skip this step! Read below for instructions on how to use SSH agent to avoid needing to type your password all the time.

These commands will create two files in your `.ssh` directory located at `$HOME/.ssh` (Mac) or `$Env:USERPROFILE/.ssh` (Windows Powershell): `id_ed25519` and `id_ed25519.pub`, or `id_rsa` and `id_rsa.pub` if you ran the second command. `$HOME` and `$Env:USERPROFILE` refer to `/Users/<username>` on Mac and `C:\Users\<username>` on Windows, respectively, where `<username>` is your account name.

The file ending in `.pub` is your public key and is meant to be copied to servers that you want to access. The other file is your private key and should *never* be shared with anyone.

## Add your public key to GitHub (all platforms)
Login to GitHub. Click your avatar in the upper right corner of the window, and select Settings. Then select SSH and GPG keys on the left. Click the New SSH key button. Give the key a name (any name will do), leave Key type as-is, and copy and paste the contents of your public key file into the text box. You can open the file in a program like Notepad or TextEdit. Or you can print its contents in the Terminal with `type id_ed25519.pub` (Windows) or `cat id_ed25519.pub` (Mac).

## Activate SSH Agent on Windows 10
If you try using your key to connect to GitHub now, you will be prompted to enter your passphrase every time you connect. SSH Agent will enable you to use your key without needing to enter the passphrase all the time.

Open Services as an administrator (from any user account, search for Services in the search bar and look for Run as administrator). Scroll down to OpenSSH Authentication Agent > right click > Properties. Change the Startup type from Disabled to Automatic. Then open PowerShell as an administrator the same way, and type `Start-Service ssh-agent` to start the agent. You only have to do this once - ssh-agent will start automatically on boot from now on):

## Add your public key to SSH Agent (all platforms)
As promised, if you do this step, you will not have to enter your key's passphrase every time it is used. In Terminal, from your `.ssh` directory run `ssh-add id_ed25519.pub` or `ssh-add id_rsa.pub` depending on which file you generated above. Enter the passphrase when prompted, and you should see an Identity added message.

## Making sure it worked
To check whether all this worked, in Terminal, run `ssh -T git@github.com`. If all is well, you should see a logged in message without needing to enter the passphrase.
