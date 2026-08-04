<p align="center">
  <img src="https://img.shields.io/badge/platform-macOS-000000?logo=apple&logoColor=white" alt="Platform: macOS">
  <img src="https://img.shields.io/badge/built%20with-Automator%20%2F%20zsh-555555" alt="Built with Automator and zsh">
  <img src="https://img.shields.io/badge/license-Custom-2ea44f" alt="License: Custom">
</p>

# [Quick Actions] 📋

A small set of macOS **Automator Quick Actions** (Finder Services) that copy the
path of the selected file or folder to the clipboard.

Nothing personal is baked in - the actions operate only on whatever Finder hands
them (`$@` / stdin paths), so they work for anyone on any Mac.

<p align="center">
  <img src="Screenshot.png" alt="Finder's right-click menu showing the Copy Path, Copy Path (Quoted), and Copy Path (URL) Quick Actions" width="720">
</p>

> **Note**
>
> The actions copy the *path* - not the file contents. Great for pasting into a
> terminal, a chat, or a document without dragging a file around.

## Included Actions 🧩

| Action | What it copies | Example output |
| --- | --- | --- |
| **Copy Path** | Raw absolute path | `/Users/you/Projects/foo/bar.txt` |
| **Copy Path (Quoted)** | `printf %q`-quoted, shell-safe | `/Users/you/Projects/foo/bar\ with\ spaces.txt` |
| **Copy Path (URL)** | `file://` URL, percent-encoded | `file:///Users/you/Projects/foo/bar.txt` |

All three handle **multiple** selected items, joined by newlines.

## Installation 🚀

> [!WARNING]
> **Do NOT double-click or drag the `.workflow` files onto Automator to install.**
> Opening a workflow in Automator causes it to re-save itself and strips the
> critical service metadata, breaking it. Always install by copying with `cp -R`
> as shown below.

**Clone and copy:**

```sh
git clone https://github.com/NikolaRHristov/QuickActions.git
cd QuickActions

cp -R "Copy Path.workflow" \
      "Copy Path (Quoted).workflow" \
      "Copy Path (URL).workflow" \
      ~/Library/Services/
```

Then force macOS to re-register the new Services and restart Finder:

```sh
/System/Library/CoreServices/pbs -update && killall Finder
```

After Finder relaunches the three actions will appear in the right-click menu
under **Quick Actions** (macOS Ventura+) or **Services** (older macOS).

### Updating / re-installing 🔄

If you need to reinstall (e.g. after a `git pull`), remove the old copies first
so macOS doesn't cache the stale versions:

```sh
rm -rf ~/Library/Services/"Copy Path.workflow" \
       ~/Library/Services/"Copy Path (Quoted).workflow" \
       ~/Library/Services/"Copy Path (URL).workflow"

cp -R "Copy Path.workflow" \
      "Copy Path (Quoted).workflow" \
      "Copy Path (URL).workflow" \
      ~/Library/Services/

/System/Library/CoreServices/pbs -update && killall Finder
```

## Usage 🖱️

1. In **Finder**, select one or more files or folders.
2. **Right-click → Quick Actions** (macOS Ventura and later) or
   **Right-click → Services** (older macOS) → pick **Copy Path**,
   **Copy Path (Quoted)**, or **Copy Path (URL)**.
3. The result is now on your clipboard - paste it anywhere with `Cmd+V`.

### Keyboard shortcut (optional) ⌨️

**System Settings → Keyboard → Keyboard Shortcuts… → Services →
Files and Folders** and assign a shortcut to each action (e.g. `Cmd+Shift+C`).

## Troubleshooting 🛠️

If the actions don't appear in the context menu after installing:

1. Make sure the workflows are in **`~/Library/Services/`** - not
   `~/Library/Automator/` or anywhere else.
2. Run the `pbs -update && killall Finder` command above again.
3. Go to **System Settings → Keyboard → Keyboard Shortcuts → Services →
   Files and Folders** and make sure all three checkboxes are **ticked**.
4. If they still don't appear, log out and back in - a full session restart
   flushes the Services cache more thoroughly than `killall Finder`.

## Requirements 💻

- macOS with Automator (all modern releases).
- The embedded scripts run under `/bin/zsh`.

## Contributing 🤝

Welcome! This project follows the
[Contributor Covenant][contributor-covenant] Code of Conduct. By participating,
you agree to uphold a welcoming, inclusive, and harassment-free environment for
everyone.

### Our Pledge

We, as members, contributors, and leaders, pledge to make participation in our
community a harassment-free experience for everyone, regardless of age, body
size, visible or invisible disability, ethnicity, sex characteristics, gender
identity and expression, level of experience, education, socio-economic status,
nationality, personal appearance, race, caste, color, religion, or sexual
identity and orientation.

### Our Standards

Examples of behavior that contributes to a positive environment:

- Demonstrating empathy and kindness toward other people
- Being respectful of differing opinions, viewpoints, and experiences
- Giving and gracefully accepting constructive feedback
- Accepting responsibility and apologizing for mistakes
- Focusing on what is best for the overall community

Examples of unacceptable behavior:

- The use of sexualized language or imagery, and sexual attention or advances
- Trolling, insulting or derogatory comments, and personal or political attacks
- Public or private harassment
- Publishing others' private information without explicit permission

### Reporting

Instances of abusive, harassing, or otherwise unacceptable behavior may be
reported through the project's issue tracker at
[`GitHub.Com/NikolaRHristov/QuickActions/issues`][issues]. All complaints will
be reviewed and investigated promptly and fairly.

### Enforcement Guidelines

Community leaders will follow these Community Impact Guidelines in determining
consequences for any action deemed in violation of this Code of Conduct:

1. **Correction** - A private, written warning for inappropriate language or
   unprofessional behavior.
2. **Warning** - A warning with consequences for continued behavior, including
   a temporary ban on interaction.
3. **Temporary Ban** - A temporary ban from any sort of interaction for a
   specified period of time.
4. **Permanent Ban** - A permanent ban from any sort of public interaction
   within the community.

> **Note**
>
> This Code of Conduct is adapted from the
> [Contributor Covenant][contributor-covenant], version 2.1.

## License 📜

Released under the **CC0 1.0 Universal** public domain dedication. See the
[`LICENSE`][license] file for the full text - you may use, modify, and
redistribute these workflows freely, including for commercial purposes, with no
attribution required.

[issues]: HTTPS://GitHub.Com/NikolaRHristov/QuickActions/issues
[license]: LICENSE
[contributor-covenant]: HTTPS://www.contributor-covenant.org
