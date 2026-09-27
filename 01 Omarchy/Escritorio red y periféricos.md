---
aliases: [Periféricos, Red Omarchy, Energía]
tags: [omarchy, red, audio, monitores, energia]
actualizado: 2026-09-27
verificado_en: "Omarchy 4.0.4-1"
---
# Escritorio, red y periféricos en Omarchy

## Barra, inactividad y energía

La barra reúne workspaces, reloj, clima, red, audio, Bluetooth, batería e indicadores. Casi todos los widgets responden al clic izquierdo; algunos al derecho, central o rueda ([tabla oficial](https://omarchy.org/manual/the-top-bar/)). `Super+Ctrl+1…9` abre los paneles por teclado.

Inactividad en `~/.config/omarchy/shell.json`:

```json
"idle": { "screensaver": 150, "lock": 300 }
```

Ambos valores son **segundos desde la última actividad**, no intervalos encadenados: aquí el salvapantallas aparece a los 2,5 min y el bloqueo a los 5 min.

| Acción | Atajo / orden |
|---|---|
| Bloquear ahora | `Super+Ctrl+L` · `omarchy system lock` |
| Alternar bloqueo por inactividad | `Super+Ctrl+I` · `omarchy toggle idle` |
| Mantener despierto (presentaciones) | `omarchy toggle idle stay-awake` |
| Perfil de energía | `Super+Ctrl+P` · `omarchy powerprofiles set battery power-saver` |
| Suspender / hibernar | menú `Super+Escape`; hibernación requiere `omarchy hibernation setup` |
| Luz nocturna | `Super+Ctrl+N` · config en `~/.config/hypr/hyprsunset.conf` |

## Red

**NetworkManager** gestiona Wi-Fi y Ethernet. Panel: `Super+Ctrl+W`; terminal: `nmtui` o `nmcli`.

```bash
nmcli device status                 # interfaces y estado
nmcli device wifi list              # redes visibles
nmcli device wifi connect "SSID" --ask
omarchy network status --verbose
omarchy dns                         # proveedor DNS actual (Cloudflare|Google|DHCP|Custom)
omarchy network band 5              # fijar banda Wi-Fi
omarchy restart wifi                # desbloquear y reiniciar Wi-Fi
```

El **cortafuegos (ufw)** bloquea las conexiones entrantes por defecto; las herramientas que lo necesitan (SSH, Sunshine, LocalSend) abren sus puertos al instalarse con Omarchy. **Tailscale** crea una red privada *sobre* tu conexión: si la Wi-Fi cae, Tailscale también. Taildrop: `omarchy tailscale send <equipo> archivo`.

## Entrada

`~/.config/hypr/input.lua` controla distribución de teclado, repetición, ratón, touchpad y gestos:

```lua
hl.config({
  input = {
    kb_layout = "us,es",
    kb_options = "compose:caps,grp:alts_toggle",
    repeat_rate = 40,
    repeat_delay = 250,
    touchpad = { natural_scroll = true },
  },
})
hl.gesture({ fingers = 3, direction = "horizontal", action = "workspace" })
```

> [!info] En tu equipo
> `input.lua` activa `natural_scroll` y define gestos: 4 dedos horizontal = cambiar workspace; 3 dedos = mover el foco en la dirección contraria al deslizamiento.

> [!tip] Bloq Mayús
> Por defecto Omarchy usa `kb_options = "compose:caps,shift:both_capslock_cancel"`: `Caps Lock` actúa como tecla *Compose* (emojis y autocompletados de `~/.XCompose`) y **pulsar ambos Shift** activa las mayúsculas fijas. Si prefieres otra tecla Compose, usa `compose:ralt`. La distribución por defecto se toma de `/etc/vconsole.conf`.

## Monitores

`~/.config/hypr/monitors.lua` controla modo, posición y escala; detalles en [[03 Hyprland/Ventanas atajos y monitores]]. Las apps GTK leen `GDK_SCALE`, distinto de la escala del monitor; tras cambiarlo reabre la aplicación. Órdenes útiles: `hyprctl monitors all`, `omarchy hyprland monitor scaling up`, `Super+/` y `Super+Alt+/`.

## Audio

**PipeWire** transporta el audio, **WirePlumber** decide rutas y dispositivos y `pipewire-pulse` sirve a apps PulseAudio. Panel: `Super+Ctrl+A`.

```bash
wpctl status                        # grafo: dispositivos, sinks, sources, streams
pactl get-default-sink
omarchy audio output switch         # siguiente salida conservando el silencio
omarchy restart audio               # recuperar servicios y USB atascados
```

## Bluetooth, impresoras y archivos

- Bluetooth: `Super+Ctrl+B`, `bluetoothctl`, `omarchy restart bluetooth`.
- Impresoras: CUPS; ver la [FAQ](https://omarchy.org/manual/faq/).
- Archivos: `Super+Shift+F` (Nautilus); `Ctrl+L` escribe una ruta. `Super+Shift+Alt+F` abre en el directorio de la terminal activa.
- Capturas: `ImprPant`; grabaciones: `Alt+ImprPant`. Destinos de este equipo en [[08 Recetas del equipo/Perfil de este equipo]].

## Autenticación física

Huella: `omarchy setup security fingerprint`; llave FIDO2: `omarchy setup security fido2`. Ambas afectan a `sudo`, polkit y (la huella) al bloqueo de pantalla. Se revierten con `omarchy remove security …`.

Fuentes: [barra](https://omarchy.org/manual/the-top-bar/), [interruptores e inactividad](https://omarchy.org/manual/toggles-idle-screensaver/), [red](https://omarchy.org/manual/networking/), [monitores](https://omarchy.org/manual/monitors/), [entrada](https://omarchy.org/manual/keyboard-mouse-trackpad/), [suspensión](https://omarchy.org/manual/system-sleep/), [autenticación física](https://omarchy.org/manual/hardware-authentication/), [ArchWiki: NetworkManager](https://wiki.archlinux.org/title/NetworkManager), [ArchWiki: PipeWire](https://wiki.archlinux.org/title/PipeWire), [Hyprland: Variables](https://wiki.hypr.land/Configuring/Basics/Variables/).
