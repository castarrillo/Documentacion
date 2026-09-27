---
aliases: [tmux avanzado, Scripting tmux, tmux.conf]
tags: [tmux, avanzado, scripting]
actualizado: 2026-09-27
verificado_en: "tmux 3.7c · man tmux · ~/.config/tmux/tmux.conf"
---
# Tmux avanzado

Para quien ya maneja sesiones, ventanas y paneles ([[06 Tmux/Sesiones y paneles]]). Referencia completa: `man tmux` (largo pero excelente; busca con `/`).

## Ruta de aprendizaje

| Paso | Practica | Sabes que lo dominas cuando… |
|---|---|---|
| 1 | `tmux new -s x`, `Prefijo, d`, `tmux a -t x` | recuperas trabajo tras cerrar la terminal |
| 2 | dividir, moverte, zoom, redimensionar | organizas 3 paneles sin ratón |
| 3 | ventanas y sesiones con nombre | tienes una sesión por proyecto y saltas con `Prefijo, s` |
| 4 | modo copia | copias 20 líneas de un registro sin ratón |
| 5 | línea de órdenes y scripting | un script levanta tu entorno de proyecto completo |
| 6 | configuración propia | entiendes cada línea de tu `tmux.conf` |

## La línea de órdenes de tmux

`Prefijo, :` abre un *prompt* donde se escribe cualquier orden de tmux (lo mismo que `tmux orden` desde Bash):

| Orden | Efecto |
|---|---|
| `split-window -h -p 30` | panel a la derecha con el 30 % |
| `select-layout even-horizontal` | reparte los paneles (`tiled`, `main-vertical`…) |
| `swap-pane -U` / `-D` | intercambiar panel |
| `break-pane` / `join-pane -s :2` | panel → ventana / traer la ventana 2 como panel |
| `move-window -t 5` | renumerar ventana |
| `setw synchronize-panes on` | escribir en **todos** los paneles a la vez (servidores en paralelo) |
| `resize-pane -Z` | zoom |
| `kill-server` | cerrar todo tmux ⚠️ |
| `source-file ~/.config/tmux/tmux.conf` | recargar |
| `list-keys -N` / `show -g` | atajos / opciones activas |

## Scripting: tu entorno en un comando

```bash
#!/usr/bin/env bash
# ~/.local/bin/dev-tmux: sesión de desarrollo para el directorio actual
set -euo pipefail
nombre=$(basename "$PWD" | tr . _)
if ! tmux has-session -t "$nombre" 2>/dev/null; then
  tmux new-session -d -s "$nombre" -n editor -c "$PWD"
  tmux send-keys -t "$nombre:editor" 'nvim .' Enter
  tmux split-window -h -p 35 -t "$nombre:editor" -c "$PWD"
  tmux new-window -t "$nombre" -n servidor -c "$PWD"
  tmux send-keys -t "$nombre:servidor" 'npm run dev' Enter
  tmux new-window -t "$nombre" -n git -c "$PWD" 'lazygit'
  tmux select-window -t "$nombre:editor"
fi
if [[ -n ${TMUX:-} ]]; then tmux switch-client -t "$nombre"; else tmux attach -t "$nombre"; fi
```

**Destinos** (`-t`): `sesión`, `sesión:ventana`, `sesión:ventana.panel` (p. ej. `trabajo:2.1`). `send-keys` teclea como si fueras tú (`Enter`, `C-c` para Ctrl+C). Omarchy trae layouts listos: `tdl`, `tds`, `tdlm`, `tsl` ([[04 Bash/Bash#Aliases y funciones de Omarchy]]); lee su código en `/usr/share/omarchy/default/bash/fns/tmux` como ejemplo.

## Formatos: consultar el estado

```bash
tmux display -p '#S:#I.#P #{pane_current_path} #{pane_current_command}'
tmux list-panes -a -F '#{session_name}:#{window_index}.#{pane_index} #{pane_current_command}'
tmux list-sessions -F '#{session_name} #{session_windows} ventanas #{?session_attached,(adjunta),}'
```

Variables útiles: `#S` sesión, `#W` ventana, `#I` índice, `#P` panel, `#{pane_current_path}`, `#{pane_pid}`. `#{?cond,sí,no}` es un condicional. Referencia: sección *FORMATS* de `man tmux`.

## Ventanas emergentes y menús

```bash
tmux display-popup -E -w 80% -h 80% lazygit              # lazygit flotante
tmux display-popup -E -d '#{pane_current_path}' 'rg --files | fzf | xargs -r nvim'
```

Omarchy usa `display-popup` para la chuleta de `Prefijo, ?`. Puedes atarlo a una tecla en tu configuración:

```tmux
bind -N "Lazygit" g display-popup -E -w 90% -h 90% -d "#{pane_current_path}" lazygit
```

## Modo copia avanzado

| Tecla (modo vi) | Acción |
|---|---|
| `Prefijo, [` | entrar |
| `/` `?` `n` `N` | buscar |
| `v` / `V` | empezar selección (configuración de Omarchy) / seleccionar línea |
| `Ctrl+v` | alternar selección rectangular (bloque) |
| `w` `b` `{` `}` `gg` `G` | moverse como en Vim |
| `y` | copiar (va al portapapeles del sistema por OSC 52) |
| `Prefijo, ]` | pegar el último búfer |
| `Prefijo, =` | elegir entre búferes anteriores |

Guardar todo el historial de un panel: `tmux capture-pane -pS - > salida.txt`.

## Configuración propia

Tu archivo es `~/.config/tmux/tmux.conf` (base de Omarchy). Para cambios personales, añádelos **al final**; recarga con `Prefijo, q`.

```tmux
# Estado a la izquierda con nombre de sesión y hora a la derecha
set -g status-left "#[bold] #S "
set -g status-right "%H:%M "

# Abrir panel nuevo a la derecha con Prefijo + |
bind -N "Split derecha" | split-window -h -c "#{pane_current_path}"

# Aviso visual de actividad en otras ventanas
setw -g monitor-activity on
set -g visual-activity off

# Hooks: renombrar la ventana al crear una sesión
set-hook -g after-new-session 'rename-window inicio'
```

> [!warning] `omarchy refresh tmux`
> Sobrescribe tu `tmux.conf` con el de Omarchy. Guarda tus añadidos en un archivo aparte (p. ej. `~/.config/tmux/local.conf`) y cárgalo al final con `source-file -q ~/.config/tmux/local.conf`, así sobreviven.

## Plugins (opcional)

tmux funciona bien sin plugins. Si los quieres, el gestor habitual es [TPM](https://github.com/tmux-plugins/tpm); los más útiles son **tmux-resurrect** / **tmux-continuum** (guardar y restaurar sesiones tras reiniciar el equipo). Si lo que buscas es persistencia tras reinicios, Herdr ya la ofrece de serie ([[07 Herdr/Herdr en Omarchy]]).

## Remoto y anidado

- En un servidor: `ssh host -t 'tmux new -A -s main'` (crea o adjunta).
- tmux dentro de tmux: pulsa el prefijo dos veces para enviarlo al interior (`Prefijo, Ctrl+Espacio`), o usa `Ctrl+b` (prefijo secundario) para el interno si el remoto usa el default.
- Varias personas en la misma sesión: ambos hacen `tmux attach -t x` (mismo usuario) — útil para programar en pareja.

Fuentes: [man tmux](https://man.openbsd.org/tmux), [tmux wiki: Getting Started](https://github.com/tmux/tmux/wiki/Getting-Started), [Formats](https://github.com/tmux/tmux/wiki/Formats), [Clipboard](https://github.com/tmux/tmux/wiki/Clipboard), [ArchWiki: tmux](https://wiki.archlinux.org/title/Tmux).
