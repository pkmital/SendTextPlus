# SendText+ for Sublime Text

This package improves [SendText](https://github.com/wch/SendText), particularly for `r`, `python` and `julia` syntaxes (Note: [IPython](https://ipython.org) is assumed for python codes.). It supports

- Mac: Terminal, iTerm, RStudio(>0.99.769), R GUI, and Jupyter running on Chrome and Safari
- Linux: screen and tmux
- Windows: Cmder (see below to configure Cmder), Cygwin, R32, R64 and RStudio
- SublimeREPL for R and python syntaxes (it works better with R)

Terminal is the default for Mac, Cmder for Windows and tmux for Linux. Use the command `SendText+: Choose Program` in command palatte to quickly change the active program.



### Installation

Via Package Control.

### Usage

- <kbd>cmd</kbd>+<kbd>enter</kbd> (Mac) or <kbd>ctrl</kbd>+<kbd>enter</kbd> (Windows/Linux)

    If text is selected, it sends the text to the program selected. If no text is selected, then it sends the current block (if found). Finally, it moves the cursor to the next line.


- <kbd>cmd</kbd>+<kbd>\\</kbd> (Mac) or <kbd>ctrl</kbd>+<kbd>\\</kbd> (Windows/Linux): change working directory (R, Julia and Python only)


- <kbd>cmd</kbd>+<kbd>b</kbd> (Mac) or <kbd>ctrl</kbd>+<kbd>b</kbd> (Windows/Linux): source current file (R, Julia and Python only)

    SendTextPlus uses Sublime build system to source files, you might have to choose the `SendTextPlus` build system before pressing the keys.


### Platform and syntax specific settings.

You can use different settings for different platforms and syntaxes by editing the default settings at `Preferences` -> `Package Settings` -> `SendText+`. It understands platform and syntax specifications. The list of supported programs are

- Mac: Terminal, iTerm, R, RStudio, Chrome-RStudio, Chrome-Jupyter, Safari-RStudio, Safari-Jupyter
- Liniux: tmux, screen,
- Windows: Cmder, Cygwin, R64, R32, RStudio
- or SublimeREPL

An example:

```json
{
    "defaults" : [
        {
            "platform": "osx",
            "scopes": ["source.r"],
            "prog": "RStudio"
        },
        {
            "platform": "osx",
            "scopes": ["source.python", "source.julia"],
            "prog": "Chrome-Jupyter"
        },
        {
            "platform": "osx",
            "prog": "Terminal"
        },
        {
            "platform": "windows",
            "prog": "Cmder"
        },
        {
            "platform": "linux",
            "prog": "tmux"
        }
    ]
}
```
Make sure [Defaults] is selected in `SendText+: Choose Program`.

### Custom Keybind

It is fairly easy to create your own keybinds for commands which you frequently use. For example, the following keybind run the `R` command `source("<the current file>")` in the active program.
SendTextPlus understands the following variables in the `cmd` field: 

- `$file`, the full path to the file
- `$file_path`, the directory contains the file
- `$file_name`, the file name
- `$file_basename`, the file name without extension
- `$file_extension`, the file extension
- `$project_path`, the active folder, if not found, use the directory of current file

```json
    {
        "keys": ["super+shift+a"], "command": "send_text_plus",
        "args": {"cmd": "source(\"$file\")"},
        "context": [
            { "key": "selector", "operator": "equal", "operand": "source.r" }
        ]
    },
```

### Cmder settings

- Go to `Paste` in the settings, uncheck, "Confirm <enter> keypress" and "Confirm pasting more than..."
- Change the default paste all lines command from <kbd>shift</kbd>+<kbd>insert</kbd> to <kbd>ctrl</kbd>+<kbd>shift</kbd>+<kbd>v</kbd>.  I actually posted an [issue](https://github.com/cmderdev/cmder/issues/710) at Cmder about the default keybind.


### Some details about block detection

- R blocks are detected by a regular expression for  `{`,`}` pairs. 
- Julia blocks are detected by `begin`, `end` pairs and indentations. 
- Python blocks are detected by indentations or by `# %%`/`# In[]` decorators.
