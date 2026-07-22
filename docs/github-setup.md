# GitHub Setup

Git is the tool that tracks changes to your code over time. GitHub is a website that stores a copy of that history online, so you can back it up, share it, and work on it from more than one computer. This guide covers getting both connected on a Mac.

## Do you already have Git?

If you've completed [Step 1 of the main guide](../README.md#step-1-xcode-command-line-tools) (Xcode Command Line Tools), you already have Git installed, it comes bundled in.

**Verify it worked:**

```shell
git --version
```

*What this does:* prints the installed Git version. If you see a version number, you're set and can skip to the next section. If you see a popup asking to install developer tools instead, click **Install**, wait a few minutes, then run this command again.

## Step 1: Create a GitHub account

Go to [github.com/join](https://github.com/join) and sign up. It's free for personal use. Pick a username, you'll see it in URLs and it's what others will use to find you, so something professional is worth it if you'll use this for work.

## Step 2: Install the GitHub CLI

The GitHub CLI (`gh`) is a tool that handles logging in and talking to GitHub from the terminal, so you don't have to manually set up separate keys or passwords.

```shell
brew install gh
```

*What this does:* downloads and installs the GitHub CLI using Homebrew.

**Verify it worked:**

```shell
gh --version
```

*What this does:* prints the installed version.

## Step 3: Log in

```shell
gh auth login
```

*What this does:* starts an interactive setup. Answer the prompts like this:

- **What account do you want to log into?** → `GitHub.com`
- **What is your preferred protocol for Git operations?** → `HTTPS`
- **Authenticate Git with your GitHub credentials?** → `Yes`
- **How would you like to authenticate?** → `Login with a web browser`

It'll show you a one-time code and open your browser. Paste the code in, approve, and you're done, `gh` and Git are both now authenticated.

> [!NOTE]
> There's an older, more manual way to do this involving SSH keys (a pair of cryptographic files that prove it's you, without a password). `gh auth login` skips all of that for you. If a tool or tutorial specifically asks for an SSH key later, you can generate one then, it's not needed to get started.

## Step 4: Tell Git who you are

GitHub needs to know a name and email to attach to your commits (a commit is a saved snapshot of changes, labeled with who made it and why).

```shell
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

*What this does:* saves your name and email in Git's global settings, so every commit on this Mac, in every project, is automatically labeled with them. Use the same email your GitHub account uses, so GitHub can match your commits to your profile.

## Step 5: Try it, clone a repo

"Cloning" means downloading a full copy of a GitHub repository (a project's folder of files plus its whole history) onto your Mac.

```shell
gh repo clone owner/repo-name
```

*What this does:* downloads the repository into a new folder named after it, in whatever folder you ran the command from. Replace `owner/repo-name` with a real one, for example `gh repo clone ngreenwall/dev-environment-starter-kit`.

## Step 6: Make your first commit and push

Inside a repo folder, after you've changed or added a file:

```shell
git add .
git commit -m "Describe what you changed"
git push
```

*What this does:*
- `git add .` stages every changed file (marks it as ready to be included in the next commit)
- `git commit -m "..."` saves a snapshot of the staged changes with a short message describing them
- `git push` uploads that snapshot to GitHub

## Undo / start over

- **GitHub CLI:** `brew uninstall gh`
- **Git login:** `gh auth logout`
- **Git identity:** re-run the Step 4 commands with new values to overwrite them

## Related

- [Main setup guide](../README.md)
- [API Key Management](api-key-management.md)
