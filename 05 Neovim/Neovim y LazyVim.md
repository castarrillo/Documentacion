---
aliases: [Neovim, LazyVim, nvim]
tags: [neovim, lazyvim, editor, moc]
actualizado: 2026-09-27
verificado_en: "Neovim 0.12.5 · LazyVim (install_version 8)"
---
# Neovim y LazyVim en Omarchy

**Neovim** es el editor; **LazyVim** es una configuración completa (plugins, atajos, LSP, formato) construida sobre **lazy.nvim**, que descarga y carga los plugins. En este equipo `~/.config/nvim/lua/config/lazy.lua` importa `LazyVim/LazyVim` y después tus plugins de `lua/plugins/`. `Super+Shift+N` abre el editor; desde Bash, `n` (alias de Omarchy) abre `nvim .`.

> [!info] En tu equipo
> Neovim `0.12.5`. Extra activo: **neo-tree** (explorador lateral en `Espacio e`). Ajustes propios: sin números relativos, **autoformato desactivado** (`vim.g.autoformat = false`; formatea a mano con `Espacio c f`), portapapeles por OSC 52 compatible con tmux/SSH (`config/remote_clipboard.lua`) y recarga en caliente del tema de Omarchy.

## Modelo mental

| Concepto | Qué es | Cómo se entra / usa |
|---|---|---|
| **Modo Normal** | moverse y dar órdenes | `Esc` (o `Ctrl+[`) |
| **Modo Insertar** | escribir texto | `i` antes, `a` después, `o` línea nueva, `I`/`A` inicio/fin de línea |
| **Modo Visual** | seleccionar | `v` caracteres, `V` líneas, `Ctrl+v` bloque |
| **Modo Comando** | órdenes `:` | `:w`, `:q`, `:%s/a/b/g` |
| **Modo Terminal** | terminal integrada | `Ctrl+/`; salir a Normal con `Esc Esc` o `Ctrl+\ Ctrl+n` |
| **Buffer** | archivo cargado | `Shift+h` / `Shift+l` |
| **Ventana** | vista de un buffer | `Espacio -` / `Espacio \|` |
| **Pestaña** | colección de ventanas | `Espacio Tab Tab` |
| **Leader** | prefijo de atajos | `Espacio`; *which-key* muestra las opciones al esperar |

## Primeros cinco minutos

```text
nvim archivo.md    abrir un archivo           i        pasar a insertar
Esc                volver a Normal            :w       guardar (también Ctrl+s)
:q                 salir                      :wq      guardar y salir
:q!                salir sin guardar          :qa      salir de todo (Espacio q q)
u / Ctrl+r         deshacer / rehacer         /texto   buscar (n, N)
```

**Aprender**: `nvim --clean +Tutor` (tutorial interactivo; en este equipo `nvim +Tutor` no está disponible porque LazyVim desactiva ese plugin). `:help tema` abre la ayuda (`Espacio s h` la busca). `:Lazy` gestiona plugins; `:LazyExtras` activa módulos opcionales (lenguajes, IA, editor); `:checkhealth` diagnostica.

Para editar archivos del sistema sin abrir el editor como root: `sudoedit /etc/archivo`.

## Neovim como IDE: mapa del capítulo

Empieza por **[[05 Neovim/Ruta de cero a pro]]**: seis niveles con ejercicios y pruebas de dominio que enlazan al resto de notas.

| Nivel | Nota | Contenido |
|---|---|---|
| 0–1 | [[05 Neovim/Edición y atajos]] | gramática de edición y atajos esenciales |
| 1, 5 | [[05 Neovim/Movimientos y edición avanzada]] | movimientos, objetos, registros, macros, `:g`, regex, deshacer, pliegues |
| 2 | [[05 Neovim/Proyectos búsqueda y navegación]] | picker, Neo-tree, buffers/ventanas, quickfix, reemplazo en el proyecto, sesiones |
| 3 | [[05 Neovim/IDE - LSP completado formato y lint]] | LSP, diagnósticos, Mason, blink.cmp, conform, nvim-lint, Treesitter |
| 3 | [[05 Neovim/IDE - Lenguajes y extras]] | extras recomendados (Python, TypeScript, Bash, Markdown…) e IA |
| 3 | [[05 Neovim/IDE - Git]] | gitsigns, lazygit, conflictos, modo diff |
| 3 | [[05 Neovim/IDE - Depuración pruebas y ejecución]] | DAP, neotest, terminal, `:make` |
| 4 | [[05 Neovim/Configuración Lua a fondo]] | arranque, Lua, opciones, atajos, autocomandos, lazy.nvim, `vim.pack` |
| 4 | [[05 Neovim/Configuración y diagnóstico]] | estructura de `~/.config/nvim`, mantenimiento, `:checkhealth` |
| todos | [[05 Neovim/Referencia de atajos LazyVim]] | todos los atajos por categoría |
| 5 | [[05 Neovim/Ayuda integrada y documentación oficial]] | dominar `:help` y el mapa de neovim.io/doc |

Fuentes: [documentación de Neovim](https://neovim.io/doc/), [manual Neovim](https://neovim.io/doc/user/), [LazyVim](https://www.lazyvim.org/), [LazyVim: configuración](https://www.lazyvim.org/configuration), [LazyVim: extras](https://www.lazyvim.org/extras), [Omarchy: Neovim](https://omarchy.org/manual/neovim/).
