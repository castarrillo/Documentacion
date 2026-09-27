---
aliases: [Changelog, Historial del vault]
tags: [fuentes, registro]
actualizado: 2026-09-27
---
# Registro de revisión

Historial de cambios del vault. Añade una entrada cada vez que actualices las notas tras una actualización del sistema.

## 2026-09-27 (c) — Ampliación para principiantes en todas las secciones

**Método**: igual que las revisiones anteriores; cada orden y fragmento de código se probó o se contrastó con el sistema (API `hl.*` consultada con `hyprctl repl`, opciones con `hyprctl getoption`, teclas efectivas de Herdr con `omarchy menu herdr keybindings --print`, `herdr config check`, subvolúmenes con `findmnt`, envoltorio de `mkinitcpio`) y con la documentación oficial (manual de Omarchy: FAQ, Coming from Mac or Windows, Making your own theme, Common tweaks; Hyprland Wiki: Workspace Rules, Binds/submapas, Window Rules, Animations; ArchWiki; man tmux; `herdr --default-config`).

### Notas nuevas

| Sección | Nota |
|---|---|
| 00 | [[00 Fundamentos/Conceptos de Linux]] |
| 01 | [[01 Omarchy/Primeros pasos]] · [[01 Omarchy/Personalización paso a paso]] |
| 02 | [[02 Arch Linux/Arranque kernel y hardware]] · [[02 Arch Linux/Rescate desde USB]] |
| 03 | [[03 Hyprland/Aspecto animaciones y workspaces]] · [[03 Hyprland/Recetas de Hyprland]] |
| 04 | [[04 Bash/Ruta de cero a pro]] · [[04 Bash/Herramientas de línea de órdenes]] |
| 06 | [[06 Tmux/Tmux avanzado]] |
| 07 | [[07 Herdr/Herdr avanzado]] |
| 08 | [[08 Recetas del equipo/Recetario de tareas comunes]] |

### Correcciones

| Nota | Antes | Ahora |
|---|---|---|
| Aplicaciones, desarrollo y automatización | «por defecto se usa `sudo docker`» | tu usuario **ya está** en el grupo `docker` (socket activo) |
| Perfil de este equipo | sin datos de kernel, teclado ni servicios expuestos | kernel `linux-omarchy`, `latam`, grupos, SSH activo, CUPS |

### Hallazgos para decidir

- **Servidor SSH activo** (sólo clave) y **Docker sin sudo** activados: útiles, pero amplían la superficie de ataque; desactívalos si no los usas.
- Aquí el initramfs se regenera con `limine-mkinitcpio` / `limine-update`, **no** con `mkinitcpio -P` (no actualiza Limine).
- Herdr tiene atajos útiles no documentados antes: ajustes `Prefijo, s`, selector `Prefijo, w`, *goto* `Prefijo, g`, worktree `Prefijo, G`.

## 2026-09-27 (b) — Neovim como IDE principal, de cero a pro

**Método**: contraste con la ayuda local de Neovim 0.12.5 (`/usr/share/nvim/runtime/doc`, idéntica a [neovim.io/doc](https://neovim.io/doc/)), con el código de LazyVim y de los extras instalados, y con la configuración real de `~/.config/nvim`.

### Notas nuevas (05 Neovim)

- [[05 Neovim/Ruta de cero a pro]] — seis niveles con ejercicios y pruebas de dominio.
- [[05 Neovim/Movimientos y edición avanzada]]
- [[05 Neovim/Proyectos búsqueda y navegación]]
- [[05 Neovim/IDE - LSP completado formato y lint]]
- [[05 Neovim/IDE - Lenguajes y extras]]
- [[05 Neovim/IDE - Git]]
- [[05 Neovim/IDE - Depuración pruebas y ejecución]]
- [[05 Neovim/Configuración Lua a fondo]]
- [[05 Neovim/Ayuda integrada y documentación oficial]]

### Correcciones

| Nota | Antes | Ahora |
|---|---|---|
| Edición y atajos | objeto `ii`/`ai` (indentación) | no existe en la configuración de mini.ai instalada; sustituido por `ig`/`ag` (todo el buffer) |

### Hallazgos

- Ningún extra de lenguaje está activo (sólo `editor.neo-tree`); Mason sólo tiene `lua-language-server`, `stylua` y `shfmt`. La selección recomendada está en [[05 Neovim/IDE - Lenguajes y extras]].
- Neovim 0.12 añade atajos `gr*` que conviven con `gr` (referencias) de LazyVim.

## 2026-09-27 — Revisión integral y ampliación

**Método**: cada afirmación se contrastó con el sistema instalado (`omarchy commands`, `omarchy menu keybindings --print`, `hyprctl -h`, archivos de `/usr/share/omarchy/` y `~/.config/`, código local de LazyVim) y con la documentación oficial (manual de Omarchy, Hyprland Wiki, ArchWiki, LazyVim, GNU Bash, Devhints, tmux, Herdr). Se comprobaron todos los enlaces externos. Copia previa: `~/Documents/Documentacion-respaldo-2026-09-27.tar.gz`.

### Correcciones

| Nota | Antes | Ahora |
|---|---|---|
| CLI, Diagnóstico | `omarchy debug --no-sudo --print` | la orden **no existe** en 4.0.4; se sustituyó por una secuencia de inspección equivalente |
| Paquetes | `omarchy pkg aur --help` | orden inválida; ahora `omarchy pkg aur add` / `omarchy pkg aur install` |
| Hyprland | «la wiki muestra sintaxis nativa; no copies `.conf`» | desde Hyprland 0.55 la configuración nativa **es Lua** (`hl.*`), hyprlang está obsoleto; Omarchy añade `o.*` |
| Neovim, Ruta | `nvim +Tutor` / `vimtutor` | no funcionan aquí (LazyVim desactiva `tutor`; vim no instalado): `nvim --clean +Tutor` |
| Neovim | `Espacio e` = explorador genérico | es **Neo-tree** (extra activo en `lazyvim.json`) |
| Atajos | `Super+Ctrl+I` = «permanecer despierto» | alterna el **bloqueo por inactividad** |
| Fuentes | enlace a ArchWiki en español con paréntesis sin codificar | URL codificada |
| Actualizaciones | «no uses `pacman -Syu`» | además: un hook de pacman lo **bloquea**; documentados los 9 pasos reales de `omarchy update` y su registro |
| Herdr/Bash | `hdl`, `tdl` sin argumentos | requieren el agente: `hdl <c\|cx\|codex> [ia2]` |

### Notas nuevas

- [[00 Fundamentos/Glosario]]
- [[01 Omarchy/Referencia completa de atajos]]
- [[01 Omarchy/Referencia de la CLI]]
- [[03 Hyprland/Referencia hyprctl]]
- [[04 Bash/Referencia rápida de Bash]]
- [[05 Neovim/Referencia de atajos LazyVim]]
- [[08 Recetas del equipo/Mantenimiento periódico]]
- [[09 Fuentes y enlaces/Registro de revisión]]

### Ampliaciones

- Todas las notas: cabecera YAML (`aliases`, `tags`, `actualizado`, `verificado_en`), callouts de riesgo y enlaces cruzados.
- Omarchy: estructura en disco, canales, hooks y eventos, extensiones del menú, servicios de usuario, snapshots paso a paso, tabla de seguridad por capas, órdenes que sobrescriben.
- Arch: chuleta de pacman, `.pacnew`, caché y degradación, firmas, systemd y journald en tablas, diagnóstico de red por capas, audio con `wpctl`.
- Hyprland: orden de carga, API Lua (atajos, dispatchers, reglas, monitores, gestos), flujo de cambio seguro, depuración.
- Bash: carga de `~/.bashrc` en Omarchy, aliases y funciones, readline, plantilla de script, anti-patrones.
- Neovim: gramática operador + objeto, registros, personalización de plugins, mantenimiento con `:Lazy`.
- tmux y Herdr: tablas completas desde la configuración real, persistencia, integraciones de agentes.

### Hallazgos del sistema (no corregidos, a decidir por el usuario)

- `~/.config/omarchy/plugins/learn-omarchy.geometry/manifest.json` **no es JSON válido**; el plugin no se carga.
- `shellcheck`, `wev` y `smartmontools` no están instalados (se mencionan como opcionales).

## Plantilla para próximas revisiones

```markdown
## AAAA-MM-DD — motivo (p. ej. actualización a Omarchy 4.x)

Versiones: Omarchy … · Hyprland … · Neovim … · tmux … · Herdr …

| Nota | Cambio |
|---|---|
| … | … |
```
