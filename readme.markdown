# Notes on Initial Linux Desktop Setup (Pop_OS! + Gnome and KDE)

## Some System-Level Basics (Gnome)
1. In settings > "Mouse and Track Pad", put "Mouse Speed" about in the dead center and turn off "Natural Scrolling"
2. In Gnome Keyboard settings, change "Switch windows of an application" OR "Switch windows of an app directly" to "Super + Escape"
  - You may also want to remap commands like "View split on left" and "Move one monitor left", as well as the Workspace commands
3. Download the Gnome Tweaks Tool via the Pop app store. In Tweaks:
  - Remap Caps Lock to Control in "Keyboard & Mouse" > "Additional Layout Options". 
  - Suggested: Change "Mouse" "Acceleration Profile" to "Flat"
4. Change your desktop background!

## bashrc

I've got a few `.bashrc`s for you in the `bash` directory. The one called `bashrc` has my preferred prompt coded out, plus the neccessary settings for both `rbenv` and Rust. To use that one, run:

```
curl -o ~/.bashrc https://raw.githubusercontent.com/sts10/linux-config/master/bash/bashrc
```

## Konsole profile and Pink Moon colorsheme

Move the contents of `plasma/konsole` to `~/.local/share/konsole`.

If you need to, set a Custom color for cursor: #71798a and turn on blinking.

## Gnome-terminal profile

Via [this gist](https://gist.github.com/reavon/0bbe99150810baa5623e5f601aa93afc): 

Load our profile with: 

```bash
dconf load /org/gnome/terminal/legacy/profiles:/:da23a4a8-af92-46a8-ad3e-65fa07a0e113/ < gnome-terminal-config/pink-moon-profile.dconf
```

Or `cd` into the `gnome-terminal-config` directory in this repo and run:

```bash
dconf load /org/gnome/terminal/legacy/profiles:/:da23a4a8-af92-46a8-ad3e-65fa07a0e113/ < pink-moon-profile.dconf
```

If there's a problem, `dconf dump /org/gnome/terminal/legacy/profiles:/` might be helpful, as it lists the profiles loaded on your system.

## Git

Before you do anything below, especially with Neovim or Vim, install git! `sudo apt install git`

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
3. Install Ricochet from the POP store (though check version).
4. Install Tor Browser (forget how exactly I did this-- maybe Tor Browser Launcher? From POP store?)
5. To install OnionShare, I followed the "Ubuntu" instructions on their [download page](https://onionshare.org/#downloads) and added their PPA. (The version in the POP store was 0.9 -- too old for me.) 
6. Install Chromium via the POP app store if you like.
7. Install Standard Notes app from [their site](https://standardnotes.org/getting-started?downloaded=linux)

## Flash/Chrome
Install Chrome from (https://www.google.com/chrome/browser/desktop/index.html). The downloaded file open in Eddy. One click install!!
- I tried Chromium (via pop_os software store), and then install Adobe Flash Player (ugh) with `sudo apt-get install flashplugin-installer` -- I restarted Chromium but it's not working.
- **Next thing to try**: all of this: https://websiteforstudents.com/installing-the-latest-flash-player-on-ubuntu-17-10/

## Dev Environment

### Ruby and rbenv
1. Install [rbenv (via "basic github checkout")](https://github.com/rbenv/rbenv#basic-github-checkout) 
2. Install [ruby-build](https://github.com/rbenv/ruby-build#installation) as an rbenv plugin. 
3. You made need to run `sudo apt-get install -y libssl-dev libreadline-dev zlib1g-dev`
4. `rbenv install -l` lists available versions of Ruby. Pick one to install. 
5. Set that version to global.


### Rust

1. [Install Rust](https://www.rust-lang.org/en-US/install.html)
 - I needed to add `export PATH="$HOME/.cargo/bin:$PATH"` to end of `~/.bashrc`, though this line is likely in the included bashrc in this repo.
  - Note: you can uninstall at anytime with `rustup self uninstall`

2. Install [rustfmt](https://github.com/rust-lang-nursery/rustfmt) and [its Vim counterpart, rust.vim](https://github.com/rust-lang/rust.vim#formatting-with-rustfmt)

3. Install [Rust Clippy](https://github.com/rust-lang-nursery/rust-clippy#usage)

4. Install pip3 with `sudo apt-get install python3-pip` and then `pip3 install neovim`

5. Check [Deoplete](https://github.com/Shougo/deoplete.nvim) installed

6. Install [Racer](https://github.com/racer-rust/racer)

7. Make sure [Vim Racer](https://github.com/racer-rust/vim-racer) is installed. My vim-racer config is:

```vim
Plug 'racer-rust/vim-racer'
set hidden
let g:racer_cmd = "/home/sschlinkert/.cargo/bin/racer"
let g:racer_experimental_completer = 1
```


### Github/ssh keys

1. Set git username (email) and email locally

2. Generate a new shh key pair on your machine, then upload the public key to Github. I followed [these instructions](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/), creating an ssh key locally, with a passphrase that I stored in my keepass database.

3. In the `bashrc` included in this repo is some code that handles your `ssh-agent`. I got it from [this section of the Arch Linux wiki](https://wiki.archlinux.org/index.php/SSH_keys#ssh-agent). Here's the bash code if you need:

```bash
if ! pgrep -u "$USER" ssh-agent > /dev/null; then
    ssh-agent > ~/.ssh-agent-thing
fi
if [[ "$SSH_AGENT_PID" == "" ]]; then
    eval "$(<~/.ssh-agent-thing)"
fi
```

4. `touch ~/.ssh/config` and in that file write `AddKeysToAgent yes`, as per [the Arch wiki entry](https://wiki.archlinux.org/index.php/SSH_keys#ssh-agent).

Alternatively, you could try [storing ssh key in KeePassXC database](https://keepassxc.org/docs/#faq-ssh-agent-how), but I haven't had luck with that in the past.

### Install and set up Jekyll for my Github blog

1. Be sure rbenv is set up and a modern version of Ruby is set to global.
2. `gem install jekyll bundler`
3. `git clone git@github.com:sts10/sts10.github.io.git`
4. cd into the repo
5. `bundle exec install`

- To publish changes: commit changes, then run `bundle exec jekyll build`, then `git push origin master`

### [Disable blinking cursor in gnome-terminal](https://askubuntu.com/a/947573): In terminal: `gsettings set org.gnome.desktop.interface cursor-blink false`

You can update the Ruby versions available to rbenv by running `rbenv_upgrade` (found in `bashrc`).

## Changing Default Fonts
Weirdly the only place I could find this is in Gnome Tweak Tool (which I think I installed via the GUI Pop software store). Here are the defaults and what I changed them to:
```
  - Window title: Fira Sans SemiBold 10 --> Noto Sans CJK KR Bold 10
  - Interface: Fira Sans Book 10        --> Noto Sans CJK KR Regular 10
  - Document: Roboto Slab Regular 11    --> IBM Plex Sans Regular 11
  - Monospace: Fira Mono Regular 11     --> Deja Vu Sans Mono Book 11
```

You can [download IBM Plex here](https://github.com/IBM/plex).

## Images/Video

For editing RAW photos, try Darktable. Here are some [free film emulators](https://github.com/t3mujin/t3mujinpack). There's also GIMP and Kdenlive (for video)!

## KDE Troubleshooting

If, after installing the proper NVIDIA drivers and restarting the machine, everything looks big, go to Fonts settings menu and "Force" the DPI to 96.

## General tips:

### How to Mount an external harddrive that's formatted as exFAT
Simply install these programs by running this line: `sudo apt-get install exfat-fuse exfat-utils` ([via](https://www.reddit.com/r/Ubuntu/comments/6r954q/mount_exfat_drive_in_ubuntu_1704/)). 

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

### Putting a new distro on Oryx Pro

Once you've got the USB stick in the computer, reboot your system. You’ll need to tell the computer to boot from the live disk by holding F7 or F1 [source](http://support.system76.com/articles/live-disk/). 

If you're NOT going with Pop_OS, you'll likely need to install System76 drivers and the appropriate NVIDIA drivers. [More info on this here](https://support.system76.com/articles/system76-driver/).

### Generic Upgrade Line

On the command line, `sudo apt-get update && sudo apt-get dist-upgrade` is a good way to upgrade everything.

### Redshift + Xorg + KDE

If you're running KDE and Xorg, you'll likely need to use Redshift to tint your screen at night. "Redshift Control" is a KDE widget that gives you a nice GUI to configure Redshift (found by searching the "Get new widgets" interface).

### System76/Pop!\_OS Links

[Support articles](http://support.system76.com/articles/)

[Pop Docs](http://pop.system76.com/docs/)

[Installing System76 and appropriate NVIDIA drivers for System76 machine](https://support.system76.com/articles/system76-driver/)
