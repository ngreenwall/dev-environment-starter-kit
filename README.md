# Dev Environment Starter Kit

A beginner-friendly, step-by-step guide to setting up the core developer tools on a Mac: [Homebrew](https://brew.sh) (a package manager for installing software from the terminal), [fnm](https://github.com/Schniz/fnm), [Node.js](https://nodejs.org) and npm (for running and managing JavaScript tools), [Python](https://www.python.org), and [Claude Code](https://claude.com/claude-code) (Anthropic's AI coding agent).

This is a companion to [Terminal Starter Kit](https://github.com/ngreenwall/terminal-starter-kit), which covers making your terminal *look* better (a nicer app and color theme). This repo covers what to actually *install* once you're in there.

New to the terminal entirely? Start with [Terminal Starter Kit's terminal guide](https://github.com/ngreenwall/terminal-starter-kit/blob/main/docs/using-the-terminal.md) first, then come back here.

## How these pieces fit together

It helps to know what each tool actually does before installing it, so here's the short version:

- **Homebrew** is how you install software from the terminal instead of downloading installers from websites. You'll use it to install almost everything else in this guide.
- **fnm** manages *versions* of Node.js. Different projects sometimes need different Node versions, so instead of installing Node directly, you install fnm and let it install and switch Node versions for you.
- **Node.js** is a program that lets JavaScript code run outside of a web browser. A lot of developer tools (including Claude Code) are built with it, so you need Node installed to run them.
- **npm** ("Node Package Manager") comes bundled with Node.js. It installs individual JavaScript tools and libraries, including Claude Code itself.
- **Python** is a separate programming language, installed here mainly because some developer tools expect it to be available.

Think of it as layers: Homebrew installs fnm → fnm installs Node.js → npm (which comes with Node) installs individual tools like Claude Code.

## Before you start

Every step below tells you exactly what to type and what it does. You don't need to understand everything up front, just follow the steps in order, since later ones depend on earlier ones.

## Step 1: Xcode Command Line Tools

This is a small, free package from Apple that includes Git and other basic tools that Homebrew depends on. You don't need the full Xcode app, just this piece of it.

```shell
xcode-select --install
```

*What this does:* triggers a system popup asking you to install. Click **Install** and wait a few minutes for it to finish.

**Verify it worked:**

```shell
xcode-select -p
```

*What this does:* prints a folder path (something like `/Library/Developer/CommandLineTools`) if the install succeeded. If you see a path, move on to Step 2.

## Step 2: Homebrew

Homebrew is a package manager, meaning it lets you install and update software with a single command instead of hunting for downloads and installers.

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

*What this does:* runs Homebrew's official installer. It checks for Xcode Command Line Tools (installing them if you skipped Step 1), downloads Homebrew, and may tell you to run a couple of extra setup lines afterward, especially on newer Macs with Apple Silicon chips. If it does, copy and run exactly what it shows you.

Once it finishes, double check everything is healthy:

```shell
brew doctor
```

*What this does:* checks your Homebrew install for common problems and tells you if anything needs fixing.

**Verify it worked:**

```shell
brew --version
```

*What this does:* prints the installed Homebrew version.

From here on, installing most software is just:

```shell
brew install <name-of-thing>
```

You'll use this exact pattern in the next two steps.

## Step 3: fnm, Node.js, and npm

**Install fnm:**

```shell
brew install fnm
```

*What this does:* downloads and installs fnm using Homebrew.

**Back up your `.zshrc` file (optional, but recommended).** `.zshrc` is a hidden config file that runs every time you open a new terminal window. If you already have one, save a copy first so you can undo this later:

```shell
cp ~/.zshrc ~/.zshrc.backup
```

*What this does:* makes a copy named `.zshrc.backup` in your home folder.

**Add fnm to your shell.** Open your `.zshrc` file:

```shell
open -e ~/.zshrc
```

*What this does:* opens the file in TextEdit. If the file doesn't exist yet, this command creates it.

Copy the "REQUIRED: fnm" block from this repo's `zsh/dev-tools-additions.txt` and paste it into the file. Save, then reload your terminal:

```shell
source ~/.zshrc
```

*What this does:* applies the change to your current terminal window immediately, instead of waiting for you to open a new one.

**Install Node.js (via fnm):**

```shell
fnm install --lts && fnm default lts-latest && fnm use default
```

*What this does:* installs the latest LTS ("Long Term Support," the stable version recommended for most work) release of Node.js, and sets it as the default for both new and current terminal windows.

> [!IMPORTANT]
> Don't install Node.js by downloading it directly from nodejs.org. That installer puts Node in a system folder that only your Mac's admin account owns, meaning you'd need to type `sudo` (admin permission) every time you install a tool with npm later. Installing through fnm keeps everything inside your own user folder, so you never need `sudo` for this.

**Verify it worked:**

```shell
node -v && npm -v
```

*What this does:* prints the installed Node.js and npm version numbers. If you see version numbers instead of a "command not found" error, you're set.

## Step 4: Python

Some developer tools expect Python to be available on your Mac, so it's worth installing a current version even if you're not writing Python code yourself.

```shell
brew install python
```

*What this does:* downloads and installs Python using Homebrew.

**Verify it worked:**

```shell
python3 --version
```

*What this does:* prints the installed Python version.

## Step 5: Claude Code

Claude Code is Anthropic's AI coding agent that runs in your terminal. It can read, edit, and create files, run commands, and work across an entire project, all from a chat-style interface. It needs Node.js and npm, which is why it's the last step.

```shell
npm install -g @anthropic-ai/claude-code
```

*What this does:* downloads and installs Claude Code globally, meaning it's available from any folder in your terminal, not just one project.

**Verify it worked:**

```shell
claude --version
```

*What this does:* prints the installed Claude Code version.

**Log in:**

```shell
claude
```

*What this does:* starts Claude Code. The first time you run it, it'll walk you through logging in with your Anthropic account.

## Optional / advanced: npm Global Toolbox

Skip this section unless you run into the specific problem it solves, or you're curious.

**The problem:** each version of Node.js you install with fnm is its own separate space. If you install a tool globally with `npm install -g` and later switch to a different Node version with fnm, that tool won't be there anymore, since it was installed into the old version's space, not your Mac in general.

**The fix:** point npm's global installs at one fixed folder that never moves, no matter what Node version you're using.

**1. Create the folder:**

```shell
mkdir -p ~/.npm-global
```

*What this does:* creates a new, empty folder at `~/.npm-global` to hold your global tools.

**2. Point npm at it:**

```shell
npm config set prefix '~/.npm-global'
```

*What this does:* tells npm to install anything you install with `-g` (global) into this new folder from now on, instead of the current Node version's own folder.

**3. Update your `.zshrc`.** Open it the same way as Step 3, then copy the "OPTIONAL / ADVANCED: npm Global Toolbox" block from `zsh/dev-tools-additions.txt` into the file, below the fnm line. Save, then reload:

```shell
source ~/.zshrc
```

**4. Reinstall your global tools one last time**, including Claude Code:

```shell
npm install -g @anthropic-ai/claude-code
```

*What this does:* reinstalls Claude Code into the new fixed folder. From now on, it (and anything else you install with `-g`) will survive switching Node versions.

## Undo / start over

- **`.zshrc` changes:** delete `~/.zshrc`, then rename `~/.zshrc.backup` (from Step 3) back to `~/.zshrc`. Run `source ~/.zshrc` or open a new terminal window.
- **fnm and Node.js:** `brew uninstall fnm`. This removes fnm and stops managing Node versions; any Node versions it installed will no longer be reachable through the `fnm` or `node` command until you reinstall.
- **Claude Code:** `npm uninstall -g @anthropic-ai/claude-code`.
- **Python:** `brew uninstall python`.
- **Homebrew itself:** Homebrew has an official uninstall script at [github.com/Homebrew/install](https://github.com/Homebrew/install#uninstall-homebrew). Removing Homebrew also removes anything you installed with it (fnm, Python, etc.), so only do this if you want to start completely fresh.

## Troubleshooting

- **"command not found" right after installing something:** you likely need to reload your terminal. Run `source ~/.zshrc`, or close and reopen your terminal window.
- **You see the same line twice in `.zshrc`:** reinstalling a tool can sometimes add its setup line again instead of noticing it's already there. Open `~/.zshrc`, delete the duplicate, save, and run `source ~/.zshrc`.
- **Global npm updates fail with a strange file error:** a leftover `.DS_Store` file (a hidden macOS folder-settings file) can end up where npm doesn't expect it. Fix it with:

  ```shell
  find $(npm root -g) -name ".DS_Store" -delete
  ```

  *What this does:* finds and removes any `.DS_Store` files inside your global npm folder.

- **You get warnings about "unapproved install scripts" when updating (Optional / Advanced section only):** this is a known rough edge with npm's newer security features when using a custom global install folder like `~/.npm-global`. Your tools still work fine, this warning is just npm's approval-tracking getting confused by the non-standard folder location. Not worth reversing your setup over.

## Why "starter"

These steps are a foundation, not the only way to do this. Once you're comfortable, feel free to explore other tools and adjust anything here to fit how you work.
