---
aliases: [Inicio, Documentación Omarchy 4, MOC]
tags: [indice, omarchy, moc]
actualizado: 2026-09-27
verificado_en: "Omarchy 4.0.4-1 · Hyprland 0.56.2 · Neovim 0.12.5 · Bash 5.3.15 · tmux 3.7c · Herdr 0.8.2"
---
# Omarchy 4 en español — guía de tu equipo

Documentación en español, organizada como **vault de Obsidian y repositorio Markdown**, para aprender cómo funciona un equipo con Omarchy 4. Explica las relaciones entre **Arch Linux, Hyprland, Omarchy Shell, Bash, Neovim/LazyVim, tmux y Herdr**, desde los primeros pasos hasta la configuración, el mantenimiento y el diagnóstico.

Las notas son una **síntesis original**, contrastada con el [manual oficial de Omarchy](https://omarchy.org/manual/) y las referencias de cada proyecto. Incluyen comandos y atajos comprobados en la instalación descrita abajo; si tu equipo tiene otra versión o configuración, verifica primero sus archivos y la ayuda local. El contenido se puede leer sin conexión; los enlaces a las fuentes requieren internet.

## ¿Para quién es?

- Para quien llega desde Windows o macOS y quiere aprender a moverse en un escritorio de ventanas en mosaico.
- Para quien quiere entender **qué hace cada componente**, en lugar de copiar comandos sin contexto.
- Para usuarios de Omarchy 4 que quieren personalizar su escritorio y saber cómo recuperarse cuando algo falla.
- Para quienes usan Neovim como editor, y tmux o Herdr para conservar y organizar sesiones de terminal.

## Cómo usar este repositorio

### Leerlo en GitHub

Abre las notas desde los [accesos directos](#accesos-directos-para-github) de este README o recorre las carpetas por número. GitHub muestra los archivos Markdown, pero los enlaces `[[nota]]` que aparecen dentro de las páginas son **wikilinks de Obsidian**: para seguir toda la red de enlaces, abre la carpeta como vault.

### Abrirlo en Obsidian

1. Descarga el repositorio como ZIP y descomprímelo, o clónalo con `git clone <URL-DEL-REPOSITORIO>`.
2. En Obsidian, selecciona **Abrir otra bóveda → Abrir carpeta como bóveda** y elige la carpeta raíz del repositorio (la que contiene este `README.md` y `.obsidian/`).
3. Abre `README.md` y sigue **Empieza aquí** o la **Ruta de aprendizaje**. Obsidian lee archivos Markdown locales; no hace falta instalar complementos comunitarios.

En la instalación original, el vault está en `~/Documents/Documentacion`. Puedes guardarlo en cualquier otra ubicación: las rutas `~/.config/...` de las guías se refieren al equipo sobre el que estés trabajando, no a la carpeta del vault.

### Accesos directos para GitHub

| Área | Notas recomendadas |
|---|---|
| Entender las capas | [Mapa del sistema](00%20Fundamentos/Mapa%20del%20sistema.md) · [Conceptos de Linux](00%20Fundamentos/Conceptos%20de%20Linux.md) · [Glosario](00%20Fundamentos/Glosario.md) · [Ruta de aprendizaje](00%20Fundamentos/Ruta%20de%20aprendizaje.md) |
| Omarchy | [Primeros pasos](01%20Omarchy/Primeros%20pasos.md) · [Omarchy 4](01%20Omarchy/Omarchy%204.md) · [Atajos](01%20Omarchy/Referencia%20completa%20de%20atajos.md) · [Manual oficial, 51 capítulos](01%20Omarchy/Gu%C3%ADa%20del%20manual%20oficial.md) |
| Arch Linux | [Base del sistema](02%20Arch%20Linux/Arch%20Linux.md) · [Paquetes y actualizaciones](02%20Arch%20Linux/Paquetes%20y%20actualizaciones.md) · [Servicios y diagnóstico](02%20Arch%20Linux/Servicios%20red%20y%20diagn%C3%B3stico.md) |
| Hyprland | [Hyprland en Omarchy](03%20Hyprland/Hyprland%20en%20Omarchy.md) · [Ventanas y monitores](03%20Hyprland/Ventanas%20atajos%20y%20monitores.md) · [Configuración Lua](03%20Hyprland/Configuraci%C3%B3n%20Lua%20y%20depuraci%C3%B3n.md) |
| Bash | [Bash](04%20Bash/Bash.md) · [Ruta de cero a pro](04%20Bash/Ruta%20de%20cero%20a%20pro.md) · [Referencia rápida](04%20Bash/Referencia%20r%C3%A1pida%20de%20Bash.md) |
| Neovim y LazyVim | [Introducción](05%20Neovim/Neovim%20y%20LazyVim.md) · [Ruta de cero a pro](05%20Neovim/Ruta%20de%20cero%20a%20pro.md) · [IDE y LSP](05%20Neovim/IDE%20-%20LSP%20completado%20formato%20y%20lint.md) |
| Sesiones persistentes | [tmux](06%20Tmux/Tmux%20en%20Omarchy.md) · [Herdr](07%20Herdr/Herdr%20en%20Omarchy.md) · [Comparativa práctica](08%20Recetas%20del%20equipo/Flujo%20de%20trabajo%20integrado.md) |
| Solución de problemas | [Diagnóstico por síntomas](08%20Recetas%20del%20equipo/Diagn%C3%B3stico%20por%20s%C3%ADntomas.md) · [Mantenimiento](08%20Recetas%20del%20equipo/Mantenimiento%20peri%C3%B3dico.md) · [Fuentes oficiales](09%20Fuentes%20y%20enlaces/%C3%8Dndice%20de%20fuentes.md) |

## Cómo encajan las piezas

```text
Equipo físico y firmware
  └─ Linux + Arch Linux ───── kernel, paquetes, servicios y controladores
      └─ Omarchy 4 ────────── integración, temas, comandos y actualizaciones
          ├─ Hyprland ─────── pantallas, ventanas, workspaces y atajos
          ├─ Omarchy Shell ── barra, menú, notificaciones y plugins (Quickshell)
          └─ Terminal
              └─ Bash ─────── comandos y scripts
                  ├─ tmux / Herdr ── sesiones y paneles persistentes
                  └─ Neovim + LazyVim ── edición y herramientas de desarrollo
```

**Ejemplo:** `Super+Enter` es un atajo del escritorio para abrir la terminal; allí Bash interpreta `nvim .`. Puedes ejecutarlo dentro de una sesión de tmux o un workspace de Herdr. Si falla el atajo, investiga Hyprland; si falla `nvim`, investiga el programa, su configuración o su paquete. El [mapa del sistema](00%20Fundamentos/Mapa%20del%20sistema.md) desarrolla esta distinción.

> [!info] Instalación verificada el 27 de septiembre de 2026
> Omarchy `4.0.4-1`, Hyprland `0.56.2`, Neovim `0.12.5` + LazyVim, Bash `5.3.15`, tmux `3.7c` y Herdr `0.8.2`. Tras una actualización, confirma versiones con los comandos de [[08 Recetas del equipo/Mantenimiento periódico]] y revisa [[09 Fuentes y enlaces/Registro de revisión]].

Las notas que dicen **«en este equipo»** describen la configuración original; no significan que esos atajos, monitores o servicios estén presentes en cualquier instalación de Omarchy. Los capítulos oficiales pueden evolucionar independientemente de esta fotografía.

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

## Fuentes, alcance y actualización

- El [índice de fuentes](09%20Fuentes%20y%20enlaces/%C3%8Dndice%20de%20fuentes.md) reúne documentación oficial y referencias rápidas; [Devhints Bash](https://devhints.io/bash) es una chuleta comunitaria, no documentación oficial de Bash.
- La [guía del manual de Omarchy](01%20Omarchy/Gu%C3%ADa%20del%20manual%20oficial.md) clasifica sus **51 capítulos** con enlaces a la fuente en [omacom/omarchy-site](https://github.com/omacom/omarchy-site/tree/master/manual). Son explicaciones y síntesis en español, **no una traducción literal íntegra** del manual ni un sustituto de la documentación original.
- Para comprobar un detalle que pueda haber cambiado, usa `omarchy commands`, `omarchy <grupo> --help`, `hyprctl version`, `man` y los archivos de usuario en `~/.config/`. La wiki de Hyprland publica por defecto la versión *Latest git*: elige la versión que tengas instalada.
- El [registro de revisión](09%20Fuentes%20y%20enlaces/Registro%20de%20revisi%C3%B3n.md) documenta comprobaciones y correcciones. Si detectas una instrucción desactualizada, indica la nota, tu versión y el enlace oficial pertinente al proponer una corrección.

Este repositorio contiene documentación y ejemplos; las órdenes que instalan, actualizan, restablecen o eliminan componentes deben leerse completas antes de ejecutarse. En Omarchy, el flujo de actualización del sistema es `omarchy update`.
