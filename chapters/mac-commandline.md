
== The Mac Command Line



=== Package Manager

If you have ever used NuGet or Chocolatey (Windows tools) or apt-get (Linux tools) to install software for your platform, you can do the same on a Mac using HomeBrew or **brew** for short. 

Head over to the [homebrew website](http://brew.sh) to install the tool. You are not going to download any dmg binaries. The installation instruction on the brew website will actually ask you to open a terminal window, copy and paste a script command and execute it. Here are the steps

Launch a terminal window. Click cmd + spacebar then type "Terminal". The first hit on Spotlight should be the Terminal.app, press enter to launch it

Head over to [brew.sh](http://brew.sh), copy and paste the script under the the heading "install homebrew". I will copy and paste the script here for purposes of convenience, but you should really to the brew website because those instructions will be updated, this document might not get updated for sometime. Anyway, the script you need to copy paste is the following

----
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
----

OSX comes with Ruby, so the script above should not give you a problem. Press ENTER to start the installation. Wait for instructions.

Once the installation is done, you can now get lots of good stuff from the homebrew repos, some of them are the following

----
brew install git
brew install tmux
----

Brew is not the only package manager for OSX, there are others like Fink or MacPorts.

=== iTerm2

Get iTerm2. The built-in Terminal.app is okay, but if you want some bells and whistles for your Terminal console, iTerm2 is the way to go. 

Head over to the [iTerm2 website](https://www.iterm2.com/) and download the binary installer.  Install it the way you have installed other binaries on the Mac. Double click and follow the prompts. 

It will ask you to drag the iTerm2 app into the **Applications** folder. Follow it.

## TMUX 

TMUX is a terminal multiplexer just like **screen**. Actually, you can still use screen in OSX but tmux is more modern and it has a very active community of devs updating it from time to time. Get it via brew

----
brew install tmux
----

You might not like the default key bindings and colors of tmux, in that case, you can change it by creating a tmux configuration file. To create the tmux config file, follow the commands

----
cd ~
touch .tmux.conf
----

Next, edit the contents of .tmux.conf using your favorite editor. If, for example, you have installed the SublimeText editor and have already mapped it to your command line, you can do this

----
subl ~/.tmux.conf
----

Now you can edit the config file. For starters, you can follow my tmux config until you can customize your own.

----
set -g prefix C-v
unbind C-b

set -g base-index 1
setw -g pane-base-index 1

bind | split-window -h
bind - split-window -v
bind D source-file ~/progtools/tdev.sh

setw -g mode-mouse on
set  -g mouse-select-pane on
set  -g mouse-resize-pane on
set  -g mouse-select-window on

set  -g pane-border-fg colour236
set  -g pane-active-border-fg  cyan
set  -g pane-active-border-bg default
# set  -g pane-active-border-bg green
set  -g default-terminal "screen-256color"
set  -g status-bg colour233
set  -g status-fg cyan

set  -g default-command "reattach-to-user-namespace -l /bin/bash"
----

Once you have saved the .tmux.conf on your home directory, you can now start using tmux. 

## Using TMUX 

Launch iTerm2. To create a new tmux session, use the `new` command

----
tmux new -s work
----

You can replace *work* in the command above with any name you want, I just used it because I want my session to be named "work". You can create as many sessions as you want, but just start with one session for now.

It may seem that nothing happened after you created a new session, but try pressing they keys `CTRL+V`, release they keys then press the minus sign. Your screen should be split vertically by now. Next, try pressing the keys `CTRL+V`, release the keys then press the `|`. That key is usually found on top of the ENTER key on US keyboards. Your screen should be split vertically by now.

You can jump from one panel to another using your mouse. Just click over a panel and the cursor focus should be on that panel.

Alternatively, you can also jump from panel to panel using the `CTRL+V`, then the arrow keys.

Once you are done with tmux, you can kill the session with the command

----
tmux kill-session -t work
----

Of course, replace *work* with whatever session name you used.

Those are the basics for tmux.





