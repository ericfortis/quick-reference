# tmux

Runs commands in tmux's vertical panes

```shell
tmux new-session -s mysession -d # Detached Session
tmux send-keys "ls -la" Enter 

tmux split-window -v
tmux send-keys "ls" Enter

tmux attach -t mysession
```