---
aliases: [looknfeel, Animaciones Hyprland, Reglas de workspace, Layouts]
tags: [hyprland, aspecto, animaciones, workspaces]
actualizado: 2026-09-27
verificado_en: "Hyprland 0.56.2 · /usr/share/omarchy/default/hypr/looknfeel.lua · wiki: Variables, Animations, Workspace Rules, Layouts"
---
# Aspecto, animaciones, layouts y workspaces

Todo lo visual se ajusta en `~/.config/hypr/looknfeel.lua`, que se carga **después** de los defaults de Omarchy (`/usr/share/omarchy/default/hypr/looknfeel.lua`, léelo como referencia). Sólo escribe lo que quieras cambiar.

## Valores por defecto de Omarchy

| Opción | Valor Omarchy | Significado |
|---|---|---|
| `general.gaps_in` / `gaps_out` | 5 / 10 | hueco entre ventanas / con el borde de la pantalla |
| `general.border_size` | 2 | grosor del borde |
| `general.col.active_border` | degradado cian→verde a 45° (el tema lo sustituye) | color del borde activo |
| `general.layout` | `dwindle` | layout de mosaico |
| `decoration.rounding` | 0 | esquinas rectas |
| `decoration.shadow.enabled` / `blur.enabled` | false / false | sin sombras ni desenfoque (rendimiento) |
| `animations.enabled` | true | animaciones activadas |
| `dwindle.preserve_split` / `force_split` | true / 2 | conserva la dirección de división; la nueva ventana va a la derecha/abajo |
| `scrolling.column_width` | 0.49 | columnas de algo menos de media pantalla |
| `misc.focus_on_activate` | true | una app que pide atención recibe el foco |
| `cursor.hide_on_key_press` | true | oculta el cursor al teclear |

> [!info] En tu equipo
> `looknfeel.lua` fija `active_opacity = 0.9` e `inactive_opacity = 0.8` (ventanas translúcidas). Omarchy además aplica `opacity = "0.985 0.96"` a las ventanas etiquetadas `default-opacity`.

Consulta el valor efectivo de cualquier opción: `hyprctl getoption decoration:rounding`.

## Recetas de aspecto

```lua
-- ~/.config/hypr/looknfeel.lua
hl.config({
  general = {
    gaps_in = 3, gaps_out = 6, border_size = 2,
    col = {
      active_border = { colors = { "rgba(e68e0dee)", "rgba(d35f5fee)" }, angle = 45 },
      inactive_border = "rgba(333333aa)",
    },
    resize_on_border = true,          -- redimensionar arrastrando el borde
  },
  decoration = {
    rounding = 8,
    active_opacity = 1.0, inactive_opacity = 0.9,
    dim_inactive = true, dim_strength = 0.1,     -- oscurecer ventanas inactivas
    shadow = { enabled = true, range = 8, render_power = 3 },
    blur = { enabled = true, size = 6, passes = 2 },   -- consume GPU; útil con transparencia
  },
})
```

> [!tip] Colores del tema
> Los colores del borde los pone el **tema** de Omarchy. Si los fijas aquí, dejarán de cambiar al cambiar de tema. Para personalizarlos por tema, crea un `hyprland.lua` dentro de tu tema propio ([[01 Omarchy/Personalización paso a paso#9. Crear tu propio tema]]).

## Animaciones

Se definen con **curvas** (`hl.curve`) y **animaciones por hoja** (`hl.animation`). Omarchy trae curvas `easeOutQuint`, `easeInOutCubic`, `linear`, `almostLinear` y `quick`, y **desactiva** la animación de cambio de workspace.

```lua
-- Desactivar todas las animaciones (máximo rendimiento / batería)
hl.config({ animations = { enabled = false } })

-- Activar el deslizamiento entre workspaces
hl.animation({ leaf = "workspaces", enabled = true, speed = 4, bezier = "easeOutQuint", style = "slide" })

-- Ventanas que aparecen deslizándose en lugar de «popin»
hl.animation({ leaf = "windowsIn", enabled = true, speed = 4, bezier = "easeOutQuint", style = "slide" })

-- Curva propia
hl.curve("rebote", { type = "bezier", points = { { 0.34, 1.56 }, { 0.64, 1 } } })
```

| Hoja (`leaf`) | Afecta a |
|---|---|
| `global` | todo (valor por defecto del resto) |
| `windows`, `windowsIn`, `windowsOut`, `windowsMove` | abrir, cerrar, mover ventanas |
| `fade`, `fadeIn`, `fadeOut`, `fadeSwitch` | fundidos |
| `border` | color del borde |
| `layers`, `layersIn`, `layersOut` | barra, menús, OSD |
| `workspaces`, `specialWorkspace` | cambio de workspace, scratchpad |

`speed` es en décimas de segundo (menor = más rápido). Estilos: `slide`, `slidevert`, `slidefade 20%`, `popin 80%`, `fade`. Ver la curva en [cubic-bezier.com](https://cubic-bezier.com/).

## Layouts

| Layout | Cómo coloca las ventanas | Cuándo usarlo |
|---|---|---|
| **dwindle** (defecto) | árbol binario: cada ventana nueva divide la activa | uso general |
| **scrolling** | columnas en una tira horizontal que se desplaza | pantallas anchas, muchas ventanas |
| **master** | una ventana principal grande + pila | una app principal (editor) y auxiliares |
| **monocle** | una ventana a pantalla completa cada vez | portátil pequeño, concentración |

`Super+L` alterna *dwindle* ↔ *scrolling* en el workspace actual; `Super+J` cambia la dirección de la división en *dwindle*. Cambiar el layout por defecto o por workspace:

```lua
hl.config({ general = { layout = "master" } })
hl.workspace_rule({ workspace = "2", layout = "scrolling" })
hl.config({ scrolling = { column_width = 0.66 } })
```

## Workspaces y reglas de workspace

Los workspaces 1–10 se crean al usarlos y desaparecen vacíos. Además existen **workspaces con nombre** (`name:código`) y **especiales** (`special:…`, como el scratchpad de `Super+S`).

```lua
-- Workspaces fijos por monitor (perfil oficina)
hl.workspace_rule({ workspace = "1", monitor = "desc:Samsung Electric Company LF24T35 HCNTC00457", default = true })
hl.workspace_rule({ workspace = "6", monitor = "desc:Lenovo Group Limited E22-28 VY537647", default = true })

-- Workspace de concentración: sin huecos ni bordes
hl.workspace_rule({ workspace = "name:foco", gaps_in = 0, gaps_out = 0, border_size = 0, no_rounding = true })

-- Abrir algo automáticamente al crear un workspace vacío
hl.workspace_rule({ workspace = "9", on_created_empty = "kitty -e btop" })

-- «Gaps inteligentes»: sin huecos cuando sólo hay una ventana en mosaico
hl.workspace_rule({ workspace = "w[tv1]", gaps_out = 0, gaps_in = 0 })
hl.window_rule({ match = { float = false, workspace = "w[tv1]" }, border_size = 0 })
```

Comprueba las reglas activas con `hyprctl workspacerules` y los workspaces con `hyprctl workspaces`. Mover el workspace actual a otro monitor: `Super+Shift+Alt+Flechas`.

## Reglas de capa (barra, menús)

Las superficies de Omarchy Shell (`omarchy-bar`, `omarchy-menu`…) son *layers*. Omarchy les quita la animación para que respondan al instante:

```lua
hl.layer_rule({ match = { namespace = "omarchy-bar" }, no_anim = true, animation = "none" })
```

Lista de capas y sus nombres: `hyprctl layers`.

## Rendimiento y batería

- Desenfoque y sombras cuestan GPU; Omarchy los deja apagados.
- `animations.enabled = false` ahorra algo de batería.
- Comprueba el impacto con `btop` y la temperatura con `sensors`.

Fuentes: [Variables](https://wiki.hypr.land/Configuring/Basics/Variables/), [Animations](https://wiki.hypr.land/Configuring/Advanced-and-Cool/Animations/), [Workspace Rules](https://wiki.hypr.land/Configuring/Basics/Workspace-Rules/), [Window Rules](https://wiki.hypr.land/Configuring/Basics/Window-Rules/), [Dwindle](https://wiki.hypr.land/Configuring/Layouts/Dwindle-Layout/), [Scrolling](https://wiki.hypr.land/Configuring/Layouts/Scrolling-Layout/), [Master](https://wiki.hypr.land/Configuring/Layouts/Master-Layout/), [Monocle](https://wiki.hypr.land/Configuring/Layouts/Monocle-Layout/), [Performance](https://wiki.hypr.land/Configuring/Advanced-and-Cool/Performance/), [Omarchy: Common tweaks](https://omarchy.org/manual/common-tweaks/).
