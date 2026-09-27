---
aliases: [Perfil del equipo, Inventario]
tags: [recetas, inventario]
actualizado: 2026-09-27
verificado_en: "inspección local del 27-09-2026"
---
# Perfil de este equipo

Fotografía informativa del **27 de septiembre de 2026**; no guarda contraseñas ni claves. Antes de seguir una receta, compara con `omarchy version`, `hyprctl version` y `pacman -Q`.

## Hardware y sistema

| Componente | Observado | Comprobar con |
|---|---|---|
| Equipo | Lenovo IdeaPad Slim 3 15ABR8 | `cat /sys/class/dmi/id/product_version` |
| CPU / GPU | AMD Ryzen 7 7730U · Radeon integrada (Barcelo) | `lscpu`, `lspci -k` |
| Pantalla interna | SNF601BS1-1, 1920×1080@60 (`eDP-1`) | `hyprctl monitors all` |
| Disco | NVMe, partición `nvme0n1p2` cifrada con LUKS | `lsblk -f` |
| Sistema de archivos | Btrfs; snapper sólo para `/` | `findmnt /`, `sudo snapper list-configs` |
| Kernel | `linux-omarchy` 7.2.5 (alternativa instalada: `linux` 7.2.3) | `uname -r` |
| Arranque / sesión | Limine 12.8 · SDDM · uwsm | `systemctl is-enabled sddm` |
| Teclado / idioma / zona | `latam` (consola `la-latin1`) · `en_US.UTF-8` · `America/Bogota` | `localectl`, `timedatectl` |
| Grupos del usuario | `wheel`, `docker` (sin sudo = equivale a root), `vboxusers` | `id` |
| Cortafuegos | ufw activo | `systemctl is-active ufw` |
| Servidor SSH | **activo**, sólo con clave (sin contraseña) | `systemctl status sshd` |
| Impresión | CUPS activo | `lpstat -p` |

## Software

| Componente | Versión | Papel |
|---|---|---|
| Omarchy | `4.0.4-1`, canal `stable` | integración del escritorio |
| Hyprland | `0.56.2` (configuración Lua) | compositor |
| Neovim | `0.12.5` + LazyVim (extra neo-tree) | editor |
| Bash | `5.3.15` | intérprete |
| tmux | `3.7c` | multiplexor |
| Herdr | `0.8.2` (paquete) | workspaces y agentes |
| Obsidian | `1.13.7` | lectura de este vault |
| Terminal / navegador | Kitty / Brave | predeterminados |
| Tema / fuente | Matte Black / JetBrainsMono Nerd Font | apariencia |

## Personalizaciones detectadas

| Área | Detalle | Archivo |
|---|---|---|
| Monitores | pantalla interna + perfiles **casa** (LG FHD + TV LG) y **oficina** (Samsung + Lenovo); escala `1`, `GDK_SCALE=1` | `~/.config/hypr/monitors.lua` |
| Atajos | volumen `Super+Ctrl+Arriba/Abajo`, play/pausa `Super+Alt+P`, Red Pill Backup `Super+Alt+B` (flotante, centrado, 800×600, fijado) | `~/.config/hypr/bindings.lua` |
| Aspecto | opacidad activa 0.9 / inactiva 0.8 | `~/.config/hypr/looknfeel.lua` |
| Entrada | desplazamiento natural; gestos de 3 y 4 dedos | `~/.config/hypr/input.lua` |
| Autostart | `redpill-notify` | `~/.config/hypr/autostart.lua` |
| Inactividad | salvapantallas 150 s, bloqueo 300 s | `~/.config/omarchy/shell.json` |
| Capturas | `~/Pictures/Screenshots`; grabaciones `~/Videos/Screenrecordings` | — |
| Plugins de terceros | `george.omarkmeter`, `nosignal.motion-wallpaper`; `learn-omarchy.geometry` con manifiesto inválido | `~/.config/omarchy/plugins/` |
| Hooks | `post-update.d/`: `install-voxtype.hook`, `setup-agent.hook`, `setup-fingerprint.hook` | `~/.config/omarchy/hooks/` |
| Bash | `fastfetch` al abrir, `alias f='sudo openfortivpn'`, `~/.local/bin/env`, completado de OpenClaw | `~/.bashrc` |
| Neovim | sin números relativos, sin autoformato, portapapeles OSC 52 | `~/.config/nvim/lua/config/` |
| tmux / Herdr | prefijo `Ctrl+Espacio` | `tmux.conf`, `herdr/config.toml` |
| Herdr | integraciones de Claude, Codex, Copilot, opencode y pi | `herdr integration status` |

## Servicios de usuario relevantes

| Servicio | Para qué | Inspeccionar |
|---|---|---|
| Sunshine | streaming hacia Moonlight | `systemctl --user status app-dev.lizardbyte.app.Sunshine.service` |
| openclaw-gateway | pasarela de OpenClaw | `systemctl --user status openclaw-gateway` |
| voxtype | dictado (`F9`, `Super+Ctrl+X`) | `systemctl --user status voxtype` |
| omarchy-tailscale-receive | recibir archivos Taildrop | `systemctl --user status omarchy-tailscale-receive` |
| omarchy-crash-watch | aviso de programas caídos | `systemctl --user status omarchy-crash-watch` |

## Omark Meter (plugin externo)

Muestra reloj, clima de Girón y un visualizador de audio. El lector de audio usa `/usr/bin/python3`, `numpy`, `parec` y el *monitor* PipeWire de la salida predeterminada; el widget multimedia usa MPRIS por D-Bus. Casos resueltos en [[08 Recetas del equipo/Diagnóstico por síntomas#Casos reales de este equipo]].

> [!tip] Clima
> El plugin externo consulta `wttr.in/Giron,Santander,Colombia`. El clima **integrado** de Omarchy tiene su propia ubicación: `omarchy weather location` (actualmente **Girón**) y `omarchy weather location --set Giron` para fijarla. [Fuente](https://omarchy.org/manual/notices/).

Este vault documenta el sistema; no es una copia de `~/.config`. Tras cambios manuales o actualizaciones, actualiza esta nota y [[09 Fuentes y enlaces/Registro de revisión]].
