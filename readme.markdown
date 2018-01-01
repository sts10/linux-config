# Notes on Initial Linux Desktop Setup (Pop_OS! and Gnome)

## Some System-Level Basics
1. In settings > "Mouse and Track Pad", put "Mouse Speed" about in the dead center and turn off "Natural Scrolling"
2. In Gnome Keyboard settings, change "Switch windows of an application" OR "Switch windows of an app directly" to "Super + Escape"
  - You may also want to remap commands like "View split on left" and "Move one monitor left", as well as the Workspace commands
3. Download the Gnome Tweaks Tool via the POP app store. In Tweaks:
  - Remap Caps Lock to Control in "Keyboard & Mouse" > "Additional Layout Options". 
  - Suggested: Change "Mouse" "Acceleration Profile" to "Flat"
4. Change your desktop background!

## bashrc

I've got a few `.bashrc`s for you in the `bash` directory. The one called `bashrc` has my preferred prompt coded out, plus the neccessary settings for both `rbenv` and Rust. To use that one, run:

```
curl -o ~/.bashrc https://raw.githubusercontent.com/sts10/linux-config/master/bash/bashrc
```

## Neovim

I think I used the stable ppa and followed [these instructions](https://github.com/neovim/neovim/wiki/Installing-Neovim#ubuntu). A decent init.vim file is contained in this repo. 

```
mkdir ~/.config/nvim
curl -o ~/.config/nvim/init.vim https://raw.githubusercontent.com/sts10/linux-config/master/neovim/init.vim
```

## Syncing and Security
1. You're probably going to want to install snap with `sudo apt install snapd`
2. I then installed KeePassXC with `snap install keepassxc`
3. Next, I installed Syncthing via the instructions at the top of [this page](https://apt.syncthing.net/)
4. I then setup Syncthing, including my KeePass database. 
5. For a GPG GUI application, ["GNU Privacy Assistant"](https://help.ubuntu.com/community/GnuPrivacyGuardHowto#Graphical_Interfaces) gets the job done. To install it, I ran: `sudo apt install gpa`.

## Other Apps
1. Installed Wire desktop via [these instructions](https://medium.com/@wireapp/a-step-forward-for-wire-for-linux-52f0538cac15)
2. [Download and install Signal](https://signal.org/download/)
3. Install Ricochet from the POP store.
4. Install Tor Browser (forget how exactly I did this-- maybe Tor Browser Launcher? From POP store?)
5. Install Chromium via the POP app store if you like.
6. Install Standard Notes app from [their site](https://standardnotes.org/getting-started?downloaded=linux)

## Flash/Chrome
Install Chrome from (https://www.google.com/chrome/browser/desktop/index.html). The downloaded file open in Eddy. One click install!!
- I tried Chromium (via pop_os software store), and then install Adobe Flash Player (ugh) with `sudo apt-get install flashplugin-installer` -- I restarted Chromium but it's not working.
- **Next thing to try**: all of this: https://websiteforstudents.com/installing-the-latest-flash-player-on-ubuntu-17-10/

## Dev Environment
1. Install [rbenv (via "basic github checkout")](https://github.com/rbenv/rbenv#basic-github-checkout) and an up-to-date Ruby (and set that to global).
2. Install Rust with `curl https://sh.rustup.rs -sSf | sh`
    - Needed to add `export PATH="$HOME/.cargo/bin:$PATH"` to end of `~/.bashrc`
    - you can uninstall at anytime with `rustup self uninstall`
3. Setup Github
  a. set git username and email
  b. Google for Github setup-- you're going to need to generate a new shh key pair on your machine, then upload the public key to Github. I followed [these instructions](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/), creating an ssh key locally, with a passphrase that I stored in my fly keepass database.
4. Install and set up Jekyll for my Github blog:
   1. Be sure rbenv is set up and a modern version of Ruby is set to global.
   2. `gem install jekyll bundler`
   3. `git clone git@github.com:sts10/sts10.github.io.git`
   4. cd into the repo
   5. `bundle exec install`

   - To publish changes: commit changes, then run `bundle exec jekyll build`, then `git push origin master`
5. [Disable blinking cursor in gnome-terminal](https://askubuntu.com/a/947573): In terminal: `gsettings set org.gnome.desktop.interface cursor-blink false`

## Changing Default Fonts
Weirdly the only place I could find this is in Gnome Tweak Tool. Here are the defaults and what I changed them to:
```
  - Window title: Fira Sans SemiBold 10 --> Noto Sans CJK TC Bold 10
  - Interface: Fira Sans Book 10        --> Noto Sans CJK TC Regular 10
  - Document: Roboto Slab Regular 11    --> IBM Plex Sans Regular 11
  - Monospace: Fira Mono Regular 11     --> Deja Vu Sans Mono Book 11
```

You can [download IBM Plex here](https://ibm.github.io/type/).

## General tips:

### How to Mount an external harddrive that's formatted as exFAT
Simply install these programs: `sudo apt-get install exfat-fuse exfat-utils` ([via](https://www.reddit.com/r/Ubuntu/comments/6r954q/mount_exfat_drive_in_ubuntu_1704/)). 

### Using a PGP private key from a Smartcard on Ubuntu

1. Add the public key to your system: Get the .asc file onto your system, then run `gpg2 --import <file-name>.asc`
2. Plug the smartcard into your computer.
3. Try running `gpg2 --card-status`. 

**Troubleshooting**
- If the above doesn't work try installing scdaemon and pcscd with `sudo apt install scdaemon` and then `sudo apt install pcscd`. You may also need to `sudo apt install gpgsm`
- You may also need to kill and restart scdaemon with:
```
killall scdaemon
pgrep scdaemon
```

You can now encrypt and decrypt files with the pgp keys on your smartkey using the gpg2 command line tool. To decrypt a file run something like this: `gpg --output test --decrypt '/home/schlinkert/keepass-databases/key-files/fly1.key.gpg'`. Check that you can decrypt a keepass database AND that you can alter settings.

For a GPG GUI application, try `sudo apt install gpa` which installs a program called ["GNU Privacy Assistant"](https://help.ubuntu.com/community/GnuPrivacyGuardHowto#Graphical_Interfaces)
