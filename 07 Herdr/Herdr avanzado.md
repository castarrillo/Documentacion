---
aliases: [Herdr avanzado, Automatizar Herdr, config.toml Herdr]
tags: [herdr, avanzado, configuracion, scripting]
actualizado: 2026-09-27
verificado_en: "Herdr 0.8.2 · herdr --default-config · omarchy menu herdr keybindings --print · herdr config check"
---
# Herdr avanzado

Configuración, funciones menos visibles y automatización por CLI. Básico: [[07 Herdr/Herdr en Omarchy]] y [[07 Herdr/Workspaces y agentes]].

## Ruta de aprendizaje

| Paso | Practica |
|---|---|
| 1 | Abrir (`Super+Ctrl+Enter`), desconectar (`Prefijo, d`) y volver |
| 2 | Panes, tabs y workspaces por proyecto |
| 3 | Selector de workspaces (`Prefijo, w`), *goto* (`Prefijo, g`), barra lateral (`Prefijo, b`) |
| 4 | Agentes: estados, integraciones, `herdr agent …` |
| 5 | Worktrees de git integrados (`Prefijo, G`) |
| 6 | Órdenes propias en `config.toml` y scripting con la CLI |

## Atajos que no estaban en la tabla básica

Salida real de `omarchy menu herdr keybindings --print` (defaults de Herdr que tu configuración no cambia):

| Atajo | Acción |
|---|---|
| `Prefijo, s` | **ajustes** interactivos |
| `Prefijo, w` | selector de workspaces |
| `Prefijo, g` | *goto*: saltar a cualquier workspace/tab/pane |
| `Prefijo, o` | abrir el destino de la última notificación (p. ej. el agente que terminó) |
| `Prefijo, G` (Shift+g) | **nuevo worktree** de git como workspace |
| `Prefijo, e` | editar el *scrollback* del pane en tu editor |
| `Prefijo, Tab` / `Prefijo, Shift+Tab` | pane siguiente / anterior |
| `Prefijo, b` | mostrar/ocultar la barra lateral |
| `Ctrl+v` | pegar imágenes en sesiones remotas (`--remote`) |
| modo navegar: `h j k l`, `↑ ↓` | moverse entre panes y workspaces |

`herdr config check` valida tu archivo (en este equipo: `Config: ok`).

## `config.toml`: secciones

`herdr --default-config` imprime todas las opciones comentadas. Las más útiles:

| Sección | Opciones destacadas |
|---|---|
| `[theme]` | `name` (catppuccin, terminal, tokyo-night, dracula, nord, gruvbox, one-dark, solarized, kanagawa, rose-pine, vesper), `auto_switch` claro/oscuro, `[theme.custom]` colores sueltos |
| `[terminal]` | `default_shell`, `shell_mode`, `new_cwd` (`follow` en este equipo, `home`, ruta fija) |
| `[keys]` | `prefix` y todas las acciones; `[[keys.command]]` órdenes propias |
| `[ui]` | barra lateral (`sidebar_width`, `sidebar_start_collapsed`), `mouse_capture`, `copy_on_select`, `confirm_close`, `pane_gaps`, `accent` |
| `[ui.toast]`, `[ui.sound]` | avisos visuales y sonoros cuando un agente termina o se bloquea |
| `[session]` | restauración de sesión |
| `[experimental]` | `pane_history = false`: guardar el historial de pantalla de cada pane (puede incluir secretos) |
| `[worktrees]` | `directory` donde se crean los worktrees |
| `[update]` | `channel`, comprobaciones de versión |

## Órdenes propias con teclas

```toml
# ~/.config/herdr/config.toml
[[keys.command]]
key = "prefix+alt+g"
type = "popup"          # ventana modal: no altera tus panes
command = "lazygit"
width = "85%"
height = "85%"

[[keys.command]]
key = "prefix+alt+t"
type = "pane"           # pane temporal que se cierra al terminar
command = "npm test"

[[keys.command]]
key = "prefix+alt+s"
type = "shell"          # en segundo plano, sin interfaz
command = "notify-send 'Herdr' 'Guardado'"
```

Aplica con `Prefijo, q` o `herdr server reload-config`.

## Automatización por CLI

La CLI habla con el servidor en ejecución (socket en `~/.config/herdr/herdr.sock`):

| Grupo | Subórdenes |
|---|---|
| `herdr workspace` | `list`, `create`, `get`, `focus`, `rename`, `close` |
| `herdr tab` | `list`, `create`, `get`, `focus`, `rename`, `close` |
| `herdr pane` | `list`, `current`, `get`, `layout`, `process-info`, `focus`, `resize`, `zoom`, `read`, `rename` |
| `herdr worktree` | `list`, `create`, `open`, `remove` |
| `herdr agent` | `list`, `get`, `read`, `send-keys`, `prompt`, `wait`, `attach`, `start`, `rename`, `focus`, `explain` |
| `herdr notification` | `show` |
| `herdr api` | `snapshot` (estado completo en JSON), `schema` |
| `herdr session` | `list`, `attach`, `stop`, `delete` |

Cada una admite `--help`. Ejemplos:

```bash
herdr api snapshot | jq -r '.result.snapshot.workspaces[] | "\(.label): \(.pane_count) panes, agentes \(.agent_status)"'
herdr pane current                                   # dónde estoy
herdr notification show --help                       # avisos propios desde scripts

# Esperar a que un agente termine y avisar
herdr agent wait revisor --help                      # ver estados admitidos
```

Dentro de un pane, Herdr exporta `HERDR_PANE_ID` y `HERDR_TAB_ID`: así sabe un script en qué pane corre. Las funciones `hdl`, `hds`, `hdlm`, `hsl` de Omarchy (`/usr/share/omarchy/default/bash/fns/herdr`) son el mejor ejemplo de scripting real.

## Worktrees integrados

`Prefijo, G` (o `herdr worktree create`) crea un **git worktree** y lo abre como workspace propio: ideal para que un agente trabaje en una rama aislada mientras tú sigues en otra. `herdr worktree list` / `remove` los gestionan. Alternativa en Bash: `ga rama` / `gd`.

## Remoto

```bash
herdr --remote usuario@servidor               # interfaz local, sesión en el servidor
herdr --remote usuario@servidor --session api
```

Requiere Herdr instalado en el servidor. Las imágenes se pegan con `Ctrl+v`.

## Solución de problemas

| Problema | Comprobación |
|---|---|
| Un atajo no responde | `omarchy menu herdr keybindings --print`; ¿lo captura Hyprland (`Super+…`)? |
| Cambios de config sin efecto | `herdr config check`, luego `herdr server reload-config` |
| Estado de agente incorrecto | `herdr agent explain <destino> --json`; `herdr integration status` |
| Servidor bloqueado | `herdr status`; `herdr server stop` y volver a abrir (se restaura la forma de la sesión) |
| Empezar de cero con los atajos | `herdr config reset-keys` (hace copia) u `omarchy refresh herdr` ⚠️ |

Fuentes: `herdr --help`, `herdr --default-config`, [docs](https://herdr.dev/docs/), [configuración](https://herdr.dev/docs/configuration/), [teclado](https://herdr.dev/docs/keyboard/), [CLI](https://herdr.dev/docs/cli-reference/), [agentes](https://herdr.dev/docs/agents/), [estado de sesión](https://herdr.dev/docs/session-state/).
