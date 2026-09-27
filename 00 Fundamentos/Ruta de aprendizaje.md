---
aliases: [Ruta de aprendizaje, Plan de estudio]
tags: [fundamentos, aprendizaje]
actualizado: 2026-09-27
verificado_en: "Omarchy 4.0.4-1"
---
# Ruta de aprendizaje

Siete sesiones cortas, en orden, pensadas para alguien nuevo en Linux. Si vienes de Windows o macOS, lee antes [[01 Omarchy/Primeros pasos]]; los conceptos básicos (archivos, permisos, procesos) están en [[00 Fundamentos/Conceptos de Linux]]. Cada una termina con una **comprobación**: si puedes hacerla sin mirar la nota, pasa a la siguiente. Vuelve a [[README]] cuando quieras.

## 1. Orientación (30 min)

1. `Super+Espacio` abre el lanzador; escribe el nombre de una app y pulsa `Enter`. `Escape` cierra. `Super+Alt+Espacio` abre el menú de aplicaciones y `Super+Escape` el menú del sistema.
2. `Super+Enter` abre la terminal (Kitty en este equipo). Ejecuta `pwd`, `ls`, `whoami`, `omarchy version`.
3. Abre otra terminal y alterna con `Super+Flechas`; intercambia posiciones con `Super+Shift+Flechas`.
4. `Super+K` muestra todos los atajos con buscador.
5. Lee [[00 Fundamentos/Mapa del sistema]] y [[01 Omarchy/Atajos y flujos]].

> [!tip] Comprobación
> Abre tres terminales en el workspace 2, pasa una al 3 con `Super+Shift+3` y cierra todo con `Super+W`.

## 2. Arch y mantenimiento (45 min)

```bash
pacman -Q omarchy hyprland    # versiones instaladas
pacman -Qi hyprland           # detalles y dependencias
omarchy version pkgs          # cuándo se actualizó el sistema por última vez
systemctl --user list-units --type=service
journalctl --user -b -n 30
```

Aprende la diferencia entre **instalar un paquete** (`omarchy pkg add`) y **actualizar el sistema entero** (`omarchy update`). Lee [[02 Arch Linux/Paquetes y actualizaciones]].

> [!tip] Comprobación
> Sabes explicar por qué no se usa `pacman -Syu` directamente en Omarchy y qué cubre (y qué no) un snapshot.

## 3. Escritorio (45 min)

Ejecuta `hyprctl monitors all`, `hyprctl clients` y `omarchy menu keybindings --print | less`. Abre `~/.config/hypr/` y lee `hyprland.lua`: verás que Omarchy carga sus defaults y después tus módulos. Desde Hyprland 0.55 la configuración nativa es **Lua** (`hl.*`); Omarchy añade ayudas `o.*`. Lee [[03 Hyprland/Hyprland en Omarchy]].

> [!tip] Comprobación
> Añades un atajo de prueba en `bindings.lua`, ejecutas `hyprctl reload` y `hyprctl configerrors` sin errores, y después lo retiras.

## 4. Terminal y scripts (1 h)

```bash
type ls cd t          # alias, builtin, alias de Omarchy
command -v bash
printf '%s\n' "$PATH" | tr ':' '\n'
man bash              # busca con /Parameter Expansion
```

Escribe un script que reciba un nombre y lo imprima con `printf`, valide que no esté vacío y devuelva código 2 si falta. Lee [[04 Bash/Scripts y expansión]] y ten a mano [[04 Bash/Referencia rápida de Bash]].

> [!tip] Comprobación
> Explicas la diferencia entre `"$@"` y `$*` y por qué `rm $archivo` sin comillas es peligroso.

## 5. Edición (1 h)

Ejecuta `nvim --clean +Tutor` (en este equipo `nvim +Tutor` no funciona porque LazyVim desactiva el plugin `tutor`, y `vimtutor` no está instalado). Practica `i`, `Esc`, `:w`, `:q`, `/`, `dd`, `u`, `ciw`, `.`. Después, en LazyVim: `Espacio Espacio` (archivos), `Espacio /` (grep), `Espacio e` (explorador), `Espacio g g` (lazygit). Lee [[05 Neovim/Edición y atajos]]. Para convertirlo en tu IDE principal, continúa con la ruta específica [[05 Neovim/Ruta de cero a pro]].

> [!tip] Comprobación
> Renombras una palabra en tres líneas con `ciw` + `n` + `.` sin tocar el ratón.

## 6. Sesiones persistentes (45 min)

Prueba **uno** de los gestores: tmux (`Super+Alt+Enter` o `t`) o Herdr (`Super+Ctrl+Enter` o `h`). Crea dos paneles, desconéctate (`Prefijo, d`) y vuelve. Compara [[06 Tmux/Tmux en Omarchy]] con [[07 Herdr/Herdr en Omarchy]].

> [!tip] Comprobación
> Cierras la ventana de terminal con un programa corriendo en tmux y lo recuperas con `tmux attach`.

## 7. Une las piezas

Reproduce [[08 Recetas del equipo/Flujo de trabajo integrado]], recorre [[08 Recetas del equipo/Diagnóstico por síntomas]] y agenda [[08 Recetas del equipo/Mantenimiento periódico]]. Anota en tus propias notas los comandos que más usaste.

## Rutas especializadas (después de las 7 sesiones)

| Tema | Ruta | Nivel final |
|---|---|---|
| Terminal y scripts | [[04 Bash/Ruta de cero a pro]] | automatizar tu equipo con scripts, atajos y temporizadores |
| Editor / IDE | [[05 Neovim/Ruta de cero a pro]] | Neovim como IDE principal y configurado por ti |
| Escritorio | [[01 Omarchy/Personalización paso a paso]] → [[03 Hyprland/Recetas de Hyprland]] | tema propio, reglas, submapas |
| Sistema | [[02 Arch Linux/Arranque kernel y hardware]] → [[02 Arch Linux/Rescate desde USB]] | recuperar un equipo que no arranca |
| Sesiones | [[06 Tmux/Tmux avanzado]] · [[07 Herdr/Herdr avanzado]] | entornos de proyecto con un comando |
| Día a día | [[08 Recetas del equipo/Recetario de tareas comunes]] | SSH, git, USB, VPN, impresoras, Docker |

## Para seguir aprendiendo

- Manual completo de Omarchy clasificado: [[01 Omarchy/Guía del manual oficial]].
- Tutorial oficial de Hyprland (sintaxis Lua): [Master tutorial](https://wiki.hypr.land/Getting-Started/Master-Tutorial/).
- ArchWiki: [General recommendations](https://wiki.archlinux.org/title/General_recommendations) y [System maintenance](https://wiki.archlinux.org/title/System_maintenance).
- Neovim: `:help user-manual`; LazyVim: [Keymaps](https://www.lazyvim.org/keymaps).
- Bash: [manual de GNU Bash](https://www.gnu.org/software/bash/manual/) y [Devhints](https://devhints.io/bash).
