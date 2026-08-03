# Finder "Copy Path" Quick Actions

A small set of macOS Automator **Quick Actions** (Finder Services) that copy the path of the selected file or folder to the clipboard.

Nothing personal is baked in - the actions operate only on whatever Finder hands them
(`$@` / stdin paths), so they work for anyone on any Mac. The repository's only
account-flavored token was the bundle-id prefix, which has been neutralised to the
placeholder `org.example.copyPath.*` (see "Customize the bundle identifier" below).

## Included actions

| Action | What it copies | Example output |
|---|---|---|
| **Copy Path** | Raw absolute path | `/Users/you/Projects/foo/bar.txt` |
| **Copy Path (Quoted)** | `printf %q`-quoted, shell-safe | `/Users/you/Projects/foo/bar\ with\ spaces.txt` |
| **Copy Path (URL)** | `file://` URL, percent-encoded | `file:///Users/you/Projects/foo/bar.txt` |

All three handle **multiple** selected items, joined by newlines.

## Install

**Option A - double-click:**

1. Download / clone this repo.
2. Double-click each `*.workflow` bundle (or drag it onto the Automator icon).
   macOS installs it into `~/Library/Services/`.

**Option B - copy manually:**

```bash
cp -R "Copy Path.workflow" "Copy Path (Quoted).workflow" "Copy Path (URL).workflow" \
  ~/Library/Services/
```

Then force the Services daemon to re-register them and restart Finder:

```bash
/System/Library/CoreServices/pbs -update && killall Finder
```

> **Note:** Some older guides say `~/Library/Automator/Quick Actions/` — that path
> is unreliable across macOS versions. Always use `~/Library/Services/`.

## Run / Use

1. In **Finder**, select one or more files or folders.
2. **Right-click → Quick Actions** (macOS Ventura and later) or **Right-click → Services** (older macOS) → pick **Copy Path**, **Copy Path (Quoted)**, or **Copy Path (URL)**.
3. The result is now on your clipboard - paste it anywhere with `Cmd+V`.

### Keyboard shortcut (optional)

**System Settings → Keyboard → Keyboard Shortcuts… → Services → Files and Folders** and assign a shortcut to each action (e.g. `Cmd+Shift+C`).

## Troubleshooting

If the actions don't appear in the context menu after installing:

1. Make sure the workflows are in **`~/Library/Services/`** (not `~/Library/Automator/`).
2. Run `killall pbs` or the re-register command above, then relaunch Finder.
3. Go to **System Settings → Privacy & Security → Extensions → Finder Extensions** and verify the actions are enabled.
4. Check **System Settings → Keyboard → Keyboard Shortcuts → Services** — the actions must have their checkboxes **ticked**.

## Customize the bundle identifier

Each `Info.plist` uses the placeholder `org.example.copyPath.*`. Replace
`org.example` with your own reverse-DNS (e.g. `com.github.yourname`) before
distributing if you want to namespace them.

## Requirements

- macOS with Automator (all modern releases).
- The embedded script runs under `/bin/zsh`.

## License

Add your own `LICENSE` file. These workflows contain no proprietary code.
