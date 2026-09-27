---
aliases: [Glosario, Vocabulario, Términos]
tags: [fundamentos, glosario]
actualizado: 2026-09-27
---
# Glosario

Términos usados en todo el vault. Cuando una palabra significa cosas distintas según la herramienta, se indica la capa entre corchetes.

## Sistema y Arch

| Término | Significado |
|---|---|
| **Kernel** | Núcleo Linux: gestiona hardware, memoria, procesos y sistemas de archivos. |
| **Distribución** | Kernel + herramientas + paquetes + política de actualización (Arch, Omarchy). |
| **Rolling release** | Modelo sin versiones mayores: el sistema se actualiza continuamente. |
| **Paquete** | Archivo comprimido con programas, metadatos y dependencias, instalado por `pacman`. |
| **Repositorio** | Colección firmada de paquetes binarios (`core`, `extra`, `multilib`, repos de Omarchy). |
| **AUR** | Arch User Repository: *recetas* (PKGBUILD) de la comunidad, no binarios verificados. |
| **PKGBUILD** | Script de Bash que describe cómo compilar/empaquetar un programa. |
| **Actualización parcial** | Actualizar unos paquetes sin sincronizar el resto; en Arch no está soportado. |
| **`.pacnew`** | Nueva versión de un archivo de `/etc` que no se sobrescribió porque lo modificaste. |
| **Unidad (unit)** | Objeto de systemd: servicio (`.service`), temporizador (`.timer`), montaje, etc. |
| **journald / journal** | Registro binario de systemd; se consulta con `journalctl`. |
| **LUKS** | Cifrado de disco completo; pide contraseña al arrancar. |
| **Btrfs** | Sistema de archivos con subvolúmenes y snapshots baratos. |
| **Snapshot** | Copia instantánea de un subvolumen (aquí, la raíz `/`), gestionada por snapper. |
| **Limine** | Bootloader de Omarchy; muestra los snapshots en el menú de arranque. |
| **sudo / root** | Ejecutar una orden con privilegios de administrador / cuenta administradora. |
| **Grupo `wheel`** | Grupo cuyos miembros pueden usar `sudo`. |
| **PID / señal** | Número de un proceso / mensaje que se le envía (`SIGTERM`, `SIGKILL`). Ver [[00 Fundamentos/Conceptos de Linux]]. |
| **Variable de entorno** | Par `NOMBRE=valor` que heredan los procesos (`PATH`, `HOME`, `EDITOR`). |
| **Montar** | Conectar un sistema de archivos (disco, USB) a una carpeta del árbol. |
| **initramfs** | Mini-sistema que carga el kernel para descifrar y montar la raíz. |
| **Parámetros del kernel** | Opciones de arranque (`/etc/default/limine` en este equipo). |
| **chroot** | Entrar en un sistema instalado desde otro (p. ej. una USB de rescate). |
| **Subvolumen Btrfs** | División lógica del sistema de archivos (`@`, `@home`…) que se puede fotografiar por separado. |

## Escritorio

| Término | Significado |
|---|---|
| **Wayland** | Protocolo gráfico moderno entre aplicaciones y compositor. |
| **XWayland** | Capa de compatibilidad para apps X11 antiguas. |
| **Compositor** | Programa que dibuja las ventanas y reparte la entrada: Hyprland. |
| **Tiling / mosaico** | Las ventanas se reparten la pantalla sin solaparse. |
| **Flotante** | Ventana que se superpone libremente (`Super+T`). |
| **Workspace [Hyprland]** | Escritorio virtual numerado (`Super+1…0`). |
| **Output / monitor** | Pantalla física reconocida por Hyprland (`eDP-1`, `HDMI-A-1`, `desc:…`). |
| **Dispatcher** | Acción que ejecuta Hyprland (enfocar, mover, cerrar…); en Lua, `hl.dsp.*`. |
| **Regla de ventana** | Condición + efecto aplicado a ventanas que coinciden (`o.window`, `hl.window_rule`). |
| **Layout** | Algoritmo de mosaico: *dwindle* (por defecto) o *scrolling* (`Super+L`). |
| **Grupo** | Pila de ventanas con pestañas en un solo hueco (`Super+G`). |
| **Scratchpad** | Workspace especial oculto que se muestra/oculta (`Super+S`). |
| **Submapa** | Modo temporal de Hyprland en el que las teclas cambian de función (p. ej. «redimensionar»). |
| **Curva / animación** | Definición de la velocidad de las transiciones (`hl.curve`, `hl.animation`). |
| **Tema [Omarchy]** | Carpeta con `colors.toml` y extras; Omarchy genera con ella la configuración de colores de todas las apps. |
| **Omarchy Shell** | Proceso Quickshell con barra, menú, paneles, OSD, notificaciones y bloqueo. |
| **Plugin [Omarchy]** | Extensión QML de la shell con `manifest.json`. |
| **Hook [Omarchy]** | Script ejecutable que Omarchy lanza ante un evento (`post-update`, `theme-set`…). |
| **Migración** | Script de Omarchy que ajusta tu sistema a una versión nueva durante `omarchy update`. |
| **Canal** | Flujo de versiones de Omarchy: `stable`, `rc`, `edge`, `dev`. |
| **Portal XDG** | Servicio que media capturas, selector de archivos y compartir pantalla. |

## Terminal

| Término | Significado |
|---|---|
| **Emulador de terminal** | Ventana gráfica que muestra texto (Alacritty, foot, Ghostty, Kitty). |
| **Shell** | Intérprete de órdenes: Bash. (No confundir con *Omarchy Shell*.) |
| **TUI** | Interfaz de texto a pantalla completa (btop, lazygit, Herdr). |
| **PATH** | Lista de directorios donde Bash busca ejecutables. |
| **Builtin** | Orden interna de Bash (`cd`, `type`, `read`). |
| **Alias / función** | Atajos definidos en `~/.bashrc` o por Omarchy. |
| **Código de salida** | Número que devuelve una orden; `0` = éxito (`$?`). |
| **mise** | Gestor de versiones de lenguajes/herramientas por usuario/proyecto. |
| **Multiplexor** | Programa que mantiene terminales persistentes (tmux, Herdr). |
| **Prefijo** | Tecla que precede a un atajo de tmux/Herdr (`Ctrl+Espacio` aquí). |
| **Sesión [tmux]** | Conjunto persistente de ventanas; sobrevive al cerrar la terminal. |
| **Ventana [tmux]** | Pestaña dentro de una sesión. |
| **Panel / pane** | División de una ventana (tmux) o tab (Herdr). |
| **Workspace [Herdr]** | Agrupación por proyecto: contiene tabs y panes. |
| **Detach / attach** | Desconectar / reconectar un cliente a una sesión persistente. |
| **Tubería (pipe)** | `a \| b`: la salida de `a` entra en `b`. |
| **stdin / stdout / stderr** | Entrada, salida y salida de errores de un programa (0, 1, 2). |
| **Worktree** | Segunda copia de trabajo de un repositorio git en otra carpeta y otra rama. |

## Neovim

| Término | Significado |
|---|---|
| **Modo** | Estado del editor: Normal, Insertar, Visual, Comando, Terminal. |
| **Buffer** | Archivo cargado en memoria. |
| **Ventana [Neovim]** | Vista de un buffer; puede haber varias. |
| **Pestaña [Neovim]** | Colección de ventanas (no equivale a pestaña de navegador). |
| **Leader** | Tecla de prefijo de LazyVim: `Espacio`. |
| **Operador + movimiento** | Gramática de Vim: `d` + `w` = borrar palabra. |
| **Registro** | Portapapeles interno con nombre (`"a`, `"+` = sistema). |
| **LSP** | Language Server Protocol: definición, referencias, diagnósticos. |
| **Treesitter** | Analizador sintáctico para resaltado y selección estructural. |
| **lazy.nvim** | Gestor de plugins; **LazyVim** es la configuración construida sobre él. |
| **Mason** | Instalador de servidores LSP, formateadores y linters. |
| **Extra [LazyVim]** | Módulo opcional activable con `:LazyExtras`. |

[[Inicio|Volver al índice]]
