---
aliases: [Bash, Terminal]
tags: [bash, terminal, moc]
actualizado: 2026-09-27
verificado_en: "GNU Bash 5.3.15 · Omarchy 4.0.4-1"
---
# Bash: intérprete de órdenes

La **terminal** (Kitty en este equipo) es la ventana; **Bash** interpreta lo que escribes. Una orden puede ser un **alias**, una **función**, un **builtin** (interno de Bash) o un **ejecutable** del `PATH`.

```bash
type ls cd t n        # ¿alias, función, builtin o archivo?
type -a python3       # todas las coincidencias en orden de prioridad
command -v omarchy    # ruta de la orden que se ejecutaría
printf '%s\n' "$PATH" | tr ':' '\n'
help cd               # ayuda de un builtin
man bash              # manual completo (busca con /)
```

## Cómo se carga Bash en Omarchy

```text
~/.bashrc
  ├─ /usr/share/omarchy/default/bash/env-bootstrap
  ├─ (si no es interactiva, termina aquí)
  └─ /usr/share/omarchy/default/bash/rc
        envs → shell → aliases → functions (+ fns/*) → init (starship, zoxide, mise, fzf) → inputrc
  └─ tus añadidos al final de ~/.bashrc
```

Tus alias y funciones van **al final de `~/.bashrc`** (así sobrescriben los de Omarchy). Aplica cambios con `source ~/.bashrc` o abriendo otra terminal.

> [!info] En tu equipo
> `~/.bashrc` añade `fastfetch` al abrir la terminal, `alias f='sudo openfortivpn'`, carga `~/.local/bin/env` y el autocompletado de OpenClaw.

## Aliases y funciones de Omarchy

| Orden | Hace |
|---|---|
| `ls`, `lsa`, `lt`, `lta` | `eza` con iconos; `lt` = árbol de 2 niveles con estado git |
| `cd` → `zd` | `zoxide`: `cd proy` salta al directorio más usado que coincida |
| `..`, `...`, `....` | subir 1, 2, 3 niveles |
| `ff` / `eff` | buscador `fzf` con vista previa / abrir la selección en `$EDITOR` |
| `n [ruta]` | `nvim` (sin argumentos abre `.`) |
| `open archivo` | abrir con la app predeterminada |
| `g`, `gcm "msg"`, `gcam "msg"`, `gcad` | git, commit, commit -a, commit -a --amend |
| `d` | `docker` |
| `t` / `h` | adjuntar o crear la sesión tmux `Work` / Herdr |
| `a`, `c`, `cx`, `cy` | agente por defecto, opencode, Claude Code, Codex |
| `tdl <ia> [ia2]`, `tds`, `tdlm <ia>`, `tsl N orden` | layouts de desarrollo en tmux (editor + IA + terminal, cuadrícula…) |
| `hdl`, `hds`, `hdlm`, `hsl` | los mismos layouts en Herdr |
| `ic`, `ix`, `icx` | `tdl` con opencode, Claude o ambos |
| `ga rama` / `gd` | crear worktree `../proyecto--rama` y entrar / borrar worktree y rama |
| `compress ruta` / `decompress archivo.tar.gz` | tar.gz |
| `iso2sd imagen.iso` / `format-drive disp nombre` | ⚠️ escribir ISO en una tarjeta / formatear un disco entero |
| `rsw origen destino`, `lsw`, `dsw` | sincronizar con rsync al cambiar archivos / listar / detener |
| `fip`, `lip`, `dip` | reenvío de puertos por SSH: crear / listar / cerrar |
| `mup` | `mise up` sin esperar la antigüedad mínima de versiones |

Revisa cualquiera con `type nombre` antes de usarlo. Código: `/usr/share/omarchy/default/bash/`.

## Redirecciones, tuberías y códigos de salida

| Sintaxis | Significado |
|---|---|
| `orden > archivo` / `>> archivo` | salida estándar: reemplazar / añadir |
| `orden 2> errores.txt` | errores a un archivo |
| `orden &> todo.txt` | salida y errores juntos |
| `orden < entrada.txt` | leer la entrada de un archivo |
| `a \| b` | salida de `a` como entrada de `b` |
| `a && b` / `a \|\| b` | `b` sólo si `a` tuvo éxito / falló |
| `a ; b` | `b` siempre |
| `orden &` | en segundo plano (`jobs`, `fg`, `bg`) |
| `$?` | código de salida de la última orden (`0` = éxito) |

## Edición de la línea (readline)

| Tecla | Acción |
|---|---|
| `Ctrl+R` | buscar en el historial (integrado con fzf) |
| `Ctrl+A` / `Ctrl+E` | inicio / final de línea |
| `Alt+B` / `Alt+F` | palabra atrás / adelante |
| `Ctrl+W` / `Ctrl+U` / `Ctrl+K` | borrar palabra anterior / hasta el inicio / hasta el final |
| `Ctrl+L` | limpiar pantalla |
| `Ctrl+C` / `Ctrl+D` | cancelar la orden / cerrar la shell (EOF) |
| `Ctrl+Z` | suspender (reanudar con `fg`) |
| `Tab` (dos veces) | completar / mostrar opciones |
| `!!`, `!$` | repetir la última orden / su último argumento |

## Buenas prácticas

- `sudo` sólo para administrar el sistema; nunca `sudo` para editar tus propios archivos (y `sudoedit` para los de `/etc`).
- Entrecomilla las variables: `"$archivo"`.
- Antes de un `rm` con comodines, prueba con `ls` el mismo patrón.
- El prompt lo dibuja **Starship** (`~/.config/starship.toml`); no es la shell. Ver [manual Omarchy: prompt](https://omarchy.org/manual/prompt/).

Si empiezas de cero sigue [[04 Bash/Ruta de cero a pro]] (ejercicios por niveles). Continúa en [[04 Bash/Herramientas de línea de órdenes]], [[04 Bash/Scripts y expansión]] y ten a mano [[04 Bash/Referencia rápida de Bash]].

Fuentes: [manual GNU Bash](https://www.gnu.org/software/bash/manual/html_node/index.html), [Bash Startup Files](https://www.gnu.org/software/bash/manual/html_node/Bash-Startup-Files.html), [Redirections](https://www.gnu.org/software/bash/manual/html_node/Redirections.html), [Readline](https://www.gnu.org/software/bash/manual/html_node/Command-Line-Editing.html), [Devhints Bash](https://devhints.io/bash), [Omarchy: shell tools](https://omarchy.org/manual/shell-tools/), [Omarchy: shell functions](https://omarchy.org/manual/shell-functions/), [ArchWiki: Bash](https://wiki.archlinux.org/title/Bash).
