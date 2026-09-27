---
aliases: [Atajos Herdr, Agentes Herdr]
tags: [herdr, atajos, agentes, referencia]
actualizado: 2026-09-27
verificado_en: "Herdr 0.8.2 · ~/.config/herdr/config.toml"
---
# Herdr: workspaces, atajos y agentes

Atajos **efectivos de `~/.config/herdr/config.toml`** (Omarchy los alinea con tmux), no necesariamente los defaults publicados por Herdr. *Prefijo* = `Ctrl+Espacio`; «directo» = sin prefijo.

## Panes

| Atajo | Acción |
|---|---|
| `Prefijo, h` · directo `Alt+Enter` | dividir (`split_horizontal`) |
| `Prefijo, v` · directo `Alt+Shift+Enter` | dividir (`split_vertical`) |
| `Prefijo, x` · directo `Alt+Esc` | cerrar pane |
| `Prefijo, z` | zoom |
| `Prefijo, ;` | último pane usado |
| directo `Ctrl+Alt+Flechas` | enfocar pane |
| directo `Ctrl+Alt+Shift+Flechas` | redimensionar |
| `Prefijo, Ctrl+Flechas` | modo redimensionar |
| `Prefijo, O` (Shift+o) | renombrar pane |

> [!tip] «Horizontal» y «vertical»
> Cada herramienta nombra la división al revés según piense en la línea divisoria o en la disposición de los paneles. Lo que importa es el resultado: compruébalo una vez con `Prefijo, ?` y memoriza la tecla, no el nombre.

## Tabs

| Atajo | Acción |
|---|---|
| `Prefijo, c` | nueva tab |
| `Prefijo, r` | renombrar tab |
| `Prefijo, k` | cerrar tab |
| `Prefijo, 1…9` · directo `Alt+1…9` | ir a la tab N |
| `Prefijo, p` / `Prefijo, n` · directo `Alt+Izq/Der` | tab anterior / siguiente |
| directo `Alt+Shift+Izq/Der` | mover la tab |

## Workspaces

| Atajo | Acción |
|---|---|
| `Prefijo, C` / `R` / `K` (con Shift) | nuevo / renombrar / cerrar workspace |
| `Prefijo, P` / `Prefijo, N` · directo `Alt+Arriba/Abajo` | workspace anterior / siguiente |

## General

Más atajos (ajustes `Prefijo, s`, selector `Prefijo, w`, *goto* `Prefijo, g`, worktree `Prefijo, G`, barra lateral `Prefijo, b`…): [[07 Herdr/Herdr avanzado#Atajos que no estaban en la tabla básica]].

| Atajo | Acción |
|---|---|
| `Prefijo, [` | modo copia |
| `Prefijo, d` | desconectar cliente |
| `Prefijo, q` | recargar configuración |
| `Prefijo, ?` | ayuda de atajos |

## Agentes

Herdr reconoce agentes de programación (Claude Code, Codex, Copilot CLI, opencode, pi, Gemini, Cursor, Devin, Kimi…) que corren en un pane y resume su estado en la barra lateral:

| Estado | Significado |
|---|---|
| **Working** | procesando |
| **Blocked** | espera tu aprobación o una respuesta |
| **Idle** | terminó o espera una nueva instrucción |

La detección usa **integraciones** (hooks del propio agente, autoritativas) cuando están instaladas y, si no, analiza la pantalla con manifiestos TOML. En este equipo están instaladas las de Claude, Codex, Copilot, opencode y pi (`herdr integration status`).

```bash
herdr agent list                         # agentes y estado
herdr agent read <destino>               # leer su salida
herdr agent prompt <destino> "texto"     # enviarle una instrucción
herdr agent wait <destino> --help        # esperar a que llegue a un estado (scripts)
herdr agent attach <nombre>              # entrar directamente en su terminal
herdr agent rename w1:p1 revisor         # nombre visible
herdr agent explain <destino> --json     # por qué se detectó ese estado
```

Un agente es **un programa en un pane**, no un plugin de Omarchy Shell (aunque el widget `omarchy.agents` de la barra muestra el uso de los agentes).

## Layouts de Omarchy para Herdr

Las funciones de Bash `hdl`, `hds`, `hdlm` y `hsl` crean disposiciones de desarrollo **dentro de Herdr** (equivalentes de `tdl`, `tds`, `tdlm`, `tsl` para tmux):

| Función | Crea |
|---|---|
| `hdl <ia> [ia2]` | editor + agente de IA (`c` = opencode, `cx` = Claude Code, `codex`…) + terminal; exige estar dentro de Herdr |
| `hds` | cuadrado: editor, vigilancia de diffs, terminal y opencode |
| `hdlm <ia> [ia2]` | un `hdl` por cada subdirectorio |
| `hsl N orden` | enjambre de N panes ejecutando la misma orden |

Pueden crear panes y lanzar editor o agentes: revisa antes con `type hdl` o en `/usr/share/omarchy/default/bash/fns/herdr`.

Fuentes: [configuración](https://herdr.dev/docs/configuration/), [teclado](https://herdr.dev/docs/keyboard/), [conceptos](https://herdr.dev/docs/concepts/), [agentes](https://herdr.dev/docs/agents/), [integraciones](https://herdr.dev/docs/integrations/), [Omarchy: shell functions](https://omarchy.org/manual/shell-functions/).
