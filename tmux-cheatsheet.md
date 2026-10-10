---
title: tmux Cheat Sheet + Flori's Setup
tags: [tmux, terminal, cli, workflow, cheatsheet]
created: 2026-10-10
---

# tmux Cheat Sheet

> **Prefix** = `Ctrl+b` by default. In the recommended config below it becomes `Ctrl+a`.
> Notation: `prefix c` means press the prefix, let go, then press `c`.

---

## 🧠 Core Concepts

<details>
<summary><b>Sessions → Windows → Panes (the mental model)</b></summary>

| Level | Like | Typical use |
|---|---|---|
| **Server** | The tmux background process | Keeps running after you close the terminal |
| **Session** | A workspace / project | `shop-nuxt`, `client-wp`, `laravel-api` |
| **Window** | A browser tab | `editor`, `dev-server`, `logs`, `git` |
| **Pane** | A split inside a tab | `npm run dev` on the left, `git status` on the right |

- **Detach** = close the view, but everything keeps running.
- **Attach** = reconnect to it later, even over SSH from somewhere else.
</details>

---

## 🖥️ Shell Commands (outside tmux)

<details>
<summary><b>Starting, attaching, killing</b></summary>

| Command | What it does |
|---|---|
| `tmux` | Start a new unnamed session |
| `tmux new -s shop` | Start a new session named `shop` |
| `tmux new -As shop` | Attach to `shop`, or create it if it doesn't exist ⭐ |
| `tmux ls` | List sessions |
| `tmux a` | Attach to the last session |
| `tmux a -t shop` | Attach to session `shop` |
| `tmux kill-session -t shop` | Kill session `shop` |
| `tmux kill-session -a` | Kill all sessions except the current one |
| `tmux kill-server` | Kill everything ☢️ |
| `tmux source ~/.tmux.conf` | Reload the config |
| `tmux list-keys` | Show every key binding |
</details>

---

## 📦 Sessions

<details>
<summary><b>Session keys</b></summary>

| Keys | Action |
|---|---|
| `prefix d` | **Detach** |
| `prefix s` | Session tree picker (also shows windows) ⭐ |
| `prefix $` | Rename the session |
| `prefix (` / `prefix )` | Previous / next session |
| `prefix L` | Switch to the last session (toggle) |
| `prefix :new -s name` | Create a new session from inside tmux |
</details>

---

## 🗂️ Windows (tabs)

<details>
<summary><b>Window keys</b></summary>

| Keys | Action |
|---|---|
| `prefix c` | Create a window |
| `prefix ,` | Rename the window |
| `prefix &` | Kill the window (asks first) |
| `prefix n` / `prefix p` | Next / previous window |
| `prefix 0…9` | Jump to window N |
| `prefix l` | Last window (toggle) |
| `prefix w` | Window picker |
| `prefix f` | Find a window by text |
| `prefix .` | Move the window to another index |
| `:swap-window -t 1` | Swap the current window with window 1 |
| `:move-window -r` | Renumber windows (or set `renumber-windows on`) |
</details>

---

## 🪟 Panes (splits)

<details>
<summary><b>Pane keys</b></summary>

| Keys | Action |
|---|---|
| `prefix %` | Split left/right (default) |
| `prefix "` | Split top/bottom (default) |
| `prefix ←↑→↓` | Move between panes |
| `prefix o` | Next pane |
| `prefix ;` | Last active pane |
| `prefix q` | Show pane numbers (press a number to jump) |
| `prefix z` | **Zoom** the pane to full window (toggle) ⭐ |
| `prefix x` | Kill the pane |
| `prefix !` | Break the pane out into its own window |
| `prefix {` / `prefix }` | Swap the pane left / right |
| `prefix Space` | Cycle through layouts |
| `prefix Alt+1…5` | Preset layouts (even-h, even-v, main-h, main-v, tiled) |
| `prefix Ctrl+←↑→↓` | Resize by 1 cell |
| `prefix Alt+←↑→↓` | Resize by 5 cells |
| `:join-pane -s 2` | Pull window 2 into the current window as a pane |
| `:setw synchronize-panes` | Type into **all** panes at once (toggle) 🔥 |
</details>

---

## 📋 Copy Mode (scrolling, searching, copying)

<details>
<summary><b>Copy mode keys (vi mode)</b></summary>

| Keys | Action |
|---|---|
| `prefix [` | Enter copy mode (scroll with arrows / PgUp) |
| `q` | Quit copy mode |
| `/` / `?` | Search forward / backward |
| `n` / `N` | Next / previous match |
| `v` | Start selection *(with the config below)* |
| `Ctrl+v` | Rectangle selection *(with the config below)* |
| `y` | Copy *(with the config below; goes to the system clipboard via tmux-yank)* |
| `prefix ]` | Paste the tmux buffer |
| `prefix =` | Pick from the paste buffer history |
| `g` / `G` | Top / bottom of the history |
</details>

---

## ⌨️ Misc & Command Mode

<details>
<summary><b>Useful one-offs</b></summary>

| Keys / command | Action |
|---|---|
| `prefix :` | Command prompt |
| `prefix ?` | List key bindings |
| `prefix t` | Big clock (yes, really) |
| `prefix ~` | Show tmux messages |
| `:set -g mouse on` | Mouse on for this server |
| `:capture-pane -S -3000` then `:save-buffer ~/log.txt` | Dump 3000 lines of scrollback to a file |
| `:setw monitor-activity on` | Highlight the window when output appears |
| `:setw monitor-silence 30` | Alert when a window goes quiet for 30 seconds (good for "is the build done?") |
</details>

---

# 🔧 Fred's Recommendations for Flori

> Context: Vue/Nuxt + WordPress/DIVI5/WooCommerce, Laravel and Svelte on the side, SSH to client servers, probably macOS, probably Claude Code running in a terminal.

<details>
<summary><b>1. The <code>~/.tmux.conf</code> you actually want</b></summary>

```tmux
# --- Prefix: Ctrl+a (closer than Ctrl+b) ---
unbind C-b
set -g prefix C-a
bind C-a send-prefix          # press Ctrl+a twice to send it to nested tmux / the shell

# --- Sanity ---
set -g mouse on
set -g base-index 1           # windows start at 1 (0 is on the wrong side of the keyboard)
setw -g pane-base-index 1
set -g renumber-windows on
set -g history-limit 50000
set -sg escape-time 0         # no lag when pressing Esc in vim/nvim
set -g focus-events on
set -g default-terminal "tmux-256color"
set -as terminal-features ",xterm-256color:RGB"
set -g set-clipboard on
set -g detach-on-destroy off  # killing a session switches to another one instead of dropping you out

# --- Splits that make sense (and keep the current path!) ---
bind | split-window -h -c "#{pane_current_path}"
bind - split-window -v -c "#{pane_current_path}"
bind c new-window -c "#{pane_current_path}"
unbind '"'
unbind %

# --- Vim-style pane movement + resize ---
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R
bind -r H resize-pane -L 5
bind -r J resize-pane -D 5
bind -r K resize-pane -U 5
bind -r L resize-pane -R 5

# --- Copy mode like vim ---
setw -g mode-keys vi
bind -T copy-mode-vi v send -X begin-selection
bind -T copy-mode-vi C-v send -X rectangle-toggle
bind -T copy-mode-vi y send -X copy-selection-and-cancel

# --- Reload ---
bind r source-file ~/.tmux.conf \; display "⚡ reloaded"

# --- Toggle typing into all panes ---
bind S setw synchronize-panes \; display "sync #{?synchronize-panes,ON,OFF}"

# --- Plugins (TPM) ---
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
set -g @plugin 'tmux-plugins/tmux-yank'
set -g @plugin 'tmux-plugins/tmux-resurrect'
set -g @plugin 'tmux-plugins/tmux-continuum'
set -g @plugin 'christoomey/vim-tmux-navigator'
set -g @plugin 'catppuccin/tmux'

set -g @resurrect-capture-pane-contents 'on'
set -g @continuum-restore 'on'

run '~/.tmux/plugins/tpm/tpm'   # keep this as the LAST line
```

Install TPM:
```bash
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
# inside tmux: prefix I   (capital i) installs the plugins
```
</details>

<details>
<summary><b>2. Nuxt/Vue project layout: one command, every pane ready</b></summary>

Save as `~/bin/dev` and run `chmod +x ~/bin/dev`. Then type `dev` inside any project folder.

```bash
#!/usr/bin/env bash
# Usage: dev [path]   → one tmux session per project, auto-attaches
dir="${1:-$PWD}"
name="$(basename "$dir" | tr . _)"

if ! tmux has-session -t "$name" 2>/dev/null; then
  tmux new-session -d -s "$name" -c "$dir" -n code
  tmux send-keys -t "$name:code" "nvim . || code ." C-m

  tmux new-window  -t "$name" -n run -c "$dir"
  tmux send-keys   -t "$name:run" "pnpm dev" C-m
  tmux split-window -h -t "$name:run" -c "$dir"
  tmux send-keys   -t "$name:run.2" "pnpm vitest" C-m

  tmux new-window  -t "$name" -n git -c "$dir"
  tmux send-keys   -t "$name:git" "lazygit || git status" C-m

  tmux new-window  -t "$name" -n claude -c "$dir"
  tmux send-keys   -t "$name:claude" "claude" C-m

  tmux select-window -t "$name:code"
fi

[ -n "$TMUX" ] && tmux switch-client -t "$name" || tmux attach -t "$name"
```

For Laravel, swap the `run` window for `php artisan serve` | `npm run dev` | `php artisan queue:work` | `php artisan pail`.
For Svelte or SvelteKit, `pnpm dev` already does the job.
</details>

<details>
<summary><b>3. WordPress / WooCommerce over SSH: the real killer feature</b></summary>

- **Always start long jobs on the server inside tmux**: `ssh client-server -t "tmux new -As wp"`
  - `wp search-replace`, `wp db export`, WooCommerce imports, `composer install`, rsync deploys… If Wi-Fi drops (hello, NZ rural broadband), the job keeps running. Reattach and carry on.
- **Nested tmux** (local tmux → SSH → remote tmux): press `Ctrl+a` twice to send the prefix to the inner session. Or set the remote server's prefix to `Ctrl+b` so the two never clash.
- **Several servers at once**: open one pane per server, press `prefix S` (synchronize-panes), then `wp plugin update --all`. Turn sync off **before** you type anything destructive. 💀
- **Watch logs side by side**: `tail -f wp-content/debug.log` in one pane, `tail -f /var/log/nginx/error.log` in another.
- Add to `~/.ssh/config`:
  ```
  Host shop
    HostName shop.example.com
    User deploy
    RequestTTY yes
    RemoteCommand tmux new -As main
  ```
  Then `ssh shop` lands straight in a persistent session.
</details>

<details>
<summary><b>4. Session jumping like a pro (sessionizer)</b></summary>

Jump between projects in two keystrokes with fuzzy search:

```bash
# brew install fzf
# ~/bin/tmux-sessionizer
#!/usr/bin/env bash
selected=$(find ~/Sites ~/Code -mindepth 1 -maxdepth 1 -type d | fzf) || exit 0
~/bin/dev "$selected"
```

```tmux
bind f display-popup -E -w 60% -h 60% "~/bin/tmux-sessionizer"
```

Other options: [sesh](https://github.com/joshmedeski/sesh) (more features, zoxide integration) or [tmuxinator](https://github.com/tmuxinator/tmuxinator) (layouts in YAML, nice for complex WP/Laravel setups).
</details>

<details>
<summary><b>5. Survive reboots: resurrect + continuum</b></summary>

| Keys | Action |
|---|---|
| `prefix Ctrl+s` | Save all sessions, windows and panes |
| `prefix Ctrl+r` | Restore them |

With `@continuum-restore 'on'`, tmux autosaves every 15 minutes and restores on start.
Gotcha: if a theme plugin overwrites `status-right`, autosave quietly stops. Put continuum **last** in the plugin list, or check with `prefix :show -g status-right`.
</details>

<details>
<summary><b>6. Terminal and macOS notes</b></summary>

- **Ghostty** or **WezTerm** with tmux works great. iTerm2 also has `tmux -CC` (control mode: tmux sessions become native iTerm tabs).
- macOS `Option` as Meta: enable "Option as Alt" in your terminal, otherwise `prefix Alt+arrows` does nothing.
- Clipboard: `tmux-yank` uses `pbcopy` on macOS automatically.
- In VS Code's integrated terminal, `Ctrl+a` may be caught by the editor. Set `"terminal.integrated.commandsToSkipShell"` or just use a real terminal.
</details>

<details>
<summary><b>7. Top 10 to memorise first (with this config)</b></summary>

1. `tmux new -As name`: attach or create
2. `prefix d`: detach
3. `prefix s`: switch session
4. `prefix c` / `prefix 1-9`: new window / jump
5. `prefix |` / `prefix -`: split
6. `prefix h j k l`: move between panes
7. `prefix z`: zoom
8. `prefix [`, then `/`, `v`, `y`: search and copy
9. `prefix r`: reload config
10. `prefix Ctrl+s` / `prefix Ctrl+r`: save / restore
</details>

---

## 🔗 References

- Official wiki: <https://github.com/tmux/tmux/wiki>
- Getting started: <https://github.com/tmux/tmux/wiki/Getting-Started>
- `man tmux` (long, but it's the source of truth)
- tmuxcheatsheet.com: <https://tmuxcheatsheet.com>
- TPM: <https://github.com/tmux-plugins/tpm>
- tmux-resurrect: <https://github.com/tmux-plugins/tmux-resurrect>
- tmux-continuum: <https://github.com/tmux-plugins/tmux-continuum>
