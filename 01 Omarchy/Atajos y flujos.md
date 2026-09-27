---
aliases: [Atajos esenciales, Hotkeys]
tags: [omarchy, atajos]
actualizado: 2026-09-27
verificado_en: "Omarchy 4.0.4-1 · omarchy menu keybindings --print"
---
# Atajos y flujos de Omarchy

**Super** es la tecla Windows/Command. Esta nota recoge los **atajos esenciales**; la lista íntegra está en [[01 Omarchy/Referencia completa de atajos]]. Fuente de verdad en este equipo: `Super+K` (buscador) o `omarchy menu keybindings --print`; referencia oficial: [Hotkeys](https://omarchy.org/manual/hotkeys/).

## Los 20 atajos que conviene memorizar

| Acción | Atajo |
|---|---|
| Lanzador / menú de apps / menú del sistema | `Super+Espacio` / `Super+Alt+Espacio` / `Super+Escape` |
| Buscar atajos | `Super+K` |
| Terminal / tmux / Herdr | `Super+Enter` / `Super+Alt+Enter` / `Super+Ctrl+Enter` |
| Navegador / archivos / editor / Obsidian | `Super+Shift+Enter` / `Super+Shift+F` / `Super+Shift+N` / `Super+Shift+O` |
| Enfocar ventana / intercambiar | `Super+Flechas` / `Super+Shift+Flechas` |
| Cerrar / flotante / pantalla completa / ancho completo | `Super+W` / `Super+T` / `Super+F` / `Super+Alt+F` |
| Ir al workspace N / mover ventana a N | `Super+1…0` / `Super+Shift+1…0` |
| Mover ventana a N sin seguirla | `Super+Shift+Alt+1…0` |
| Workspace siguiente / anterior / último usado | `Super+Tab` / `Super+Shift+Tab` / `Super+Ctrl+Tab` |
| Copiar / pegar / cortar universal | `Super+C` / `Super+V` / `Super+X` |
| Historial del portapapeles / emojis | `Super+Ctrl+V` / `Super+Ctrl+E` |
| Captura / grabación / OCR / selector de color | `ImprPant` / `Alt+ImprPant` / `Super+Ctrl+ImprPant` / `Super+ImprPant` |
| Audio / red / Bluetooth / pantalla | `Super+Ctrl+A` / `Super+Ctrl+W` / `Super+Ctrl+B` / `Super+Ctrl+D` |
| Bloquear | `Super+Ctrl+L` |
| Tema / fondo | `Super+Ctrl+Shift+Espacio` / `Super+Ctrl+Espacio` |
| Mostrar/ocultar barra | `Super+Shift+Espacio` |
| Luz nocturna / bloqueo por inactividad | `Super+Ctrl+N` / `Super+Ctrl+I` |
| Notificación: descartar / descartar todas / historial | `Super+,` / `Super+Shift+,` / `Super+Shift+Alt+,` |
| Chuleta de tmux / Herdr | `Super+Alt+K` / `Super+Ctrl+K` |
| Cerrar todas las ventanas | `Ctrl+Alt+Supr` |

> [!info] En tu equipo
> `bindings.lua` añade `Super+Ctrl+Arriba/Abajo` (volumen, funciona con pantalla bloqueada), `Super+Alt+P` (reproducir/pausar) y `Super+Alt+B` (Red Pill Backup, ventana flotante centrada 800×600).

## Ventanas y workspaces

Hyprland coloca ventanas en mosaico automáticamente. Un **workspace** puede contener varias ventanas; hay 10 accesibles con `Super+1…0`. `Super+J` alterna la dirección de la división, `Super+L` alterna el layout del workspace entre *dwindle* y *scrolling*, `Super+O` «saca» una ventana (flotante y fijada), `Super+G` agrupa ventanas en pestañas y `Super+S` muestra el *scratchpad* (`Super+Alt+S` envía la ventana allí). Para varios monitores: [[03 Hyprland/Ventanas atajos y monitores]].

## Menú, notificaciones y capturas

El menú (`Super+Espacio`, `Super+Escape`) es una interfaz para las mismas herramientas que la CLI; todo se maneja con teclado. La barra, sus paneles (`Super+Ctrl+1…9`) y el historial de notificaciones son parte de Omarchy Shell. `ImprPant` abre la captura inteligente (región, ventana o pantalla), la copia al portapapeles y permite guardarla; `Super+Ctrl+C` abre el menú de captura completo (incluye QR y webcam).

> [!info] En tu equipo
> Capturas en `~/Pictures/Screenshots`, grabaciones en `~/Videos/Screenrecordings`.

## Flujo típico de una sesión

1. `Super+Ctrl+Enter` → Herdr (o `Super+Alt+Enter` → tmux) con el proyecto.
2. `Super+Shift+Enter` en el workspace 2 para documentación del navegador.
3. `Super+Shift+O` en el workspace 3 para notas en Obsidian.
4. `Super+1/2/3` para alternar; `Super+Ctrl+Tab` vuelve al anterior.
5. `Super+Ctrl+L` al levantarte.

Más: [navegación](https://omarchy.org/manual/navigation/), [barra](https://omarchy.org/manual/the-top-bar/), [portapapeles](https://omarchy.org/manual/unified-clipboard-history/), [capturas](https://omarchy.org/manual/screenshots-recording/), [interruptores e inactividad](https://omarchy.org/manual/toggles-idle-screensaver/).
