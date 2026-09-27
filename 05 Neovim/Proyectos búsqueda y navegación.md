---
aliases: [Navegar proyectos Neovim, Quickfix, Buffers y ventanas]
tags: [neovim, lazyvim, proyectos, quickfix]
actualizado: 2026-09-27
verificado_en: "Neovim 0.12.5 · LazyVim (snacks picker, neo-tree, grug-far, flash, persistence)"
---
# Proyectos, búsqueda y navegación

Nivel 2 de la [[05 Neovim/Ruta de cero a pro]]. Objetivo: llegar a cualquier archivo, símbolo o texto del proyecto en segundos y trabajar con muchos archivos a la vez.

## Abrir un proyecto

```bash
cd ~/Work/proyecto && nvim        # o: n   (alias de Omarchy → nvim .)
nvim archivo.py +42               # abrir en la línea 42
nvim -d a.txt b.txt               # modo diff
```

**Directorio raíz**: LazyVim calcula la *raíz* del archivo actual por el LSP, luego por marcadores (`.git`, `lua`) y si no por el directorio de trabajo (`cwd`). Los atajos «(Root Dir)» buscan en la raíz; las variantes en mayúscula (`Espacio f F`, `Espacio s G`, `Espacio E`) usan el `cwd`. `:pwd` muestra el cwd; `:cd ruta` / `:lcd ruta` lo cambian (global / sólo ventana). `Espacio f p` abre proyectos recientes.

Al abrir `nvim` sin archivos aparece el **panel de inicio** (snacks dashboard): archivos recientes, proyectos, restaurar sesión, configuración.

## Encontrar cosas (picker)

| Quiero… | Atajo |
|---|---|
| Archivo por nombre | `Espacio Espacio` (raíz) · `Espacio f F` (cwd) · `Espacio f g` (sólo rastreados por git) |
| Archivo reciente | `Espacio f r` |
| Buffer abierto | `Espacio ,` |
| Texto en el proyecto | `Espacio /` (raíz) · `Espacio s G` (cwd) · `Espacio s B` (sólo buffers abiertos) |
| Palabra bajo el cursor / selección | `Espacio s w` |
| Línea del archivo actual | `Espacio s b` |
| Símbolo (función, clase) | `Espacio s s` (archivo) · `Espacio s S` (workspace) |
| Comentarios TODO/FIXME | `Espacio s t` |
| Diagnósticos | `Espacio s d` / `Espacio s D` |
| Orden, atajo, ayuda, man | `Espacio s C` · `Espacio s k` · `Espacio s h` · `Espacio s M` |
| Registros, marcas, saltos | `Espacio s "` · `Espacio s m` · `Espacio s j` |
| Repetir la última búsqueda | `Espacio s R` |

**Dentro del picker**: escribe para filtrar (búsqueda difusa); `Ctrl+j`/`Ctrl+k` o flechas para moverte; `Enter` abre; `Ctrl+v` / `Ctrl+s` abre en división vertical / horizontal; `Ctrl+t` en pestaña; `Tab` marca varios; `Ctrl+q` envía los resultados a la **lista quickfix**; `Alt+h` muestra ocultos, `Alt+i` ignorados; `Esc` cierra. `?` (en modo Normal del picker) muestra su ayuda.

## Explorador de archivos (Neo-tree)

`Espacio e` (raíz) / `Espacio E` (cwd). Dentro:

| Tecla | Acción | Tecla | Acción |
|---|---|---|---|
| `Enter` / `l` | abrir | `h` | cerrar carpeta |
| `a` | crear (termina en `/` para carpeta) | `d` | borrar |
| `r` | renombrar | `c` / `m` | copiar / mover |
| `y` / `x` / `p` | copiar / cortar / pegar | `H` | mostrar ocultos |
| `/` | filtrar | `s` / `S` | abrir en división vertical / horizontal |
| `<` / `>` | cambiar entre archivos, buffers y git | `?` | ayuda |

> [!tip] El árbol no es el camino principal
> Usa el picker para **abrir** y el árbol para **entender la estructura** o manipular archivos. Así evitas navegar carpeta a carpeta.

## Saltar dentro del código

| Tecla | Destino |
|---|---|
| `s` + 2 letras + etiqueta | cualquier lugar visible (flash) |
| `S` | seleccionar un nodo de Treesitter con flash |
| `gd` / `gr` / `gI` / `gy` | definición / referencias / implementación / tipo |
| `]]` / `[[` | siguiente / anterior referencia del símbolo |
| `]f` / `[f`, `]c` / `[c`, `]a` / `[a` | siguiente / anterior función / clase / parámetro (Treesitter) |
| `%` | pareja del paréntesis |
| `gf` | abrir el archivo cuya ruta está bajo el cursor |
| `Ctrl+o` / `Ctrl+i` | volver / avanzar |

## Buffers, ventanas y pestañas

- **Buffer** = archivo abierto. Muchos a la vez es normal. `Shift+h` / `Shift+l`, `Espacio ,`, `Espacio b d` cierra, `Espacio b o` cierra los demás, `Espacio b p` fija uno en la barra.
- **Ventana** = vista. `Espacio -` / `Espacio |` dividen; `Ctrl+h/j/k/l` navegan; `Ctrl+Flechas` redimensionan; `Ctrl+w =` iguala; `Ctrl+w o` deja sólo la actual; `Espacio w m` zoom.
- **Pestaña** = disposición de ventanas. Úsala para contextos distintos (p. ej. código y pruebas): `Espacio Tab Tab`, `Espacio Tab ]`.

Regla práctica: **un buffer por archivo, pocas ventanas, casi ninguna pestaña**.

## Quickfix: la lista de resultados

La **quickfix** es una lista global de ubicaciones (errores del compilador, resultados de grep, diagnósticos). La **location list** es igual pero propia de cada ventana. Es la base de las refactorizaciones grandes (`:help quickfix`).

| Orden / tecla | Acción |
|---|---|
| picker → `Ctrl+q` | enviar resultados a la quickfix |
| `:grep patrón` | rellenarla con ripgrep (`grepprg` es `rg`) |
| `:copen` / `:cclose` · `Espacio x q` | abrir / cerrar (Trouble) |
| `]q` / `[q` | siguiente / anterior elemento |
| `:cdo orden` | ejecutar en **cada elemento** |
| `:cfdo orden` | ejecutar en **cada archivo** de la lista |
| `:colder` / `:cnewer` | listas anteriores / posteriores |
| `Espacio s q` | buscar dentro de la quickfix |

## Buscar y reemplazar en el proyecto

**Opción A — grug-far (visual)**: `Espacio s r` abre un panel con campos *Search*, *Replace*, *Files filter*; muestra la vista previa de cada cambio y los aplica con `\r` (`\` es la *localleader* de LazyVim; `g?` muestra todos los atajos del panel). Con texto seleccionado, lo usa como búsqueda.

**Opción B — quickfix (nativa, muy potente)**:

```vim
:grep "\bcalcularTotal\b"                      " 1. encontrar (rg)
:copen                                         " 2. revisar
:cdo s/calcularTotal/computeTotal/gc | update   " 3. reemplazar confirmando y guardar
```

**Opción C — LSP (sólo símbolos)**: `Espacio c r` renombra una variable o función en todos los archivos con conocimiento del lenguaje. Es la más segura para código.

## Sesiones

LazyVim guarda automáticamente la sesión por directorio (plugin *persistence*).

| Atajo | Acción |
|---|---|
| `Espacio q s` | restaurar la sesión de este directorio |
| `Espacio q l` | restaurar la última sesión |
| `Espacio q S` | elegir sesión |
| `Espacio q d` | no guardar la sesión al salir |

En Neovim 0.12, `:restart` reinicia el editor conservando la sesión (útil tras cambiar la configuración).

## Marcas de archivos favoritos (opcional)

Con el extra `editor.harpoon2` (`:LazyExtras`): `Espacio H` marca el archivo, `Espacio h` abre el menú, `Espacio 1…9` salta al archivo N. Útil cuando trabajas siempre con 3–5 archivos. Alternativa nativa: marcas globales `mA`, `'A`.

Fuentes: [usr_07](https://neovim.io/doc/user/usr_07/), [usr_08](https://neovim.io/doc/user/usr_08/), [usr_22](https://neovim.io/doc/user/usr_22/), [windows](https://neovim.io/doc/user/windows/), [tabpage](https://neovim.io/doc/user/tabpage/), [quickfix](https://neovim.io/doc/user/quickfix/), [LazyVim keymaps](https://www.lazyvim.org/keymaps), [snacks.nvim picker](https://github.com/folke/snacks.nvim/blob/main/docs/picker.md), [neo-tree](https://github.com/nvim-neo-tree/neo-tree.nvim), [grug-far](https://github.com/MagicDuck/grug-far.nvim), [flash.nvim](https://github.com/folke/flash.nvim).
