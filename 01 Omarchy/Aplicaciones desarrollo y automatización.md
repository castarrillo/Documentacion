---
aliases: [Hooks Omarchy, mise, Automatización]
tags: [omarchy, desarrollo, hooks, automatizacion]
actualizado: 2026-09-27
verificado_en: "Omarchy 4.0.4-1"
---
# Aplicaciones, desarrollo y automatización

## Tipos de aplicación

| Tipo | Qué es | Ejemplos | Cómo gestionarlas |
|---|---|---|---|
| **GUI** | ventana gráfica nativa | Obsidian, Nautilus, LibreOffice | `omarchy pkg add`, menú *Install* |
| **TUI** | interfaz de texto en terminal | btop, lazygit, lazydocker, Herdr | `omarchy tui install` / `tui remove` |
| **Web app** | sitio con ventana y lanzador propios | YouTube, WhatsApp, Google Maps | `omarchy webapp install` / `webapp remove` |
| **Servicio** | proceso en segundo plano | Tailscale, Sunshine, Dropbox | `omarchy install service …` / `remove service …` |

Quitar o instalar apps puede crear o dejar huérfanos atajos: revisa [[01 Omarchy/Referencia completa de atajos]]. `omarchy remove preinstalls` / `install preinstalls` quitan o restauran las apps preinstaladas.

Predeterminadas: `omarchy default browser|editor|terminal|agent <valor>`. En este equipo: terminal **Kitty**, navegador **Brave**.

## Desarrollo

**mise** gestiona versiones de lenguajes y herramientas por usuario o por proyecto (`mise.toml`). `omarchy install dev-env <lenguaje>` prepara node, python, ruby, go, rust, java, bun, deno, php, elixir, zig, ocaml, dotnet, clojure o scala; `omarchy update mise` los actualiza.

> [!warning] Dos Pythons distintos
> Si `python3` del `PATH` apunta a mise y `pacman` instala `python-numpy` para `/usr/bin/python3`, son **entornos distintos**. Averigua cuál ejecuta un script con `command -v python3`, `python3 -V` y `python3 -c 'import sys; print(sys.executable)'`. Esto explicó los fallos del visualizador y del widget multimedia: ver [[08 Recetas del equipo/Diagnóstico por síntomas]].

Utilidades incluidas: `rg`, `fd`, `fzf`, `bat`, `eza`, `zoxide`, `lazygit`, `lazydocker`, Starship, Git, Docker. Funciones de Bash (layouts de tmux/Herdr, worktrees, rsync, túneles SSH): [[04 Bash/Bash#Aliases y funciones de Omarchy]].

> [!warning] Docker sin sudo
> `omarchy setup security sudoless docker` añade tu usuario al grupo `docker`, lo que **equivale a acceso root**. **En este equipo ya está activado** (`id` muestra el grupo `docker`), así que `docker`/`d` funcionan sin `sudo`; el servicio `docker` arranca bajo demanda. Revertir: `omarchy remove security sudoless docker`.

## Automatizaciones que sobreviven a actualizaciones

| Necesidad | Mecanismo | Dónde |
|---|---|---|
| Lanzar algo al iniciar sesión | `o.launch_on_start("programa")` | `~/.config/hypr/autostart.lua` |
| Reaccionar a un evento de Omarchy | hook ejecutable | `~/.config/omarchy/hooks/<evento>.d/` |
| Añadir entradas al menú | extensión JSONC | `~/.config/omarchy/extensions/omarchy-menu.jsonc` |
| Programa persistente con reinicio automático | servicio `systemd --user` | `~/.config/systemd/user/` |
| Tarea periódica | temporizador `systemd --user` (`.timer`) | `~/.config/systemd/user/` |

### Hooks de Omarchy

| Evento | Cuándo se ejecuta |
|---|---|
| `post-boot` | al arrancar el escritorio |
| `post-update` | durante `omarchy update`, tras las migraciones |
| `pre-refresh-pacman` | antes de `omarchy refresh pacman` (p. ej. añadir un repositorio propio) |
| `theme-set` | tras cambiar de tema (recibe el nombre del tema) |
| `font-set` | tras cambiar la fuente |
| `battery-low` | con batería baja |

```bash
omarchy hook install post-boot ~/scripts/mi-hook   # copia y marca ejecutable
omarchy hook post-boot                              # ejecutarlo a mano para probar
```

Sólo se ejecutan los archivos **ejecutables**; los `*.sample` son ejemplos inertes.

> [!info] En tu equipo
> `post-update.d/` contiene `install-voxtype.hook`, `setup-agent.hook` y `setup-fingerprint.hook`; el resto de carpetas sólo tiene ejemplos `.sample`. `omarchy-menu.jsonc` está vacío (sólo comentarios).

### Ejemplo de entrada de menú

```jsonc
{
  "personal": {"icon": "", "label": "Personal"},
  "personal.docs": {
    "icon": "󰎞", "label": "Documentación",
    "action": "uwsm-app -- obsidian obsidian://open?vault=Documentacion"
  }
}
```

El padre se deduce del ID con puntos; `when` oculta la fila si una condición de shell falla y `checked` añade ✓.

### Ejemplo de servicio de usuario

```ini
# ~/.config/systemd/user/sincronizar.service
[Unit]
Description=Sincronizar notas

[Service]
ExecStart=%h/scripts/sincronizar.sh
Restart=on-failure

[Install]
WantedBy=default.target
```

```bash
systemctl --user daemon-reload
systemctl --user enable --now sincronizar.service
journalctl --user -u sincronizar -f
```

Fuentes: [desarrollo](https://omarchy.org/manual/development-tools/), [shell tools](https://omarchy.org/manual/shell-tools/), [shell functions](https://omarchy.org/manual/shell-functions/), [TUI](https://omarchy.org/manual/tuis/), [GUI](https://omarchy.org/manual/guis/), [web apps](https://omarchy.org/manual/web-apps/), [dotfiles](https://omarchy.org/manual/dotfiles/), [ArchWiki: systemd/User](https://wiki.archlinux.org/title/Systemd/User), [mise](https://mise.jdx.dev/).
