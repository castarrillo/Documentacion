---
aliases: [LazyVim keymaps, Atajos LazyVim]
tags: [neovim, lazyvim, atajos, referencia]
actualizado: 2026-09-27
verificado_en: "lazyvim.org/keymaps + código local de LazyVim (extra neo-tree activo)"
---
# Referencia de atajos de LazyVim

Atajos predeterminados de LazyVim según [lazyvim.org/keymaps](https://www.lazyvim.org/keymaps), contrastados con el código instalado en `~/.local/share/nvim/lazy/LazyVim/`. `<leader>` = **Espacio**. Modos: `n` Normal, `i` Insertar, `x`/`v` Visual, `o` operador, `t` terminal. Los plugins y extras que actives pueden añadir o cambiar atajos: la verdad está en `Espacio s k`.

## General

| Tecla | Acción | Modo |
|---|---|---|
| `Ctrl+s` | Guardar archivo | i, x, n, s |
| `Esc` | Salir y limpiar resaltado de búsqueda | i, n, s |
| `j` / `k` | Bajar / subir (por línea visual si hay ajuste) | n, x |
| `Alt+j` / `Alt+k` | Mover línea o selección abajo / arriba | n, i, v |
| `gco` / `gcO` | Añadir comentario debajo / encima | n |
| `gcc` / `gc` | Comentar línea / selección | n / x |
| `<leader>fn` | Archivo nuevo | n |
| `<leader>l` | Lazy (gestor de plugins) | n |
| `<leader>L` | Registro de cambios de LazyVim | n |
| `<leader>K` | Keywordprg (ayuda del programa externo) | n |
| `<leader>ur` | Redibujar / limpiar búsqueda / actualizar diff | n |
| `<leader>qq` | Salir de todo | n |

## Buscar (picker)

| Tecla | Acción |
|---|---|
| `<leader><space>` / `<leader>ff` | Buscar archivos (raíz del proyecto) |
| `<leader>fF` | Buscar archivos (directorio actual) |
| `<leader>fg` | Archivos rastreados por git |
| `<leader>fr` | Recientes |
| `<leader>fc` | Archivo de configuración de Neovim |
| `<leader>,` / `<leader>fb` | Buffers |
| `<leader>/` / `<leader>sg` | Grep (raíz) |
| `<leader>sw` | Palabra bajo el cursor o selección (raíz) |
| `<leader>sb` | Líneas del buffer |
| `<leader>:` | Historial de órdenes |
| `<leader>sh` | Páginas de ayuda |
| `<leader>sk` | Atajos |
| `<leader>sj` / `<leader>sm` | Saltos / marcas |
| `<leader>sd` / `<leader>sD` | Diagnósticos (proyecto / buffer) |
| `<leader>ss` / `<leader>sS` | Símbolos LSP (documento / workspace) |
| `<leader>sr` | Buscar y reemplazar (grug-far) |
| `<leader>sR` | Reanudar la última búsqueda |
| `<leader>uC` | Esquemas de color |
| `<leader>n` | Historial de notificaciones |
| `<leader>?` | Atajos del buffer (which-key) |

## Explorador (extra neo-tree en este equipo)

| Tecla | Acción |
|---|---|
| `<leader>e` / `<leader>fe` | Neo-tree (raíz) |
| `<leader>E` / `<leader>fE` | Neo-tree (directorio actual) |

Dentro de Neo-tree: `a` crear, `d` borrar, `r` renombrar, `c`/`m` copiar/mover, `H` ocultos, `?` ayuda.

## Buffers

| Tecla | Acción |
|---|---|
| `Shift+h` / `Shift+l` · `[b` / `]b` | Buffer anterior / siguiente |
| `<leader>bb` · ``<leader>` `` | Alternar con el otro buffer |
| `<leader>bd` | Cerrar buffer |
| `<leader>bD` | Cerrar buffer y ventana |
| `<leader>bo` | Cerrar los demás buffers |
| `<leader>bi` | Cerrar buffers no visibles |

## Ventanas y pestañas

| Tecla | Acción |
|---|---|
| `Ctrl+h/j/k/l` | Ir a la ventana izquierda / abajo / arriba / derecha |
| `Ctrl+Flechas` | Cambiar alto/ancho de la ventana |
| `<leader>-` / `<leader>\|` | Dividir abajo / a la derecha |
| `<leader>wd` | Cerrar ventana |
| `<leader>wm` · `<leader>uZ` | Zoom de ventana |
| `<leader>uz` | Modo zen |
| `<leader><tab><tab>` | Pestaña nueva |
| `<leader><tab>]` / `<leader><tab>[` | Pestaña siguiente / anterior |
| `<leader><tab>f` / `<leader><tab>l` | Primera / última pestaña |
| `<leader><tab>d` / `<leader><tab>o` | Cerrar pestaña / cerrar las demás |

## LSP y código

| Tecla | Acción | Modo |
|---|---|---|
| `gd` / `gD` | Ir a definición / declaración | n |
| `gr` | Referencias | n |
| `gI` / `gy` | Implementación / definición de tipo | n |
| `K` | Documentación (hover) | n |
| `gK` · `Ctrl+k` | Ayuda de firma | n · i |
| `gai` / `gao` | Llamadas entrantes / salientes | n |
| `]]` / `[[` · `Alt+n` / `Alt+p` | Referencia siguiente / anterior | n |
| `<leader>ca` | Acción de código | n, x |
| `<leader>cA` | Acción de código fuente | n |
| `<leader>cr` | Renombrar símbolo | n |
| `<leader>cR` | Renombrar archivo | n |
| `<leader>co` | Organizar imports | n |
| `<leader>cc` / `<leader>cC` | Ejecutar / refrescar codelens | n |
| `<leader>cf` | Formatear | n, x |
| `<leader>cl` | Información de LSP | n |
| `<leader>cm` | Mason | n |
| `<leader>cd` | Diagnósticos de la línea | n |

## Diagnósticos y listas

| Tecla | Acción |
|---|---|
| `]d` / `[d` | Diagnóstico siguiente / anterior |
| `]e` / `[e` | Error siguiente / anterior |
| `]w` / `[w` | Advertencia siguiente / anterior |
| `<leader>xx` / `<leader>xX` | Diagnósticos en Trouble (proyecto / buffer) |
| `<leader>xl` / `<leader>xq` | Lista de ubicaciones / quickfix |
| `]q` / `[q` | Quickfix siguiente / anterior |

## Git

| Tecla | Acción |
|---|---|
| `<leader>gg` / `<leader>gG` | lazygit (raíz / cwd) |
| `<leader>gs` | Estado de git |
| `<leader>gd` | Diff (hunks) |
| `<leader>gl` / `<leader>gL` | Log (raíz / cwd) |
| `<leader>gf` | Historial del archivo actual |
| `<leader>gb` | Blame de la línea |
| `<leader>gB` / `<leader>gY` | Abrir en el navegador / copiar URL remota |
| `]h` / `[h` | Hunk siguiente / anterior (gitsigns) |

## Interruptores de interfaz (`<leader>u…`)

| Tecla | Alterna | Tecla | Alterna |
|---|---|---|---|
| `<leader>uf` / `uF` | Autoformato global / buffer | `<leader>uw` | Ajuste de línea |
| `<leader>us` | Ortografía | `<leader>ul` / `uL` | Números / números relativos |
| `<leader>ud` | Diagnósticos | `<leader>uh` | *Inlay hints* |
| `<leader>uc` | Nivel de ocultación | `<leader>ug` | Guías de indentación |
| `<leader>uT` | Resaltado Treesitter | `<leader>ub` | Fondo oscuro |
| `<leader>uD` | Atenuación | `<leader>ua` | Animaciones |
| `<leader>uS` | Desplazamiento suave | `<leader>uA` | Barra de pestañas |
| `<leader>un` | Descartar notificaciones | `<leader>ui` / `uI` | Inspeccionar posición / árbol |

## Terminal, sesiones y movimiento rápido

| Tecla | Acción |
|---|---|
| `Ctrl+/` · `<leader>ft` | Terminal flotante (raíz) |
| `<leader>fT` | Terminal (cwd) |
| `<leader>qs` / `<leader>ql` | Restaurar sesión del directorio / última sesión |
| `<leader>qS` / `<leader>qd` | Elegir sesión / no guardar la actual |
| `s` / `S` | Flash: saltar / seleccionar por Treesitter |
| `r` (tras un operador) | Flash remoto |
| `<leader>dpp` / `<leader>dph` | Perfilador / resaltados del perfilador |

> [!info] Atajos de extras
> Los extras añaden sus propios atajos: depuración `Espacio d …` y pruebas `Espacio t …` ([[05 Neovim/IDE - Depuración pruebas y ejecución]]), IA `Espacio a …` y Python `Espacio c v` ([[05 Neovim/IDE - Lenguajes y extras]]). Git por hunks `Espacio g h …`: [[05 Neovim/IDE - Git]].

Fuentes: [LazyVim keymaps](https://www.lazyvim.org/keymaps), [extra neo-tree](https://www.lazyvim.org/extras/editor/neo-tree), código local `lazyvim/config/keymaps.lua` y `lazyvim/plugins/`.
