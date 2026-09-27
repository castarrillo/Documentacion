---
aliases: [hyprctl]
tags: [hyprland, hyprctl, referencia]
actualizado: 2026-09-27
verificado_en: "hyprctl de Hyprland 0.56.2 (hyprctl -h)"
---
# Referencia de `hyprctl`

`hyprctl` habla con el Hyprland en ejecución a través de su socket: consulta estado, ejecuta acciones y evalúa Lua. Todo lo que hace es **temporal** (se pierde al recargar) salvo que lo escribas en tus `.lua`.

## Opciones globales

| Opción | Efecto |
|---|---|
| `-j` | salida JSON (combínala con `jq`) |
| `--batch 'orden1 ; orden2'` | varias órdenes en una llamada (más rápido en scripts) |
| `-r` | refrescar estado tras la orden |
| `-i N` | usar otra instancia (`hyprctl instances`) |
| `-q` | silencioso |

## Consultas

| Orden | Devuelve |
|---|---|
| `version` | versión, commit y flags de compilación |
| `monitors [all]` | pantallas activas (`all` incluye las inactivas) con modos disponibles |
| `workspaces` / `activeworkspace` | workspaces y el actual |
| `clients` / `activewindow` | ventanas con `class`, `title`, `address`, `pid`, workspace… |
| `layers` | superficies de capa (barra, OSD, fondos) |
| `devices` | teclados, ratones, touchpads |
| `binds` | atajos registrados |
| `globalshortcuts` | atajos globales exportados por apps |
| `layouts` | layouts disponibles |
| `getoption sección:opción` | valor efectivo de una opción |
| `workspacerules` | reglas de workspace |
| `animations` | animaciones y curvas |
| `decorations <regex>` | decoraciones de una ventana |
| `cursorpos` | posición del cursor |
| `configerrors` | **errores de configuración** |
| `rollinglog [-f]` | cola del registro (seguir con `-f`) |
| `systeminfo` | información para informes de fallos |
| `status` / `splash` / `instances` | estado interno / frase de inicio / instancias |

## Acciones

| Orden | Efecto |
|---|---|
| `reload [config-only]` | recargar configuración (`config-only` no toca monitores) |
| `dispatch '<dispatcher Lua>'` | ejecutar un dispatcher, p. ej. `hyprctl dispatch 'hl.dsp.focus({ workspace = "2" })'` |
| `eval '<código Lua>'` | ejecutar Lua en el compositor |
| `repl ['código']` | consola Lua interactiva, o evaluar e imprimir resultado |
| `keyword <nombre> <valor>` | cambiar una opción en caliente |
| `setprop` / `getprop` | propiedad de una ventana |
| `switchxkblayout <teclado> next` | siguiente distribución de teclado |
| `setcursor <tema> <tamaño>` | tema de cursor |
| `notify` / `dismissnotify` | notificación nativa de Hyprland |
| `seterror <color> <mensaje>` | barra de error (se borra al recargar) |
| `kill` | modo «clic para matar ventana» (`Esc` para salir) |
| `output create\|remove` | salidas virtuales (headless) |
| `hyprsunset …` / `hyprpaper …` | hablar con esos demonios |

## Recetas

```bash
# Clase y título de la ventana activa (para escribir una regla)
hyprctl activewindow -j | jq '{class, title, floating, workspace: .workspace.name}'

# Todas las ventanas de un workspace
hyprctl clients -j | jq -r '.[] | select(.workspace.id==2) | "\(.class)\t\(.title)"'

# Descripción y modos de cada monitor
hyprctl monitors all -j | jq -r '.[] | "\(.name)\t\(.description)\t\(.availableModes|join(" "))"'

# Mover la ventana activa al workspace 4 sin seguirla
hyprctl dispatch 'hl.dsp.window.move({ workspace = "4", follow = false })'

# Evaluar una expresión Lua
hyprctl repl 'return hl.get_config("general.border_size")'

# Seguir el registro mientras pruebas un cambio
hyprctl rollinglog -f
```

Fuentes: `hyprctl -h` local, [Using hyprctl](https://wiki.hypr.land/Configuring/Advanced-and-Cool/Using-hyprctl/), [Dispatchers](https://wiki.hypr.land/Configuring/Basics/Dispatchers/).
