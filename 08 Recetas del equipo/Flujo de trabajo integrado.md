---
aliases: [Flujo de trabajo]
tags: [recetas, flujo]
actualizado: 2026-09-27
verificado_en: "Omarchy 4.0.4-1"
---
# Flujo de trabajo integrado

Ejemplo completo: desarrollar un proyecto sin perder el contexto, usando cada capa para lo que hace mejor.

## Paso a paso

1. **Escritorio** — `Super+Ctrl+Enter` abre Herdr (o `Super+Alt+Enter` → tmux). Hyprland recibe la tecla y lanza la terminal en el workspace actual.
2. **Shell** — `cd proyecto` (zoxide salta al directorio aunque no escribas la ruta completa) y `git status`. Arch aporta los programas; Omarchy, los alias (`g`, `gcm`…).
3. **Layout** — `hdl cx` (Herdr) o `tdl cx` (tmux) crea editor + Claude Code + terminal en un paso. O a mano: `Prefijo, c` para una tab/ventana nueva, `Alt+Enter` para dividir.
4. **Editor** — en el panel del editor, `n` abre `nvim .`. `Espacio Espacio` busca archivos, `Espacio /` busca texto, `gd` va a la definición, `Espacio g g` abre lazygit.
5. **Agente** — el panel del agente trabaja; Herdr marca cuándo está **Blocked** esperando tu aprobación.
6. **Rama aislada** — `ga nueva-funcion` crea un worktree `../proyecto--nueva-funcion` y entra en él; `gd` lo borra al terminar.
7. **Navegador y notas** — `Super+2`, `Super+Shift+Enter` para documentación; `Super+3`, `Super+Shift+O` para Obsidian. `Super+1` vuelve al código.
8. **Pausa** — `Super+Ctrl+L` bloquea. Al volver, todo sigue ahí.
9. **Mañana** — `Super+Ctrl+Enter` (o `t` para tmux) te devuelve a la sesión. Si el equipo **se reinició**, Herdr restaura la forma de la sesión (workspaces, tabs, directorios) pero **no** los procesos: vuelve a lanzar servidores y agentes.

```text
tecla Super → Hyprland → terminal (Kitty) → Bash → Herdr/tmux → Neovim/LazyVim · agente · servidor
                      ↘ Omarchy Shell: barra, menú, notificaciones, OSD
                      ↘ PipeWire: audio del navegador · NetworkManager: red
```

## Qué herramienta consultar

| Quiero… | Capa | Nota |
|---|---|---|
| Mover o redimensionar la ventana de la terminal | Hyprland | [[03 Hyprland/Ventanas atajos y monitores]] |
| Otro panel de terminal | tmux / Herdr | [[06 Tmux/Sesiones y paneles]] · [[07 Herdr/Workspaces y agentes]] |
| Dividir el editor | Neovim | [[05 Neovim/Edición y atajos]] |
| Buscar un atajo del editor | LazyVim | [[05 Neovim/Referencia de atajos LazyVim]] |
| Cambiar el tema de todo | Omarchy | [[01 Omarchy/Shell temas y plugins]] |
| Instalar un programa | Arch / Omarchy | [[02 Arch Linux/Paquetes y actualizaciones]] |
| Automatizar algo al iniciar | Omarchy / systemd | [[01 Omarchy/Aplicaciones desarrollo y automatización]] |
| Escribir un script | Bash | [[04 Bash/Scripts y expansión]] |

Fuentes: [manual Omarchy](https://omarchy.org/manual/), [Omarchy: shell functions](https://omarchy.org/manual/shell-functions/), [Herdr](https://herdr.dev/docs/), [tmux](https://github.com/tmux/tmux/wiki/Getting-Started), [Neovim](https://neovim.io/doc/user/).
