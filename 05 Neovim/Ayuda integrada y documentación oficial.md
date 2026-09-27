---
aliases: [":help", Documentación Neovim, neovim.io/doc]
tags: [neovim, ayuda, fuentes]
actualizado: 2026-09-27
verificado_en: "Neovim 0.12.5 · /usr/share/nvim/runtime/doc · neovim.io/doc"
---
# Ayuda integrada y documentación oficial

La documentación de [neovim.io/doc](https://neovim.io/doc/) es **la misma** que trae el editor en `:help`, generada desde `/usr/share/nvim/runtime/doc/`. La ayuda local tiene una ventaja: corresponde **exactamente a tu versión** (0.12.5), funciona sin conexión y se navega con el teclado. Saber leerla es lo que distingue a un usuario avanzado.

## Usar `:help`

| Orden / tecla | Acción |
|---|---|
| `:help tema` · `:h tema` | abrir la ayuda (`Tab` completa, `Ctrl+d` lista coincidencias) |
| `Espacio s h` | buscar páginas de ayuda con el picker |
| `Ctrl+]` · `K` | seguir el enlace (`\|etiqueta\|`) bajo el cursor |
| `Ctrl+o` / `Ctrl+t` | volver |
| `gO` | índice de la página actual |
| `:helpgrep patrón` + `:copen` | buscar texto en **toda** la ayuda |
| `:vert h tema` · `:tab h tema` | abrir en división vertical / pestaña |
| `:help!` | ayuda del contexto de la línea |
| `q` | cerrar la ventana de ayuda |

### Prefijos: cómo pedir exactamente lo que quieres

| Qué | Prefijo | Ejemplo |
|---|---|---|
| Tecla en modo Normal | (ninguno) | `:h dd`, `:h CTRL-O`, `:h gv` |
| Tecla en modo Insertar | `i_` | `:h i_CTRL-R` |
| Tecla en modo Visual | `v_` | `:h v_an`, `:h v_b_I` (bloque) |
| Tecla en la línea de órdenes | `c_` | `:h c_CTRL-R` |
| Orden `:` | `:` | `:h :s`, `:h :g`, `:h :cdo` |
| Opción | `'…'` | `:h 'scrolloff'` |
| Función de Vimscript | `()` | `:h expand()` |
| Función Lua | `vim.…` | `:h vim.keymap.set()`, `:h vim.lsp.config()` |
| Evento de autocomando | nombre | `:h BufWritePre` |
| Variable | `v:` / `g:` | `:h v:count` |
| Error | código | `:h E492` |
| Expresiones regulares | `/` | `:h /\v`, `:h /\zs` |

## Mapa de la documentación oficial

### Manual de usuario (`:help usr_toc`) — aprender en orden

| Bloque | Capítulos | Para el nivel de la [[05 Neovim/Ruta de cero a pro\|ruta]] |
|---|---|---|
| Primeros pasos | [usr_02](https://neovim.io/doc/user/usr_02/) primeros pasos · [usr_03](https://neovim.io/doc/user/usr_03/) moverse · [usr_04](https://neovim.io/doc/user/usr_04/) cambios pequeños · [usr_05](https://neovim.io/doc/user/usr_05/) ajustes | 0–1 |
| Varios archivos | [usr_07](https://neovim.io/doc/user/usr_07/) varios archivos · [usr_08](https://neovim.io/doc/user/usr_08/) ventanas | 2 |
| Editar a lo grande | [usr_10](https://neovim.io/doc/user/usr_10/) cambios grandes · [usr_12](https://neovim.io/doc/user/usr_12/) trucos | 1, 5 |
| Productividad | [usr_20](https://neovim.io/doc/user/usr_20/) línea de órdenes · [usr_21](https://neovim.io/doc/user/usr_21/) irse y volver · [usr_22](https://neovim.io/doc/user/usr_22/) encontrar archivos · [usr_24](https://neovim.io/doc/user/usr_24/) insertar rápido · [usr_26](https://neovim.io/doc/user/usr_26/) repetir · [usr_27](https://neovim.io/doc/user/usr_27/) búsqueda y patrones · [usr_28](https://neovim.io/doc/user/usr_28/) plegado | 1–2, 5 |
| Programar | [usr_29](https://neovim.io/doc/user/usr_29/) moverse por programas · [usr_30](https://neovim.io/doc/user/usr_30/) editar programas · [usr_32](https://neovim.io/doc/user/usr_32/) árbol de deshacer | 3, 5 |
| Personalizar | [usr_40](https://neovim.io/doc/user/usr_40/) órdenes nuevas · [usr_41](https://neovim.io/doc/user/usr_41/) Vimscript · [usr_43](https://neovim.io/doc/user/usr_43/) tipos de archivo | 4 |

Índice completo: [usr_toc](https://neovim.io/doc/user/usr_toc/).

### Manual de referencia — consultar

| Tema | Página | Úsala para |
|---|---|---|
| Índice de todas las teclas | [index](https://neovim.io/doc/user/vimindex/) | qué hace cada tecla en cada modo |
| Referencia rápida | [quickref](https://neovim.io/doc/user/quickref/) | chuleta oficial |
| Movimientos y objetos | [motion](https://neovim.io/doc/user/motion/) | [[05 Neovim/Movimientos y edición avanzada]] |
| Cambios, registros, `:s` | [change](https://neovim.io/doc/user/change/) | operadores, registros, sustituir |
| Insertar y completar | [insert](https://neovim.io/doc/user/insert/) | teclas de Insertar, `Ctrl+x` |
| Visual | [visual](https://neovim.io/doc/user/visual/) | bloques |
| Patrones | [pattern](https://neovim.io/doc/user/pattern/) | expresiones regulares |
| Repetir, macros, `:g` | [repeat](https://neovim.io/doc/user/repeat/) | automatizar |
| Deshacer | [undo](https://neovim.io/doc/user/undo/) | árbol de deshacer |
| Ventanas / pestañas | [windows](https://neovim.io/doc/user/windows/) · [tabpage](https://neovim.io/doc/user/tabpage/) | disposición |
| Quickfix | [quickfix](https://neovim.io/doc/user/quickfix/) | grep, `:make`, `:cdo` |
| Plegado | [fold](https://neovim.io/doc/user/fold/) | pliegues |
| Diff | [diff](https://neovim.io/doc/user/diff/) | comparar y fusionar |
| Terminal | [terminal](https://neovim.io/doc/user/terminal/) | `:terminal` |
| Opciones | [options](https://neovim.io/doc/user/options/) | todas las opciones |
| Atajos | [map](https://neovim.io/doc/user/map/) | `:map`, `<leader>` |
| Autocomandos | [autocmd](https://neovim.io/doc/user/autocmd/) | eventos |
| Arranque | [starting](https://neovim.io/doc/user/starting/) | `init.lua`, `--clean`, `NVIM_APPNAME` |
| Diferencias con Vim | [vim_diff](https://neovim.io/doc/user/vim_diff/) | atajos y opciones por defecto de Neovim |

### Neovim moderno: Lua, LSP, Treesitter

| Tema | Página |
|---|---|
| Guía de Lua (empieza aquí) | [lua-guide](https://neovim.io/doc/user/lua-guide/) |
| Referencia Lua (`vim.*`) | [lua](https://neovim.io/doc/user/lua/) |
| API (`vim.api.nvim_*`) | [api](https://neovim.io/doc/user/api/) |
| Cliente LSP | [lsp](https://neovim.io/doc/user/lsp/) |
| Diagnósticos | [diagnostic](https://neovim.io/doc/user/diagnostic/) |
| Treesitter | [treesitter](https://neovim.io/doc/user/treesitter/) |
| Paquetes y `vim.pack` | [pack](https://neovim.io/doc/user/pack/) |
| Salud del sistema | [health](https://neovim.io/doc/user/health/) (`:checkhealth`) |
| Novedades de la versión | [news](https://neovim.io/doc/user/news/) (`:help news`) |
| Preguntas frecuentes | [faq](https://neovim.io/doc/user/faq/) |

## Novedades de Neovim 0.12 que conviene conocer

- **`vim.pack`**: gestor de plugins integrado (experimental). Ver [[05 Neovim/Configuración Lua a fondo]].
- **`:restart`**: reinicia Neovim conservando la sesión (`:restart!` sin sesión).
- **`:lsp`**: `:lsp enable|disable|restart|stop` para gestionar servidores.
- Nuevos atajos LSP por defecto: `grt` (definición de tipo) y `grx` (codelens), que se suman a `grn`, `gra`, `grr`, `gri`, `gO` y `Ctrl+s` (firma en Insertar) de la 0.11.
- **Selección incremental por Treesitter**: `an` / `in` en Visual, `]n` / `[n` entre nodos.
- LSP: completado en línea, rangos de edición enlazados, diagnósticos de todo el workspace.
- `vim.diagnostic.status()` para la línea de estado; ventanas flotantes con información relacionada (`gf` salta).
- Interfaz experimental **ui2** que elimina los mensajes «Press ENTER».
- Cambio: `Ctrl+r` en Insertar pega los registros **literalmente** (más rápido, sin reformatear).

Lista completa y cambios incompatibles: `:help news`, `:help news-breaking`.

## Otras fuentes fiables

| Fuente | Para |
|---|---|
| `:checkhealth` | estado real de tu instalación |
| `:Tutor` (con `nvim --clean +Tutor`) | aprendizaje interactivo |
| [LazyVim](https://www.lazyvim.org/) y su código en `~/.local/share/nvim/lazy/LazyVim/` | qué añade LazyVim |
| `:help` de cada plugin (`:h snacks`, `:h blink-cmp`, `:h gitsigns`) | documentación de plugins, instalada con ellos |
| [kickstart.nvim](https://github.com/nvim-lua/kickstart.nvim) | configuración mínima comentada línea a línea |
| [Neovim en GitHub Discussions](https://github.com/neovim/neovim/discussions) · [r/neovim](https://www.reddit.com/r/neovim/) | preguntas de la comunidad |

> [!tip] Hábito pro
> Cuando alguien te enseñe un truco, búscalo en `:help` y lee los párrafos de alrededor: casi siempre hay dos o tres variantes más útiles.

Fuentes: [neovim.io/doc](https://neovim.io/doc/), [helphelp](https://neovim.io/doc/user/helphelp/), [usr_toc](https://neovim.io/doc/user/usr_toc/), [news](https://neovim.io/doc/user/news/), ayuda local `:help news` de 0.12.5.
