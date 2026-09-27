---
aliases: [Hyprland]
tags: [hyprland, wayland, moc]
actualizado: 2026-09-27
verificado_en: "Hyprland 0.56.2 · Omarchy 4.0.4-1"
---
# Hyprland en Omarchy 4

Hyprland es el **compositor Wayland**: dibuja las aplicaciones en los monitores, organiza el mosaico (*tiling*), recibe teclado, ratón y gestos, y aplica reglas de ventana y animaciones. **Omarchy decide los valores iniciales** y te deja sobrescribirlos en módulos Lua de `~/.config/hypr/`. La barra, el menú y las notificaciones **no** son de Hyprland: los dibuja Omarchy Shell.

> [!important] Configuración en Lua desde Hyprland 0.55
> Desde la 0.55 el formato `hyprlang` (`hyprland.conf`, líneas `bind = SUPER, Q, exec, …`) está **obsoleto** y la configuración nativa es `~/.config/hypr/hyprland.lua` con la API `hl.*`. La wiki actual ya documenta Lua; para la sintaxis antigua remite a las páginas de la 0.54. **No pegues ejemplos `hyprland.conf` de foros antiguos**: tradúcelos a Lua. Omarchy añade además ayudas `o.*` (`o.bind`, `o.window`, `o.launch_on_start`).

## Orden de carga

```text
~/.config/hypr/hyprland.lua
  ├─ dofile(/usr/share/omarchy/default/hypr/bootstrap.lua)   rutas y helpers o.*
  ├─ require("default.hypr.omarchy")                         defaults de Omarchy
  │     envs, input, looknfeel, windows, apps/*, bindings/*, autostart…
  ├─ require("hypr.monitors")      ~/.config/hypr/monitors.lua
  ├─ require("hypr.input")         ~/.config/hypr/input.lua
  ├─ require("hypr.bindings")      ~/.config/hypr/bindings.lua
  ├─ require("hypr.looknfeel")     ~/.config/hypr/looknfeel.lua
  ├─ require("hypr.autostart")     ~/.config/hypr/autostart.lua
  └─ require("default.hypr.toggles")   interruptores de omarchy hyprland toggle
```

Lo que se carga después gana. Por eso tus ajustes sobreviven a las actualizaciones y los defaults pueden mejorar sin reescribir tus archivos.

## Conceptos

| Concepto | Qué es | Atajo / orden |
|---|---|---|
| **Output** | pantalla física (`eDP-1`, `HDMI-A-1`, o `desc:Fabricante Modelo Serie`) | `hyprctl monitors all` |
| **Workspace** | escritorio virtual; los normales son numerados | `Super+1…0` |
| **Workspace especial** | oculto, se superpone (scratchpad) | `Super+S` |
| **Mosaico / flotante** | comparte superficie / se superpone | `Super+T` |
| **Layout** | algoritmo de mosaico: *dwindle* (árbol binario) o *scrolling* (columnas que se desplazan) | `Super+L` |
| **Grupo** | varias ventanas con pestañas en un hueco | `Super+G` |
| **Dispatcher** | acción ejecutable (`hl.dsp.*`) | `hyprctl dispatch '…'` |
| **Regla de ventana** | condición (clase, título…) + efecto (flotar, tamaño, opacidad…) | `o.window(…)` |
| **Regla de capa** | igual, para superficies *layer-shell* (barra, OSD) | `hl.layer_rule(…)` |

## Primeras consultas

```bash
hyprctl version | head -1   # versión y commit
hyprctl monitors all        # pantallas conectadas y desconectadas
hyprctl activewindow        # clase, título y propiedades de la ventana activa
hyprctl clients             # todas las ventanas
hyprctl configerrors        # errores de configuración (vacío = bien)
```

Referencia completa: [[03 Hyprland/Referencia hyprctl]].

> [!tip] Usa la wiki de tu versión
> La portada de [wiki.hypr.land](https://wiki.hypr.land/) muestra **Latest git**; con el [selector de versión](https://wiki.hypr.land/version-selector/) elige la que indica `hyprctl version` (0.56.2 en este equipo).

Continúa en [[03 Hyprland/Ventanas atajos y monitores]], [[03 Hyprland/Configuración Lua y depuración]], [[03 Hyprland/Aspecto animaciones y workspaces]] y [[03 Hyprland/Recetas de Hyprland]] (ejemplos listos para copiar).

Fuentes: [wiki oficial](https://wiki.hypr.land/), [Configuring/Start](https://wiki.hypr.land/Configuring/Start/), [Master tutorial](https://wiki.hypr.land/Getting-Started/Master-Tutorial/), [layouts](https://wiki.hypr.land/Configuring/Layouts/), [Omarchy: monitores](https://omarchy.org/manual/monitors/), [Omarchy: dotfiles](https://omarchy.org/manual/dotfiles/).
