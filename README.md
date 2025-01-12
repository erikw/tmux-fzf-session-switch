↖️ (Feeling lost? Use the GitHub TOC!)

# TMUX FZF Session Switch

![image_2021-11-30-17-22-26](img/image_2021-11-30-17-22-26.png)

## Install

- Install the [tpm](https://github.com/tmux-plugins/tpm) Tmux Plugin Manager.
- Put `set -g @plugin 'thuanOwa/tmux-fzf-session-switch'` into your tmux config
- Use tpm to install this plugin. Default you can press `prefix + I` (`I` is
  `shift + i` = I)
- Finally activate the session switcher: `prefix` followed by `C-f` (control +
  f)

## Customize

> 🫰Thanks to [@erikw](https://github.com/erikw)

### Key binding

```bash
set -g @fzf-goto-session 'key binding'
```

> Eg. to override the default session switcher in tmux available at `prefix` + s`:

```bash
set -g @fzf-goto-session 's'
```

### Window dimensions

```bash
set -g @fzf-goto-win-width WIDTH
set -g @fzf-goto-win-height HEIGHT
```

> Eg.

```bash
set -g @fzf-goto-win-width 70
set -g @fzf-goto-win-height 20
```

## Functionality

- `Prefix +`: Open up fzf in a new tab. (e.g. prefix = ctrl + a. Hold ctrl ->
  press a -> press f -> done)
- If you type a name that doesn't exist, you will be prompted to create it.

## Requirements

- [fzf](https://github.com/junegunn/fzf)
- Rg (recommended but not required)

## Customization from me

- Work with session name has `space` character. e.g. "Thuan Pham is handsome"
- Don't confirm `y` to create a new session, I just lazy press 2 times `Enter` to create a new session.
- Pop-up window instead of the new window (required tmux >= v3.2)

## Tips

### Use in command line

```bash
function tmuxSessionSwitch() {
  local session
  session=$(tmux list-sessions -F "#{session_name}" | fzfDown)
  tmux switch-client -t "$session"
}
alias af='tmuxSessionSwitch'
```

> fzfDown is my customize fzf ui, you can simply use fzf instead of fzfDown

```bash
fzfDown() { fzf --height 50% --min-height 20 --bind ctrl-/:toggle-preview "$@" --reverse }
```

```bash
function killAllUnnameTmuxSession() {
  echo "kill all unname tmux session"
  cd /tmp/
  tmux ls | awk '{print $1}' | grep -o '[0-9]\+' >/tmp/killAllUnnameTmuxSessionOutput.sh
  sed -i 's/^/tmux kill-session -t /' killAllUnnameTmuxSessionOutput.sh
  chmod +x killAllUnnameTmuxSessionOutput.sh
  ./killAllUnnameTmuxSessionOutput.sh
  cd -
  tmux ls
}
```

> use with `clear` command is the best

```
alias clear='killAllUnnameTmuxSession ; clear -x'
```

### Easy to press

- In my use case, I don't use this keybinding for switch sessions, I use `hold space + ;` mapping for `hold Ctrl + a + f`
- How can I use `hold space + ;` mapping?
  -> I use [input remapper](https://github.com/sezanzeb/input-remapper), also you can see [my dotfiles](https://github.com/thuanOwa/dotfiles)

> config in GUI

```python
space: if_single(key(KEY_SPACE), ,timeout=10000)
space + semicolon: KEY_RIGHTCTRL+a+f
```

![input remapper][img_input_remapper]

[img_input_remapper]: ./img/input_remapper.png
