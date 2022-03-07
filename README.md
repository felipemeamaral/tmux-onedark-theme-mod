# tmux-onedark-theme-mod
A dark tmux color scheme for terminal that support [True Color](https://en.wikipedia.org/wiki/Color_depth#True_color_.2824-bit.29), based on [onedark.vim](https://github.com/joshdick/onedark.vim), which is inspired by [One Dark syntax theme](https://github.com/atom/one-dark-syntax) for the [Atom text editor](https://atom.io).

## Why?

I wanted both vim and tmux to share the same color scheme.
I tried [tmuxline.vim](https://github.com/edkolev/tmuxline.vim) but it didn't render the colors correctly.
Furthermore, with `tmuxline.vim`, you can't control the widgets on right status bar, which is a key feature IMO.

A picture of my terminal with *@onedark_widgets*.  
These widgets are available in [tmux-status-variables](https://github.com/odedlaz/tmux-status-variables).
![tmux-onedark-theme Preview](https://github.com/felipemeamaral/tmux-onedark-theme-mod/raw/main/preview-terminal.png)

### Set Options

**!** Set the following options in your `.tmux.conf`

#### widgets

Widgets can be controlled by setting `@onedark_widgets`, for example:

```
set -g @onedark_widgets "#(date +%s)"
```

Once set, these widgets will show on the right.

**default**: empty string.

#### Time format

Time format can be controlled by setting `@onedark_time_format`, for example:

```
set -g @onedark_time_format "%I:%M %p"
```

`%I` - The hour as a decimal number using a 12-hour clock  
`%M` - The minute as a decimal number  
`%p` -  Either "AM" or "PM" according to the given time value.

**default**: `%R` - The time in 24-hour notation (%H:%M).

These modifiers were taken from from [strftime manpage](http://man7.org/linux/man-pages/man3/strftime.3.html).

#### Date format

Date format can be controlled by setting `@onedark_date_format`, for example:

```
set -g @onedark_date_format "%D"
```

`%D` - Equivalent to %m/%d/%y (American format).   
`%m` - The month as a decimal number.  
`%d` - The day of the month as a decimal number  
`%y` - The year as a decimal number without the century.  

**default**: `%d/%m/%Y` - The date in non-American format.

These modifiers were taken from from [strftime manpage](http://man7.org/linux/man-pages/man3/strftime.3.html).

### Installation with [Tmux Plugin Manager](https://github.com/tmux-plugins/tpm) (recommended)

Add plugin to the list of TPM plugins in `.tmux.conf`:

```
set -g @plugin 'odedlaz/tmux-onedark-theme'
```

Hit `prefix + I` to fetch the plugin and source it.

### Manual Installation

Clone the repo:

```
$ git clone https://github.com/felipemeamaral/tmux-onedark-theme-mod /a/path/you/choose
```

Add this line to the bottom of `.tmux.conf`:

```
run-shell /a/path/you/choose tmux-onedark-theme.tmux
```

Reload TMUX environment (type this in terminal)
```
$ tmux source-file ~/.tmux.conf
```
## Additional steps  
### ZSH Shell
To setup the terminal just like mine, you have to install ZSH using your package manager.  
**macOS**  
```
brew install zsh
```
And set it as your default terminal with these two commands:
```
chsh -s $(which zsh)
sudo chsh -s $(which zsh)
```

### Powerlevel10k
It is required to install powerlevel10k as well if you want the same theme on your terminal.
Follow [these instructions](https://github.com/romkatv/powerlevel10k) to install it on your system.

### Autostart
To autostart tmux on every new terminal, just add this command as your runing command:  
```
tmux new -As 0
```

### Visual Studio Code
To integrate the theme into Visual Studio Code install One Dark Pro theme from [here](https://marketplace.visualstudio.com/items?itemName=zhuangtongfa.Material-theme).
And set it as your terminal adding these lines to your `settings.json` file:  
```json
"terminal.integrated.profiles.osx": {
    "zsh": {
      "path": "zsh",
      "args": [
        "-i",
        "-c",
        "tmux new -As 0"
      ]
    },
  },
```
And set it as your terminal adding these lines to your `settings.json` file:  

### tmux.conf example file
```
#List of plugins
set -g @plugin "tmux-plugins/tpm"
set -g @plugin "tmux-plugins/tmux-sensible"
set -g @plugin "felipemeamaral/tmux-onedark-theme-mod"
set -g @plugin "tmux-plugins/tmux-resurrect"
set -g @plugin "tmux-plugins/tmux-continuum"

set -g default-terminal "screen-256color"

# Enable tmux continuum
set -g @continuum-restore "on"

# Scroll History
set -g history-limit 10000

# Set ability to capture on start and restore on exit window data when running an application
setw -g alternate-screen on

# Lower escape timing from 500ms to 50ms for quicker response to scroll-buffer access.
set -s escape-time 50

setw -q -g utf8 on
unbind C-b
set -g prefix C-a
set -g base-index 1
set -g pane-base-index 1
set-option -g history-limit 10000
bind -n S-Right next-window
bind -n S-Left previous-window
bind -n M-Left select-pane -L
bind -n M-Right select-pane -R
bind -n M-Up select-pane -U
bind -n M-Down select-pane -D
bind r source-file ~/.tmux.conf \; display "~/.tmux.conf sourced"
bind C-f command-prompt -p find-session "switch-client -t %%"
setw -g monitor-activity on
set -g visual-activity on
bind | split-window -h
bind _ split-window -v
setw -g automatic-rename on   # rename window to reflect current program
set -g renumber-windows on    # renumber windows when a window is closed
set -g status-interval 10     # redraw status line every 10 seconds

new -As 0

# Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf)
run "~/.tmux/plugins/tpm/tpm"
```
With these you can navigate through windows using `Alt + Arrow` keys.  
Navigate between tabs using Shift + `Left/Right` arrow keys.  
Split horizontally/vertically using `Ctrl + A, |` or `Ctrl + A _`.  
Resize windows using `Ctrl + A, Alt + Arrow` keys.  
Reload tmux config file using `Ctrl + A, R`.  
Install new packages after modifying `.tmux.conf` file using `Ctrl + A, I`.  

## Issues

### Symbols are missing

   The theme requires Nerd Fonts symbols exist and set on your system. Follow [these instructions](https://github.com/ryanoasis/nerd-fonts) to install them, then update your terminal fonts to use them.

### Symbols are corrupted

   Patched Powerline fonts aren't picked up when `$LANG` isn't set to `en_US`.  
   You can change the default locale settings at `/etc/default/locale`.

   
### Widgets not working

   Make sure that you put the `set -g @plugin 'felipemeamaral/tmux-onedark-theme-mod'` before other scripts that alter the status line, or they won't be able to pickup the plugin's changes.

### True Color

   tmux version <= 2.3, don't support true color in the status line.
   [Support has been added](https://github.com/tmux/tmux/issues/490), and will probably ship in the next release.
   You can compile tmux and enjoy True Color right away!

   Make sure TrueColor is enabled and working. follow [these instructions](https://sunaku.github.io/tmux-24bit-color.html#usage) to do so.

### License

[MIT](LICENSE)
