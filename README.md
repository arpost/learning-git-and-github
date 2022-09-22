# Learning Git and GitHub

All of the instructions below assume you have administrator access to your computer.

## Installing Git on MacOS
On Mavericks (10.9) or newer, try to run git from the Terminal:

```$ git --version```

If you don’t have it installed already, Terminal will prompt you to install it. The rest of what you need is installed by default or will be installed along with git.

## Installing Git on Windows
Go to https://git-scm.com/download/win and the download will start automatically. For more information, go to https://gitforwindows.org. 

## Installing Windows Terminal
Go to the Microsoft Store, search for Windows Terminal, and install it. It is free of charge from Microsoft. You will probably want to set the default shell to PowerShell, which is far more capable than the regular command prompt. There is also a built in PowerShell app in Windows that you can use, but Terminal is much nicer.

## Installing OpenSSH Client on Windows 10
In an administrator account, go to Settings, and go to Apps & Features. Click on Optional Features. If OpenSSH Client is not listed under Installed Features, click Add a feature and install it. If you don't see the OpenSSH Client listed, you probably are not in an administrator account. 

## Activate SSH Agent on Windows 10
Open Services as an administrator (from any user account, search for Services in the search bar and look for Run as administrator). Scroll down to OpenSSH Authentication Agent > right click > Properties. Change the Startup type from Disabled to Automatic. Then open PowerShell as an administrator the same way, and type `Start-Service ssh-agent` to start the agent (this will happen automatically after every reboot):

Next, in Terminal, open a PowerShell and run `get-command ssh` to get the path to your ssh command. It most likely is `C:\Windows\System32\OpenSSH\ssh.exe`. Then open a Terminal, and run the following command to configure git to use Microsoft's ssh command, substituting `<the git command from above>` with what `get-command ssh` just told you:

```
git config --global core.sshCommand <the git command from above>
```

## Create a ssh key (all platforms)
Run the following in Terminal:

```
ssh-keygen -t ed25519 -C "your_email@example.com"
```

If your operating system doesn't support the ed25519 encryption algorithm, run this instead:

```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

The email address just labels the key for your personal reference. When prompted to save your new key, press `<Enter>` to use the default location. When prompted for a passphrase (a password), do not skip this step! Read below for instructions on how to use SSH agent to avoid needing to type your password all the time.

These commands will create two files in your `.ssh` directory located at `$HOME/.ssh` (Mac) or `$Env:USERPROFILE/.ssh` (Windows Powershell): `id_ed25519` and `id_ed25519.pub`, or `id_rsa` and `id_rsa.pub` if you ran the second command. `$HOME` and `$Env:USERPROFILE` refer to `/Users/<username>` on Mac and `C:\Users\<username>` on Windows, respectively, where `<username>` is your account name.

The file ending in `.pub` is your public key and is meant to be copied to servers that you want to access without needing to enter your username and password every time. The other file is your private key and should *never* be shared with anyone.

## Add your public key to GitHub
Final step! Login to GitHub. Click your avatar in the upper right corner of the window, and select Settings. Then select SSH and GPG keys on the left. Click the New SSH key button. Give the key a name, leave Key type as-is, and copy and paste the contents of your public key file into the text box. You can open the file in a program like Notepad (Windows) or Textedit (Mac). Or you can print its contents in the Terminal with `type id_ed25519.pub` (Windows) or `cat id_ed25519.pub` (Mac).

## Add your public key to SSH Agent
As promised, if you do this step, you will not have to enter your key's passphrase every time it is used. In Terminal, from your `.ssh` directory run `ssh-add id_ed25519.pub` or `ssh-add id_rsa.pub` depending on which file you generated above. Enter the passphrase when prompted, and you should see an Identity added message.

## Making sure it worked
To check whether all this worked, in Terminal, run `ssh -T git@github.com`. If all is well, you should see a logged in message without needing to enter the passphrase.
