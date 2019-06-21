# Introduction to tmux

Tmux is a **terminal multiplexer**. Using tmux we can create, access and control terminals from a single terminal. Tmux sessions can be detached, and continue running in the background, then we can reattach. This is extremely helpful and it is one of the coolest features of tmux. To get up and running with tmux, you need to learn some tmux commands.

## Starting Tmux

To start tmux, enter *tmux* in your terminal.

When tmux is started, it creates a single session with a single window in it. At the bottom, the status bar displays the information about the current session. If you don't like the look of tmux and want to customize the information displayed on the status bar, we can do that using a tmux config file, which we will see later in this series. Now, get out of tmux, by typing *exit*.

## Sessions, Attach and Detach

To create a new tmux session, enter *tmux new -s text-editor* in your terminal. This will create a session named *text-editor*. In a session, you can create multiple windows. An window can have multiple panes. To acheive this, you need to know about the prefix key.

The default prefix key is *ctrl+ b*, that is you should press the *ctrl* and *b* keys simultaneously. Hereafter, I will refer the *ctrl+b* combination  as *prefix*, so keep in mind.

### New Window

To create a new window in your current session, type *prefix c*, while will create a new window and the focus will be on the newly created window. To navigate between the windows, use *prefix p* aka previous window and *prefix n* aka next window.

### Panes

As I mentioned earlier, a single window can contain multiple panes. To create a vertical pane type *prefix %* (prefix key is *ctrl+b*). This will create a verical pane on the left side and the focus will be on the newly created window. To create a horizontal pane, type *prefix "*. If you find the key combination  hard to remember and type, later in this series, you will learn how to change the default keybindings.

### Attach and Detach

Now, we want our session continue to run in the background, so that we can later pick up where we left off. In tmux world, this is called **attach** and **detach**. To detach from our current session use *prefix d*. If you have detached from multiple sessions, you can see the list of all the sessions you have detached by entering *tmux ls* in your terminal. If you have a single session enter *tmux attach* in your terminal, you will be present at where you left off. Incase of more than one sessions listed in the output of *tmux ls*, you need to specify the session name like this *tmux attach -t [session_name]*.

Additionally, if you don't want any of the sessionns to persist in the background, you can kill them using this command *tmux kill-session -t [session_name]*.

### Configuring tmux

As of now, you know how to create a new session, detach from a session, attach to a session, creating multiple windows and panes, it's time to configure tmux especially the keybindings, which you may find hard. Tmux looks for a configuration file in your home directory, so we need to create this file first.

```sh
touch ~/.tmux.conf
```    

#### Prefix key

The default prefix key is *ctrl+b*. To change this default binding, see the snippet below, where I am setting it to *ctrl+a*.

```sh
set -g prefix C-a
unbind C-b
```
Exit out of tmux and enter tmux again, to see the new prefix key in action.

#### Reload file

Everytime you make changes to your config file, you need to reload it to see the effect. The below snippet does this. If you type *prefix r*, it will the reload the config and display the message "reloaded!" at the status bar.

```sh
bind r source-file ~/.tmux.conf \; "reloaded!"
```

#### Resizing panes

```sh
bind -r H resize-pane -L 5
bind -r L resize-pane -R 5
bind -r J resize-pane -D 5
bind -r K resize-pane -U 5
```

Reload the file. Now, you can resize the panes using *prefix* and H,J,K,L keys. For example, if you have two horizontal panes and the focus is on the top pane, you can increase its size by pressing *prefix J*. H for left, L for right, J for down and K for up.

#### Splitting panes

```sh
bind | split-window -h
bind - split-window -v
unbind '""'
unbind %
```

After reloading, you can use *prefix |* for vertical split and *prefix -* for horizontal split.

#### Navigating panes and windows

```sh
# for panes
bind -n M-Left  select-pane -L
bind -n M-Right select-pane -R
bind -n M-Up    select-pane -U
bind -n M-Down  select-pane -D

# for windows
bind-key -n C-Right select-window -n
bind-key -n C-Left  select-window -p
```
After reloading the file, you can navigate between windows using **Ctrl+Left** and **Ctrl+Right** keybindings. For panes, use the **Left Alt** key with **ARROW KEYS**.


