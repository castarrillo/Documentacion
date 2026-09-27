---
aliases: [Omarchy Shell, Plugins, Temas]
tags: [omarchy, shell, quickshell, plugins, temas]
actualizado: 2026-09-27
verificado_en: "Omarchy 4.0.4-1 · omarchy plugin list"
---
# Omarchy Shell, temas y plugins

**Omarchy Shell** es un único proceso **Quickshell** (Qt Quick/QML) que dibuja la barra, el menú, los paneles, el OSD, las notificaciones y la pantalla de bloqueo, y que además ejecuta servicios (inactividad, luz nocturna, polkit, batería). Casi todo es un **plugin**: los integrados viven en `/usr/share/omarchy/shell/plugins/` (sólo lectura) y los tuyos en `~/.config/omarchy/plugins/<id>/`.

| Archivo | Contenido |
|---|---|
| `~/.config/omarchy/shell.json` | claves `bar` (disposición y widgets), `plugins` (activación), `idle` (tiempos), `version` |
| `~/.config/omarchy/plugins/<id>/manifest.json` | declaración de un plugin de usuario |
| `/usr/share/omarchy/shell/README.md` | documentación técnica del esquema y del IPC |

## Barra y widgets

La barra tiene secciones `left`, `center` y `right`. Cada widget tiene un ID (`omarchy.clock`, `omarchy.weather`, `omarchy.workspaces`…). Los clics (izquierdo, derecho, central, rueda) abren paneles o acciones: ver [tabla oficial](https://omarchy.org/manual/the-top-bar/).

```bash
omarchy plugin list                                      # ID, estado, origen y tipo
omarchy bar move omarchy.clock --section center --index 0
omarchy bar put omarchy.keyboard-layout --after omarchy.clock
omarchy bar set omarchy.clock format HH:mm
omarchy bar position top                                 # top|bottom|left|right
omarchy plugin enable omarchy.active-window --section left
omarchy bar reset                                        # vuelve al diseño por defecto
```

> [!warning] La disposición personalizada congela los defaults
> En cuanto `shell.json` tiene una disposición propia, pasa a ser la fuente de verdad: los widgets nuevos que Omarchy añada a la barra por defecto **no se mezclan automáticamente**. Revisa `omarchy plugin list` tras actualizar. Evita reemplazar el archivo entero por ejemplos de internet; usa `omarchy bar …` o guarda copia antes de editar.

## Plugins

| Tipo (`kinds`) | Qué aporta |
|---|---|
| `bar` | Una barra completa alternativa (`omarchy bar use <id>`) |
| `bar-widget` | Un elemento de la barra |
| `panel` | Panel desplegable (red, audio, calendario…) |
| `overlay` | Capa a pantalla completa (portapapeles, emojis, selector de imágenes) |
| `menu` | Menú o lanzador |
| `service` | Lógica en segundo plano; puede crear ventanas QML de escritorio |

Un plugin de terceros lleva `manifest.json` con `schemaVersion`, `id`, `kinds` y `entryPoints`. **Se ejecuta dentro del proceso de la shell con tus permisos**: puede leer tus archivos y lanzar procesos. Revisa su código antes de instalarlo.

```bash
omarchy plugin add https://github.com/usuario/plugin.git --enable
omarchy plugin validate ~/.config/omarchy/plugins/george.omarkmeter
omarchy plugin clone omarchy.clock --edit     # personalizar un integrado sin tocar /usr/share
omarchy plugin update                          # actualizar plugins instalados con git
omarchy-shell shell rescanPlugins              # releer plugins sin reiniciar la shell
omarchy-shell shell listPlugins                # vista del proceso en ejecución
```

> [!info] En tu equipo
> Plugins de terceros: `george.omarkmeter` (servicio: reloj, clima de Girón y visualizador de audio), `nosignal.motion-wallpaper` (fondo animado). La carpeta `learn-omarchy.geometry` existe pero `omarchy plugin list` no la detecta (manifiesto ausente o inválido: compruébalo con `omarchy plugin validate`). Deshabilitados de serie: `omarchy.active-window`, `omarchy.dropbox`, `omarchy.spacer`. Detalles del visualizador en [[08 Recetas del equipo/Perfil de este equipo]].

## Temas y fondos

```bash
omarchy theme list
omarchy theme current                    # Matte Black en este equipo
omarchy theme set "Tokyo Night"
omarchy theme install https://github.com/usuario/omarchy-tema.git
omarchy theme bg next                    # siguiente fondo del tema
omarchy dev theme preview "Matte Black"  # vista previa de la paleta en terminal
```

Un tema define su paleta en `colors.toml`; Omarchy la propaga mediante plantillas a la shell, Hyprland (bordes), terminales, Neovim, btop, Mako/Walker, SDDM/Plymouth y otras apps compatibles. Tras aplicar un tema se ejecutan los hooks `theme-set`.

| Ruta | Uso |
|---|---|
| `~/.config/omarchy/themes/<tema>/` | temas propios o clonados |
| `~/.config/omarchy/backgrounds/<tema>/` | fondos adicionales para un tema |
| `~/.config/omarchy/themed/` | plantillas propias (`*.tpl`) que sustituyen a las de Omarchy para una app; aquí hay ejemplos `*.sample` |

> [!tip] Obsidian
> Obsidian no se sincroniza solo: elige el tema **Omarchy** en *Ajustes → Apariencia*. Este vault ya incluye `.obsidian/themes/Omarchy/`.

Fuentes: [barra](https://omarchy.org/manual/the-top-bar/), [plugins](https://omarchy.org/manual/shell-plugins/), [temas](https://omarchy.org/manual/themes/), [crear tema](https://omarchy.org/manual/making-your-own-theme/), [fondos](https://omarchy.org/manual/backgrounds/), [README de la shell](https://github.com/omacom/omarchy/blob/master/shell/README.md), [Quickshell](https://quickshell.org/docs/v0.2.1/).
