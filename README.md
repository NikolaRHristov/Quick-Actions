# Finder "Copy Path" Quick Actions

A small set of macOS Automator **Quick Actions** (Finder Services) that copy the path of the selected file or folder to the clipboard.

Nothing personal is baked in — the actions operate only on whatever Finder hands them
(`$@` / stdin paths), so they work for anyone on any Mac.

## Included actions

| Action | What it copies | Example output |
|---|---|---|
| **Copy Path** | Raw absolute path | `/Users/you/Projects/foo/bar.txt` |
| **Copy Path (Quoted)** | `printf %q`-quoted, shell-safe | `/Users/you/Projects/foo/bar\ with\ spaces.txt` |
| **Copy Path (URL)** | `file://` URL, percent-encoded | `file:///Users/you/Projects/foo/bar.txt` |

All three handle **multiple** selected items, joined by newlines.

## Install

> ⚠️ **Do NOT double-click or drag the `.workflow` files onto Automator to install.**
> Opening a workflow in Automator causes it to re-save itself and strips the critical
> service metadata, breaking it. Always install by copying with `cp -R` as shown below.

**Clone and copy:**

```bash
git clone https://github.com/NikolaRHristov/Quick-Actions.git
cd Quick-Actions

cp -R "Copy Path.workflow" \
      "Copy Path (Quoted).workflow" \
      "Copy Path (URL).workflow" \
      ~/Library/Services/
```

Then force macOS to re-register the new Services and restart Finder:

```bash
/System/Library/CoreServices/pbs -update && killall Finder
```

After Finder relaunches the three actions will appear in the right-click menu under
**Quick Actions** (macOS Ventura+) or **Services** (older macOS).

### Updating / re-installing

If you need to reinstall (e.g. after a `git pull`), remove the old copies first so
macOS doesn't cache the stale versions:

```bash
rm -rf ~/Library/Services/"Copy Path.workflow" \
       ~/Library/Services/"Copy Path (Quoted).workflow" \
       ~/Library/Services/"Copy Path (URL).workflow"

cp -R "Copy Path.workflow" \
      "Copy Path (Quoted).workflow" \
      "Copy Path (URL).workflow" \
      ~/Library/Services/

/System/Library/CoreServices/pbs -update && killall Finder
```

## Run / Use

1. In **Finder**, select one or more files or folders.
2. **Right-click → Quick Actions** (macOS Ventura and later) or **Right-click → Services** (older macOS) → pick **Copy Path**, **Copy Path (Quoted)**, or **Copy Path (URL)**.
3. The result is now on your clipboard — paste it anywhere with `Cmd+V`.

### Keyboard shortcut (optional)

**System Settings → Keyboard → Keyboard Shortcuts… → Services → Files and Folders**
and assign a shortcut to each action (e.g. `Cmd+Shift+C`).

## Troubleshooting

If the actions don't appear in the context menu after installing:

1. Make sure the workflows are in **`~/Library/Services/`** — not `~/Library/Automator/` or anywhere else.
2. Run the `pbs -update && killall Finder` command above again.
3. Go to **System Settings → Keyboard → Keyboard Shortcuts → Services → Files and Folders** and make sure all three checkboxes are **ticked**.
4. If they still don't appear, log out and back in (a full session restart flushes the Services cache more thoroughly than `killall Finder`).

## Requirements

- macOS with Automator (all modern releases).
- The embedded scripts run under `/bin/zsh`.

## License

Add your own `LICENSE` file. These workflows contain no proprietary code.
