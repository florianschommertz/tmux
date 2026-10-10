---
title: tmux Video Guide (Hypermedia Edition)
tags: [tmux, terminal, cli, workflow, video, learning]
created: 2026-10-10
related: "[[tmux-cheatsheet]]"
---

# 🎬 tmux Video Guide

> This is the companion to [[tmux-cheatsheet]]. Same topics, but each one comes with a video to watch.
> **Obsidian**: the `![](youtube-url)` lines embed the player directly in the note.
> **GitHub / other viewers**: click the thumbnail.
> Total watch time is about 1.5 hours. Watch in order, and practise each step in a real terminal right after.

---

## Learning Path

| # | Step | Video | Length | Goal |
|---|---|---|---|---|
| 1 | What is tmux? | Fireship: *Tmux in 100 Seconds* | ~2 min | The mental model |
| 2 | Hands-on basics | NetworkChuck: *you need to learn tmux RIGHT NOW!!* | ~20 min | Sessions, windows, panes |
| 3 | Deep dive per topic | Learn Linux TV: *Getting Started with tmux* (5 parts) | ~60 min | Solid fundamentals |
| 4 | Pro config | Dreams of Code: *Tmux has forever changed the way I write code* | ~13 min | TPM, plugins, theme, Ctrl+Space prefix |
| 5 | Workflow | ThePrimeagen: tmux-sessionizer | varies | Jumping between projects |

---

## 1️⃣ The 100-Second Overview

<details open>
<summary><b>Fireship: Tmux in 100 Seconds</b></summary>

![](https://www.youtube.com/watch?v=vtB1J_zCv8I)

[![Tmux in 100 Seconds](https://img.youtube.com/vi/vtB1J_zCv8I/hqdefault.jpg)](https://www.youtube.com/watch?v=vtB1J_zCv8I)

**Takeaways**
- tmux is a terminal multiplexer: one terminal, many shells, persistent.
- Server → Session → Window → Pane.
- Detach (`prefix d`) and reattach (`tmux a`) are the magic.

**Try it now:** `tmux new -s test` → `prefix %` → `prefix d` → `tmux a -t test`
</details>

---

## 2️⃣ Hands-On Basics

<details>
<summary><b>NetworkChuck: you need to learn tmux RIGHT NOW!!</b></summary>

![](https://www.youtube.com/watch?v=nTqu6w2wc68)

[![you need to learn tmux RIGHT NOW!!](https://img.youtube.com/vi/nTqu6w2wc68/hqdefault.jpg)](https://www.youtube.com/watch?v=nTqu6w2wc68)

**Takeaways**
- Installing tmux, creating and naming sessions.
- Windows as tabs, panes as splits.
- Why tmux on a remote server saves your SSH sessions when the connection drops.

**Cheat sheet cross-reference:** *Sessions*, *Windows*, *Panes* sections.

**Practise (Flori edition):** SSH into a staging server, run `tmux new -As wp`, start `wp db export`, close the laptop lid, reconnect, then `tmux a`. It's still running. 🎉
</details>

---

## 3️⃣ Deep Dive: One Topic per Episode

<details>
<summary><b>Learn Linux TV: Getting Started with tmux (5-part series)</b></summary>

📺 Series page (each episode has an embedded player): <https://www.learnlinux.tv/category/getting-started-with-tmux/>

| Part | Topic | Cheat sheet section |
|---|---|---|
| 1 | Overview & features | Core Concepts |
| 2 | Panes | Panes |
| 3 | Windows | Windows |
| 4 | Sessions | Sessions |
| 5 | Customization | Recommendations → `.tmux.conf` |

🔎 Or on YouTube: [search "Learn Linux TV Getting Started with tmux"](https://www.youtube.com/results?search_query=learn+linux+tv+getting+started+with+tmux)

**Takeaways**
- Calm, structured, no hype. The best "reference" series.
- Part 5 explains what each `.tmux.conf` option does, so you're not just copy-pasting blindly.
</details>

---

## 4️⃣ Pro Config: Plugins, Theme, Navigation

<details>
<summary><b>Dreams of Code: Tmux has forever changed the way I write code</b></summary>

![](https://www.youtube.com/watch?v=DzNmUNvnB04)

[![Tmux has forever changed the way I write code](https://img.youtube.com/vi/DzNmUNvnB04/hqdefault.jpg)](https://www.youtube.com/watch?v=DzNmUNvnB04)

**Takeaways**
- **TPM** (plugin manager) setup, then `prefix I` to install plugins.
- **vim-tmux-navigator**: `Ctrl+h/j/k/l` moves across nvim splits *and* tmux panes.
- **tmux-yank**: copies to the system clipboard.
- **Catppuccin** theme, mouse support, windows starting at 1.
- Uses a `Ctrl+Space` prefix. My cheat sheet uses `Ctrl+a`. Pick one and keep it.

**Cheat sheet cross-reference:** *Recommendations → 1. `.tmux.conf`*
</details>

<details>
<summary><b>Plugins to read about after this video</b></summary>

| Plugin | Why | Link |
|---|---|---|
| tpm | Plugin manager | <https://github.com/tmux-plugins/tpm> |
| tmux-sensible | Sane defaults | <https://github.com/tmux-plugins/tmux-sensible> |
| tmux-resurrect | Save and restore sessions (`prefix Ctrl+s` / `prefix Ctrl+r`) | <https://github.com/tmux-plugins/tmux-resurrect> |
| tmux-continuum | Autosave every 15 minutes, auto-restore on start | <https://github.com/tmux-plugins/tmux-continuum> |
| tmux-yank | Clipboard support | <https://github.com/tmux-plugins/tmux-yank> |
| vim-tmux-navigator | Seamless nvim ↔ tmux movement | <https://github.com/christoomey/vim-tmux-navigator> |
| catppuccin/tmux | Pretty status bar | <https://github.com/catppuccin/tmux> |
</details>

---

## 5️⃣ Workflow: Project Sessionizer

<details>
<summary><b>ThePrimeagen: tmux-sessionizer (fuzzy-jump between projects)</b></summary>

🔎 [YouTube search: "ThePrimeagen tmux sessionizer"](https://www.youtube.com/results?search_query=theprimeagen+tmux+sessionizer)
🎙️ Also: [Sourcegraph Dev Tool Time with ThePrimeagen](https://sourcegraph.com/blog/dev-tool-time-theprimeagen), on cutting cognitive overhead with tmux and nvim.

**Takeaways**
- One tmux session per project folder, created on demand.
- `fzf` over `~/Sites` and `~/Code` → Enter → you're in that project's session.
- Pairs perfectly with the `dev` layout script (Nuxt: `pnpm dev` + vitest + lazygit + claude).

**Alternatives:** [sesh](https://github.com/joshmedeski/sesh) · [tmuxinator](https://github.com/tmuxinator/tmuxinator) · [tms (Rust)](https://crates.io/crates/tmux-sessionizer)

**Cheat sheet cross-reference:** *Recommendations → 2. Nuxt layout* and *4. Sessionizer*
</details>

---

## 🎯 Flori's Use-Case Playlist (what to watch for which job)

<details>
<summary><b>Nuxt / Vue / Svelte local development</b></summary>

- Watch: **4️⃣ Dreams of Code** + **5️⃣ Sessionizer**
- Build: the `dev` script → windows `code | run (pnpm dev + vitest) | git | claude`
- Key habits: `prefix z` to zoom the failing test pane, `prefix [` then `/error` to search the dev server output.
</details>

<details>
<summary><b>WordPress / DIVI5 / WooCommerce on client servers</b></summary>

- Watch: **2️⃣ NetworkChuck** (the SSH-persistence part)
- Build: `~/.ssh/config` with `RemoteCommand tmux new -As main`
- Key habits: long `wp` jobs always inside tmux. Use `synchronize-panes` for multi-site updates, and turn it off before anything destructive.
</details>

<details>
<summary><b>Laravel</b></summary>

- Watch: **3️⃣ Learn Linux TV, Part 2 (panes)**
- Build: a 4-pane tiled window: `artisan serve` | `npm run dev` | `queue:work` | `pail`, then `prefix Alt+5` for the tiled layout.
- Laravel Sail? Run `sail up` in one pane and `sail artisan …` in another.
</details>

<details>
<summary><b>Claude Code / AI agents</b></summary>

- Run `claude` in its own window per project session, so it survives terminal crashes.
- Run agents in parallel across git worktrees: one session per worktree, all switchable with `prefix s`.
- `:setw monitor-silence 30` alerts you when the agent stops producing output (it's probably waiting for your input).
</details>

---

## ✅ Progress Checklist

- [ ] Watched 1️⃣ and ran the "try it now" commands
- [ ] Watched 2️⃣ and survived a real SSH disconnect with tmux
- [ ] Watched 3️⃣ parts 1–5
- [ ] Installed TPM and the plugins from 4️⃣
- [ ] Created `~/bin/dev` and `~/bin/tmux-sessionizer`
- [ ] Memorised the Top 10 from [[tmux-cheatsheet]]
- [ ] One full workday without touching the mouse 🐭❌
