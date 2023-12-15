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
If you are not interested in this, and just want to get going, please skip this section.

## Install WSL
* Open PowerShell as Administrator (find in the menu, right-click, select Run as Administrator)
* Type `wsl --install` and press enter, it will do it all and install Ubuntu. See [Microsoft's documentation](https://learn.microsoft.com/en-us/windows/wsl/install) for configuration options, or if anything doesn't work here
* After the installation and a reboot, open the Ubuntu App from the Start menu and it will install. If you get an error, open Windows Updates and install any pending updates, then try again. Follow the prompts to create a user account. This can be the same user/password as your Windows account, or something new. The password will be used for `sudo` commands, and it will **not** echo back to the screen as you type it. Don't worry that the password isn't going in, just type it out and press enter.
* Make sure that you're up to date: `sudo apt update && sudo apt upgrade -y`

## Set up Git
It will come automatically installed on Ubuntu WSL, so we just need to configure it.
```bash
git config --global color.ui
git config --global user.name "YOUR_NAME"
git config --global user.email "YOUR@EMAIL.com"
ssh-keygen -t ed25519 -C "YOUR@EMAIL.com"
```
This makes the output more readable with red for removed lines and green for added lines, adds the name and email that will be used when you author commits, and generates a new SSH key to use with GitHub.

Print the SSH key to the screen and copy it to your clipboard:
```bash
cat ~/.ssh/id_ed25519.pub
```
Now add the key to GitHub. Go to [Add new SSH key](https://github.com/settings/ssh/new) and paste the key into the box. The title can be anything, maybe something like "WSL Ubuntu". Click "Add SSH key".

Come back to the terminal and test the connection:
```bash
ssh -T git@github.com
```
You should see something like this:
```bash
Hi YOUR_NAME! You've successfully authenticated, but GitHub does not provide shell access.
```
That's it! You're ready to move on.

## Install Ruby with rbenv
```bash
sudo apt install git curl libssl-dev libreadline-dev zlib1g-dev autoconf bison build-essential libyaml-dev libreadline-dev libncurses5-dev libffi-dev libgdbm-dev

curl -fsSL https://github.com/rbenv/rbenv-installer/raw/HEAD/bin/rbenv-installer | bash

echo 'eval "$(~/.rbenv/bin/rbenv init - bash)"' >> ~/.bashrc
source ~/.bashrc
```
This installs all the dependencies needed, installs rbenv, and adds the rbenv path to your bashrc file so that it's available every time you open a new terminal.

To install the latest version of Ruby, run `rbenv install -l`, alternatively you can install a specific version with `rbenv install X.Y.Z`. Once you've installed a version, you can set it as the global default with `rbenv global X.Y.Z`. To see what versions are available, run `rbenv versions`.

Run `ruby -v` to see that it's worked, and what version you're using.

## Install Rails
Run `gem install rails` to get the latest version, or `gem install rails -v X.Y.Z` to install a specific version. Run `rbenv rehash` to make sure that rbenv is aware of the new gem.

Now, create a new project! `rails new MYPROJECTNAME`, and then `cd MYPROJECTNAME`. Run `rails server` to start the server, and then open your browser (in Windows) to `http://localhost:3000` to see the default Rails page.

## Integrate with Visual Studio Code

