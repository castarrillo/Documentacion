---
aliases: [Neovim de cero a pro, Aprender Neovim, Ruta Neovim]
tags: [neovim, lazyvim, aprendizaje, moc]
actualizado: 2026-09-27
verificado_en: "Neovim 0.12.5 · LazyVim · :help usr_toc"
---
# Neovim como IDE principal: de cero a pro

Plan en **seis niveles** para pasar de no saber salir del editor a usar Neovim como IDE principal y configurarlo tú mismo. Sigue la estructura del [manual de usuario oficial](https://neovim.io/doc/user/usr_toc/) (`:help usr_toc`), adaptada a LazyVim y a este equipo. Cada nivel tiene objetivos, ejercicios, lecturas y una **prueba de dominio**: no avances hasta superarla sin mirar la chuleta.

> [!tip] Cómo estudiar
> - **15–30 minutos al día** rinden más que una tarde a la semana: el objetivo es la memoria muscular.
> - Durante las 2 primeras semanas, usa Neovim para **todo** (notas, configuración, código) aunque vayas más lento.
> - Cuando repitas algo tres veces, busca la forma corta: `:help`, `Espacio s k` o [[05 Neovim/Referencia de atajos LazyVim]].
> - No toques la configuración hasta el nivel 4: aprende primero el editor.

## Mapa de la ruta

| Nivel | Meta | Tiempo orientativo | Notas |
|---|---|---|---|
| 0 · Supervivencia | abrir, escribir, guardar, salir | 1 día | [[05 Neovim/Neovim y LazyVim]] |
| 1 · Edición eficiente | editar sin ratón con la gramática de Vim | 1–2 semanas | [[05 Neovim/Edición y atajos]], [[05 Neovim/Movimientos y edición avanzada]] |
| 2 · Proyectos | moverte por un proyecto entero en segundos | 1 semana | [[05 Neovim/Proyectos búsqueda y navegación]] |
| 3 · IDE | LSP, completado, formato, Git, depuración, pruebas | 2 semanas | [[05 Neovim/IDE - LSP completado formato y lint]], [[05 Neovim/IDE - Lenguajes y extras]], [[05 Neovim/IDE - Git]], [[05 Neovim/IDE - Depuración pruebas y ejecución]] |
| 4 · Personalización | configurar con Lua sin romper nada | 1–2 semanas | [[05 Neovim/Configuración Lua a fondo]], [[05 Neovim/Configuración y diagnóstico]] |
| 5 · Pro | macros, quickfix, `:g`, flujos propios, leer la ayuda con soltura | continuo | [[05 Neovim/Ayuda integrada y documentación oficial]] |

---

## Nivel 0 · Supervivencia

**Objetivos**: entender los modos; abrir, editar, guardar y salir sin pánico.

1. `nvim --clean +Tutor` y completa las lecciones 1 y 2 (en este equipo `nvim +Tutor` no está disponible por la configuración de LazyVim).
2. Abre un archivo: `nvim ~/prueba.md`. `i` escribe, `Esc` vuelve a Normal, `:w` guarda, `:q` sale, `:q!` sale sin guardar.
3. Muévete con `h j k l`, `w`, `b`, `0`, `$`, `gg`, `G`. Deshaz con `u`, rehaz con `Ctrl+r`.
4. Pulsa `Espacio` y **espera**: el menú *which-key* muestra todo lo que puedes hacer.

**Lectura**: `:help usr_02` (primeros pasos), `:help usr_03` (moverse), [[05 Neovim/Neovim y LazyVim]].

> [!success] Prueba de dominio
> Creas un archivo, escribes tres párrafos, corriges una palabra, borras una línea, deshaces el borrado, guardas y sales, todo sin usar las flechas ni el ratón.

## Nivel 1 · Edición eficiente

**Objetivos**: pensar en **operador + movimiento/objeto**; repetir con `.`; buscar y reemplazar.

| Semana | Practica | Ejercicio |
|---|---|---|
| 1 | `w b e`, `f t ; ,`, `ciw`, `ci"`, `dap`, `yy p`, `.` | Toma un JSON y cambia todos los valores de cadena con `ci"` + `n` + `.` |
| 1 | `/`, `*`, `n N`, `:%s/a/b/gc` | Renombra una variable en un archivo con `*` + `cgn` + `.` |
| 2 | `V`, `Ctrl+v` (bloque), `>`, `gc`, `J`, `gu/gU` | Comenta 10 líneas y añade `- ` al inicio de otras 10 con `Ctrl+v` + `I` |
| 2 | registros `"a`, `"+`, `"_`; marcas `ma` `'a`; `Ctrl+o` / `Ctrl+i` | Copia tres fragmentos a registros distintos y pégalos en otro archivo |

**Lectura**: `:help usr_04`, `usr_10` (cambios grandes), `usr_24` (insertar rápido), `usr_26` (repetir), `usr_27` (búsqueda y patrones). Notas: [[05 Neovim/Edición y atajos]] y [[05 Neovim/Movimientos y edición avanzada]].

> [!success] Prueba de dominio
> Dado un archivo de 50 líneas, haces 10 cambios diferentes (renombrar, reordenar, comentar, envolver en comillas…) usando `.` al menos 5 veces y sin entrar en modo Visual más de 3 veces.

## Nivel 2 · Proyectos

**Objetivos**: abrir cualquier archivo en 2 segundos, buscar en todo el proyecto, trabajar con varios buffers y ventanas.

- `Espacio Espacio` (archivos), `Espacio /` (texto), `Espacio ,` (buffers), `Espacio f r` (recientes), `Espacio e` (Neo-tree).
- Ventanas: `Espacio -`, `Espacio |`, `Ctrl+h/j/k/l`, `Espacio w d`.
- Saltos: `s` (flash), `gd`, `Ctrl+o` para volver.
- Reemplazo en el proyecto: `Espacio s r` (grug-far) o quickfix + `:cdo`.
- Sesiones: `Espacio q s` restaura la sesión del directorio.

**Lectura**: `:help usr_07` (varios archivos), `usr_08` (ventanas), `usr_22` (encontrar archivos), `:help quickfix`. Nota: [[05 Neovim/Proyectos búsqueda y navegación]].

> [!success] Prueba de dominio
> En un proyecto real encuentras dónde se define una función, todas sus llamadas, abres dos de ellas en ventanas lado a lado y reemplazas su nombre en todo el proyecto revisando cada cambio, sin usar el árbol de archivos.

## Nivel 3 · IDE

**Objetivos**: que Neovim entienda tu código (LSP), te complete, formatee, marque errores, gestione Git, depure y ejecute pruebas.

1. Activa los extras de tus lenguajes con `:LazyExtras` ([[05 Neovim/IDE - Lenguajes y extras]]). En este equipo: Python, TypeScript/JavaScript, JSON, YAML, Markdown, Docker, Bash (ya soportado).
2. Aprende el ciclo del LSP: `gd`, `gr`, `K`, `Espacio c a`, `Espacio c r`, `]d`, `Espacio x x` ([[05 Neovim/IDE - LSP completado formato y lint]]).
3. Git dentro del editor: `]h`, `Espacio g h s`, `Espacio g g` ([[05 Neovim/IDE - Git]]).
4. Activa `dap.core` y `test.core`: pon un punto de ruptura (`Espacio d b`), ejecuta (`Espacio d c`), corre la prueba cercana (`Espacio t r`) ([[05 Neovim/IDE - Depuración pruebas y ejecución]]).

**Lectura**: `:help lsp`, `:help diagnostic`, `:help treesitter`, `:help usr_29` (moverse por programas), `usr_30` (editar programas).

> [!success] Prueba de dominio
> Arreglas un fallo real: localizas el error con diagnósticos, lo reproduces con una prueba, lo depuras con un punto de ruptura, lo corriges, formateas, revisas el diff por hunks y haces commit desde lazygit, todo sin salir de Neovim.

## Nivel 4 · Personalización

**Objetivos**: entender cómo se carga la configuración y cambiarla con Lua de forma segura y versionada.

- Opciones (`vim.opt`), atajos (`vim.keymap.set`), autocomandos (`vim.api.nvim_create_autocmd`), órdenes propias (`nvim_create_user_command`).
- Especificaciones de lazy.nvim: `opts`, `keys`, `event`, `ft`, `cmd`, `enabled`, `dependencies`.
- Archivos por tipo (`after/ftplugin/python.lua`), un plugin local mínimo.
- Versionar `~/.config/nvim` con git.

**Lectura**: `:help lua-guide`, `:help lua`, `:help usr_05` (ajustes), `usr_40` (órdenes nuevas), `usr_43` (tipos de archivo). Notas: [[05 Neovim/Configuración Lua a fondo]], [[05 Neovim/Configuración y diagnóstico]].

> [!success] Prueba de dominio
> Añades un atajo, un autocomando por tipo de archivo, una orden `:MiOrden` y un plugin con carga perezosa; `:checkhealth` y `:Lazy` no muestran errores, y todo está en un commit.

## Nivel 5 · Pro

**Objetivos**: automatizar ediciones complejas, dominar quickfix y la ayuda, y construir tus propios flujos.

- Macros recursivas y sobre selección (`:'<,'>norm @q`), `:g/patrón/orden`, rangos y expresiones regulares *very magic* (`\v`).
- Quickfix como herramienta de refactorización: grep → `:cdo s/…/…/ | update`.
- Árbol de deshacer (`:help undo-tree`, `g-`, `g+`, `:earlier 10m`).
- Plegado por Treesitter, modo diff (`nvim -d`), terminal integrada, `:restart`.
- Leer la ayuda como referencia primaria (`:help` + `Ctrl+]`), las notas de versión (`:help news`) y el código de los plugins.

**Lectura**: `:help usr_12` (trucos), `usr_20` (línea de órdenes), `usr_21` (irse y volver), `usr_28` (plegado), `usr_32` (árbol de deshacer), `:help pattern`, `:help quickfix`. Nota: [[05 Neovim/Ayuda integrada y documentación oficial]].

> [!success] Prueba de dominio
> Resuelves un cambio masivo (p. ej. migrar una API en 30 archivos) con grep + quickfix + macro o `:cdo`, y documentas el flujo en una nota propia.

---

## Hábitos de un usuario avanzado

- **Nunca** mantengas `j`/`k` pulsadas: usa búsqueda (`/`, `s`), `}` / `{`, `Ctrl+d` / `Ctrl+u` o números relativos.
- Piensa en **objetos de texto** (`ci(`, `daf`) antes que en caracteres.
- Haz que los cambios sean **repetibles** con `.`: prefiere `cgn` a reemplazos manuales.
- Confía en `Ctrl+o` para volver de cualquier salto.
- Si una acción no tiene atajo, busca con `Espacio s k` (atajos) o `Espacio s C` (órdenes); no memorices todo.
- Configuración mínima y versionada; cada cambio con un motivo.

Fuentes: [manual de usuario](https://neovim.io/doc/user/usr_toc/), [Lua guide](https://neovim.io/doc/user/lua-guide/), [LSP](https://neovim.io/doc/user/lsp/), [quickfix](https://neovim.io/doc/user/quickfix/), [LazyVim](https://www.lazyvim.org/), [LazyVim keymaps](https://www.lazyvim.org/keymaps).
