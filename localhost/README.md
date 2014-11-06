# Configure a Mac with Ansible

## Background

My first attempt at using Ansible:  configure my new Mac.

All the cool kids break up their Ansible projects into modules and roles, but for starters I wrote a single playbook.

## Prerequisites

Here are the manual steps I did before I could execute this playbook:

1. Upgrade OS X to the current version (10.9.5) through the App Store.
2. Install Xcode through the App Store.
3. Make sure the command-line tools (CLT) are installed.  As of OS X 10.9.5
   and Xcode 6.1, one way to do this is (thanks to
   [this issue](https://github.com/Homebrew/homebrew-php/issues/241)
   on the Homebrew-PHP issues queue)
   `$ xcode-select --install`
   and then confirm installation.
4. Install [Homebrew](http://brew.sh/)
5. Install the one program I *really* need:  `$ brew install macvim`
6. Install Ansible:  `$ brew install ansible`
7. Tell Ansible to save a log by adding `.ansible.cfg` in your home directory
   with the line
   `log_path = /usr/local/var/log/ansible.log`
8. By default, Ansible uses ssh to connect.  On OS X, this means you should do
   the following so that you can connect to localhost without giving your
   password:
  1. Enable remote login (a.k.a. ssh) in System Preferences.
  2. Follow the
     [GitHub instructions](https://help.github.com/articles/generating-ssh-keys/)
     for generating SSH keys.
  3. `$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys`

## Running the playbook

Once you have the prerequisites, edit the variables in `vars/main.yml` and
then

    $ ansible-playbook --ask-sudo-pass -i hosts playbook.yml

(You can use `-K`, the short form of `--ask-sudo-pass`, if you prefer.)  When you get the prompt `sudo password:`, enter your password.

## What gets installed

Most things are installed using Homebrew under `/usr/local/bin`.

- PHP
- composer
- drush
- Vim configuration for editing Drupal:  the
  [Vimrc project](https://drupal.org/project/vimrc)
- some basic Git configuration files

## Further thoughts

I like the Homebrew approach of installing under `/usr/local` and not needing
to sudo all the time.  Next time around, I might change my mind and install
Ansible first, with some sudo access, and then use it to install Homebrew.

There must be a way to tell Homebrew to use the version of PHP already on
the system.  I should add a variable to specify which version of PHP to use.

## Credits

The drush-specific steps are based on Jeff Geerling's
[drush role](https://github.com/geerlingguy/ansible-role-drush)
for Ansible (also avaiable at
[Ansible Galaxy](https://galaxy.ansible.com/list#/roles/433)).
