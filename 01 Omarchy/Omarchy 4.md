---
aliases: [Omarchy, Omarchy 4]
tags: [omarchy, moc]
actualizado: 2026-09-27
verificado_en: "Omarchy 4.0.4-1 (canal stable)"
---
# Omarchy 4

Omarchy es una distribución **«omakase» basada en Arch Linux**: elige e integra Hyprland como gestor de ventanas, Quickshell para la interfaz y una selección de aplicaciones (Neovim, navegador, herramientas de desarrollo, multimedia). No es un tema superpuesto a Arch: instala paquetes propios, configuraciones predeterminadas, una CLI, migraciones y un flujo de actualización con snapshots. Su filosofía, según el manual, prioriza un sistema bonito y centrado en el teclado y la terminal, sin imitar las convenciones de Windows o macOS.

> [!info] Este equipo
> `omarchy version` → `4.0.4-1` · `omarchy channel current` → `stable` · tema **Matte Black** · fuente **JetBrainsMono Nerd Font** · terminal **Kitty** · navegador **Brave**.

## Piezas principales

| Pieza | Qué hace | Dónde se configura | Nota |
|---|---|---|---|
| **CLI `omarchy`** | `omarchy <grupo> <acción>`: los mismos flujos que el menú | — | [[01 Omarchy/Referencia de la CLI]] |
| **Menú** (`Super+Espacio`, `Super+Escape`) | apps, instalar/quitar, ajustes, capturas, actualizar, sistema | `~/.config/omarchy/extensions/omarchy-menu.jsonc` | [[01 Omarchy/Atajos y flujos]] |
| **Hyprland** | ventanas, monitores, atajos | `~/.config/hypr/*.lua` | [[03 Hyprland/Hyprland en Omarchy]] |
| **Omarchy Shell** | barra, widgets, paneles, OSD, notificaciones, bloqueo, plugins | `~/.config/omarchy/shell.json`, `plugins/` | [[01 Omarchy/Shell temas y plugins]] |
| **Temas** | colores sincronizados entre shell, Hyprland, terminal, Neovim, btop… | `~/.config/omarchy/themes/` | [[01 Omarchy/Shell temas y plugins#Temas y fondos]] |
| **Hooks y extensiones** | automatizar tras eventos (`post-update`, `theme-set`…) | `~/.config/omarchy/hooks/` | [[01 Omarchy/Aplicaciones desarrollo y automatización]] |
| **Actualización** | paquetes + migraciones + snapshot | `omarchy update` | [[02 Arch Linux/Paquetes y actualizaciones]] |

## Estructura en disco

```text
/usr/share/omarchy/          ← del paquete; NO editar
├── bin/                     binarios omarchy-* (la CLI los envuelve)
├── default/                 defaults: hypr/, bash/, limine/, pacman/, snapper/…
├── config/                  plantillas copiadas a ~/.config en la primera instalación
├── themes/                  temas incluidos
├── shell/                   Omarchy Shell (QML) y plugins integrados
└── migrations/              scripts que ejecuta `omarchy update`

~/.config/omarchy/           ← tuyo
├── shell.json               barra, widgets, inactividad
├── plugins/                 plugins de usuario
├── hooks/<evento>.d/        scripts ejecutables por evento
├── extensions/              entradas propias del menú
├── themes/  backgrounds/    temas y fondos propios
└── branding/                arte de «About» y salvapantallas
```

## Tres órdenes para empezar

```bash
omarchy commands                        # catálogo completo (añade --all para internas)
omarchy menu keybindings --print        # atajos vigentes, incluidos tus cambios
omarchy theme list                      # temas disponibles
```

Cualquier orden acepta `--help` y muestra uso, ejemplos y el binario real (`omarchy-…`) que ejecuta.

## Lectura del capítulo

- [[01 Omarchy/Primeros pasos]] — **empieza aquí** si vienes de Windows o macOS.
- [[01 Omarchy/Personalización paso a paso]] — tema, barra, teclado, atajos, menú, hooks y crear tu tema.
- [[01 Omarchy/Atajos y flujos]] — lo esencial para moverse.
- [[01 Omarchy/Referencia completa de atajos]] — todos los atajos vigentes, agrupados.
- [[01 Omarchy/CLI configuración y mantenimiento]] — qué archivo editar y cómo actualizar.
- [[01 Omarchy/Referencia de la CLI]] — grupos de órdenes `omarchy` con ejemplos.
- [[01 Omarchy/Shell temas y plugins]] — barra, plugins y temas.
- [[01 Omarchy/Escritorio red y periféricos]] — energía, red, entrada, audio.
- [[01 Omarchy/Aplicaciones desarrollo y automatización]] — apps, mise, hooks.
- [[01 Omarchy/Instalación seguridad y recuperación]] — cifrado, cortafuegos, snapshots.
- [[01 Omarchy/Guía del manual oficial]] — los 51 capítulos oficiales clasificados.

Personalizaciones de este equipo: [[08 Recetas del equipo/Perfil de este equipo]].

Fuentes: [manual oficial](https://omarchy.org/manual/) · [CLI](https://omarchy.org/manual/omarchy-cli/) · [dotfiles](https://omarchy.org/manual/dotfiles/) · [código Omarchy](https://github.com/omacom/omarchy) · [fuente del manual](https://github.com/omacom/omarchy-site/tree/master/manual).
