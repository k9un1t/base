# AGENTS — Themes Base

## Project Description

Themes Base (linuxthemes.org/base) is a git-based desktop configuration manager for
Linux. Instead of a symlink farm or a config daemon, it makes your **home directory
itself a git repository** and uses **git branches to represent complete desktop
configurations** ("themes"). Installing, switching, backing up, and sharing OS/desktop
configurations is done with plain git commands wrapped in `themes-*` bash aliases.

## How It Works

### 1. Your home directory is a git repo
`themes-install` runs `git init $HOME`, adds the Themes remote
(https://github.com/linuxthemes/base), and pulls the configuration. From then on
git tracks your system- and user-space configuration files in place — no symlinks.

### 2. Everything is ignored; only selected files are tracked
`.gitignore` is the heart of the mechanism. It starts with `*` (ignore everything)
and then whitelists precisely the files that should be versioned:

- top-level config: `.bashrc`, `install`, `programs`, `klayout`, `post-checkout`
- docs: `README.md`, `CONTRIBUTING.md`, `AGENTS.md`, `notes`
- theme assets: `.themes/**`, `.icons/**`, `Pictures/*`
- app configs under `.config/` (tmux, eza, micro, rofi, fastfetch, ghostty, btop,
  VS Code, GTK, KDE, XFCE, Hyprland, ...)

Anything not whitelisted (caches, downloads, private data) is invisible to git.

### 3. Branches are configurations
Each branch is a full, self-contained configuration:

- `base` — the default/base theme
- `nord`, `dracula`, `tokoyo`, `catppucino` — color-theme configurations
- `github`, `main`, `master` — additional configurations

### 4. Switching configurations = checking out a branch
`themes-switch nord` runs `git checkout nord`, which swaps every tracked config
file to that branch's version. The `post-checkout` git hook then reloads the
desktop environment so the new config takes effect immediately
(KDE → `plasmashell --replace`, XFCE → `xfce-session -r`, GNOME/i3 placeholders).

### 5. Config changes appear as diffs
Because only selected files are tracked, every live change you make to a tracked
config file (`.bashrc`, `.config/gtk-3.0/settings.ini`, ...) immediately shows up
in `git status` / `git diff`. Commit and push with `themes-push` to back it up or
share it across machines.

## Themes Bash Aliases

Defined in `.bashrc` (section: `# --- Themes ---`).

| Alias | Underlying command | Description |
|---|---|---|
| `themes-install` | `git init $HOME && git remote add origin https://github.com/linuxthemes/base && git pull origin main` | First-time setup: turn `$HOME` into a git repo and connect to the Themes remote |
| `themes-update` | `git pull` | Pull the latest configuration changes from the remote |
| `themes-switch` | `git checkout` | Switch to another configuration branch, e.g. `themes-switch nord` |
| `themes-create` | `git checkout -b` | Create a new configuration branch, e.g. `themes-create mytheme` |
| `themes-push` | `git push` | Push your local configuration changes to the remote |
| `themes-programs` | `sh $HOME/programs` | Install/update the software defined in the `programs` script |

## Directories & Files

### Top-level files

| Path | Description |
|---|---|
| `.bashrc` | Shell init: prompt, package-manager aliases (Arch `update/install/remove/list/clean`, `installa/updater/lister`; commented Gentoo/Apt/Nala/DNF), program aliases (`dev`, `sysd`, `ls`, `lsa`, `fzfa`, `ip`), the `themes-*` aliases, PATH exports, `extract()` helper |
| `.gitignore` | "Ignore everything, whitelist the config" rule set that defines what git tracks |
| `install` | Bootstrap script: init `$HOME`, add remote, pull current branch, back up user settings, install `post-checkout` hook |
| `post-checkout` | Git hook run after every checkout; reloads the desktop environment (KDE/XFCE/GNOME/i3) |
| `programs` | Multi-package-manager installer (apt, pacman/yay, emerge, flatpak) driven by `PROGRAMS_*` lists |
| `klayout` | Konsole tabs layout: opens btop, systemd, docker, vpn monitors and two OpenCode agents — launched by the `dev` alias |
| `notes` | Scratch file |
| `README.md`, `CONTRIBUTING.md`, `.config/README.md` | Project docs |

### `.config/` — application configurations

| Path | Description |
|---|---|
| `.config/tmux/` | Tmux config (`tmux.conf`, `tmux.conf.local`) |
| `.config/eza/` | Eza color themes: catppuccin, dracula, nord, onedark, tokoyo (`theme.yml` selects) |
| `.config/micro/` | Micro editor settings, key bindings, colorschemes |
| `.config/rofi/` | Rofi launcher config (`config.rasi`) |
| `.config/fastfetch/` | Fastfetch system-info config |
| `.config/ghostty/` | Ghostty terminal config plus a large collection of terminal themes |
| `.config/btop/` | Btop system monitor config and themes (catppuccin, nord, tokoyo, ...) |
| `.config/Code/` and `.config/Code - OSS/` | VS Code / Code-OSS `settings.json` + `keybindings.json` |
| `.config/gtk-3.0/`, `.config/gtk-4.0/` | GTK settings (`settings.ini`, `gtk.css`, `colors.css`, bookmarks) |
| `.config/xsettingsd/` | X settings daemon config |
| `.config/kdedefaults/` | KDE defaults (kdeglobals, kwinrc, plasmarc, ksplashrc, kcminputrc) |
| KDE (`.config/kdeglobals`, `kglobalshortcutsrc`, `konsolerc`, `krunnerrc`, `plasmarc`, `plasmashellrc`, `plasmanotifyrc`, `plasma-org.kde.plasma.desktop-appletsrc`) | Plasma desktop appearance, shortcuts, and panel config |
| `.config/xfce4/`, `.config/Thunar/`, `.config/Mousepad/` | XFCE desktop, Thunar file manager, Mousepad editor |
| `.config/hypr/`, `.config/waybar/` | Hyprland compositor (`hyprland.conf`, `hyprlock.conf`, `hyprpaper.conf`) and Waybar panel |

### `.themes/` — desktop themes

| Path | Description |
|---|---|
| `.themes/nord/` | Nord GTK theme (GTK 2/3/4, gnome-shell, metacity, xfwm4, cinnamon) |
| `.themes/dracula/` | Dracula theme (GTK, metacity, unity, xfwm4) |
| `.themes/tokoyo/` | Tokoyo (Tokyo Night) theme (GTK 2/3/4, gnome-shell, metacity, xfwm4, plank) |
| `.themes/whitesur/` | WhiteSur theme (GTK 2/3/4, gnome-shell, metacity, unity, xfwm4, plank) |
| `.themes/fleX/` | fleX XFWM window-manager theme |
| `.themes/tacity/` | Tacity theme (metacity) |

### `.icons/` and `.local/share/icons/` — icon themes

| Path | Description |
|---|---|
| `.icons/WhiteSur/`, `WhiteSur-dark/`, `WhiteSur-light/` | WhiteSur icon sets |
| `.icons/We10X/`, `We10X-dark/` | Windows 10-style icon sets |
| `.icons/Win11/`, `Win11-dark/` | Windows 11-style icon sets |
| `.icons/MoreWaita/` | Gnome-style scalable icon set |
| `.local/share/icons/` | Additional user-local icon themes (BigSur, We10X, WhiteSur, Win11, hicolor) |

## Installation

```sh
source <(curl https://linuxthemes.org/install)
```

## Contributing

Issues/PRs: https://github.com/linuxthemes/base