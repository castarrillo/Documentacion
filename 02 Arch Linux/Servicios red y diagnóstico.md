---
aliases: [systemd, journalctl, Diagnóstico Arch]
tags: [arch, systemd, journald, red, audio, diagnostico]
actualizado: 2026-09-27
verificado_en: "systemd · NetworkManager · PipeWire/WirePlumber · Omarchy 4.0.4-1"
---
# Servicios, red, audio y diagnóstico

## systemd: servicios del sistema y del usuario

`systemd` arranca y vigila procesos. Hay **dos instancias**:

- **Sistema** (`systemctl …`, requiere `sudo` para cambiar): NetworkManager, ufw, SDDM, bluetooth.
- **Usuario** (`systemctl --user …`, sin `sudo`): PipeWire, WirePlumber, portales, Sunshine, servicios de Omarchy (`omarchy-crash-watch`, `omarchy-sleep-lock`…) y la propia sesión (`wayland-wm@hyprland.desktop.service`, vía uwsm).

| Objetivo | Orden |
|---|---|
| Estado de un servicio | `systemctl status NetworkManager` · `systemctl --user status pipewire` |
| Arrancar / parar / reiniciar | `sudo systemctl start\|stop\|restart nombre` |
| Activar al arranque (y arrancar ya) | `sudo systemctl enable --now nombre` |
| Desactivar | `sudo systemctl disable --now nombre` |
| Unidades con error | `systemctl --failed` · `systemctl --user --failed` |
| Servicios en ejecución | `systemctl --user list-units --type=service` |
| Temporizadores | `systemctl list-timers` |
| Ver la definición de una unidad | `systemctl cat nombre` |
| Modificar sin tocar el original | `sudo systemctl edit nombre` (crea un *drop-in*) |
| Recargar tras editar unidades | `systemctl --user daemon-reload` |

> [!tip] Nombres inesperados
> Las apps lanzadas por uwsm se ejecutan como unidades `app-…`. Un proceso lanzado por Hyprland puede no tener la unidad con el nombre que imaginas: búscalo con `systemctl --user list-units | grep -i nombre` antes de adivinar.

## journald: leer registros

| Objetivo | Orden |
|---|---|
| Advertencias y errores de este arranque | `journalctl -b -p warning` |
| Sólo errores | `journalctl -b -p err` |
| Sesión de usuario | `journalctl --user -b -n 100` |
| Un servicio concreto | `journalctl -u NetworkManager -b` · `journalctl --user -u pipewire -b` |
| Seguir en vivo | `journalctl -f` (añade `-u nombre` para filtrar) |
| Arranque anterior (tras un cuelgue) | `journalctl -b -1 -e` |
| Mensajes del kernel | `journalctl -k -b` |
| Por intervalo | `journalctl --since "10 min ago"` |
| Espacio usado | `journalctl --disk-usage` |

Prioridades: `emerg 0 · alert 1 · crit 2 · err 3 · warning 4 · notice 5 · info 6 · debug 7`.

Si un programa se cae, systemd-coredump guarda un volcado: `coredumpctl list` y `coredumpctl info PID`. Omarchy vigila estos fallos con `omarchy-crash-watch` y puede abrir un agente con `omarchy agent crash`.

## Red

| Objetivo | Orden |
|---|---|
| Panel gráfico | `Super+Ctrl+W` |
| Estado de interfaces | `nmcli device status` · `ip -brief address` |
| Redes Wi-Fi | `nmcli device wifi list` |
| Conectar | `nmcli device wifi connect "SSID" --ask` · `nmtui` |
| Rutas y puerta de enlace | `ip route` |
| DNS en uso | `resolvectl status` · `omarchy dns` |
| ¿Hay conectividad? | `ping -c3 1.1.1.1` (IP) y `ping -c3 archlinux.org` (DNS) |
| Puertos en escucha | `ss -tulpn` |
| Cortafuegos | `sudo ufw status verbose` |
| Registro de NetworkManager | `journalctl -u NetworkManager -b` |

Diagnóstico por capas: **interfaz** (¿`connected`?) → **IP** (¿tiene dirección?) → **ruta** (¿puerta de enlace?) → **DNS** (¿resuelve nombres?). Si `ping 1.1.1.1` funciona pero `ping archlinux.org` no, el problema es DNS. Tailscale añade una interfaz `tailscale0`; no sustituye a la LAN.

## Audio

**PipeWire** transporta audio y vídeo; **WirePlumber** decide qué dispositivo usa cada flujo; **pipewire-pulse** sirve a las apps PulseAudio (`pactl` sigue funcionando).

| Objetivo | Orden |
|---|---|
| Vista completa del grafo | `wpctl status` |
| Salida por defecto | `pactl get-default-sink` · `wpctl inspect @DEFAULT_AUDIO_SINK@` |
| Listar salidas / entradas | `pactl list short sinks` / `pactl list short sources` |
| Volumen | `wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%+` · `omarchy audio output volume raise` |
| Cambiar de salida | `omarchy audio output switch` · `Shift+Silencio` |
| Reiniciar la pila | `omarchy restart audio` · `systemctl --user restart pipewire wireplumber` |

Un *monitor* (`<salida>.monitor`) es la fuente que «escucha» lo que suena en una salida: el visualizador de Omark Meter lo lee, así que si cambia la salida por defecto puede quedarse sin señal.

## Disco, memoria y procesos

| Objetivo | Orden |
|---|---|
| Espacio por sistema de archivos | `df -h` |
| Tamaño de un directorio | `du -sh ruta` · `du -h -d1 ~ \| sort -h` |
| Uso real en Btrfs | `sudo btrfs filesystem usage /` |
| Memoria | `free -h` |
| Procesos (interactivo) | `btop` (`Super+Ctrl+T`) |
| Qué proceso usa un archivo o puerto | `lsof ruta` · `ss -tulpn` |
| Salud del disco NVMe | `sudo smartctl -a /dev/nvme0n1` (paquete `smartmontools`) |
| Dispositivos | `lsblk -f` · `lspci -k` · `lsusb` |

Recetas por síntoma: [[08 Recetas del equipo/Diagnóstico por síntomas]].

Fuentes: [systemd](https://wiki.archlinux.org/title/Systemd), [systemd/User](https://wiki.archlinux.org/title/Systemd/User), [systemd/Journal](https://wiki.archlinux.org/title/Systemd/Journal), [Core dump](https://wiki.archlinux.org/title/Core_dump), [NetworkManager](https://wiki.archlinux.org/title/NetworkManager), [Network configuration](https://wiki.archlinux.org/title/Network_configuration), [PipeWire](https://wiki.archlinux.org/title/PipeWire), [WirePlumber](https://wiki.archlinux.org/title/WirePlumber), [Btrfs](https://wiki.archlinux.org/title/Btrfs), [Omarchy: red](https://omarchy.org/manual/networking/).
