---
aliases: [Inicio, Documentación Omarchy 4, MOC]
tags: [indice, omarchy, moc]
actualizado: 2026-09-27
verificado_en: "Omarchy 4.0.4-1 · Hyprland 0.56.2 · Neovim 0.12.5 · Bash 5.3.15 · tmux 3.7c · Herdr 0.8.2"
---
# Documentación de tu equipo — Omarchy 4

Este vault en español explica **qué hace cada capa del sistema**, cómo usarla, cómo diagnosticarla y dónde profundizar. Es una síntesis original contrastada con la documentación oficial ([manual de Omarchy](https://omarchy.org/manual/), [Hyprland Wiki](https://wiki.hypr.land/), [ArchWiki](https://wiki.archlinux.org/title/Main_page), [LazyVim](https://www.lazyvim.org/keymaps), [manual de GNU Bash](https://www.gnu.org/software/bash/manual/)) y **verificada contra la instalación real** de este equipo. Las explicaciones funcionan sin conexión; los enlaces externos la requieren.

> [!info] Instalación verificada el 27 de septiembre de 2026
> Omarchy `4.0.4-1`, Hyprland `0.56.2`, Neovim `0.12.5` + LazyVim, Bash `5.3.15`, tmux `3.7c` y Herdr `0.8.2`. Tras una actualización, confirma versiones con los comandos de [[08 Recetas del equipo/Mantenimiento periódico]] y revisa [[09 Fuentes y enlaces/Registro de revisión]].

## Empieza aquí

1. **¿Vienes de Windows o macOS?** Empieza por [[01 Omarchy/Primeros pasos]].
2. [[00 Fundamentos/Mapa del sistema]] — del encendido a la aplicación, y quién controla qué.
3. [[00 Fundamentos/Conceptos de Linux]] — archivos, permisos, procesos y variables.
4. [[00 Fundamentos/Glosario]] — vocabulario compartido por todas las notas.
5. [[00 Fundamentos/Ruta de aprendizaje]] — ejercicios progresivos con objetivos verificables.
6. [[01 Omarchy/Omarchy 4]] — el escritorio, la CLI y sus archivos.
7. [[02 Arch Linux/Arch Linux]] — paquetes, servicios y mantenimiento.
8. [[03 Hyprland/Hyprland en Omarchy]] — ventanas, monitores y configuración Lua.
9. [[04 Bash/Bash]] — la terminal y los scripts.
10. [[05 Neovim/Neovim y LazyVim]] — edición modal; para usarlo como IDE sigue [[05 Neovim/Ruta de cero a pro]].
11. [[06 Tmux/Tmux en Omarchy]] y [[07 Herdr/Herdr en Omarchy]] — sesiones persistentes.

## Mapa del vault

| Carpeta | Notas | Preguntas que responde |
|---|---|---|
| **00 Fundamentos** | [[00 Fundamentos/Mapa del sistema\|Mapa]] · [[00 Fundamentos/Conceptos de Linux\|Conceptos de Linux]] · [[00 Fundamentos/Glosario\|Glosario]] · [[00 Fundamentos/Ruta de aprendizaje\|Ruta]] | ¿Qué es cada capa y qué debo saber de Linux antes de empezar? |
| **01 Omarchy** | [[01 Omarchy/Primeros pasos\|Primeros pasos]] · [[01 Omarchy/Atajos y flujos\|Atajos]] · [[01 Omarchy/Referencia completa de atajos\|Referencia de atajos]] · [[01 Omarchy/Personalización paso a paso\|Personalización]] · [[01 Omarchy/CLI configuración y mantenimiento\|CLI y mantenimiento]] · [[01 Omarchy/Referencia de la CLI\|Referencia CLI]] · [[01 Omarchy/Shell temas y plugins\|Shell y plugins]] · [[01 Omarchy/Escritorio red y periféricos\|Periféricos]] · [[01 Omarchy/Aplicaciones desarrollo y automatización\|Apps y hooks]] · [[01 Omarchy/Instalación seguridad y recuperación\|Seguridad]] · [[01 Omarchy/Guía del manual oficial\|Manual (51 cap.)]] | ¿Cómo navego, personalizo, actualizo y recupero el escritorio? |
| **02 Arch Linux** | [[02 Arch Linux/Paquetes y actualizaciones\|Paquetes]] · [[02 Arch Linux/Servicios red y diagnóstico\|Servicios y diagnóstico]] · [[02 Arch Linux/Arranque kernel y hardware\|Arranque y hardware]] · [[02 Arch Linux/Rescate desde USB\|Rescate]] | ¿Qué son pacman, systemd, el kernel y cómo recupero un equipo que no arranca? |
| **03 Hyprland** | [[03 Hyprland/Ventanas atajos y monitores\|Ventanas y monitores]] · [[03 Hyprland/Configuración Lua y depuración\|Configuración Lua]] · [[03 Hyprland/Aspecto animaciones y workspaces\|Aspecto]] · [[03 Hyprland/Recetas de Hyprland\|Recetas]] · [[03 Hyprland/Referencia hyprctl\|hyprctl]] | ¿Dónde van los cambios seguros y cómo se validan? |
| **04 Bash** | [[04 Bash/Ruta de cero a pro\|Ruta de cero a pro]] · [[04 Bash/Herramientas de línea de órdenes\|Herramientas]] · [[04 Bash/Scripts y expansión\|Scripts]] · [[04 Bash/Referencia rápida de Bash\|Referencia rápida]] | ¿Cómo uso la terminal y automatizo tareas? |
| **05 Neovim** | [[05 Neovim/Ruta de cero a pro\|Ruta de cero a pro]] · [[05 Neovim/Edición y atajos\|Edición]] · [[05 Neovim/Movimientos y edición avanzada\|Edición avanzada]] · [[05 Neovim/Proyectos búsqueda y navegación\|Proyectos]] · [[05 Neovim/IDE - LSP completado formato y lint\|LSP]] · [[05 Neovim/IDE - Lenguajes y extras\|Lenguajes]] · [[05 Neovim/IDE - Git\|Git]] · [[05 Neovim/IDE - Depuración pruebas y ejecución\|Depurar y probar]] · [[05 Neovim/Configuración Lua a fondo\|Lua]] · [[05 Neovim/Configuración y diagnóstico\|Configuración]] · [[05 Neovim/Referencia de atajos LazyVim\|Atajos]] · [[05 Neovim/Ayuda integrada y documentación oficial\|:help]] | ¿Cómo uso Neovim como IDE principal, desde cero hasta nivel experto? |
| **06 Tmux** | [[06 Tmux/Sesiones y paneles\|Sesiones y paneles]] · [[06 Tmux/Tmux avanzado\|Avanzado]] | ¿Cómo retomo mi trabajo después de cerrar la terminal? |
| **07 Herdr** | [[07 Herdr/Workspaces y agentes\|Workspaces y agentes]] · [[07 Herdr/Herdr avanzado\|Avanzado]] | ¿Cómo organizo terminales y agentes por proyecto? |
| **08 Recetas** | [[08 Recetas del equipo/Recetario de tareas comunes\|Recetario]] · [[08 Recetas del equipo/Perfil de este equipo\|Perfil]] · [[08 Recetas del equipo/Diagnóstico por síntomas\|Diagnóstico]] · [[08 Recetas del equipo/Flujo de trabajo integrado\|Flujo integrado]] · [[08 Recetas del equipo/Mantenimiento periódico\|Mantenimiento]] | ¿Cómo hago tareas comunes y qué hago si algo falla? |
| **09 Fuentes** | [[09 Fuentes y enlaces/Índice de fuentes\|Fuentes]] · [[09 Fuentes y enlaces/Registro de revisión\|Registro de revisión]] | ¿De dónde sale cada dato y qué cambió? |

## Tareas frecuentes

| Quiero… | Ve a |
|---|---|
| Ver todos los atajos vigentes | `Super+K` o [[01 Omarchy/Referencia completa de atajos]] |
| Actualizar el sistema | [[02 Arch Linux/Paquetes y actualizaciones#Actualizar el sistema]] |
| Cambiar un atajo o un monitor | [[03 Hyprland/Configuración Lua y depuración]] |
| Deshacer una actualización fallida | [[01 Omarchy/Instalación seguridad y recuperación#Snapshots y reversión]] |
| Saber por qué algo no funciona | [[08 Recetas del equipo/Diagnóstico por síntomas]] |
| El equipo no arranca | [[02 Arch Linux/Rescate desde USB]] |
| Hacer algo concreto (SSH, USB, VPN, impresora…) | [[08 Recetas del equipo/Recetario de tareas comunes]] |
| Cambiar el aspecto del escritorio | [[01 Omarchy/Personalización paso a paso]] |
| Aprender Bash con ejercicios | [[04 Bash/Ruta de cero a pro]] |
| Recordar una sintaxis de Bash | [[04 Bash/Referencia rápida de Bash]] |
| Recordar un atajo de LazyVim | [[05 Neovim/Referencia de atajos LazyVim]] |
| Aprender Neovim como IDE | [[05 Neovim/Ruta de cero a pro]] |

## Convenciones de este vault

- `[[...]]` son enlaces internos de Obsidian. `Ctrl+O` busca notas; `Ctrl+Shift+F` busca texto; `Ctrl+clic` abre en otro panel.
- **En tu equipo** / **Este equipo** describe esta instalación concreta, no reglas universales de Arch.
- Las teclas se escriben como `Super+Shift+Enter` (simultáneas) o `Prefijo, luego c` (secuencia).
- Los bloques `bash` son ejemplos; lee la explicación antes de ejecutarlos, sobre todo si modifican el sistema.
- Callouts: `[!info]` contexto, `[!tip]` recomendación, `[!warning]` riesgo de pérdida de datos o configuración, `[!important]` regla que conviene no romper.
- Cada nota lleva en su cabecera `actualizado` y `verificado_en`. Si tu versión es posterior, verifica con upstream mediante [[09 Fuentes y enlaces/Índice de fuentes]].
