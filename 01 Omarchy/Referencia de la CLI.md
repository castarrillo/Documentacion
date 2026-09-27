---
aliases: [omarchy CLI, Comandos omarchy]
tags: [omarchy, cli, referencia]
actualizado: 2026-09-27
verificado_en: "Omarchy 4.0.4-1 · omarchy commands"
---
# Referencia de la CLI `omarchy`

Catálogo comentado de `omarchy commands` en este equipo, agrupado por uso. `<…>` = obligatorio, `[…]` = opcional. Para la sintaxis exacta usa siempre `omarchy <grupo> [acción] --help`. Nota de uso: [[01 Omarchy/CLI configuración y mantenimiento]].

> [!warning] Órdenes que sobrescriben o borran
> Las marcadas con ⚠️ sustituyen configuraciones, eliminan datos o requieren `sudo`. Lee su `--help` antes de usarlas.

## Mantenimiento y paquetes

| Orden | Qué hace |
|---|---|
| `omarchy update [-y]` | Actualización completa: snapshot, paquetes, migraciones, hooks, AUR, mise |
| `omarchy update available` | ¿Hay actualización de Omarchy? |
| `omarchy update firmware` | Firmware por fwupd |
| `omarchy update orphan pkgs` | Revisar y quitar paquetes huérfanos |
| `omarchy update pkg prune` | Podar la caché de pacman |
| `omarchy update analyze logs` | Buscar fallos conocidos en el último registro |
| `omarchy update keyring` | Reparar llaveros de firmas |
| `omarchy update time` | Reiniciar sincronización horaria |
| `omarchy version` / `version channel` / `version pkgs` | Versión, canal/espejo, fecha de la última actualización |
| `omarchy channel current` / `channel set <stable\|rc\|edge\|dev>` | Canal de paquetes |
| `omarchy migrate [--pending]` | Ejecutar / listar migraciones |
| `omarchy pkg add <pkgs…>` / `pkg drop <pkgs…>` | Instalar si faltan / quitar si están |
| `omarchy pkg present\|missing <pkgs…>` | Comprobaciones para scripts (código de salida) |
| `omarchy pkg install` / `pkg remove` | Selector interactivo (fzf) |
| `omarchy pkg aur add <pkgs…>` / `pkg aur install` | AUR directo / selector |
| `omarchy snapshot <create\|restore>` | Snapshots con snapper |
| `omarchy cmd missing` / `cmd present` | ¿Faltan órdenes requeridas? |

## Apariencia

| Orden | Qué hace |
|---|---|
| `omarchy theme list` / `current` / `set <tema>` | Temas |
| `omarchy theme switcher` / `bg-switcher` | Selectores gráficos |
| `omarchy theme install <git-url>` / `update` / `remove [tema]` | Temas de terceros |
| `omarchy theme bg next` / `bg set <imagen>` / `bg current` | Fondos |
| `omarchy font list` / `current` / `set <fuente>` | Fuente monoespaciada |
| `omarchy display text size [tamaño\|reset]` | Escalar texto en shell, GTK y terminales |
| `omarchy bar position <top\|bottom\|left\|right>` | Posición de la barra |
| `omarchy bar move <id> --section <left\|center\|right> [--index N]` | Mover widget |
| `omarchy bar put <id> --after <id>` / `bar set <id> <clave> <valor>` | Añadir / ajustar widget |
| `omarchy bar reset` / `bar defaults` | Volver al diseño por defecto |
| `omarchy toggle bar` | Ocultar/mostrar barra |
| `omarchy branding about\|screensaver <image\|text\|reset>` | Arte personalizado |
| `omarchy plymouth list` / `set by theme <tema>` / `reset` | Pantalla de arranque |

## Shell y plugins

| Orden | Qué hace |
|---|---|
| `omarchy plugin list [--json]` | Plugins detectados y estado |
| `omarchy plugin add <git-url> [--enable]` | Instalar desde git |
| `omarchy plugin enable <id> [--section …]` / `disable <id>` | Activar / desactivar |
| `omarchy plugin clone <id> [--edit]` | Copiar un plugin integrado a tu configuración |
| `omarchy plugin validate <carpeta>` | Validar el manifiesto |
| `omarchy plugin update [id]` / `remove [id]` | Mantener plugins git |
| `omarchy shell <target> <método> [args]` | Llamada IPC a la shell (p. ej. `omarchy shell shell listPlugins`) |
| `omarchy restart shell` | Reiniciar Omarchy Shell |
| `omarchy osd -m "texto" -p 50` | Mostrar un OSD |
| `omarchy notification send "Título" "Texto"` | Notificación |

## Hyprland, ventanas y monitores

| Orden | Qué hace |
|---|---|
| `omarchy hyprland monitor scaling [up\|down\|ESCALA]` | Escala del monitor enfocado |
| `omarchy hyprland monitor internal <on\|off\|toggle>` | Pantalla del portátil |
| `omarchy hyprland monitor internal mirror <on\|off\|toggle>` | Espejo |
| `omarchy hyprland window pop` / `gaps toggle` / `transparency toggle` | Ventana |
| `omarchy hyprland workspace layout toggle` | *dwindle* ↔ *scrolling* |
| `omarchy hyprland toggle <flag> [on\|off]` | Interruptores permanentes |
| `omarchy brightness display [+N%\|N%-\|N%]` | Brillo (DDC/CI con `brightness display ddc`) |

## Audio, red y dispositivos

| Orden | Qué hace |
|---|---|
| `omarchy audio output switch` / `output volume <raise\|lower\|+N>` | Salida y volumen |
| `omarchy audio input mute` | Silenciar micrófono |
| `omarchy network status [--verbose]` / `network speedtest` | Red |
| `omarchy network qr` / `network password <iface>` | Compartir Wi-Fi |
| `omarchy dns [Cloudflare\|Google\|DHCP\|Custom]` | Proveedor DNS |
| `omarchy bluetooth power <on\|off\|toggle>` / `bluetooth device …` | Bluetooth |
| `omarchy powerprofiles set <ac\|battery> <perfil>` | Perfil de energía |
| `omarchy toggle nightlight\|idle\|touchpad\|suspend\|screensaver` | Interruptores |
| `omarchy restart audio\|wifi\|bluetooth\|trackpad` | Recuperar un subsistema |
| `omarchy hibernation available\|setup\|remove` | Hibernación |

## Captura, utilidades y recordatorios

| Orden | Qué hace |
|---|---|
| `omarchy capture screenshot [smart\|region\|windows\|fullscreen]` | Captura |
| `omarchy capture screenrecording [--with-desktop-audio] [--with-microphone-audio]` | Grabación |
| `omarchy capture text` / `capture qr` | OCR / leer QR |
| `omarchy reminder <minutos> [mensaje]` / `reminder show` / `clear` | Recordatorios |
| `omarchy weather location [--set Lugar]` | Ubicación del clima integrado |
| `omarchy share <clipboard\|file\|folder>` | LocalSend |
| `omarchy transcode [entrada] [formato]` | Convertir imágenes/vídeos |
| `omarchy disk speedtest` | Velocidad del disco |

## Aplicaciones y entornos

| Orden | Qué hace |
|---|---|
| `omarchy default browser\|editor\|terminal\|agent <valor>` | Aplicaciones predeterminadas |
| `omarchy install dev-env <lenguaje>` / `remove dev env <lenguaje>` | Entornos (node, python, rust, go…) vía mise |
| `omarchy install docker dbs` | Bases de datos en contenedores |
| `omarchy install editor <vscode\|zed\|helix\|emacs>` | Editores con tema |
| `omarchy install terminal <alacritty\|foot\|ghostty\|kitty>` | Terminal |
| `omarchy install service <tailscale\|sunshine\|1password\|…>` | Servicios |
| `omarchy webapp install` / `webapp remove` | Web apps |
| `omarchy tui install` / `tui remove` | Lanzadores de TUI |
| `omarchy launch terminal tmux` / `launch terminal herdr` | Terminal con sesión persistente |
| `omarchy mise install <paquete>` | Envoltorio mise para una herramienta |

## Seguridad y sistema

| Orden | Qué hace |
|---|---|
| `omarchy setup security fingerprint\|fido2` | Huella / llave FIDO2 para sudo y polkit |
| `omarchy setup security sshd [--key=…]` ⚠️ | Activar SSH y abrir el cortafuegos |
| `omarchy setup security sudoless docker` ⚠️ | Docker sin sudo (equivale a root) |
| `omarchy remove security <fingerprint\|fido2\|sshd\|sudoless docker>` | Revertir |
| `omarchy drive password` ⚠️ | Cambiar contraseña LUKS |
| `omarchy sudo passwordless [MIN]` ⚠️ | sudo sin contraseña temporal |
| `omarchy system lock\|logout\|reboot\|shutdown` | Sesión y energía |
| `omarchy hook install <evento> <archivo>` / `hook <evento>` | Hooks |

## Restablecer (último recurso)

| Orden | Qué sobrescribe |
|---|---|
| `omarchy refresh config <ruta>` | Un archivo de `~/.config` (guarda respaldo) |
| `omarchy refresh hyprland` ⚠️ | Todos los `~/.config/hypr/*.lua` |
| `omarchy refresh shell` ⚠️ | `shell.json` |
| `omarchy refresh tmux` / `herdr` / `hyprsunset` ⚠️ | Su configuración de usuario |
| `omarchy refresh pacman` / `limine` / `plymouth` / `sddm` ⚠️ | Configuración del sistema (sudo) |
| `omarchy reinstall configs` ⚠️ | Todas tus configuraciones |
| `omarchy reinstall` / `reinstall pkgs` ⚠️ | Paquetes y configuraciones por defecto |
| `omarchy setup factory reset` ⚠️⚠️ | Restablecimiento de fábrica |

Fuentes: salida local de `omarchy commands`, [manual: Omarchy CLI](https://omarchy.org/manual/omarchy-cli/).
