---
aliases: [Atajos tmux]
tags: [tmux, atajos, referencia]
actualizado: 2026-09-27
verificado_en: "tmux 3.7c · ~/.config/tmux/tmux.conf"
---
# Tmux: sesiones, ventanas y paneles

Atajos **efectivos en este equipo** (`~/.config/tmux/tmux.conf`). *Prefijo* = `Ctrl+Espacio` (o `Ctrl+b`); «`Prefijo, c`» significa: pulsa el prefijo, suelta, pulsa `c`. Las filas marcadas «directo» no llevan prefijo.

## Paneles

| Atajo | Efecto |
|---|---|
| `Prefijo, h` · directo `Alt+Enter` | dividir: panel nuevo **debajo** |
| `Prefijo, v` · directo `Alt+Shift+Enter` | dividir: panel nuevo **a la derecha** |
| `Prefijo, x` · directo `Alt+Escape` | cerrar panel |
| `Prefijo, z` | ampliar/restaurar panel (zoom) |
| directo `Ctrl+Alt+Flechas` | enfocar panel en esa dirección |
| directo `Ctrl+Alt+Shift+Flechas` | redimensionar 5 celdas |
| `Prefijo, o` / `Prefijo, ;` | siguiente panel / último panel usado (default de tmux) |
| `Prefijo, Espacio` | siguiente disposición de paneles (default de tmux) |
| `Prefijo, !` | convertir el panel en ventana (default de tmux) |

## Ventanas

| Atajo | Efecto |
|---|---|
| `Prefijo, c` | nueva ventana (en el directorio actual) |
| `Prefijo, r` | renombrar ventana |
| `Prefijo, k` | cerrar ventana |
| directo `Alt+1…9` | ir a la ventana N |
| directo `Alt+Izq` / `Alt+Der` | ventana anterior / siguiente |
| directo `Alt+Shift+Izq/Der` | mover la ventana a la izquierda / derecha |
| `Prefijo, w` | árbol de sesiones y ventanas (default de tmux) |

## Sesiones

| Atajo | Efecto |
|---|---|
| `Prefijo, C` (Shift+c) | nueva sesión |
| `Prefijo, R` | renombrar sesión |
| `Prefijo, K` | cerrar sesión |
| `Prefijo, P` / `Prefijo, N` · directo `Alt+Arriba/Abajo` | sesión anterior / siguiente |
| `Prefijo, s` | elegir sesión (default de tmux) |
| `Prefijo, d` | **desconectar** sin detener programas |

## Modo copia (estilo vi)

| Atajo | Efecto |
|---|---|
| `Prefijo, [` | entrar (o rueda del ratón hacia arriba) |
| `h j k l`, `Ctrl+u/d`, `g`/`G` | moverse |
| `/` / `?` | buscar adelante / atrás (`n`/`N`) |
| `v` | empezar selección |
| `y` | copiar y salir (llega al portapapeles del sistema por OSC 52) |
| `q` | salir |
| `Prefijo, ]` | pegar el búfer de tmux |

## Otros

| Atajo | Efecto |
|---|---|
| `Prefijo, q` | recargar `tmux.conf` |
| `Prefijo, ?` | chuleta de atajos (ventana emergente) |
| `Prefijo, :` | línea de órdenes de tmux |
| `Prefijo, t` | reloj (default de tmux) |

El índice de ventanas y paneles empieza en **1**; algunas guías asumen 0. Para ver dónde estás: `tmux display-message -p '#S:#I.#P #{pane_current_path}'`.

## Flujos

**Proyecto persistente**

```bash
tmux new -s estudio -c ~/Work/proyecto
# Prefijo, c → nvim .   ·   Prefijo, c → pruebas   ·   Prefijo, d → salir
tmux attach -t estudio      # al día siguiente
```

**Layout de desarrollo de Omarchy**: dentro de tmux, `tdl` crea editor + agente de IA + terminal; `tdl cx` usa Claude Code; `tdlm` crea uno por subdirectorio; `tsl 4 'npm test'` hace una cuadrícula de 4 paneles con la misma orden. Ver [[04 Bash/Bash#Aliases y funciones de Omarchy]].

**SSH**: ejecuta tmux en el **equipo remoto** (`ssh servidor -t tmux new -A -s main`) para que sus procesos sobrevivan a un corte de red. En un tmux anidado, pulsa el prefijo dos veces para enviarlo al interior (`Prefijo, Ctrl+Espacio`).

**Guardar tras reiniciar**: tmux no sobrevive a un reinicio del equipo. Para restaurar diseños existen plugins como *tmux-resurrect*; Herdr sí guarda su estado ([[07 Herdr/Herdr en Omarchy]]).

## Portapapeles

La configuración activa `set-clipboard on`, modo vi y `terminal-features …:clipboard`. Si copiar no llega al sistema, comprueba que el emulador admite OSC 52 (Kitty, foot, Ghostty y Alacritty lo hacen) y revisa [Clipboard de tmux](https://github.com/tmux/tmux/wiki/Clipboard).

Fuentes: [Getting Started](https://github.com/tmux/tmux/wiki/Getting-Started), [manual](https://man.openbsd.org/tmux), [Clipboard](https://github.com/tmux/tmux/wiki/Clipboard), [Omarchy: hotkeys](https://omarchy.org/manual/hotkeys/), [Omarchy: shell functions](https://omarchy.org/manual/shell-functions/).
