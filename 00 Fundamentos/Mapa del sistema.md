---
aliases: [Arquitectura, Capas del sistema]
tags: [fundamentos, arquitectura]
actualizado: 2026-09-27
verificado_en: "Omarchy 4.0.4-1 · Hyprland 0.56.2"
---
# Mapa del sistema

```text
Hardware AMD (Ryzen 7 7730U + Radeon) + firmware UEFI
    ↓
Limine (bootloader; menú de snapshots) → Plymouth (pide la clave LUKS)
    ↓
kernel Linux → desbloquea LUKS → monta la raíz Btrfs
    ↓
systemd (PID 1) → servicios del sistema: NetworkManager, ufw, SDDM, bluetooth…
    ↓
SDDM (inicio de sesión) → uwsm → sesión systemd de usuario
    ↓
Hyprland (compositor Wayland) ── ventanas de aplicaciones (Wayland nativo o XWayland)
    ├── Omarchy Shell (Quickshell/QML): barra, menú, paneles, OSD, notificaciones, bloqueo
    ├── servicios de usuario: PipeWire, WirePlumber, portales XDG, Sunshine…
    └── terminal → Bash → tmux o Herdr → Neovim/LazyVim, git, agentes…

Arch Linux: paquetes y bibliotecas que sostienen todas las capas.
Omarchy 4: integración, valores predeterminados, CLI `omarchy`, temas, migraciones y actualizaciones.
```

> [!info] En tu equipo
> Verificado: disco `nvme0n1p2` cifrado con **LUKS**, raíz **Btrfs** con configuración de **snapper** sólo para `/`, inicio de sesión **SDDM**, sesión lanzada por **uwsm** (`wayland-wm@hyprland.desktop.service`) y cortafuegos **ufw** activo.

## Qué hace cada capa

| Capa | Qué es | Qué controla | Nota |
|---|---|---|---|
| **Linux** | kernel | dispositivos, memoria, procesos, sistemas de archivos | `uname -r` |
| **Arch Linux** | distribución *rolling release* | paquetes, bibliotecas, `pacman`, systemd | [[02 Arch Linux/Arch Linux]] |
| **systemd** | gestor de servicios e inicio | servicios del sistema y de usuario, registros (journald) | [[02 Arch Linux/Servicios red y diagnóstico]] |
| **Wayland** | protocolo | comunicación entre aplicaciones y compositor | — |
| **Hyprland** | compositor Wayland en mosaico | ventanas, workspaces, entrada, monitores, atajos | [[03 Hyprland/Hyprland en Omarchy]] |
| **Quickshell** | framework QML | superficies de Omarchy Shell | [[01 Omarchy/Shell temas y plugins]] |
| **Omarchy** | distribución «omakase» basada en Arch | selección de apps, defaults, CLI, temas, migraciones | [[01 Omarchy/Omarchy 4]] |
| **Terminal** | emulador (Alacritty/foot/Ghostty/Kitty) | dibuja texto y entrega teclas | [[01 Omarchy/Referencia de la CLI]] (`omarchy default terminal`) |
| **Bash** | intérprete de órdenes | expansión, variables, scripts | [[04 Bash/Bash]] |
| **tmux / Herdr** | multiplexores | sesiones persistentes, paneles | [[06 Tmux/Tmux en Omarchy]], [[07 Herdr/Herdr en Omarchy]] |
| **Neovim / LazyVim** | editor / distribución de configuración | edición, LSP, plugins | [[05 Neovim/Neovim y LazyVim]] |

> [!important] Tres «ventanas» distintas
> Una **ventana de Hyprland** es gráfica; una **ventana de tmux** vive *dentro* de la terminal; una **ventana de Neovim** es una vista dentro del editor. Del mismo modo, un *workspace* de Hyprland no es un *workspace* de Herdr. Ver [[00 Fundamentos/Glosario]].

## Dónde se guarda cada cosa

| Ruta | Responsable | Ejemplo | ¿Editar? |
|---|---|---|---|
| `/usr/share/omarchy/` | paquete Omarchy | binarios `bin/`, `default/`, `themes/`, `migrations/`, `shell/` | **No** (la actualización lo sustituye) |
| `/etc/` | sistema Arch | `pacman.conf`, servicios, red | Con `sudoedit` y criterio |
| `~/.config/hypr/` | tus ajustes Hyprland | `hyprland.lua`, `bindings.lua`, `monitors.lua`… | **Sí** |
| `~/.config/omarchy/` | Omarchy de usuario | `shell.json`, `plugins/`, `hooks/`, `extensions/`, `themes/`, `backgrounds/` | **Sí** |
| `~/.config/nvim/` | Neovim/LazyVim | `lua/config/`, `lua/plugins/`, `lazy-lock.json` | **Sí** |
| `~/.config/tmux/tmux.conf` | tmux | prefijo y paneles | **Sí** |
| `~/.config/herdr/config.toml` | Herdr | teclas y presentación | **Sí** |
| `~/.bashrc` | Bash interactivo | aliases, funciones, exports | **Sí** |
| `~/.local/share/`, `~/.cache/` | datos y caché de apps | plugins de Neovim, miniaturas | Rara vez |
| `~/Documents/Documentacion/` | tú | este vault | **Sí** |

> [!important] Regla de personalización
> Lo que vive en `/usr/share/omarchy/` pertenece a Omarchy y se reemplaza al actualizar. Tus cambios van en `~/.config/`, que Omarchy carga **después** de sus valores predeterminados. Ver [[01 Omarchy/CLI configuración y mantenimiento]].

## Cómo averiguar quién controla un problema

| Síntoma | Capa probable | Primer paso |
|---|---|---|
| Una app no abre | paquete / PATH / servicio | `command -v app`, `pacman -Q app`, `journalctl --user -b` |
| Ventana, monitor o atajo `Super+…` | Hyprland | `hyprctl configerrors`, [[03 Hyprland/Configuración Lua y depuración]] |
| Barra, menú, OSD, plugin | Omarchy Shell | `omarchy plugin list`, [[01 Omarchy/Shell temas y plugins]] |
| «command not found», comillas raras | Bash / PATH | `type orden`, [[04 Bash/Bash]] |
| Atajo dentro del editor | Neovim/LazyVim | `:verbose nmap <tecla>` |
| Atajo dentro de paneles | tmux o Herdr | ayuda activa `Prefijo, luego ?` |
| Red, sonido, Bluetooth | systemd + servicios | [[02 Arch Linux/Servicios red y diagnóstico]] |

Índice de síntomas completo: [[08 Recetas del equipo/Diagnóstico por síntomas]].

Fuentes: [manual Omarchy](https://omarchy.org/manual/), [ArchWiki: Arch boot process](https://wiki.archlinux.org/title/Arch_boot_process), [ArchWiki: systemd](https://wiki.archlinux.org/title/Systemd), [Hyprland Wiki](https://wiki.hypr.land/), [Quickshell](https://quickshell.org/docs/v0.2.1/).
