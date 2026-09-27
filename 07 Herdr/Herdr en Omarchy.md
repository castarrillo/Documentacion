---
aliases: [Herdr]
tags: [herdr, terminal, agentes, moc]
actualizado: 2026-09-27
verificado_en: "Herdr 0.8.2 · ~/.config/herdr/config.toml"
---
# Herdr en Omarchy

Herdr es un **gestor de workspaces de terminal persistentes pensado para agentes de programación**. Como tmux, un **servidor** mantiene los procesos cuando cierras el cliente; además organiza el trabajo por proyecto y reconoce en qué estado está cada agente de IA (trabajando, bloqueado esperando aprobación, inactivo).

```text
sesión «default» (servidor persistente, socket en ~/.config/herdr/herdr.sock)
└── workspace «proyecto-a»      ← un proyecto
    ├── tab «editor»            ← una vista
    │   ├── pane: nvim .
    │   └── pane: claude        ← agente detectado
    └── tab «servidor»
        └── pane: npm run dev
```

> [!important] La terminología importa
> Una **sesión** de Herdr no es un **workspace** de Herdr, y ninguno de los dos es un **workspace de Hyprland**. Ver [[00 Fundamentos/Glosario]].

> [!info] En tu equipo
> Herdr `0.8.2` instalado como paquete, sesión `default` en ejecución, prefijo `Ctrl+Espacio` como tmux, directorio de paneles nuevos que sigue al actual (`new_cwd = "follow"`), ratón capturado, sin bordes ni huecos, y el tema de la terminal. Integraciones de estado instaladas: **Claude Code, Codex, Copilot, opencode y pi**.

## Abrir, salir y volver

| Acción | Cómo |
|---|---|
| Abrir o volver a la sesión | `Super+Ctrl+Enter` · `herdr` · alias `h` |
| Sesión con nombre | `herdr --session trabajo` · `herdr session attach trabajo` |
| Desconectar sin cerrar nada | `Prefijo, d` |
| Ayuda de atajos | `Prefijo, ?` · `Super+Ctrl+K` · `omarchy menu herdr keybindings --print` |
| Remoto por SSH | `herdr --remote usuario@host` (o abre `herdr` tras `ssh`) |

No se puede iniciar Herdr dentro de un pane de Herdr (se impide el anidamiento).

## Qué sobrevive a qué

| Evento | Qué se conserva |
|---|---|
| Cerrar la ventana o `Prefijo, d` | **todo**: los procesos siguen corriendo en el servidor |
| `herdr server stop` o reinicio del equipo | la **forma** de la sesión (workspaces, tabs, panes, directorios, disposición, foco); **no** los procesos. Los agentes con integración pueden reanudar su conversación |
| Historial de pantalla de los panes | desactivado por defecto (puede contener secretos); se activa en la configuración |

## CLI útil

```bash
herdr status                       # cliente y servidor
herdr session list                 # sesiones (stop, delete para limpiarlas)
herdr workspace --help             # crear/listar workspaces desde scripts
herdr agent list                   # agentes detectados y su estado
herdr agent explain <destino>      # por qué se clasificó así
herdr integration status           # integraciones por agente
herdr integration install claude   # instalar una integración
herdr server reload-config         # aplicar cambios de config.toml
herdr config reset-keys            # respaldar config y quitar atajos propios
herdr --default-config             # ver la configuración por defecto completa
# herdr update  → evítalo: aquí Herdr es un paquete del sistema y se actualiza con omarchy update
```

Tras editar `config.toml`: `Prefijo, q`, `herdr server reload-config` u `omarchy restart herdr`. `omarchy refresh herdr` ⚠️ restablece el archivo a los defaults de Omarchy.

Continúa en [[07 Herdr/Workspaces y agentes]] y [[07 Herdr/Herdr avanzado]] (ajustes, atajos ocultos, órdenes propias, CLI y worktrees). Comparación con tmux: [[06 Tmux/Tmux en Omarchy#tmux frente a Herdr]].

Fuentes: [documentación Herdr](https://herdr.dev/docs/), [conceptos](https://herdr.dev/docs/concepts/), [estado de sesión](https://herdr.dev/docs/session-state/), [teclado](https://herdr.dev/docs/keyboard/), [CLI](https://herdr.dev/docs/cli-reference/), [Omarchy: TUI](https://omarchy.org/manual/tuis/).
