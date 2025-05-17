# Terminal Management

## iTerm2 (macOS)

split window vertically:

    Command + D (⌘D)

split window horizontally:

    Command + Shift + D (⌘⇧D)

navigate between panes:

    Command + Option + Arrow keys (⌘⌥←↑→↓)

maximize pane:

    Command + Shift + Enter (⌘⇧Enter)

close pane:

    Command + W (⌘W)

## screen (Unix/Linux terminal multiplexer)

start a new screen session:

    screen

split window horizontally:

    Ctrl + a S

split window vertically:

    Ctrl + a |

move between windows:

    Ctrl + a Tab

close current window:

    Ctrl + a X

detach from session:

    Ctrl + a d

reattach to session:

    screen -r

list sessions:

    screen -ls

create named session:

    screen -S session_name

## tmux (modern terminal multiplexer)

start a new session:

    tmux

split window horizontally:

    Ctrl + b "

split window vertically:

    Ctrl + b %

navigate between panes:

    Ctrl + b Arrow keys

close current pane:

    Ctrl + b x

detach from session:

    Ctrl + b d

list sessions:

    tmux ls

attach to session:

    tmux attach -t session_name

create named session:

    tmux new -s session_name

resize pane:

    Ctrl + b Ctrl + Arrow keys

toggle pane zoom:

    Ctrl + b z

# Terminal Line Editing Shortcuts

## Cursor Movement

move to start of line:

    Ctrl+A

move to end of line:

    Ctrl+E

move forward one word:

    Alt+F (or Esc+F)

move backward one word:

    Alt+B (or Esc+B)

## Deletion

delete character under cursor:

    Ctrl+D

delete from cursor to end of line:

    Ctrl+K

delete from cursor to start of line:

    Ctrl+U

delete word before cursor:

    Ctrl+W

delete word after cursor:

    Alt+D (or Esc+D)

## Other Editing

clear screen (keep current line):

    Ctrl+L

transpose characters:

    Ctrl+T

undo:

    Ctrl+_ (or Ctrl+X followed by Ctrl+U)

paste from kill ring (what was deleted):

    Ctrl+Y

paste previous deletion from kill ring:

    Alt+Y (after Ctrl+Y)

capitalize word:

    Alt+C

uppercase word:

    Alt+U

lowercase word:

    Alt+L

## History

previous command in history:

    Ctrl+P (or Up Arrow)

next command in history:

    Ctrl+N (or Down Arrow)

search history backward:

    Ctrl+R

search history forward:

    Ctrl+S (may be captured by terminal)

abort current edit:

    Ctrl+C
