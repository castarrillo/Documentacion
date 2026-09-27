---
aliases: [Edición avanzada Vim, Macros, Registros, Regex Vim]
tags: [neovim, edicion, referencia]
actualizado: 2026-09-27
verificado_en: "Neovim 0.12.5 · :help motion, change, pattern, repeat, undo, fold"
---
# Movimientos y edición avanzada

Referencia de niveles 1 y 5 de la [[05 Neovim/Ruta de cero a pro]]. Lo básico está en [[05 Neovim/Edición y atajos]]; aquí está todo lo que te hace rápido. Entre paréntesis, la entrada de `:help` correspondiente.

## Movimientos (`:help motion.txt`)

### Horizontales y por palabra

| Tecla | Destino |
|---|---|
| `0` / `^` / `$` / `g_` | columna 0 / primer carácter no blanco / final / último no blanco |
| `w` / `b` / `e` / `ge` | inicio de palabra siguiente / anterior / final / final anterior |
| `W` `B` `E` `gE` | igual, pero por **PALABRA** (separada sólo por espacios: `foo.bar()` es una sola) |
| `f{c}` / `F{c}` | hasta el carácter `c` (adelante / atrás), incluido |
| `t{c}` / `T{c}` | hasta justo antes de `c` |
| `;` / `,` | repetir el último `f/t/F/T` / en sentido contrario |
| `%` | paréntesis, corchete o llave emparejado |
| `{n}\|` | columna `n` |

### Verticales y por pantalla

| Tecla | Destino |
|---|---|
| `gg` / `G` / `{n}G` o `:{n}` | primera / última / línea `n` |
| `{` / `}` | párrafo anterior / siguiente (línea en blanco) |
| `(` / `)` | frase anterior / siguiente |
| `H` / `M` / `L` | arriba / centro / abajo de la pantalla |
| `Ctrl+d` / `Ctrl+u` | media pantalla abajo / arriba |
| `Ctrl+f` / `Ctrl+b` | pantalla completa |
| `zz` / `zt` / `zb` | colocar la línea actual en el centro / arriba / abajo |
| `gj` / `gk` | línea visual (con ajuste) — LazyVim ya lo hace con `j`/`k` |
| `{n}j` / `{n}k` | `n` líneas; activa números relativos (`Espacio u L`) para verlas |

### Saltos y listas (`:help jump-motions`, `changelist`)

| Tecla | Acción |
|---|---|
| `Ctrl+o` / `Ctrl+i` | posición anterior / siguiente en la **jumplist** (`:jumps`, `Espacio s j`) |
| `g;` / `g,` | cambio anterior / siguiente (**changelist**, `:changes`) |
| `` `. `` / `'.` | último cambio (posición exacta / línea) |
| `` `^ `` | donde saliste de Insertar (`gi` vuelve y entra en Insertar) |
| `` `[ `` / `` `] `` | inicio / fin del último texto cambiado o pegado |
| `gv` | reseleccionar la última selección visual |

Cuentan como **salto**: `G`, `/`, `n`, `%`, `(`, `{`, `gd`, `Ctrl+]`, `'marca`… pero no `j`/`k`.

### Marcas (`:help mark-motions`)

| Tecla | Acción |
|---|---|
| `m{a-z}` | marca local al archivo |
| `m{A-Z}` | marca **global** (recuerda el archivo: salta entre proyectos) |
| `'a` / `` `a `` | ir a la línea / a la posición exacta de la marca |
| `:marks` · `Espacio s m` | listar |
| `:delmarks a` / `:delm!` | borrar una / todas las locales |

## Objetos de texto (`:help text-objects`)

`i` = *inner* (sin delimitadores/espacios), `a` = *a*/*around* (con ellos). Úsalos tras un operador (`d`, `c`, `y`, `v`…).

| Objeto | Selecciona | Objeto | Selecciona |
|---|---|---|---|
| `iw` / `aw` | palabra | `iW` / `aW` | PALABRA |
| `is` / `as` | frase | `ip` / `ap` | párrafo |
| `i"` `i'` `` i` `` | dentro de comillas | `a"` … | con comillas |
| `i(` `ib` / `a(` | paréntesis | `i{` `iB` / `a{` | llaves |
| `i[` / `a[` | corchetes | `i<` / `a<` | ángulos |
| `it` / `at` | etiqueta HTML/XML | | |

**Añadidos por LazyVim** (mini.ai + Treesitter): `if`/`af` función, `ic`/`ac` clase, `io`/`ao` bloque, bucle o condicional, `iu`/`au` llamada a función (`iU`/`aU` sin punto en el nombre), `it`/`at` etiqueta, `ig`/`ag` todo el buffer, `ie`/`ae` trozo de palabra en camelCase/snake_case, `id`/`ad` dígitos. Además `in`/`an` + el objeto actúan sobre el *siguiente* (`cin"` = cambiar el siguiente texto entre comillas) e `il`/`al` sobre el *anterior*.

**Nativo de Neovim 0.12** (Treesitter): en Visual, `an` amplía la selección al nodo padre e `in` la reduce al hijo; `]n` / `[n` saltan entre nodos hermanos.

Ejemplos: `daf` borra la función entera · `yi(` copia los argumentos · `vip` selecciona el párrafo · `ci{` reescribe el cuerpo de un bloque · `=io` reindenta el bloque · `yag` copia todo el archivo.

## Operadores (`:help operator`)

| Operador | Acción | Operador | Acción |
|---|---|---|---|
| `d` | borrar | `c` | cambiar |
| `y` | copiar | `=` | reindentar |
| `>` / `<` | indentar / desindentar | `gc` | comentar |
| `gu` / `gU` / `g~` | minúsculas / mayúsculas / invertir | `gq` / `gw` | formatear texto (ajustar a `textwidth`) |
| `g?` | ROT13 | `zf` | crear pliegue |
| `!` | filtrar por orden externa (`!ip sort`) | | |

Atajos de línea: `dd`, `cc`, `yy`, `>>`, `gcc`, `guu`. Variantes con mayúscula actúan hasta el final de línea: `D` (= `d$`), `C` (= `c$`), `Y` (= `y$` en Neovim).

## Modo Visual (`:help visual-mode`)

| Tecla | Uso |
|---|---|
| `v` / `V` / `Ctrl+v` | carácter / línea / **bloque** |
| `o` | ir al otro extremo de la selección |
| `Ctrl+v` + `I` texto `Esc` | insertar el mismo texto al inicio de varias líneas |
| `Ctrl+v` + `$` + `A` texto `Esc` | añadir al final de varias líneas de longitud distinta |
| `Ctrl+v` + `c` | reemplazar una columna |
| `g Ctrl+a` | numerar en secuencia (0,0,0 → 1,2,3) |
| `:` | abre `:'<,'>` para ejecutar sobre la selección |
| `Alt+j` / `Alt+k` | mover la selección (LazyVim) |
| `>` / `<` | indentar (LazyVim mantiene la selección) |

## Insertar sin salir (`:help ins-special-keys`)

| Tecla | Acción |
|---|---|
| `Ctrl+w` / `Ctrl+u` | borrar palabra / línea antes del cursor |
| `Ctrl+r {reg}` | pegar un registro (`Ctrl+r +` portapapeles, `Ctrl+r =` resultado de una expresión) |
| `Ctrl+o {orden}` | ejecutar una orden de Normal y volver a Insertar |
| `Ctrl+t` / `Ctrl+d` | indentar / desindentar la línea |
| `Ctrl+a` | reinsertar el último texto insertado |
| `Ctrl+v {tecla}` | insertar literalmente (p. ej. un tabulador real) |
| `Ctrl+k {a}{b}` | dígrafo: `Ctrl+k n?` = ñ, `Ctrl+k e'` = é (`:digraphs`) |
| `Ctrl+x Ctrl+f` | completar ruta de archivo |
| `Ctrl+x Ctrl+l` | completar línea entera |

## Registros (`:help registers`)

| Registro | Contenido |
|---|---|
| `""` | por defecto (último borrado o copia) |
| `"0` | última **copia** (`y`); no lo pisan los borrados |
| `"1`–`"9` | historial de borrados de línea |
| `"-` | último borrado pequeño (dentro de una línea) |
| `"a`–`"z` | con nombre; `"A` **añade** al registro `a` |
| `"+` / `"*` | portapapeles del sistema / selección primaria |
| `"_` | agujero negro: borrar sin tocar registros |
| `".` `":` `"/` `"%` | último texto insertado / última orden / última búsqueda / nombre del archivo |
| `"=` | registro de expresión: `"=2*21<Enter>p` pega 42 |

Uso: `"ayy` copia en `a`; `"ap` pega; `"_dd` borra sin perder lo copiado; `:reg` o `Espacio s "` lista. En LazyVim el registro por defecto ya sincroniza con el portapapeles del sistema.

## Repetir, macros y órdenes sobre muchas líneas (`:help repeat.txt`)

| Herramienta | Uso |
|---|---|
| `.` | repite el último cambio |
| `*` + `cgn` + texto + `Esc`, luego `.` `.` | reemplazo «a la carta» de la siguiente coincidencia |
| `q{a-z}` … `q` | grabar macro en un registro |
| `@a` / `@@` / `{n}@a` | reproducir / repetir la última / `n` veces |
| `Q` | reproducir la última macro grabada (Neovim) |
| `:'<,'>norm @a` | ejecutar la macro en cada línea seleccionada |
| `qA` … `q` | añadir pasos a la macro `a` |
| `:let @a = '…'` o editar el registro | corregir una macro |

> [!tip] Macros robustas
> Empieza con un movimiento absoluto (`0`, `^`), busca en lugar de contar (`f,`, `/`), y termina colocándote en la siguiente línea (`j`). Si falla en una línea, la macro se detiene: eso es una ventaja.

### Rangos y la orden `:g`

| Rango | Líneas |
|---|---|
| `:5,10` | 5 a 10 |
| `:.,+3` | actual y 3 siguientes |
| `:%` | todo el archivo |
| `:'<,'>` | selección visual |
| `:/inicio/,/fin/` | entre dos búsquedas |

| Orden | Efecto |
|---|---|
| `:g/patrón/d` | borrar las líneas que coinciden |
| `:v/patrón/d` (o `:g!`) | borrar las que **no** coinciden |
| `:g/TODO/t$` | copiar las líneas con TODO al final |
| `:g/^$/,/./-j` | colapsar líneas en blanco repetidas |
| `:g/patrón/norm A;` | añadir `;` al final de las que coinciden |
| `:sort` / `:sort u` / `:sort n` | ordenar / sin duplicados / numérico |
| `:m +1` / `:t .` | mover línea / duplicarla |
| `:norm {teclas}` | ejecutar teclas de Normal en cada línea del rango |

## Búsqueda y sustitución (`:help pattern.txt`, `:help :s`)

```vim
:%s/viejo/nuevo/g          " todo el archivo
:%s/viejo/nuevo/gc         " confirmando cada una (y/n/a/q)
:s//nuevo/g                " patrón vacío = última búsqueda (busca primero con / o *)
:%s/\vfoo(\d+)/bar\1/g     " \v very magic: grupos sin escapar; \1 = grupo 1
:%s/\<id\>/uid/g           " palabra completa
:%s/\s\+$//e               " quitar espacios finales (e = sin error si no hay)
:%s/nombre/\u&/g           " & = coincidencia; \u mayúscula inicial, \U…\E todo
:%s/x/\=line('.')/         " \= expresión: sustituir por el número de línea
&                          " repetir la última :s en la línea (Neovim: con sus flags)
```

| Patrón | Significado | Patrón | Significado |
|---|---|---|---|
| `\v` | *very magic* (sintaxis tipo regex moderna) | `\c` / `\C` | ignorar / respetar mayúsculas |
| `.` `*` `\+` `\=` | cualquiera, 0+, 1+, 0-1 | `\{2,4}` | entre 2 y 4 |
| `\d` `\w` `\s` | dígito, palabra, espacio | `\a` `\u` `\l` | letra, mayúscula, minúscula |
| `^` `$` | inicio / fin de línea | `\<` `\>` | límites de palabra |
| `\zs` `\ze` | inicio / fin de la coincidencia real | `\n` | salto de línea |

LazyVim ajusta `ignorecase` + `smartcase`: la búsqueda ignora mayúsculas salvo que escribas alguna. `:set hlsearch` resalta; `Esc` lo limpia. Para reemplazar en **todo el proyecto**, ver [[05 Neovim/Proyectos búsqueda y navegación#Buscar y reemplazar en el proyecto]].

## Deshacer (`:help undo.txt`)

| Orden | Acción |
|---|---|
| `u` / `Ctrl+r` | deshacer / rehacer |
| `U` | deshacer todos los cambios de la última línea |
| `g-` / `g+` | moverse por el **árbol** de deshacer (recupera ramas «perdidas») |
| `:earlier 10m` / `:later 5m` | volver en el tiempo (`s`, `m`, `h`, `f` = guardados) |
| `Espacio s u` | árbol de deshacer visual |

LazyVim activa `undofile`: el historial sobrevive al cerrar el archivo (`~/.local/state/nvim/undo/`).

## Plegado (`:help fold.txt`)

| Tecla | Acción |
|---|---|
| `za` | abrir/cerrar el pliegue |
| `zo` / `zc` | abrir / cerrar |
| `zR` / `zM` | abrir / cerrar todos |
| `zj` / `zk` | siguiente / anterior pliegue |
| `zf{mov}` | crear pliegue manual |

LazyVim pliega por Treesitter (`foldmethod=expr`) cuando el lenguaje tiene analizador y por indentación en los demás casos, empezando con todo abierto (`foldlevel=99`).

## Formato de texto y ortografía

- `gqip` reajusta un párrafo a `textwidth`; `:set tw=80`.
- `Espacio u s` activa la ortografía; `]s` / `[s` siguiente/anterior error; `z=` sugerencias; `zg` añadir palabra. Para español: `:set spelllang=es,en` (descarga el diccionario la primera vez).

Fuentes: [motion](https://neovim.io/doc/user/motion/), [change](https://neovim.io/doc/user/change/), [visual](https://neovim.io/doc/user/visual/), [insert](https://neovim.io/doc/user/insert/), [pattern](https://neovim.io/doc/user/pattern/), [repeat](https://neovim.io/doc/user/repeat/), [undo](https://neovim.io/doc/user/undo/), [fold](https://neovim.io/doc/user/fold/), [usr_10](https://neovim.io/doc/user/usr_10/), [usr_12](https://neovim.io/doc/user/usr_12/), [usr_26](https://neovim.io/doc/user/usr_26/), [usr_27](https://neovim.io/doc/user/usr_27/), [mini.ai](https://github.com/echasnovski/mini.ai).
