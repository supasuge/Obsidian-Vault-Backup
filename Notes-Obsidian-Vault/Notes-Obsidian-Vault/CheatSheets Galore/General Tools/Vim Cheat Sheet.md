## Table of Contents

  - [Table of Contents](#Table\of\Contents)
- [Vim Cheat Sheet](#vim\cheat\sheet)
    - [Vim Modes](#Vim\Modes)
    - [Basic Commands](#Basic\Commands)

## Table of Contents

- [Vim Cheat Sheet](#vim\cheat\sheet)
    - [Vim Modes](#Vim\Modes)
    - [Basic Commands](#Basic\Commands)

# Vim Cheat Sheet
### Vim Modes
1. **Normal Mode**: This is the default mode when you open a file with Vim. Here you can navigate and use various commands.
2. **Insert Mode**: This mode allows you to insert text. You can enter this mode from the normal mode by pressing various keys.
3. **Visual Mode**: You can select text with this mode.
4. **Command-Line Mode**: Here you can enter Vim commands.

### Basic Commands
#insertmode #insert
1. **Entering Insert Mode**
   - `i`: Insert before the cursor.
   - `I`: Insert at the beginning of the line.
   - `a`: Append after the cursor.
   - `A`: Append at the end of the line.
   - `o`: Open (append) blank line below current line.
   - `O`: Open blank line above current line.
#insert mode
2. **Exiting Insert Mode**
   - `Esc`: Return to Normal mode.
#saving #quitting #savingandquitting
3. **Saving and Quitting**
   - `:w`: Save the changes.
   - `:q`: Quit without saving.
   - `:wq` or `:x`: Save and quit.
   - `ZZ`: Save and quit (from Normal mode).
   - `:q!`: Quit without saving, discarding changes.
#movement
4. **Movement**
   - `h`: Move left.
   - `j`: Move down.
   - `k`: Move up.
   - `l`: Move right.
   - `w`: Move to start of next word.
   - `b`: Move to start of previous word.
   - `^`: Move to the first non-blank character of the line.
   - `$`: Move to the end of the line.
   - `G`: Move to the end of the file.
   - `gg`: Move to the beginning of the file.
   - `123G`: Move to the 123rd line.
#searchregex
5. **Searching**
   - `/pattern`: Search for a pattern in the file. Press `n` to go to the next occurrence and `N` for the previous one.
#replace
6. **Replacing**
   - `:s/old/new`: Replace first occurrence of 'old' with 'new' in the current line.
   - `:s/old/new/g`: Replace all occurrences of 'old' with 'new' in the current line.
   - `:%s/old/new/g`: Replace all occurrences of 'old' with 'new' in the entire file.
#undo #redo #vim
7. **Undo and Redo**
   - `u`: Undo the last operation.
   - `Ctrl + r`: Redo.
#Copy #paste
8. **Copy, Cut, and Paste**
   - `yy`: Yank (copy) a line.
   - `2yy`: Yank 2 lines.
   - `dd`: Delete (cut) a line.
   - `2dd`: Delete 2 lines.
   - `p`: Put (paste) after the cursor.
   - `P`: Put (paste) before the cursor.
#VisualMode
9. **Visual Mode**
   - `v`: Start visual mode, then move cursor to select text.
   - `V`: Start linewise visual mode.
   - `y`: Yank (copy) selected text.
   - `d`: Delete (cut) selected text.
#commandline
10. **Command-Line Commands**
   - `:set number`: Display line numbers.
   - `:set nonumber`: Hide line numbers.

This is a very basic introduction to Vim. The editor has a lot more to offer in terms of functionality and customization. Once you get familiar with these commands, you can explore plugins, configuration options, and more advanced commands to enhance your Vim experience.

Of course! Here's a more organized table of essential Vim commands:


#Vim
#commandchart
#chart
#vimhelp 

| Command | Description | How to Use |
|---------|-------------|------------|
| **Mode Switching** | | |
| `i` | Enter Insert Mode (before cursor) | Press `i` |
| `Esc` | Exit Insert Mode to Normal Mode | Press `Esc` |
| `v` | Enter Visual Mode | Press `v` |
| `:` | Enter Command-Line Mode | Press `:` |
| **Movement** | | |
| `h` | Move left | Press `h` |
| `j` | Move down | Press `j` |
| `k` | Move up | Press `k` |
| `l` | Move right | Press `l` |
| `w` | Start of next word | Press `w` |
| `b` | Start of previous word | Press `b` |
| `^` | Start of line | Press `^` |
| `$` | End of line | Press `$` |
| `gg` | Start of file | Press `gg` |
| `G` | End of file | Press `G` |
| **Editing** | | |
| `u` | Undo | Press `u` |
| `Ctrl + r` | Redo | Press `Ctrl` + `r` |
| `yy` | Yank (copy) line | Press `yy` |
| `dd` | Delete (cut) line | Press `dd` |
| `p` | Paste after cursor | Press `p` |
| **Searching & Replacing** | | |
| `/pattern` | Search for pattern | Press `/`, type pattern, then `Enter` |
| `:s/old/new` | Replace in line | Press `:`, type command, then `Enter` |
| **Saving & Exiting** | | |
| `:w` | Save | Press `:`, type `w`, then `Enter` |
| `:q` | Quit | Press `:`, type `q`, then `Enter` |
| `:wq` | Save & Quit | Press `:`, type `wq`, then `Enter` |
| `:q!` | Quit without saving | Press `:`, type `q!`, then `Enter` |
| **Settings** | | |
| `:set number` | Display line numbers | Press `:`, type command, then `Enter` |
| `:set nonumber` | Hide line numbers | Press `:`, type command, then `Enter` |
Certainly! Here's a more organized and detailed table of Vim commands, including mode-specific commands:

| Mode | Command | Description | How to Use |
|------|---------|-------------|------------|
| **Normal Mode** | | |
| | `i` | Enter Insert Mode (before cursor) | Press `i` |
| | `v` | Enter Visual Mode | Press `v` |
| | `:` | Enter Command-Line Mode | Press `:` |
| | `h` | Move left | Press `h` |
| | `j` | Move down | Press `j` |
| | `k` | Move up | Press `k` |
| | `l` | Move right | Press `l` |
| | `w` | Start of next word | Press `w` |
| | `b` | Start of previous word | Press `b` |
| | `^` | Start of line | Press `^` |
| | `$` | End of line | Press `$` |
| | `gg` | Start of file | Press `gg` |
| | `G` | End of file | Press `G` |
| | `u` | Undo | Press `u` |
| | `Ctrl + r` | Redo | Press `Ctrl` + `r` |
| | `yy` | Yank (copy) line | Press `yy` |
| | `dd` | Delete (cut) line | Press `dd` |
| | `p` | Paste after cursor | Press `p` |
| **Visual Mode** | | |
| | `y` | Yank (copy) selected text | Press `y` |
| | `d` | Delete (cut) selected text | Press `d` |
| | `>` | Indent selected text right | Press `>` |
| | `<` | Indent selected text left | Press `<` |
| **Command-Line Mode** | | |
| | `/pattern` | Search for pattern | Type `/`, pattern, then `Enter` |
| | `:s/old/new` | Replace in line | Type `:s/old/new` then `Enter` |
| | `:w` | Save | Type `:w` and `Enter` |
| | `:q` | Quit | Type `:q` and `Enter` |
| | `:wq` | Save & Quit | Type `:wq` and `Enter` |
| | `:q!` | Quit without saving | Type `:q!` and `Enter` |
| | `:set number` | Display line numbers | Type `:set number` and `Enter` |
| | `:set nonumber` | Hide line numbers | Type `:set nonumber` and `Enter` |

This table is mode-specific, so it's important to understand which mode you are in when using Vim. Remember, the `Esc` key will always take you back to Normal mode from any other mode.
