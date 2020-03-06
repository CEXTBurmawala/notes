## Tmux

Tmux is a terminal multiplexer that allows connecting to a remote server using SSH in a detached mode. The detached mode add protection against losing connection with the remote server as one can reattach and continue the session.

Cheatsheet: <https://tmuxcheatsheet.com/>

#### To run tmux
- Run `tmux new -s <project_name>` to start a new session

#### To detach tmux
- Run `tmux detach` to detach from the session

#### To kill a session
- run `tmux kill-ses -t <session_name>` to kill the session
- run `tmux kill-session -a` to kill all but current session


#### To re-attach to a tmux session
- Run `tmux ls`
- run `tmux a -t <session_name>`


***
[Table of Contents](../README.md)