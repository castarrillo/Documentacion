---
aliases: [Recetas Hyprland, Submapas, Ejemplos Hyprland]
tags: [hyprland, recetas, lua]
actualizado: 2026-09-27
verificado_en: "Hyprland 0.56.2 (API hl.* consultada con hyprctl repl) · wiki: Binds, Window Rules"
---
# Recetas de Hyprland

Ejemplos listos para copiar a tus archivos de `~/.config/hypr/`. Todos usan la API Lua de Hyprland 0.56 (`hl.*`) y las ayudas de Omarchy (`o.*`). Después de cada cambio: `hyprctl configerrors`. Fundamentos: [[03 Hyprland/Configuración Lua y depuración]].

> [!tip] Averigua la clase de la ventana
> Casi todas las reglas necesitan la **clase** (`class`) o el **título**. Abre la app y ejecuta `hyprctl activewindow -j | jq '{class, title}'`.

## 1. Una app siempre en el mismo workspace

```lua
-- hyprland.lua (al final) o bindings.lua
o.window("^(Spotify|spotify)$", { workspace = "9 silent" })     -- silent: no te lleva allí
o.window({ class = "^obsidian$" }, { workspace = "3" })
```

## 2. Ventanas flotantes con tamaño fijo (calculadora, diálogos)

```lua
o.window({ class = "^org.gnome.Calculator$" }, { float = true, size = { 400, 600 }, center = true })
o.window({ title = "^(Abrir archivo|Guardar como)$" }, { float = true, center = true })
```

## 3. Imagen en imagen siempre visible

```lua
o.window({ title = "^Picture.in.[Pp]icture$" }, {
  float = true, pin = true, size = { 640, 360 },
  move = { "(monitor_w-660)", "(monitor_h-380)" },   -- esquina inferior derecha (expresiones entre paréntesis)
})
```

`pin` la muestra en todos los workspaces. Para cualquier ventana: `Super+O` («sacar» y fijar).

## 4. Opacidad por aplicación

```lua
o.window({ class = "^kitty$" }, { opacity = "0.9 0.8" })          -- activa / inactiva
o.window({ class = "^(brave-browser|chromium)$" }, { opacity = "1.0 override 1.0 override" })
```

`override` ignora el multiplicador global de `looknfeel.lua`.

## 5. Modo «redimensionar» con un submapa

Un **submapa** es un modo temporal donde las teclas cambian de significado (como un modo de Vim):

```lua
-- bindings.lua
hl.bind("SUPER + R", hl.dsp.submap("redimensionar"))
hl.define_submap("redimensionar", function()
  hl.bind("right", hl.dsp.window.resize({ x = 20, y = 0, relative = true }), { repeating = true })
  hl.bind("left",  hl.dsp.window.resize({ x = -20, y = 0, relative = true }), { repeating = true })
  hl.bind("up",    hl.dsp.window.resize({ x = 0, y = -20, relative = true }), { repeating = true })
  hl.bind("down",  hl.dsp.window.resize({ x = 0, y = 20, relative = true }), { repeating = true })
  hl.bind("escape", hl.dsp.submap("reset"))
  hl.bind("return", hl.dsp.submap("reset"))
end)
```

Pulsa `Super+R`, ajusta con las flechas y sal con `Esc`. `hyprctl submap` muestra el submapa activo. Comprueba antes que `Super+R` está libre (`Super+K`).

## 6. Lanzar o enfocar (no abrir duplicados)

```lua
-- Forma abreviada de Omarchy: o.bind acepta una tabla en lugar de una orden
o.bind("SUPER + SHIFT + M", "Música", { focus = "spotify", launch = "spotify" })
o.bind("SUPER + ALT + T", "btop", { tui = "btop", focus = true })
o.bind("SUPER + ALT + N", "Notion", { webapp = "https://notion.so", focus = true })
-- Equivalente explícito: "omarchy-launch-or-focus spotify spotify"
```

Claves admitidas por `o.bind`: `launch`, `focus` + `launch`, `tui`, `webapp` (con `focus = true` para no duplicar) y `omarchy` (lanza `omarchy-launch-<valor>`). Omarchy usa este patrón en todos sus atajos de apps.

## 7. Atajo con lógica en Lua

```lua
-- Alternar entre layout dwindle y master en el workspace actual
o.bind("SUPER + ALT + L", "Alternar master", function()
  local actual = hl.get_config("general.layout")
  local nuevo = (actual == "master") and "dwindle" or "master"
  hl.config({ general = { layout = nuevo } })
  hl.exec_cmd("notify-send 'Layout' '" .. nuevo .. "'")
end)
```

Funciones de consulta disponibles: `hl.get_active_window()`, `hl.get_active_workspace()`, `hl.get_monitors()`, `hl.get_windows()`, `hl.get_config(opción)`… Explóralas con `hyprctl repl`.

## 8. Workspace de trabajo con apps preparadas

```lua
hl.workspace_rule({ workspace = "name:dev", on_created_empty = "kitty -e herdr" })
o.bind("SUPER + D", "Workspace dev", hl.dsp.focus({ workspace = "name:dev" }))
```

## 9. Reaccionar a eventos del compositor

```lua
-- Al iniciar Hyprland (equivale a o.launch_on_start)
hl.on("hyprland.start", function()
  hl.exec_cmd("notify-send 'Bienvenido' 'Sesión iniciada'")
end)
```

Para eventos como conectar un monitor, Omarchy ya ejecuta `omarchy-hyprland-monitor-watch`; para lógica propia compleja, prefiere un script que escuche el socket de eventos (ver [IPC](https://wiki.hypr.land/IPC/)).

## 10. Teclas multimedia en teclados sin ellas

```lua
o.bind("SUPER + F10", "Silenciar", "omarchy-audio-output-volume mute-toggle", { locked = true })
o.bind("SUPER + F11", "Bajar volumen", "omarchy-audio-output-volume lower", { locked = true, repeating = true })
o.bind("SUPER + F12", "Subir volumen", "omarchy-audio-output-volume raise", { locked = true, repeating = true })
```

`locked = true` hace que funcionen con la pantalla bloqueada.

## 11. Desactivar el touchpad al conectar un ratón / por dispositivo

```lua
-- Configuración por dispositivo (nombre en: hyprctl devices)
hl.device({ name = "elan0001:00-04f3:3140-touchpad", sensitivity = 0.3, natural_scroll = true })
```

O alterna el touchpad con `omarchy toggle touchpad`.

## 12. Deshacer un default de Omarchy sin reemplazarlo

```lua
hl.unbind("SUPER + SHIFT + Y")        -- quitar el atajo de YouTube
-- o todos los de apps preinstaladas (en hyprland.lua, antes de require("default.hypr.omarchy")):
-- omarchy_preinstalled_bindings = false
```

Fuentes: [Binds (submapas)](https://wiki.hypr.land/Configuring/Basics/Binds/), [Window Rules](https://wiki.hypr.land/Configuring/Basics/Window-Rules/), [Workspace Rules](https://wiki.hypr.land/Configuring/Basics/Workspace-Rules/), [Dispatchers](https://wiki.hypr.land/Configuring/Basics/Dispatchers/), [Devices](https://wiki.hypr.land/Configuring/Advanced-and-Cool/Devices/), [IPC](https://wiki.hypr.land/IPC/), defaults de Omarchy en `/usr/share/omarchy/default/hypr/`.
