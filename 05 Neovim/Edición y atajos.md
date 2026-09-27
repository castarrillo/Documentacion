---
aliases: [Edición Vim, Movimientos Vim]
tags: [neovim, lazyvim, atajos]
actualizado: 2026-09-27
verificado_en: "Neovim 0.12.5 · LazyVim"
---
# Neovim: edición y atajos esenciales

## Gramática: operador + movimiento (u objeto)

Vim se «habla»: **[número] operador movimiento**. `d3w` = borra 3 palabras; `ci"` = cambia el texto entre comillas; `yap` = copia el párrafo.

| Operadores | | Movimientos | | Objetos de texto (`i` = interior, `a` = con bordes) | |
|---|---|---|---|---|---|
| `d` | borrar | `h j k l` | ← ↓ ↑ → | `iw` / `aw` | palabra |
| `c` | cambiar (borrar + insertar) | `w` / `b` / `e` | palabra siguiente / anterior / final | `is` / `ip` | frase / párrafo |
| `y` | copiar (*yank*) | `0` / `^` / `$` | inicio / primer carácter / fin de línea | `i"` `i'` `` i` `` | entre comillas |
| `>` / `<` | indentar | `gg` / `G` | inicio / final del archivo | `i(` `i[` `i{` | entre paréntesis, corchetes, llaves |
| `gc` | comentar | `f x` / `t x` | hasta el carácter `x` (incl. / excl.) | `it` | entre etiquetas HTML |
| `=` | autoindentar | `%` | paréntesis emparejado | `ig` / `ag` | todo el buffer (LazyVim) |
| `gu` / `gU` | minúsculas / mayúsculas | `{` / `}` | párrafo anterior / siguiente | `if` / `af` | función (Treesitter) |

Doblar el operador actúa sobre la línea: `dd`, `yy`, `cc`, `>>`, `gcc`.

## Órdenes sueltas imprescindibles

| Tecla | Acción | Tecla | Acción |
|---|---|---|---|
| `x` | borrar carácter | `p` / `P` | pegar después / antes |
| `r x` | reemplazar por `x` | `u` / `Ctrl+r` | deshacer / rehacer |
| `J` | unir líneas | `.` | **repetir el último cambio** |
| `~` | cambiar mayúscula | `*` / `#` | buscar la palabra bajo el cursor |
| `Ctrl+a` / `Ctrl+x` | sumar / restar al número | `:%s/viejo/nuevo/gc` | reemplazar en el archivo confirmando |
| `Ctrl+o` / `Ctrl+i` | saltar atrás / adelante | `m a` / `` ` a `` | marcar / volver a la marca |
| `zz` | centrar la línea | `q a` … `q` / `@a` | grabar macro / reproducir |

## LazyVim: lo esencial (modo Normal)

| Teclas | Función |
|---|---|
| `Espacio` (esperar) | menú *which-key* con todos los grupos |
| `Espacio Espacio` / `Espacio f f` | buscar archivos (raíz del proyecto) |
| `Espacio /` / `Espacio s g` | buscar texto (grep) |
| `Espacio ,` / `Espacio f r` | buffers abiertos / archivos recientes |
| `Espacio e` / `Espacio E` | explorador Neo-tree (raíz / cwd) |
| `Espacio g g` | lazygit |
| `Espacio s k` / `Espacio ?` | buscar atajos / atajos del buffer |
| `Shift+h` / `Shift+l` | buffer anterior / siguiente |
| `Espacio b d` | cerrar buffer |
| `Ctrl+h/j/k/l` | ir a la ventana izquierda / abajo / arriba / derecha |
| `Espacio -` / `Espacio \|` | dividir abajo / a la derecha |
| `gd` / `gr` / `K` | definición / referencias / documentación (LSP) |
| `Espacio c a` / `Espacio c r` | acción de código / renombrar símbolo |
| `Espacio c f` | formatear (el autoformato está desactivado aquí) |
| `]d` / `[d` | diagnóstico siguiente / anterior |
| `s` | *flash*: saltar a cualquier lugar visible tecleando 2 letras (`S` = selección por Treesitter) |
| `Ctrl+/` | terminal flotante |
| `Espacio q q` | salir de todo |

*Surround* (`gsa`, `gsd`, `gsr`) no está instalado: es el extra `coding.mini-surround` de `:LazyExtras`. Lista completa: [[05 Neovim/Referencia de atajos LazyVim]]. Estado real: `Espacio s k`, `:map`, `:verbose nmap <tecla>`.

## Copiar y pegar

Neovim tiene **registros**: `""` (por defecto), `"0` (último *yank*), `"a`–`"z` (con nombre), `"+` (portapapeles del sistema), `"_` (agujero negro: borrar sin copiar). LazyVim sincroniza el registro por defecto con el portapapeles del sistema (`clipboard=unnamedplus`, salvo en sesiones SSH), así que `y` y `p` ya comparten con otras apps. En este equipo `remote_clipboard.lua` además emite cada copia por OSC 52, de modo que funciona también dentro de tmux y por SSH. `:reg` los muestra.

> [!info] Capas de atajos
> `Espacio …` es de LazyVim; `Ctrl+Espacio` es el prefijo de tmux/Herdr; `Super+…` es de Hyprland. `Super+C/V` son copiar/pegar universales del escritorio, una capa distinta de los registros de Neovim. Ver [[03 Hyprland/Ventanas atajos y monitores#Conflictos entre teclas]].

Fuentes: [Neovim quickref](https://neovim.io/doc/user/quickref/), [manual de usuario](https://neovim.io/doc/user/usr_toc/), [motion](https://neovim.io/doc/user/motion/), [LazyVim keymaps](https://www.lazyvim.org/keymaps), [flash.nvim](https://github.com/folke/flash.nvim), [mini.ai](https://github.com/echasnovski/mini.ai).
