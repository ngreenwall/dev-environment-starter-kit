# Customizing `.zshrc`

The main [README](../README.md) has you add one required line to `~/.zshrc` (for fnm) and an optional block (for the npm Global Toolbox). This guide goes a little further: a small, well-explained starter config for `.zshrc` itself, the file that runs every time you open a new terminal window.

You don't need any of this to finish the main setup. Treat it as an upgrade once you're comfortable.

## What `.zshrc` is, again

`.zshrc` is a hidden config file in your home folder. Every time you open a new terminal window, your Mac reads this file top to bottom and runs whatever's in it. That's how tools like Homebrew and fnm "hook into" your terminal, and it's also where you can add your own shortcuts and preferences.

You can see hidden files like this one in Finder by pressing `Cmd+Shift+.` while your home folder is open.

**After any edit, apply the change to your current window:**

```shell
source ~/.zshrc
```

*What this does:* re-reads the file without needing to close and reopen your terminal. Skip this and your changes only show up in new windows.

## A minimum useful `.zshrc`

This is the smallest version worth having: sensible defaults, command history that doesn't disappear, tab completion, and a prompt that shows you where you are. Open your file with `open -e ~/.zshrc`, then add these blocks (skip any you already have from earlier steps, like the fnm line):

```shell
# --- ENVIRONMENT ---
# LANG sets your language and character encoding.
# UTF-8 ensures emoji, accented characters, and symbols display correctly.
export LANG=en_US.UTF-8


# --- HOMEBREW ---
# Makes sure Homebrew's tools are available in every terminal window.
[[ -x /opt/homebrew/bin/brew ]] && eval "$(/opt/homebrew/bin/brew shellenv)"


# --- HISTORY ---
# Your terminal remembers every command you've typed.
# HISTFILE is where that history is saved on disk.
# HISTSIZE is how many commands are kept in memory during a session.
# SAVEHIST is how many commands are written to the file when you close the terminal.
HISTFILE=~/.zsh_history
HISTSIZE=10000
SAVEHIST=10000

# SHARE_HISTORY syncs history across multiple terminal windows in real time.
# HIST_IGNORE_SPACE means any command you prefix with a space won't be saved,
# handy for one-off commands containing something sensitive like a password.
setopt SHARE_HISTORY
setopt HIST_IGNORE_SPACE


# --- TAB COMPLETION ---
# Press Tab and your terminal tries to finish the command, file name, or flag
# you're typing. The check below only rebuilds the completion cache (a saved
# copy of this data it reuses instead of regenerating from scratch every
# time) once a day, which keeps your terminal opening quickly.
autoload -Uz compinit
if [[ -n ~/.zcompdump(#qN.mh+24) ]]; then
	compinit
else
	compinit -C
fi


# --- ALIASES ---
# Aliases are shortcuts: type the short version, your terminal runs the full command.
alias upbrew='brew update && brew upgrade'


# --- PROMPT ---
# Controls what your terminal prompt looks like before your cursor.
# %~ = current folder (~ = home)
# %# = % for a normal user, # if you're root (the Mac's all-powerful admin account, rare to be logged in as)
# %F{n} sets text color to a numbered color code your terminal understands (an "ANSI color"), %f resets it back to default
PROMPT='%F{1}%~%f %F{7}%#%f '
```

That's genuinely enough for most people. Add more as you find you want it.

## Want more? Use the Recommended file instead

Once you're comfortable with the minimum version above, `zsh/recommended-zshrc.txt` in this repo is a complete, ready-to-use `.zshrc` that builds on it. It adds:

- **fnm**, already wired in if you followed the main [README](../README.md)
- A few handy **aliases**: `ll` (a detailed folder listing) and `gst` (`git status`)
- **Shell functions** you can just type by name: `upbrew` (update Homebrew and everything installed with it), `upnpm` (update npm and your global npm tools), `upnode` (install and switch to the latest Node LTS), and `checkup` (checks all three at once and tells you what's out of date)
- A fancier **prompt** that also shows your username

Copy the whole file into `~/.zshrc` (back up your existing one first, same as the main README's fnm step) rather than appending it on top of the minimum version above, so you don't end up with duplicate lines doing the same thing twice.

One thing to note: the minimum version above has `upbrew` as an alias that runs `brew update && brew upgrade`. The recommended file replaces it with a function of the same name that also upgrades GUI apps (`brew upgrade --cask`). Copying the recommended file changes what typing `upbrew` actually does, not just how it's defined.

## Aliases: shortcuts for commands you type a lot

An alias expands to a longer command when you hit Enter. If you chain multiple commands with `&&`, it stops running as soon as one of them fails.

```shell
# gst - shortcut for git status
alias gst='git status'
```

Naming convention: lowercase, either with a dash (`up-brew`) or run together (`upbrew`). Either works, just be consistent.

To remove an alias from your *current* terminal session without editing the file (useful if you renamed it and the old one is still loaded):

```shell
unalias gst
```

*What this does:* removes the `gst` shortcut for this terminal window only. `.zshrc` itself is untouched, so `gst` comes back the next time you open a new window.

## Troubleshooting

- **Changes not taking effect:** run `source ~/.zshrc`, or open a new terminal window.
- **"command not found" after installing a tool:** the tool installed fine, but its folder isn't in your `PATH` yet (the list of folders your terminal searches for commands). Check that tool's install instructions for a line to add here, then `source ~/.zshrc`.
- **Prompt shows odd characters or looks broken:** every `%F{n}` (start color) needs a matching `%f` (reset color) later in the same line.
- **Terminal takes a while to open:** something in `.zshrc` is slow, usually a network call or a lookup. If you added anything beyond this guide, that's the first place to look.
- **Syntax error when opening a new terminal:** run `zsh -n ~/.zshrc` to check the file for mistakes without actually running it. It'll point you to the problem line.

## A note for multiple Macs

`.zshrc` lives only on the machine it's on, changes don't sync automatically between computers. If you set up a second Mac, copy the file over or re-add these blocks by hand. Use `$HOME` instead of writing out `/Users/yourname/` directly, so the file works no matter which Mac it's on.

## Related

- [API Key Management](api-key-management.md) — adds a "Secrets" block to `.zshrc` for pulling tokens out of macOS Keychain
- [Main setup guide](../README.md)
