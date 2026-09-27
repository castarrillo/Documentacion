---
aliases: [hyprland.lua, Configurar Hyprland]
tags: [hyprland, lua, configuracion, diagnostico]
actualizado: 2026-09-27
verificado_en: "Hyprland 0.56.2 · /usr/share/omarchy/default/hypr/helpers.lua"
---
# Configuración Lua y depuración de Hyprland

## Archivos de usuario

| Ruta | Uso |
|---|---|
| `~/.config/hypr/hyprland.lua` | punto de entrada: carga defaults y tus módulos; admite ajustes extra al final |
| `bindings.lua` | atajos propios, desactivar o reemplazar defaults, reglas simples |
| `monitors.lua` | modo, posición, escala, `GDK_SCALE` |
| `input.lua` | teclado, ratón, touchpad, gestos |
| `looknfeel.lua` | huecos, bordes, opacidad, animaciones, layout |
| `autostart.lua` | programas al iniciar la sesión |
| `hyprsunset.conf` | luz nocturna (proceso aparte; `omarchy restart hyprsunset`) |
| `xdph.conf` | portal de capturas y pantalla compartida (proceso aparte) |

Referencia de sólo lectura: `/usr/share/omarchy/default/hypr/` (`bindings/*.lua`, `apps/*.lua`, `looknfeel.lua`, `helpers.lua`).

## Chuleta de la API

### Atajos

```lua
-- o.bind(teclas, descripción, acción, opciones)
-- acción = orden de shell (texto) o dispatcher (hl.dsp.*)
o.bind("SUPER + SHIFT + R", "SSH al servidor", "kitty -e ssh mi-servidor")
o.bind("SUPER + CTRL + UP", "Volume up", "omarchy-audio-output-volume raise",
       { locked = true, repeating = true })
o.bind("SUPER + W", "Close window", hl.dsp.window.close())

-- Reemplazar un atajo existente: primero quitarlo
hl.unbind("SUPER + SPACE")
o.bind("SUPER + SPACE", "Omarchy menu", "omarchy-menu toggle root")

-- Desactivar un default sin reemplazarlo
hl.unbind("SUPER + SHIFT + B")
```

| Opción | Efecto |
|---|---|
| `locked = true` | funciona con la pantalla bloqueada |
| `repeating = true` | se repite mientras se mantiene |
| `release = true` | se dispara al soltar |
| `mouse = true` | atajo de botón de ratón (mover/redimensionar) |

La descripción es lo que aparece en `Super+K`. Para averiguar el nombre de una tecla, usa `wev` (no instalado: `omarchy pkg add wev`).

En `hyprland.lua`, **antes** de `require("default.hypr.omarchy")`: `omarchy_default_bindings = false` quita todos los atajos de Omarchy; `omarchy_preinstalled_bindings = false` sólo los de apps preinstaladas.

### Dispatchers frecuentes

| Dispatcher | Acción |
|---|---|
| `hl.dsp.exec_cmd("orden")` | ejecutar |
| `hl.dsp.window.close()` | cerrar |
| `hl.dsp.window.float({ action = "toggle" })` | flotante |
| `hl.dsp.window.fullscreen({ mode = "fullscreen" })` | pantalla completa (`"maximized"` = ancho completo) |
| `hl.dsp.window.pseudo()` | pseudo-mosaico |
| `hl.dsp.focus({ direction = "l" })` | enfocar (`l r u d`) |
| `hl.dsp.focus({ workspace = "3" })` | ir al workspace |
| `hl.dsp.window.move({ workspace = "3", follow = false })` | mover ventana |
| `hl.dsp.window.swap(…)` / `hl.dsp.window.resize(…)` | intercambiar / redimensionar |
| `hl.dsp.workspace.toggle_special(…)` | scratchpad |
| `hl.dsp.group.toggle()` / `group.next()` / `group.prev()` | grupos |
| `hl.dsp.layout("togglesplit")` | mensaje al layout |

Desde la terminal: `hyprctl dispatch 'hl.dsp.focus({ workspace = "1" })'`.

### Reglas de ventana

```lua
-- o.window(coincidencia, efectos); coincidencia = clase (regex) o tabla
o.window("localsend", { float = true, center = true, size = { 1100, 700 } })
o.window({ class = "^TUI.redpill$" }, { float = true, pin = true })
o.window({ class = "^firefox$", title = ".*Picture-in-Picture.*" }, { float = true, pin = true })
o.window("qemu", { workspace = "5" })
```

Campos de coincidencia habituales: `class`, `title`, `xwayland`, `float`, `fullscreen`, `pin`, `tag`. Efectos: `float`, `center`, `size`, `workspace`, `opacity`, `pin`, `no_focus`, `tag`. Obtén `class` y `title` con `hyprctl clients` o `hyprctl activewindow`. `o.window` es un envoltorio de `hl.window_rule({ match = {…}, … })`.

### Opciones, monitores, entorno y arranque

```lua
hl.config({ general = { gaps_in = 5, gaps_out = 10, border_size = 2 } })
hl.config({ decoration = { active_opacity = 0.9, inactive_opacity = 0.8 } })
hl.monitor({ output = "HDMI-A-1", mode = "1920x1080@75", position = "1920x0", scale = 1 })
hl.env("GDK_SCALE", "1")
hl.gesture({ fingers = 4, direction = "horizontal", action = "workspace" })
o.launch_on_start("redpill-notify")           -- autostart.lua
hl.on("hyprland.start", function() hl.exec_cmd("orden") end)   -- equivalente bajo nivel
```

> [!info] En tu equipo
> `looknfeel.lua` fija `active_opacity = 0.9` e `inactive_opacity = 0.8`; `autostart.lua` lanza `redpill-notify`; `bindings.lua` añade volumen, play/pausa y la regla de Red Pill.

## Flujo de cambio seguro

1. Mira el default equivalente en `/usr/share/omarchy/default/hypr/` y comprueba si la tecla ya existe (`Super+K`).
2. Copia el archivo: `cp ~/.config/hypr/bindings.lua{,.bak}`.
3. Edita con `omarchy launch config editor ~/.config/hypr/bindings.lua` o `nvim`.
4. Guarda. Hyprland suele recargar solo; si no, `hyprctl reload`.
5. **Siempre** `hyprctl configerrors`. Vacío = correcto.
6. Prueba el cambio. Si algo falla, restaura el `.bak` y recarga.

> [!warning] Si te quedas sin atajos
> Un error de Lua puede dejar atajos sin cargar. Abre una terminal desde el lanzador (`Super+Espacio`) o una TTY (`Ctrl+Alt+F2`), restaura la copia y ejecuta `hyprctl reload`. Como último recurso, `omarchy refresh hyprland` sustituye **todos** tus `.lua` por los defaults.

## Depuración

| Problema | Comprobación |
|---|---|
| Error de sintaxis | `hyprctl configerrors` |
| El atajo no hace nada | `hyprctl binds \| grep -i tecla`; ¿otra capa lo intercepta? |
| Regla de ventana ignorada | `hyprctl clients` para ver `class`/`title` exactos |
| Valor efectivo de una opción | `hyprctl getoption general:gaps_in` · `hyprctl repl 'return hl.get_config("general.border_size")'` |
| Ver el registro en vivo | `hyprctl rollinglog -f` |
| Probar Lua interactivamente | `hyprctl repl` (salir con `Ctrl+D`) |
| Capturas/compartir pantalla | `systemctl --user status xdg-desktop-portal-hyprland` |
| Luz nocturna | `omarchy restart hyprsunset` |

Fuentes: [Configuring/Start](https://wiki.hypr.land/Configuring/Start/), [Binds](https://wiki.hypr.land/Configuring/Basics/Binds/), [Dispatchers](https://wiki.hypr.land/Configuring/Basics/Dispatchers/), [Window Rules](https://wiki.hypr.land/Configuring/Basics/Window-Rules/), [Variables](https://wiki.hypr.land/Configuring/Basics/Variables/), [Autostart](https://wiki.hypr.land/Configuring/Basics/Autostart/), [Environment variables](https://wiki.hypr.land/Configuring/Advanced-and-Cool/Environment-variables/), [Omarchy: dotfiles](https://omarchy.org/manual/dotfiles/), código local `/usr/share/omarchy/default/hypr/`.
