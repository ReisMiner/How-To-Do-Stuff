# How to use gpg key on yubikey with git

## Prerequisites
  - Yubikey with gpg keys set up
  - gpg4win installed
  - putty installed

## Steps
- go to `%appdata%/gnupg` and open `gpg-agent.conf`
  - add the following lines to the file 
 ```enable-putty-support
enable-ssh-support
use-standard-socket
default-cache-ttl 600
max-cache-ttl 7200
```
- save the file and enter `gpg-connect-agent killagent /bye; gpg-connect-agent /bye` in the powershell to apply the changes
- [export your gpg key as ssh-key](https://github.com/ReisMiner/How-To-Do-Stuff/blob/master/Linux/How%20To%20export%20GPG%20key%20as%20SSH%20key.md)
- add the key to your git host (github/gitlab/etc)
- enter this in powershell to configure git to use putty for the yubikey support `git config --global core.sshcommand 'plink -agent'`
  - remove it again by using `git config --unset-all core.sshcommand`
  - if that did not work use `git config core.sshcommand ssh` to set the ssh agent to windows default
