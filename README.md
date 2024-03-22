# Install Rails on Windows 10/11 with Subsystem for Linux (WSL)
## Contents
- [Install Rails on Windows 10/11 with Subsystem for Linux (WSL)](#install-rails-on-windows-1011-with-subsystem-for-linux-wsl)
  - [Contents](#contents)
  - [Decisions made](#decisions-made)
  - [Install WSL](#install-wsl)
  - [Set up Git](#set-up-git)
  - [Install Ruby with rbenv](#install-ruby-with-rbenv)
  - [Install Rails](#install-rails)
  - [Integrate with Visual Studio Code](#integrate-with-visual-studio-code)

## Decisions made
If you just want to get going, please skip this section.

There are other tutorials for installing Rails on Windows. A lot of them are good, but they all made at least one decision that I didn't think was for the best. This method has assisted many people after they were failed by some tutorial. Please raise an issue or PR if you think something is wrong or could be improved.

* Running Rails from WSL is the best option for me. Running it directly from Windows is possible, but Rails development is geared towards Mac/Linux so it feels better to do it this way, and it is easier to set up as well.
* Connect with Github because that's almost always needed. It would be similar to connect with Gitlab or Bitbucket, but I'm not going to cover that here.
* Creating a new key for Github in WSL. It is apparently possible to use the same key as Windows with [Git credential manager](https://learn.microsoft.com/en-us/windows/wsl/tutorials/wsl-git#git-credential-manager-setup). I haven't tried it, but I'm guessing it's more hassle than it's worth.
* Using Rbenv over RVM or ASDF. Just because I've always used it and it's worked well for me. I'm sure the others are great too.
* Integrating with VS Code. Because that's what we are all using, right? If you are using VIM or something else, cool, but it's not covered here.

## Install WSL
* Open PowerShell as Administrator (find in the menu, right-click, select Run as Administrator)
* Type `wsl --install` and press enter, it will do it all and install Ubuntu. See [Microsoft's documentation](https://learn.microsoft.com/en-us/windows/wsl/install) for configuration options, or if anything doesn't work with this.
* After the installation **and a reboot**, open the Ubuntu App from the Start menu and it will install. If you get an error, open Windows Updates and install any pending updates, restart then try again. Follow the prompts to create a user account. This can be the same user/password as your Windows account, or something new. The password will be used for `sudo` commands, and it will **not** echo back to the screen as you type it. Don't worry that the password isn't going in, just type it out and press enter.
* You will not need to do anything else in PowerShell from this point on. All commands from this point on will be in the Ubuntu terminal.
* Make sure that you're up to date: `sudo apt update && sudo apt upgrade -y`

## Set up Git
Git will come automatically installed on Ubuntu WSL, so we just need to configure it.
Type the following commands into the Ubuntu terminal (replace with your own Github username and email):
```bash
git config --global color.ui
git config --global user.name "YOUR_NAME"
git config --global user.email "YOUR@EMAIL.com"
ssh-keygen -t ed25519 -C "YOUR@EMAIL.com"
```
This makes the output more readable with red for removed lines and green for added lines, adds the name and email that will be used when you author commits, and generates a new SSH key to use with GitHub.

Print the SSH key to the screen with the command below and copy it to your clipboard (select the text with your mouse, right-click, "Copy"):
```bash
cat ~/.ssh/id_ed25519.pub
```
Now add the key to GitHub. Go to [Add new SSH key](https://github.com/settings/ssh/new) and paste the key into the box. The title can be anything, maybe something like "WSL Ubuntu". Then click "Add SSH key".

Come back to the Ubuntu terminal and test the connection:
```bash
ssh -T git@github.com
```
It will come up with a warning about the authenticity of the host, type `yes` and press enter.

You should see something like this:
```bash
Hi YOUR_NAME! You've successfully authenticated, but GitHub does not provide shell access.
```
That's it! You're ready to move on.

## Install Ruby with rbenv
```bash
sudo apt install git curl libssl-dev libreadline-dev zlib1g-dev autoconf bison build-essential libyaml-dev libreadline-dev libncurses5-dev libffi-dev libgdbm-dev -y

curl -fsSL https://github.com/rbenv/rbenv-installer/raw/HEAD/bin/rbenv-installer | bash

echo 'eval "$(~/.rbenv/bin/rbenv init - bash)"' >> ~/.bashrc
source ~/.bashrc
```
This installs all the dependencies needed, installs rbenv, and adds the rbenv path to your bashrc file so that it's available every time you open a new terminal.

Run `rbenv install -l` to see what versions of Ruby are available, I'd personally pick the latest version. Install with `rbenv install X.Y.Z` for example `rbenv install 3.2.1`. Once you've installed a version, you can set it as the global default with `rbenv global X.Y.Z`.

Run `ruby -v` to see that it's worked, and what version you're using.

## Install Rails
Run `gem install rails` to get the latest version, or `gem install rails -v X.Y.Z` to install a specific version if you need that. Run `rbenv rehash` to make sure that rbenv is aware of the new gem.

Now, create a new project! `rails new MYPROJECTNAME`, and then `cd MYPROJECTNAME`. Run `rails server` to start the server, and then open your browser of choice like Chrome or Firefox (in Windows) and goto `http://localhost:3000` to see the default Rails page.

## Integrate with Visual Studio Code

Install Visual Studio Code (in Windows) [VS Code](https://code.visualstudio.com/). Make sure you keep the option ticked to add it to your path. Then install the [Remote - WSL](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl) extension in Visual Studio Code. This will allow you to open a WSL terminal in VS Code, and open files from the WSL terminal in VS Code.

In the Ubuntu terminal, run `code .` from your project's directory. It will open your project in VS Code so that you can edit it. If this doesn't work, and it says it can't find the `code` command, reinstall [VS Code](https://code.visualstudio.com/) and make sure you tick the option to add it to your path, close and reopen the Ubuntu terminal, and try again.

Sorted.
