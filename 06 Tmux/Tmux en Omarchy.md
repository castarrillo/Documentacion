---
aliases: [tmux]
tags: [tmux, terminal, moc]
actualizado: 2026-09-27
verificado_en: "tmux 3.7c · ~/.config/tmux/tmux.conf"
---
# Tmux en Omarchy

tmux es un **multiplexor de terminales**. Un **servidor** en segundo plano mantiene **sesiones**; cada sesión tiene **ventanas** (pestañas) y cada ventana, **paneles** (divisiones) donde corren shells y programas. Puedes desconectarte (*detach*) sin terminar los procesos y reconectarte (*attach*) después, incluso desde otra terminal o por SSH.

```text
servidor tmux
└── sesión «Work»
    ├── ventana 1: editor   ── panel 1 (nvim) │ panel 2 (bash)
    └── ventana 2: logs     ── panel 1 (journalctl -f)
```

La ventana de tmux vive **dentro** de la ventana gráfica de la terminal, que a su vez gestiona Hyprland.

## Órdenes básicas

```bash
tmux new -s trabajo           # nueva sesión con nombre
tmux ls                       # sesiones existentes
tmux attach -t trabajo        # reconectar (o: tmux a -t trabajo)
tmux kill-session -t trabajo  # terminar la sesión y sus programas
t                             # alias Omarchy: adjuntar o crear la sesión «Work»
```

`Super+Alt+Enter` abre una terminal adjunta a la sesión `Work` (`omarchy launch terminal tmux`).

## Prefijo de este equipo

Una instalación estándar usa `Ctrl+b`. **Omarchy usa `Ctrl+Espacio` como prefijo principal y mantiene `Ctrl+b` como secundario** (`prefix2`). Es una **secuencia**: pulsa el prefijo, suelta, y pulsa la segunda tecla. Además, varios atajos (`Alt+…`, `Ctrl+Alt+…`) funcionan **sin prefijo**.

Tabla completa: [[06 Tmux/Sesiones y paneles]]; scripting, formatos, ventanas emergentes y configuración: [[06 Tmux/Tmux avanzado]].

Ayuda: `Prefijo, ?` (chuleta en ventana emergente), `Super+Alt+K` desde el escritorio, o `tmux list-keys -N` en Bash.

## Configuración de Omarchy (resumen)

| Ajuste | Efecto |
|---|---|
| `mouse on` | clic para enfocar, arrastrar bordes, rueda para desplazarse |
| `base-index 1`, `renumber-windows on` | ventanas numeradas desde 1, sin huecos |
| `history-limit 50000` | historial largo por panel |
| `mode-keys vi` | modo copia con teclas de Vim |
| `set-clipboard on`, `allow-passthrough on` | copia al portapapeles del sistema por OSC 52 |
| `detach-on-destroy off` | al cerrar una sesión pasa a otra en lugar de salir |
| `extended-keys on` (csi-u) | combinaciones como `Ctrl+Shift+…` llegan a Neovim |
| `status-position top` | barra de estado arriba |
| `automatic-rename-format '#{b:pane_current_path}'` | la ventana se nombra por su directorio |
| divisiones con `-c "#{pane_current_path}"` | los paneles nuevos heredan el directorio |

Recarga: `Prefijo, q` u `omarchy restart tmux`. Restablecer a los defaults de Omarchy: `omarchy refresh tmux` (⚠️ sobrescribe tu archivo).

## tmux frente a Herdr

| | tmux | Herdr |
|---|---|---|
| Jerarquía | sesión → ventana → panel | sesión → workspace → tab → pane |
| Agentes de IA | sin conciencia especial | detecta estado (trabajando, bloqueado, listo) |
| Configuración | `tmux.conf` (órdenes tmux) | `config.toml` |
| Ubicuidad | en cualquier servidor Unix | hay que instalarlo |

Ambos usan el mismo prefijo en este equipo y teclas parecidas. No uses órdenes `tmux` para Herdr ni al revés. Ver [[07 Herdr/Herdr en Omarchy]].

Fuentes: [wiki oficial](https://github.com/tmux/tmux/wiki), [Getting Started](https://github.com/tmux/tmux/wiki/Getting-Started), [manual](https://man.openbsd.org/tmux), [Clipboard](https://github.com/tmux/tmux/wiki/Clipboard), [Omarchy: terminal](https://omarchy.org/manual/terminal/), [ArchWiki: tmux](https://wiki.archlinux.org/title/Tmux).
