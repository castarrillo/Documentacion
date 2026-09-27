---
aliases: [Configuración Omarchy, Mantenimiento Omarchy, omarchy update]
tags: [omarchy, cli, mantenimiento, configuracion]
actualizado: 2026-09-27
verificado_en: "Omarchy 4.0.4-1 · /usr/share/omarchy/bin/omarchy-update"
---
# CLI, configuración y mantenimiento

## Explorar órdenes sin adivinarlas

```bash
omarchy commands            # catálogo de órdenes públicas con su sintaxis
omarchy commands --all      # incluye órdenes internas
omarchy theme --help        # ayuda de un grupo
omarchy capture screenshot --help   # uso, ejemplos y binario real
```

Cada orden `omarchy grupo acción` ejecuta un binario `omarchy-grupo-acción` de `/usr/share/omarchy/bin/`; la ayuda lo indica en la línea **Binary**. Catálogo comentado: [[01 Omarchy/Referencia de la CLI]].

| Objetivo | Orden |
|---|---|
| Actualizar todo (Omarchy + paquetes + migraciones) | `omarchy update` |
| ¿Hay actualización? / ¿cuándo actualicé? | `omarchy update available` / `omarchy version pkgs` |
| Instalar / quitar paquetes de repositorio | `omarchy pkg add NOMBRE…` / `omarchy pkg drop NOMBRE…` |
| Instalar desde AUR | `omarchy pkg aur add NOMBRE…` |
| Buscar e instalar con interfaz | `omarchy pkg install` / `omarchy pkg aur install` |
| Aplicar tema | `omarchy theme set "Matte Black"` |
| Estado de plugins | `omarchy plugin list` |
| Reiniciar la shell | `omarchy restart shell` |
| Crear un snapshot manual | `omarchy snapshot create` |
| Canal de actualización | `omarchy channel current` / `omarchy channel set stable` |

## Archivo propio frente a archivo distribuido

`~/.config/hypr/hyprland.lua` ejecuta `bootstrap.lua`, carga los defaults (`require("default.hypr.omarchy")`) y **después** tus módulos `monitors`, `input`, `bindings`, `looknfeel`, `autostart`, y por último los interruptores (`default.hypr.toggles`). Así tus ajustes ganan y sobreviven a las actualizaciones.

| Archivo de usuario | Contenido |
|---|---|
| `~/.config/hypr/*.lua` | Hyprland: ver [[03 Hyprland/Configuración Lua y depuración]] |
| `~/.config/omarchy/shell.json` | barra, widgets, plugins, tiempos de inactividad |
| `~/.config/omarchy/hooks/<evento>.d/` | scripts por evento |
| `~/.config/omarchy/extensions/omarchy-menu.jsonc` | entradas propias del menú |
| `~/.config/kitty/`, `~/.config/foot/foot.ini`… | terminal |
| `~/.XCompose` | secuencias de composición (emojis, autocompletados) |
| `~/.bashrc` | aliases, funciones y variables propias |

Validación tras cambios:

- **Hyprland**: `hyprctl reload` y `hyprctl configerrors` (suele recargarse solo al guardar).
- **shell.json / plugins**: se recargan solos; si no, `omarchy-shell shell rescanPlugins` o `omarchy restart shell`.
- **tmux / Herdr**: `omarchy restart tmux` / `omarchy restart herdr`, o `Prefijo, q`.
- **Terminal**: `omarchy restart terminal`.

> [!warning] `refresh` y `reinstall` sobrescriben
> `omarchy refresh hyprland` sustituye **todos** tus `~/.config/hypr/*.lua`; `omarchy refresh shell` restablece `shell.json`; `omarchy refresh tmux` / `herdr` / `pacman` / `limine` hacen lo mismo con su área. `omarchy refresh config <ruta>` copia un archivo concreto y guarda respaldo del tuyo. `omarchy reinstall configs` restablece **todas** las configuraciones. Úsalos como último recurso y respalda antes.

## Actualizar el sistema

`omarchy update` (o *Update → Omarchy* en el menú) ejecuta, en este orden (leído de `/usr/share/omarchy/bin/omarchy-update`):

1. Comprueba espacio libre y pide confirmación (`-y` la omite).
2. Poda versiones antiguas de la caché de pacman.
3. **Crea un snapshot** de la raíz con snapper.
4. Asegura los *keyrings* de Arch y Omarchy.
5. Actualiza paquetes del sistema (pacman).
6. **Ejecuta migraciones** pendientes de Omarchy.
7. Lanza los hooks `post-update`.
8. Actualiza paquetes AUR y herramientas de mise.
9. Revisa paquetes huérfanos, analiza el registro y ofrece reiniciar si hace falta.

Todo queda en `/tmp/omarchy-update.log` (se pierde al reiniciar). `omarchy update analyze logs` busca fallos conocidos.

> [!important] No uses `pacman -Syu` ni `yay -Syu` para actualizar todo
> Se saltan snapshot, migraciones y hooks. Omarchy instala un hook de pacman (`00-omarchy-update-guard.hook`) que bloquea la actualización directa y te remite a `omarchy update`. Instalar **un** paquete concreto sí es normal: `omarchy pkg add`.

### Canales

| Canal | Para quién |
|---|---|
| `stable` (este equipo) | por defecto; versiones oficiales con ~1 mes de margen para detectar incompatibilidades |
| `rc` | validación previa a una versión mayor |
| `edge` | últimas compilaciones y paquetes Arch recientes; requiere experiencia |
| `dev` | enlaza el código fuente en `~/omarchy`; sólo desarrolladores |

### Firmware

*Update → Firmware* o `omarchy update firmware` usa `fwupd` (LVFS) para BIOS, SSD y docks compatibles.

## Snapshots y recuperación

Omarchy crea un snapshot antes de cada actualización. Si algo falla, reinicia y elige el snapshot previo en el menú de **Limine**; al arrancar en él aparece una notificación para restaurarlo (o `omarchy snapshot restore`). El snapshot cubre **sólo la raíz**: `/home`, tus documentos y `~/.config` **no** retroceden. Detalles: [[01 Omarchy/Instalación seguridad y recuperación#Snapshots y reversión]].

## Diagnóstico y ayuda

El manual menciona `omarchy-debug` para generar un informe para Discord, pero **no existe en Omarchy 4.0.4**. Recopila en su lugar:

```bash
omarchy version; omarchy version channel; omarchy version pkgs
hyprctl version | head -1; hyprctl configerrors
omarchy cmd missing                 # ¿falta alguna orden requerida?
omarchy update analyze logs
journalctl -b -p warning -n 80 --no-pager
journalctl --user -b -n 80 --no-pager
```

Fuentes: [CLI](https://omarchy.org/manual/omarchy-cli/), [actualizaciones](https://omarchy.org/manual/updates/), [dotfiles](https://omarchy.org/manual/dotfiles/), [snapshots](https://omarchy.org/manual/system-snapshots/), [solución de problemas](https://omarchy.org/manual/troubleshooting/).
