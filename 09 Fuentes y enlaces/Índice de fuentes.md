---
aliases: [Fuentes, Enlaces oficiales, Bibliografía]
tags: [fuentes, indice]
actualizado: 2026-09-27
verificado_en: "todos los enlaces comprobados (HTTP 200) el 27-09-2026"
---
# Índice de fuentes y enlaces oficiales

Las notas son **síntesis originales en español**; los sitios enlazados son la referencia técnica actualizada. Todos los enlaces del vault se comprobaron el **27-09-2026**. Ante una discrepancia, prima: **1)** el comportamiento del sistema instalado (`--help`, `man`, archivos locales), **2)** la documentación oficial de esa versión, **3)** chuletas comunitarias.

## Nivel de autoridad

| Fuente | Tipo | Úsala para |
|---|---|---|
| [Manual de Omarchy](https://omarchy.org/manual/) | oficial | uso, configuración y flujos de Omarchy |
| [Hyprland Wiki](https://wiki.hypr.land/) | oficial | API Lua, dispatchers, reglas, monitores |
| [ArchWiki](https://wiki.archlinux.org/title/Main_page) | oficial (comunitaria curada) | pacman, systemd, red, audio, hardware |
| [LazyVim](https://www.lazyvim.org/) | oficial | atajos, extras y configuración de LazyVim |
| [Manual de Neovim](https://neovim.io/doc/user/) | oficial | edición, opciones, Lua, LSP |
| [Manual de GNU Bash](https://www.gnu.org/software/bash/manual/) | oficial | sintaxis y semántica de Bash |
| [Devhints Bash](https://devhints.io/bash) | comunitaria | chuleta rápida; **no** es el manual |
| [tmux wiki](https://github.com/tmux/tmux/wiki) · [man tmux](https://man.openbsd.org/tmux) | oficial | tmux |
| [Documentación de Herdr](https://herdr.dev/docs/) | oficial | Herdr |

## Omarchy

- [Manual (51 capítulos)](https://omarchy.org/manual/) → [[01 Omarchy/Guía del manual oficial]].
- Para principiantes: [Coming From Mac or Windows](https://omarchy.org/manual/coming-from-mac-or-windows/), [Getting Started](https://omarchy.org/manual/getting-started/), [FAQ](https://omarchy.org/manual/faq/), [Making your own theme](https://omarchy.org/manual/making-your-own-theme/), [Common tweaks](https://omarchy.org/manual/common-tweaks/).
- Capítulos clave: [Hotkeys](https://omarchy.org/manual/hotkeys/), [CLI](https://omarchy.org/manual/omarchy-cli/), [Dotfiles](https://omarchy.org/manual/dotfiles/), [Updates](https://omarchy.org/manual/updates/), [System snapshots](https://omarchy.org/manual/system-snapshots/), [Shell plugins](https://omarchy.org/manual/shell-plugins/), [Troubleshooting](https://omarchy.org/manual/troubleshooting/), [Security](https://omarchy.org/manual/security/).
- Código: [omacom/omarchy](https://github.com/omacom/omarchy), [README de la shell](https://github.com/omacom/omarchy/blob/master/shell/README.md), [fuente del manual](https://github.com/omacom/omarchy-site/tree/master/manual).
- [Quickshell](https://quickshell.org/docs/v0.2.1/).

## Hyprland

- [Portada](https://wiki.hypr.land/) — muestra **Latest git**; usa el [selector de versión](https://wiki.hypr.land/version-selector/) con la de `hyprctl version`. Desde la 0.55 la sintaxis es **Lua**; la antigua (hyprlang) está en las páginas de la 0.54.
- Empezar: [Configuring/Start](https://wiki.hypr.land/Configuring/Start/), [Master tutorial](https://wiki.hypr.land/Getting-Started/Master-Tutorial/).
- Básico: [Binds](https://wiki.hypr.land/Configuring/Basics/Binds/), [Dispatchers](https://wiki.hypr.land/Configuring/Basics/Dispatchers/), [Window Rules](https://wiki.hypr.land/Configuring/Basics/Window-Rules/), [Workspace Rules](https://wiki.hypr.land/Configuring/Basics/Workspace-Rules/), [Monitors](https://wiki.hypr.land/Configuring/Basics/Monitors/), [Variables](https://wiki.hypr.land/Configuring/Basics/Variables/), [Autostart](https://wiki.hypr.land/Configuring/Basics/Autostart/).
- Layouts: [Dwindle](https://wiki.hypr.land/Configuring/Layouts/Dwindle-Layout/), [Scrolling](https://wiki.hypr.land/Configuring/Layouts/Scrolling-Layout/), [Master](https://wiki.hypr.land/Configuring/Layouts/Master-Layout/).
- Aspecto y recetas: [Animations](https://wiki.hypr.land/Configuring/Advanced-and-Cool/Animations/), [Devices](https://wiki.hypr.land/Configuring/Advanced-and-Cool/Devices/), [Monocle](https://wiki.hypr.land/Configuring/Layouts/Monocle-Layout/), [IPC](https://wiki.hypr.land/IPC/).
- Avanzado: [Using hyprctl](https://wiki.hypr.land/Configuring/Advanced-and-Cool/Using-hyprctl/), [Environment variables](https://wiki.hypr.land/Configuring/Advanced-and-Cool/Environment-variables/), [Gestures](https://wiki.hypr.land/Configuring/Advanced-and-Cool/Gestures/), [Multi-GPU](https://wiki.hypr.land/Configuring/Advanced-and-Cool/Multi-GPU/), [Performance](https://wiki.hypr.land/Configuring/Advanced-and-Cool/Performance/).

## Arch Linux

- [Portada](https://wiki.archlinux.org/title/Main_page) · [en español](https://wiki.archlinux.org/title/Main_page_%28Espa%C3%B1ol%29) · [noticias](https://archlinux.org/news/) · [AUR](https://aur.archlinux.org/).
- Paquetes: [pacman](https://wiki.archlinux.org/title/Pacman), [Tips and tricks](https://wiki.archlinux.org/title/Pacman/Tips_and_tricks), [Pacnew and Pacsave](https://wiki.archlinux.org/title/Pacman/Pacnew_and_Pacsave), [AUR](https://wiki.archlinux.org/title/Arch_User_Repository), [System maintenance](https://wiki.archlinux.org/title/System_maintenance).
- Sistema: [Arch boot process](https://wiki.archlinux.org/title/Arch_boot_process), [systemd](https://wiki.archlinux.org/title/Systemd), [systemd/User](https://wiki.archlinux.org/title/Systemd/User), [systemd/Journal](https://wiki.archlinux.org/title/Systemd/Journal), [General troubleshooting](https://wiki.archlinux.org/title/General_troubleshooting).
- Principiantes: [Users and groups](https://wiki.archlinux.org/title/Users_and_groups), [File permissions](https://wiki.archlinux.org/title/File_permissions_and_attributes), [Sudo](https://wiki.archlinux.org/title/Sudo), [Environment variables](https://wiki.archlinux.org/title/Environment_variables), [Core utilities](https://wiki.archlinux.org/title/Core_utilities), [SSH keys](https://wiki.archlinux.org/title/SSH_keys), [CUPS](https://wiki.archlinux.org/title/CUPS), [udisks](https://wiki.archlinux.org/title/Udisks).
- Arranque y rescate: [Limine](https://wiki.archlinux.org/title/Limine), [Kernel parameters](https://wiki.archlinux.org/title/Kernel_parameters), [mkinitcpio](https://wiki.archlinux.org/title/Mkinitcpio), [Microcode](https://wiki.archlinux.org/title/Microcode), [AMDGPU](https://wiki.archlinux.org/title/AMDGPU), [chroot](https://wiki.archlinux.org/title/Chroot), [USB flash installation medium](https://wiki.archlinux.org/title/USB_flash_installation_medium).
- Almacenamiento y seguridad: [Btrfs](https://wiki.archlinux.org/title/Btrfs), [Snapper](https://wiki.archlinux.org/title/Snapper), [dm-crypt](https://wiki.archlinux.org/title/Dm-crypt), [ufw](https://wiki.archlinux.org/title/Uncomplicated_Firewall).
- Escritorio y red: [Wayland](https://wiki.archlinux.org/title/Wayland), [NetworkManager](https://wiki.archlinux.org/title/NetworkManager), [PipeWire](https://wiki.archlinux.org/title/PipeWire), [WirePlumber](https://wiki.archlinux.org/title/WirePlumber), [XDG Base Directory](https://wiki.archlinux.org/title/XDG_Base_Directory).

## Neovim y LazyVim

- Neovim: [portada de la documentación](https://neovim.io/doc/), [documentación de usuario](https://neovim.io/doc/user/), [manual de usuario (usr_toc)](https://neovim.io/doc/user/usr_toc/), [novedades](https://neovim.io/doc/user/news/), [pack / vim.pack](https://neovim.io/doc/user/pack/), [diagnostic](https://neovim.io/doc/user/diagnostic/), [treesitter](https://neovim.io/doc/user/treesitter/), [quickfix](https://neovim.io/doc/user/quickfix/), [quickref](https://neovim.io/doc/user/quickref/), [manual de usuario](https://neovim.io/doc/user/usr_toc/), [Lua guide](https://neovim.io/doc/user/lua-guide/), [LSP](https://neovim.io/doc/user/lsp/), [health](https://neovim.io/doc/user/health/).
- LazyVim: [inicio](https://www.lazyvim.org/), [**keymaps**](https://www.lazyvim.org/keymaps), [configuración](https://www.lazyvim.org/configuration), [extras](https://www.lazyvim.org/extras). Gestor: [lazy.nvim](https://lazy.folke.io/).
- Plugins del IDE: [nvim-lspconfig](https://github.com/neovim/nvim-lspconfig), [mason.nvim](https://github.com/mason-org/mason.nvim), [blink.cmp](https://cmp.saghen.dev/), [conform.nvim](https://github.com/stevearc/conform.nvim), [nvim-lint](https://github.com/mfussenegger/nvim-lint), [nvim-dap](https://github.com/mfussenegger/nvim-dap), [neotest](https://github.com/nvim-neotest/neotest), [gitsigns.nvim](https://github.com/lewis6991/gitsigns.nvim), [snacks.nvim](https://github.com/folke/snacks.nvim).
- Guía de estudio: [[05 Neovim/Ruta de cero a pro]] · cómo usar la ayuda: [[05 Neovim/Ayuda integrada y documentación oficial]].
- Local: `:help`, `:checkhealth`, `Espacio s k`.

## Bash

- [Manual de GNU Bash](https://www.gnu.org/software/bash/manual/html_node/index.html): [expansiones](https://www.gnu.org/software/bash/manual/html_node/Shell-Expansions.html), [parámetros](https://www.gnu.org/software/bash/manual/html_node/Shell-Parameter-Expansion.html), [comillas](https://www.gnu.org/software/bash/manual/html_node/Quoting.html), [condicionales](https://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html), [set](https://www.gnu.org/software/bash/manual/html_node/The-Set-Builtin.html).
- [Devhints Bash](https://devhints.io/bash) (chuleta comunitaria) → [[04 Bash/Referencia rápida de Bash]].
- [ShellCheck](https://www.shellcheck.net/) · [ArchWiki: Bash](https://wiki.archlinux.org/title/Bash).
- Herramientas: [ripgrep](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md), [fd](https://github.com/sharkdp/fd), [fzf](https://github.com/junegunn/fzf), [jq](https://jqlang.org/manual/), [GNU sed](https://www.gnu.org/software/sed/manual/sed.html), [GNU awk](https://www.gnu.org/software/gawk/manual/gawk.html), [gum](https://github.com/charmbracelet/gum), [uv](https://docs.astral.sh/uv/).
- Local: `man bash`, `help <builtin>`.

## tmux y Herdr

- tmux: [wiki](https://github.com/tmux/tmux/wiki), [Formats](https://github.com/tmux/tmux/wiki/Formats), [TPM](https://github.com/tmux-plugins/tpm), [Getting Started](https://github.com/tmux/tmux/wiki/Getting-Started), [Clipboard](https://github.com/tmux/tmux/wiki/Clipboard), [manual](https://man.openbsd.org/tmux).
- Herdr: [docs](https://herdr.dev/docs/), [conceptos](https://herdr.dev/docs/concepts/), [teclado](https://herdr.dev/docs/keyboard/), [configuración](https://herdr.dev/docs/configuration/), [CLI](https://herdr.dev/docs/cli-reference/), [agentes](https://herdr.dev/docs/agents/), [integraciones](https://herdr.dev/docs/integrations/), [estado de sesión](https://herdr.dev/docs/session-state/).

## Fuentes locales (reflejan tu versión exacta)

```text
omarchy commands [--all]              catálogo de la CLI
omarchy menu keybindings --print      atajos vigentes
/usr/share/omarchy/default/           defaults de Omarchy (sólo lectura)
/usr/share/omarchy/shell/README.md    esquema de la shell y plugins
/usr/share/omarchy/bin/omarchy-*      implementación de cada orden
hyprctl -h                            referencia de hyprctl
~/.config/hypr/  ~/.config/omarchy/   tus ajustes
~/.config/nvim/  ~/.config/tmux/tmux.conf  ~/.config/herdr/config.toml
man bash · man tmux · man pacman · :help (Neovim)
```

Historial de cambios del vault: [[09 Fuentes y enlaces/Registro de revisión]] · [[Inicio|Volver al índice]]
