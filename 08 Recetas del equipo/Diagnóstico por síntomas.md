---
aliases: [Troubleshooting, Solución de problemas]
tags: [diagnostico, recetas]
actualizado: 2026-09-27
verificado_en: "Omarchy 4.0.4-1 · Hyprland 0.56.2"
---
# Diagnóstico por síntomas

Empieza preguntando **qué capa falla** ([[00 Fundamentos/Mapa del sistema#Cómo averiguar quién controla un problema]]). Copia el mensaje de error exacto **antes** de restablecer nada.

## Secuencia de inspección segura (no modifica nada)

```bash
omarchy version; omarchy version channel; omarchy version pkgs
hyprctl version | head -1
hyprctl configerrors
omarchy cmd missing
systemctl --failed; systemctl --user --failed
journalctl -b -p err -n 50 --no-pager
journalctl --user -b -n 80 --no-pager
```

> [!info] Sobre `omarchy-debug`
> El manual oficial sugiere `omarchy-debug` para generar un informe, pero **no existe en Omarchy 4.0.4**. La secuencia anterior reúne la misma información.

## Tabla de síntomas

| Síntoma | Primero comprueba | Responsable probable | Arreglo habitual |
|---|---|---|---|
| Un atajo `Super+…` no responde | `Super+K`; `hyprctl configerrors`; `hyprctl binds \| grep -i tecla` | Hyprland / error Lua / conflicto | corregir `bindings.lua`, `hyprctl reload` |
| Sin atajos tras editar Lua | `hyprctl configerrors` desde el lanzador o `Ctrl+Alt+F2` | error de sintaxis | restaurar `.bak`; último recurso `omarchy refresh hyprland` ⚠️ |
| Monitor no aparece | `hyprctl monitors all`; `monitors.lua` | cable / modo / regla | `hyprctl reload`; `omarchy hw recover internal monitor` |
| Apps gigantes o diminutas | `GDK_SCALE` en `monitors.lua`; `hyprctl monitors` (escala) | escala | `GDK_SCALE` 1 en pantallas estándar; `Super+/` |
| Barra o plugin desapareció | `omarchy plugin list`; `omarchy plugin validate RUTA`; `journalctl --user -b \| grep -i quickshell` | Omarchy Shell / plugin | `omarchy-shell shell rescanPlugins`; `omarchy restart shell` |
| Plugin propio no aparece en la lista | `omarchy plugin validate ~/.config/omarchy/plugins/<id>` | `manifest.json` inválido | corregir el JSON |
| Script no encuentra módulo Python | `type -a python3`; `/usr/bin/python3 -c 'import modulo'` | PATH / mise / paquete Python | usar `/usr/bin/python3` o instalar en el entorno correcto |
| Sin música en widget multimedia | `/usr/bin/python3 ~/.config/omarchy/plugins/george.omarkmeter/get-media.py`; `busctl --user list \| grep mpris` | reproductor, D-Bus/MPRIS o plugin | abrir el reproductor; revisar dependencias Python |
| Visualizador sin barras | `pactl get-default-sink`; `pactl list short sources` | PipeWire / salida cambiada | elegir la salida correcta en `Super+Ctrl+A` |
| Sin sonido | `wpctl status`; panel de audio | salida equivocada / PipeWire | `omarchy audio output switch`; `omarchy restart audio` |
| Altavoces del portátil suenan mal | `omarchy audio tuning status` | ajuste de altavoces | `omarchy audio tuning off` |
| Sin red | `nmcli device status`; `ping -c3 1.1.1.1`; `resolvectl status` | NetworkManager / DNS | `omarchy restart wifi`; `omarchy dns DHCP` |
| Wi-Fi/Bluetooth/touchpad bloqueado | menú *Hardware* (`Super+Ctrl+H`) | driver/servicio | `omarchy restart wifi\|bluetooth\|trackpad` |
| `Caps Lock` no pone mayúsculas | — | es la tecla *Compose* en Omarchy | pulsa **ambos Shift**; o cambia `kb_options` |
| Contraseña bloqueada tras fallos | `faillock --user castarrillo` | pam_faillock | `Ctrl+Alt+F2`, root, `faillock --reset --user castarrillo` |
| `pacman -Syu` se niega | mensaje del hook | guardia de Omarchy | usar `omarchy update` |
| La actualización falló | `/tmp/omarchy-update.log`; `omarchy update analyze logs` | paquete / migración / red | corregir y repetir; si no arranca, snapshot en Limine |
| Error de firmas de paquetes | salida de pacman | llaveros | `omarchy update keyring` |
| Un programa se cierra solo | `coredumpctl list`; `journalctl --user -b` | fallo de la app | `omarchy agent crash <pid>` para un análisis guiado |
| No funciona `gd` en Neovim | `<leader>cl`; `:checkhealth vim.lsp` | falta servidor LSP | `:Mason` o `:LazyExtras` del lenguaje |
| `nvim +Tutor` falla | — | LazyVim desactiva `tutor` | `nvim --clean +Tutor` |
| tmux/Herdr no recibe una tecla | ayuda activa; `hyprctl binds` | la captura una capa exterior (Hyprland) | cambiar el atajo en la capa exterior |
| Copiar en tmux no llega al sistema | `tmux show -g set-clipboard` | OSC 52 / terminal | ver [[06 Tmux/Sesiones y paneles#Portapapeles]] |
| Disco lleno | `df -h`; `sudo btrfs filesystem usage /`; `du -h -d1 ~ \| sort -h` | caché / snapshots / descargas | `omarchy update pkg prune`; revisar `sudo snapper list` |

## Principios

1. **Una capa cada vez**: reinicia sólo la pieza sospechosa (`omarchy restart …`, `systemctl --user restart …`) antes que el equipo.
2. **Lee el registro** antes de buscar en internet: el mensaje exacto ahorra tiempo.
3. **Respaldo antes de restablecer**: `omarchy refresh …` y `omarchy reinstall …` sobrescriben.
4. **Usa el snapshot** si el sistema no arranca tras actualizar ([[01 Omarchy/Instalación seguridad y recuperación#Snapshots y reversión]]).
5. **Anota la solución** en [[08 Recetas del equipo/Perfil de este equipo]] si es específica de este equipo.

## Casos reales de este equipo

- `get-media.py` no importaba `dbus` desde el Python de **mise** (primero en el `PATH`); el Python del sistema sí. Solución: ejecutar con `/usr/bin/python3`.
- `get-audio.py` necesitaba `numpy` instalado para `/usr/bin/python3` (`python-numpy` de pacman), no en mise.
- Si un proceso cambia la salida de audio predeterminada, el monitor del sink anterior deja de mostrar señal en el visualizador.
- El plugin `learn-omarchy.geometry` no aparece en `omarchy plugin list` porque su `manifest.json` no es JSON válido (verificado el 27-09-2026).

Fuentes: [Omarchy: solución de problemas](https://omarchy.org/manual/troubleshooting/), [Omarchy: FAQ](https://omarchy.org/manual/faq/), [ArchWiki: systemd](https://wiki.archlinux.org/title/Systemd), [ArchWiki: General troubleshooting](https://wiki.archlinux.org/title/General_troubleshooting), [Neovim: health](https://neovim.io/doc/user/health/).
