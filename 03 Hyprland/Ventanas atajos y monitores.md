---
aliases: [Monitores, Ventanas Hyprland]
tags: [hyprland, monitores, ventanas]
actualizado: 2026-09-27
verificado_en: "Hyprland 0.56.2 · ~/.config/hypr/monitors.lua"
---
# Ventanas, atajos y monitores

## Ventanas y workspaces

Abre una terminal y un navegador: Hyprland los reparte automáticamente con el layout **dwindle** (cada ventana nueva divide el hueco de la activa).

| Acción | Atajo |
|---|---|
| Enfocar / intercambiar | `Super+Flechas` / `Super+Shift+Flechas` |
| Alternar dirección de la división | `Super+J` |
| Flotante ↔ mosaico | `Super+T` (arrastra con `Super+clic izquierdo`, redimensiona con `Super+clic derecho`) |
| Redimensionar con teclado | `Super+-`, `Super+=` (y con `Shift`, `Alt`, `Ctrl`) |
| Ir / mover a workspace | `Super+N` / `Super+Shift+N` (sin seguir: `Super+Shift+Alt+N`) |
| Layout *scrolling* | `Super+L`: columnas que se desplazan horizontalmente, útil en pantallas anchas |
| Agrupar en pestañas | `Super+G`; recorrer con `Super+Alt+Tab` |
| Scratchpad | `Super+S` / `Super+Alt+S` |
| Sacar ventana (flotante y fijada) | `Super+O` |

Atajos vigentes: `Super+K`. Lista completa: [[01 Omarchy/Referencia completa de atajos]].

## Monitores de este equipo

`~/.config/hypr/monitors.lua` identifica cada pantalla por su **descripción de hardware** (`desc:…`), así la regla funciona aunque cambie el puerto:

| Perfil | Pantalla | Modo | Posición |
|---|---|---|---|
| Portátil | SNF601BS1-1 (interna) | 1920×1080@60 | `0x1080` (debajo del principal) |
| Casa | LG FULL HD | 1920×1080@75 | `0x0` (principal) |
| Casa | LG TV | 1280×720@60 | `1920x0` (a la derecha) |
| Oficina | Samsung LF24T35 | 1920×1080@75 | `0x0` (principal, izquierda) |
| Oficina | Lenovo E22-28 | 1920×1080@75 | `1920x0` (derecha) |

Escala del monitor y `GDK_SCALE`: `1`. Hyprland sólo aplica las reglas de las pantallas conectadas; comprueba la realidad con `hyprctl monitors all`.

## Añadir o ajustar un monitor

1. Conecta la pantalla y ejecuta `hyprctl monitors all`. Anota `description`, modos disponibles (`availableModes`) y el nombre (`HDMI-A-1`…).
2. Añade una línea en `monitors.lua`:

```lua
hl.monitor({ output = "desc:Dell Inc. DELL P2422H ABC123", mode = "1920x1080@60", position = "1920x0", scale = 1 })
-- Genérico para cualquier pantalla no listada:
hl.monitor({ output = "", mode = "preferred", position = "auto", scale = 1 })
-- Desactivar una salida:
hl.monitor({ output = "HDMI-A-2", disabled = true })
```

3. Guarda y ejecuta `hyprctl configerrors`.

**Coordenadas**: las posiciones son globales en píxeles *lógicos* (tras aplicar la escala). Una pantalla 1920×1080 a escala 1 en `0x0` deja la siguiente a su derecha en `1920x0` y debajo en `0x1080`. Con escala 1.5, la anchura lógica es 1920/1.5 = 1280.

> [!tip] Escalas recomendadas
> Usa escalas que dividan bien la resolución (1, 1.25, 1.5, 2). `Super+/` y `Super+Alt+/` cambian la escala del monitor enfocado al vuelo para probar antes de fijarla.

| Situación | Herramienta |
|---|---|
| Apagar/encender la pantalla del portátil | `Super+Ctrl+Supr` · `omarchy hyprland monitor internal toggle` |
| Duplicar la pantalla del portátil | `Super+Ctrl+Alt+Supr` |
| Mover el workspace a otro monitor | `Super+Shift+Alt+Flechas` |
| Brillo de monitor externo (DDC/CI) | `omarchy brightness display ddc <monitor> +10%` |
| Pantalla interna desaparecida tras desconectar | `omarchy hw recover internal monitor` |

## Conflictos entre teclas

Las teclas atraviesan capas de fuera hacia dentro: **Hyprland → terminal → tmux/Herdr → Neovim**. Un atajo de Hyprland (`Super+…`) nunca llega a la terminal. Por eso Omarchy reserva `Super` para el escritorio, `Ctrl+Espacio` como prefijo de multiplexores y `Espacio` como *leader* de LazyVim. Para cambiar un atajo del escritorio: `hl.unbind(…)` y después `o.bind(…)` en `bindings.lua` ([[03 Hyprland/Configuración Lua y depuración]]).

Fuentes: [Monitors](https://wiki.hypr.land/Configuring/Basics/Monitors/), [Dwindle](https://wiki.hypr.land/Configuring/Layouts/Dwindle-Layout/), [Scrolling](https://wiki.hypr.land/Configuring/Layouts/Scrolling-Layout/), [Binds](https://wiki.hypr.land/Configuring/Basics/Binds/), [Dispatchers](https://wiki.hypr.land/Configuring/Basics/Dispatchers/), [Omarchy: monitores](https://omarchy.org/manual/monitors/), [Omarchy: navegación](https://omarchy.org/manual/navigation/).
