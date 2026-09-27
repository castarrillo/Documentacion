---
aliases: [Personalizar Omarchy, Crear tema Omarchy, Tutoriales Omarchy]
tags: [omarchy, personalizacion, temas, tutorial]
actualizado: 2026-09-27
verificado_en: "Omarchy 4.0.4-1 · /usr/share/omarchy/themes · manual: Themes, Making your own theme, Common tweaks"
---
# Personalización paso a paso

Tutoriales cortos, de lo más fácil a lo más avanzado. Todos tocan **sólo archivos de tu usuario** y se pueden deshacer. Antes de editar un archivo, haz copia: `cp archivo{,.bak}`.

## 1. Tema, fondo y fuente (sin editar archivos)

| Qué | Atajo | Orden |
|---|---|---|
| Tema | `Super+Ctrl+Shift+Espacio` | `omarchy theme set "Tokyo Night"` |
| Fondo | `Super+Ctrl+Espacio` | `omarchy theme bg next` · `omarchy theme bg set ~/Imágenes/foto.jpg` |
| Fondos propios para el tema actual | — | `omarchy theme bg install` abre su carpeta (`~/.config/omarchy/backgrounds/<tema>/`) |
| Fuente monoespaciada | menú *Setup → Font* | `omarchy font list` · `omarchy font set "JetBrainsMono Nerd Font"` |
| Tamaño de texto global | — | `omarchy display text size 12` · `reset` |
| Pantalla de arranque a juego con el tema | — | `omarchy plymouth set by theme "Matte Black"` |

Temas incluidos (22): Catppuccin, Catppuccin Latte, Ethereal, Everforest, Flexoki Light, Gruvbox, Hackerman, Kanagawa, Last Horizon, Lumon, Lupine, Matte Black, Miasma, Nord, Osaka Jade, Retro 82, Ristretto, Rose Pine, Solitude, Tokyo Night, Vantablack y White. Además, los que instales con `omarchy theme install <url-git>` (aquí: City 783, Harbordark, Kali Linux).

## 2. La barra

```bash
omarchy plugin list                                  # ID de cada widget
omarchy bar position bottom                          # top | bottom | left | right
omarchy bar transparent toggle
omarchy bar set omarchy.clock format "dddd HH:mm"    # formato del reloj
omarchy bar move omarchy.clock --section center --index 0
omarchy plugin enable omarchy.active-window --section left
omarchy plugin disable omarchy.tailscale
omarchy bar reset                                    # deshacer todo lo anterior
```

Iconos de la bandeja ocultos tras la flecha: clic derecho en la flecha para fijarlos. Más en [[01 Omarchy/Shell temas y plugins]].

## 3. Aspecto de las ventanas

En `~/.config/hypr/looknfeel.lua`:

```lua
hl.config({
  decoration = { rounding = 8 },                  -- esquinas redondeadas (por defecto 0)
  general = { gaps_in = 3, gaps_out = 6, border_size = 2 },
})
```

Sin huecos ni bordes temporalmente: `Super+Shift+Retroceso`. Transparencia de la ventana actual: `Super+Retroceso`. Detalle (animaciones, desenfoque, sombras): [[03 Hyprland/Aspecto animaciones y workspaces]].

## 4. Teclado, ratón y touchpad

En `~/.config/hypr/input.lua`:

```lua
hl.config({
  input = {
    kb_layout = "latam,us",
    kb_options = "compose:caps,shift:both_capslock_cancel,grp:alts_toggle",
    repeat_rate = 40, repeat_delay = 250,
    sensitivity = 0,                 -- -1.0 a 1.0
    touchpad = { natural_scroll = true, scroll_factor = 0.6 },
  },
})
```

Alternar distribución: `Alt izq + Alt der`. Ver [[01 Omarchy/Escritorio red y periféricos#Entrada]].

## 5. Un atajo propio

En `~/.config/hypr/bindings.lua`:

```lua
o.bind("SUPER + SHIFT + T", "Tareas", "kitty -e nvim ~/Documents/tareas.md")
o.bind("SUPER + ALT + D", "Documentación", "uwsm-app -- obsidian")
```

Comprueba antes que la tecla está libre (`Super+K`) y valida con `hyprctl configerrors`. Más: [[03 Hyprland/Configuración Lua y depuración]].

## 6. Programas al iniciar sesión

En `~/.config/hypr/autostart.lua`: `o.launch_on_start("syncthing")`. Si debe reiniciarse solo al fallar, mejor un servicio de usuario ([[01 Omarchy/Aplicaciones desarrollo y automatización#Ejemplo de servicio de usuario]]).

## 7. Entradas propias en el menú

`~/.config/omarchy/extensions/omarchy-menu.jsonc`:

```jsonc
{
  "personal": { "icon": "", "label": "Personal" },
  "personal.vault": { "icon": "󰎞", "label": "Documentación", "action": "uwsm-app -- obsidian" },
  "personal.backup": { "icon": "󰁯", "label": "Copia de seguridad", "action": "xdg-terminal-exec --app-id=TUI.redpill -e redpill" }
}
```

## 8. Reaccionar a eventos (hooks)

Ejemplo: avisar al cambiar de tema.

```bash
cat > ~/aviso-tema <<'EOF'
#!/bin/bash
notify-send "Tema aplicado" "$1"
EOF
omarchy hook install theme-set ~/aviso-tema
omarchy theme set "Nord"      # debería aparecer la notificación
```

Eventos: `post-boot`, `post-update`, `pre-refresh-pacman`, `theme-set`, `font-set`, `battery-low` ([[01 Omarchy/Aplicaciones desarrollo y automatización#Hooks de Omarchy]]).

## 9. Crear tu propio tema

1. **Copia** uno existente como base:

   ```bash
   cp -r /usr/share/omarchy/themes/matte-black ~/.config/omarchy/themes/mi-tema
   ```

2. **Edita la paleta** `~/.config/omarchy/themes/mi-tema/colors.toml`. Estructura real (Matte Black):

   ```toml
   mode = "dark"                 # "light" para temas claros
   accent = "#e68e0d"            # color de acento (bordes, selección, barra)
   selection = "#2a2a2a"
   muted = "#333333"
   background = "#121212"
   dark_background = "#0d0d0d"
   darker_background = "#090909"
   lighter_background = "#1e1e1e"
   foreground = "#bebebe"
   dark_foreground = "#555555"
   light_foreground = "#8a8a8d"
   bright_foreground = "#bebebe"
   red = "#D35F5F"   # …y yellow, orange, green, cyan, blue, magenta, brown
   bright_red = "#B91C1C"   # …y las variantes bright_*
   ```

   Con esta paleta Omarchy **genera** la configuración de terminales (Kitty, Alacritty, Foot, Ghostty), btop, Hyprland, Neovim, Helix, VS Code, Obsidian, Chromium y la propia shell (barra, menú, notificaciones, OSD, bloqueo).

3. **Archivos opcionales** en la carpeta del tema: `backgrounds/` (fondos), `icons.theme` (tema de iconos, p. ej. `Yaru-blue`), `neovim.lua` (esquema de color de Neovim), `vscode.json`, `unlock.png` (imagen del desbloqueo; transparente), `preview.png`. También puedes incluir archivos completos por app (`kitty.conf`, `btop.theme`, `hyprland.lua`…) si no quieres la versión generada.

4. **Aplica y prueba**: `omarchy theme set "mi-tema"`; tras cada cambio de colores, `omarchy theme refresh`. `omarchy dev theme preview mi-tema` muestra la paleta en la terminal. La app **Aether** (`Super+Alt+Espacio`) permite ajustar colores de forma visual.

5. **Plantillas propias** para apps que Omarchy no tematiza: crea `~/.config/omarchy/themed/<app>.tpl` usando variables `{{ background }}`, `{{ foreground }}`, `{{ accent }}`, `{{ red }}`, `{{ color0 }}`…`{{ color15 }}` y los modificadores `_strip` (sin `#`) y `_rgb` (decimal). Hay un ejemplo comentado en `~/.config/omarchy/themed/alacritty.toml.tpl.sample`.

6. **Compartir**: sube la carpeta a un repositorio git llamado `omarchy-<nombre>-theme`; otros lo instalan con `omarchy theme install <url>`.

> [!warning] Temas de terceros
> Al instalar un tema de un repositorio externo, Omarchy **elimina** sus archivos `.lua`, configuraciones de terminal y `vscode.json` para no ejecutar código ajeno: sólo aplica los colores. Aun así, revisa el repositorio antes.

## 10. Arte de marca y pantalla de bloqueo

```bash
omarchy branding about image          # elegir una imagen para el «About» (fastfetch)
omarchy branding about text           # editar el arte ASCII del «About»
omarchy branding screensaver text     # editar el texto del salvapantallas
omarchy branding about reset          # volver al original
omarchy plymouth switcher             # elegir la pantalla de desbloqueo del disco
```

## Si algo sale mal

| Área | Deshacer |
|---|---|
| Un archivo concreto | restaura tu `.bak`, o `omarchy refresh config <ruta>` |
| La barra | `omarchy bar reset` / `omarchy refresh shell` ⚠️ |
| Todo Hyprland | `omarchy refresh hyprland` ⚠️ (sobrescribe tus `.lua`) |
| Pantalla de arranque | `omarchy plymouth reset` |
| El tema | `omarchy theme set "Matte Black"` |

Fuentes: [Themes](https://omarchy.org/manual/themes/), [Making your own theme](https://omarchy.org/manual/making-your-own-theme/), [Backgrounds](https://omarchy.org/manual/backgrounds/), [Fonts](https://omarchy.org/manual/fonts/), [Branding](https://omarchy.org/manual/branding/), [Common tweaks](https://omarchy.org/manual/common-tweaks/), [The Top Bar](https://omarchy.org/manual/the-top-bar/), [Dotfiles](https://omarchy.org/manual/dotfiles/).
