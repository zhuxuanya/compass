# Tmux Usage

Tmux is a terminal multiplexer to manage multiple terminal sessions within a single window. 

## Install Tmux

To install tmux in different os. For example:

```sh
# Ubuntu
sudo apt-get install tmux
# CentOS
sudo yum install tmux
# macOS
brew install tmux
```

## Create a Tmux Session

To start a new tmux session named `mysession`, use the following command:

```sh
tmux new -s mysession
```

## Detach and Attach to Sessions

To detach from a session (keep it running in the background) by pressing:

```sh
Ctrl-b d
```

To list all existing sessions, use:

```sh
tmux ls
```

To reattach to a session, use:

```sh
tmux a -t mysession
```

## Basic Tmux Shortcuts

Here are some essential tmux shortcuts to manage sessions, windows, and panes:

### Create and Manage Windows

Create a new window:
```sh
Ctrl-b c
```

Switch to the next window:
```sh
Ctrl-b n
```

Switch to the previous window:
```sh
Ctrl-b p
```

### Split Windows into Panes

Split the window vertically:
```sh
Ctrl-b %
```

Split the window horizontally:
```sh
Ctrl-b "
```

### Navigate Panes

Switch to the next pane:
```sh
Ctrl-b o
```

Switch to a specific pane (using arrow keys):
```sh
Ctrl-b <arrow key>
```

### Scroll and Copy Mode

Enter copy mode to scroll up/down and copy:
```sh
Ctrl-b [
```

Exit copy mode:
```sh
q
```

### Exit Tmux

Detach from the session:
```sh
Ctrl-b d
```

Kill the current pane:
```sh
Ctrl-b x
```

Kill the current window:
```sh
Ctrl-b &
```

## Conclusion

These are the basic commands to start with tmux. It is a powerful tool that can significantly improve the terminal workflow. For more advanced usage, refer to the [tmux wiki](https://github.com/tmux/tmux/wiki).
