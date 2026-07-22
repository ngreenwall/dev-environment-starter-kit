# API Key Management

Once you start connecting tools together (Claude Code to an MCP server, a CLI to a service's API, etc.) you'll end up needing to store API keys and tokens somewhere. This guide covers the safe way to do it on a Mac: **macOS Keychain**, not a plain text file.

## Why not just paste it into a config file?

Plenty of tools let you type your API key straight into a settings file (`.env`, `opencode.json`, etc.). The problem: those files can get backed up, synced to the cloud, committed to git, or shared with someone else without you noticing, and now your key is out in the open.

macOS Keychain stores the value encrypted. Your config file only ever holds the *name* of a variable, never the secret itself.

## The pattern, in three steps

1. **Store** the key in macOS Keychain (encrypted, never in plain text)
2. **Export** it as an environment variable in `~/.zshrc` (so it loads automatically every time you open a terminal)
3. **Reference** it by name in whatever tool needs it

Once this is set up, rotating a key later (getting a new one and swapping it in) never requires touching any tool's config file, just the Keychain entry.

## Step 1: Store the key in Keychain

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

*What this does:* adds a new entry to Keychain named `my-api-token`, tied to your Mac username, storing the key you pasted.

> [!IMPORTANT]
> Use the **login** keychain, not iCloud Keychain. The command-line tool used below only reliably reads from the login keychain. This also means keys stored this way are local to this Mac, you'll need to re-add them on any other Mac you use.

## Step 2: Load it into your terminal via `.zshrc`

Add a line to `~/.zshrc` that reads the key out of Keychain every time you open a terminal:

```shell
export MY_API_TOKEN=$(security find-generic-password -a "$USER" -s "my-api-token" -w 2>/dev/null)
```

*What this does:* looks up the `my-api-token` entry in Keychain and saves its value into an environment variable called `MY_API_TOKEN`, available in that terminal window from then on. Swap `MY_API_TOKEN` and `my-api-token` for names that make sense for your key.

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

Most command-line tools, including Claude Code, automatically pick up environment variables like this with no extra setup. If a tool's config file supports variable references, use the variable name instead of pasting the real key. For example, a JSON config might look like:

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

3. Add the new one, same as Step 1:

   ```shell
    security add-generic-password -a "$USER" -s "my-api-token" -w "your-new-key-here"
   ```

4. Reload: `source ~/.zshrc`

No tool configs need to change, they were only ever pointed at the variable name, not the value.

## Related

- [Customizing .zshrc](customizing-zshrc.md)
- [Main setup guide](../README.md)
