---
aliases: [Primeros pasos Omarchy, Desde Windows, Desde Mac, Primera semana]
tags: [omarchy, principiantes, aprendizaje]
actualizado: 2026-09-27
verificado_en: "Omarchy 4.0.4-1 · manual: Coming from Mac or Windows, FAQ"
---
# Primeros pasos con Omarchy

Guía para quien llega de **Windows o macOS** y empieza de cero. Complementa la [[00 Fundamentos/Ruta de aprendizaje]] (orden de estudio de todo el vault) y [[01 Omarchy/Atajos y flujos]] (atajos esenciales).

## Lo que cambia respecto a Windows/macOS

| Antes | En Omarchy |
|---|---|
| Ventanas superpuestas que arrastras | **Mosaico**: cada ventana nueva divide el espacio; nada se tapa. `Super+T` para flotar una |
| Menú Inicio, Spotlight, Raycast | `Super+Espacio` (lanzador) |
| Barra de tareas con ventanas abiertas | **Workspaces** numerados: `Super+1…0`; la barra muestra cuáles están ocupados |
| `Ctrl/Cmd+C`, `Ctrl/Cmd+V` | `Super+C` / `Super+V` funcionan en todas partes, **incluida la terminal** |
| Historial del portapapeles (`Win+V`) | `Super+Ctrl+V` |
| Recortes / captura | `ImprPant` |
| AirDrop / Nearby Share | LocalSend: `Super+Ctrl+S` |
| Centro de notificaciones | `Super+Shift+Alt+,` |
| Time Machine / Restaurar sistema | snapshots automáticos antes de cada actualización (no copian `/home`) |
| Tienda de apps | menú *Install* o `omarchy pkg add` |
| Configuración / Panel de control | menú *Setup*: abre y edita archivos de texto |
| Windows Update | `omarchy update` (sistema y apps a la vez) |
| Cerrar ventana ≠ cerrar app | cerrar la ventana (`Super+W`) **cierra la app** (no queda en segundo plano) |
| Ratón para todo | teclado primero; el ratón sigue funcionando |

> [!tip] La idea de fondo
> Toda la configuración son **archivos de texto** en `~/.config`: puedes leerlos, copiarlos a otro equipo y guardarlos en git. Nada se oculta tras paneles. Por eso el manual insiste en aprender los atajos y la terminal.

## Primer día (1 hora)

- [ ] `Super+K` y lee la lista de atajos por encima.
- [ ] Abre la terminal (`Super+Enter`), el navegador (`Super+Shift+Enter`) y el gestor de archivos (`Super+Shift+F`). Muévete entre ellos con `Super+Flechas`.
- [ ] Reparte ventanas en workspaces: `Super+Shift+2` envía una al 2; `Super+2` vas a él.
- [ ] Conéctate a la Wi-Fi (`Super+Ctrl+W`), revisa el audio (`Super+Ctrl+A`) y el Bluetooth (`Super+Ctrl+B`).
- [ ] Explora los menús: `Super+Espacio` (lanzador), `Super+Alt+Espacio` (apps), `Super+Escape` (sistema).
- [ ] Cambia el tema (`Super+Ctrl+Shift+Espacio`) y el fondo (`Super+Ctrl+Espacio`).
- [ ] Haz una captura (`ImprPant`) y pégala en algún sitio con `Super+V`.
- [ ] Bloquea (`Super+Ctrl+L`) y vuelve a entrar.

## Primera semana

| Día | Objetivo | Nota |
|---|---|---|
| 1 | Moverse por el escritorio sin ratón | [[01 Omarchy/Atajos y flujos]] |
| 2 | Terminal básica: `ls`, `cd`, `cp`, `mv`, `rm`, `man` | [[00 Fundamentos/Conceptos de Linux]], [[04 Bash/Bash]] |
| 3 | Instalar y quitar software; actualizar | [[02 Arch Linux/Paquetes y actualizaciones]] |
| 4 | Personalizar: tema, fuente, barra, teclado, monitores | [[01 Omarchy/Personalización paso a paso]] |
| 5 | Editor: Neovim nivel 0–1 | [[05 Neovim/Ruta de cero a pro]] |
| 6 | Sesiones persistentes con Herdr o tmux | [[07 Herdr/Herdr en Omarchy]] |
| 7 | Seguridad y copias; qué hacer si algo falla | [[01 Omarchy/Instalación seguridad y recuperación]], [[08 Recetas del equipo/Diagnóstico por síntomas]] |

## Preguntas frecuentes de principiante

**¿Cómo cambio entre distribuciones de teclado?** En `~/.config/hypr/input.lua` pon por ejemplo `kb_layout = "latam,us"` y añade `grp:alts_toggle` a `kb_options`; entonces `Alt izquierdo + Alt derecho` alterna. La barra muestra la distribución activa y permite cambiarla con un clic. (Este equipo usa `latam` en consola y en el escritorio.)

**¿Reloj en formato 12 horas?** Clic derecho sobre el reloj para ir cambiando de formato, o `omarchy bar set omarchy.clock format "dddd h:mm AP"`.

**¿Zona horaria u hora desfasada?** Menú *Update → Timezone* (`omarchy menu timezone`); si la hora se desvía, *Update → Time* (`omarchy update time`). Este equipo: `America/Bogota`.

**¿Dónde se guardan las capturas?** En las carpetas de `OMARCHY_SCREENSHOT_DIR` y `OMARCHY_SCREENRECORD_DIR`, definidas aquí en `~/.config/uwsm/env` (`~/Pictures/Screenshots`, `~/Videos/Screenrecordings`). Cámbialas ahí y reinicia la sesión.

**¿Cómo añado una impresora?** Lanzador → *Print Settings* → *Add*. Las de red suelen funcionar por IPP con la dirección de la impresora y la cola `ipp/print`. CUPS ya está activo en este equipo.

**¿Cómo quito programas preinstalados?** Menú *Remove → Package*, *Remove → Web App* o *Remove → Preinstalls* (todos los extras de golpe; se recuperan con `omarchy install preinstalls`).

**¿No puedo iniciar sesión en Google desde Chromium?** Menú *Install → Service → Chromium Account* y reinicia el navegador. (Tu navegador por defecto es Brave.)

**¿Las apps se ven enormes o diminutas?** Ajusta `GDK_SCALE` y la escala del monitor en `~/.config/hypr/monitors.lua` ([[03 Hyprland/Ventanas atajos y monitores]]).

**¿Bloq Mayús no funciona?** Es la tecla *Compose*; pulsa **ambos Shift** para las mayúsculas fijas ([[01 Omarchy/Escritorio red y periféricos#Entrada]]).

**¿Se congeló una ventana?** `Super+W`; si no responde, `hyprctl kill` y clic sobre ella. ¿Se fue la barra? `omarchy restart shell`.

## Errores típicos de principiante

- Actualizar con `sudo pacman -Syu` en lugar de `omarchy update` (Omarchy lo bloquea por buenos motivos).
- Editar archivos de `/usr/share/omarchy/` (se pierden en la siguiente actualización).
- Pegar un `hyprland.conf` de internet: desde Hyprland 0.55 la configuración es Lua.
- Usar `sudo` para abrir o editar archivos propios.
- Confiar en los snapshots como copia de seguridad de tus documentos.
- Instalar desde AUR sin revisar el PKGBUILD.

Fuentes: [Coming From Mac or Windows](https://omarchy.org/manual/coming-from-mac-or-windows/), [Getting Started](https://omarchy.org/manual/getting-started/), [Navigation](https://omarchy.org/manual/navigation/), [FAQ](https://omarchy.org/manual/faq/).
