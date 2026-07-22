# API Key Management

An API key (also called a token) is like a password that lets one piece of software prove its identity to another, for example, letting a tool on your Mac connect to an online service on your behalf. Once you start connecting tools together, you'll end up needing to store one or more of these somewhere. This guide covers the safe way to do it on a Mac: **macOS Keychain** (the built-in, encrypted password manager on every Mac), not a plain text file.

## Why not just paste it into a config file?

Plenty of tools let you type your API key straight into a settings file (`.env`, `opencode.json`, etc.). The problem: those files can get backed up, synced to the cloud, committed to git, or shared with someone else without you noticing, and now your key is out in the open.

macOS Keychain stores the value encrypted. Your config file only ever holds the *name* of a variable, never the secret itself.

## The pattern, in three steps

1. **Store** the key in macOS Keychain (encrypted, never in plain text)
2. **Export** it as an environment variable in `~/.zshrc` (so it loads automatically every time you open a terminal)
3. **Reference** it by name in whatever tool needs it

Once this is set up, rotating a key later (getting a new one and swapping it in) never requires touching any tool's config file, just the Keychain entry.

## Step 1: Store the key in Keychain

Throughout this guide, `my-api-token` and `MY_API_TOKEN` are placeholder names, not real values. Swap them for something that describes your actual key (like `openai-api-key`), and use the same name consistently in every step below.

**Easiest way: the Keychain Access app.** This avoids the key ever appearing in your terminal or its history.

1. Open **Keychain Access** (search for it with Spotlight, `Cmd+Space`)
2. In the sidebar, select the **login** keychain
3. Go to **File > New Password Item**
   - **Keychain Item Name:** a label for this key, e.g. `my-api-token`
   - **Account Name:** your Mac username
   - **Password:** paste the actual key here
4. Save

**Alternative: the terminal.** Works, but type it carefully, and put a leading space before the command so it doesn't get saved to your command history:

```shell
 security add-generic-password -a "$USER" -s "my-api-token" -w "paste-your-actual-key-here"
```

*What this does:* adds a new entry to Keychain named `my-api-token`, tied to your Mac username (`$USER` is a built-in variable that always holds your own Mac username, so you don't have to type it out), storing the key you pasted.

> [!IMPORTANT]
> Use the **login** keychain, not iCloud Keychain. The command-line tool used below only reliably reads from the login keychain. This also means keys stored this way are local to this Mac, you'll need to re-add them on any other Mac you use.

## Step 2: Load it into your terminal via `.zshrc`

An environment variable is a named value your terminal keeps in memory, so any command or tool you run can read it just by using its name, without you retyping the actual value every time. Add a line to `~/.zshrc` that creates one by reading the key out of Keychain every time you open a terminal:

```shell
export MY_API_TOKEN=$(security find-generic-password -a "$USER" -s "my-api-token" -w 2>/dev/null)
```

*What this does:* the `$(...)` part runs the Keychain lookup command and captures whatever it prints; `export` then saves that result into an environment variable called `MY_API_TOKEN`, available in that terminal window from then on. The `2>/dev/null` at the end just hides an error message in case the entry isn't found yet, so it fails quietly instead of printing something alarming-looking.

Reload your shell so the change takes effect right away:

```shell
source ~/.zshrc
```

**Verify it worked:**

```shell
echo $MY_API_TOKEN
```

*What this does:* prints the value. If you see your key, it's loaded. If the line is empty, double check the Keychain entry name matches exactly what you typed in Step 1.

## Step 3: Use it in tool configs and commands

Most command-line tools, including Claude Code, automatically pick up environment variables like this with no extra setup. If a tool's config file supports variable references, use the variable name instead of pasting the real key. For example, a JSON config (JSON is a common plain-text format config files use, built out of `{ }` braces and `"key": "value"` pairs) might look like:

```json
"env": {
  "MY_API_TOKEN": "{env:MY_API_TOKEN}"
}
```

*What this does:* tells the tool "read this from the environment variable named `MY_API_TOKEN`," rather than storing the real value in the file.

You can also reference it directly in a terminal command:

```shell
some-tool --token $MY_API_TOKEN
```

*What this does:* substitutes the actual key value at the moment the command runs. The key itself never has to be typed into the command by hand.

### Keep sensitive commands out of your history

By default, your terminal saves every command you type to a history file. If a command includes a raw key value (rather than a `$VARIABLE` reference), prefix it with a single leading space to skip saving it:

```shell
 some-tool --token paste-raw-key-here
```

For this to work, make sure this line is somewhere in your `~/.zshrc` (it's included in the [Minimum Useful `.zshrc`](customizing-zshrc.md)):

```shell
setopt HIST_IGNORE_SPACE
```

## Rotating a key

1. Generate a new key from whatever service issued it
2. Delete the old Keychain entry:

   ```shell
   security delete-generic-password -a "$USER" -s "my-api-token"
   ```

   *What this does:* removes the old `my-api-token` Keychain entry so it doesn't linger or get mixed up with the new one.

3. Add the new one, same as Step 1:

   ```shell
    security add-generic-password -a "$USER" -s "my-api-token" -w "your-new-key-here"
   ```

   *What this does:* stores the new key under the same Keychain entry name as before.

4. Reload: `source ~/.zshrc`

No tool configs need to change, they were only ever pointed at the variable name, not the value.

## Related

- [Customizing .zshrc](customizing-zshrc.md)
- [Main setup guide](../README.md)
