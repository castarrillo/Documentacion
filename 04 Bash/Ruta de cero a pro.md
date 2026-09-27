---
aliases: [Bash de cero a pro, Aprender Bash, Ejercicios Bash]
tags: [bash, aprendizaje, moc]
actualizado: 2026-09-27
verificado_en: "GNU Bash 5.3.15 · Omarchy 4.0.4-1"
---
# Bash de cero a pro

Plan en cinco niveles con **ejercicios resueltos** y una prueba de dominio por nivel. Trabaja en una carpeta de práctica para no tocar nada importante:

```bash
mkdir -p ~/practica-bash && cd ~/practica-bash
```

| Nivel | Meta | Notas |
|---|---|---|
| 0 · Terminal | moverte y manipular archivos | [[00 Fundamentos/Conceptos de Linux]], [[04 Bash/Bash]] |
| 1 · Tuberías | combinar herramientas | [[04 Bash/Herramientas de línea de órdenes]] |
| 2 · Primeros scripts | variables, condiciones, bucles | [[04 Bash/Scripts y expansión]] |
| 3 · Scripts robustos | argumentos, errores, funciones | [[04 Bash/Referencia rápida de Bash]] |
| 4 · Automatizar tu equipo | scripts útiles integrados con Omarchy | [[01 Omarchy/Aplicaciones desarrollo y automatización]] |

---

## Nivel 0 · Terminal

Órdenes: `pwd ls cd mkdir touch cp mv rm cat less head tail man tldr`. Teclas: `Tab`, `Ctrl+R`, `Ctrl+A/E`, `Ctrl+C`.

**Ejercicio 0.1** — Crea `proyectos/{web,scripts,notas}`, un archivo vacío en cada una, lista el árbol y borra `web`.

```bash
mkdir -p proyectos/{web,scripts,notas}
touch proyectos/{web,scripts,notas}/leeme.txt
lt proyectos        # o: ls -R proyectos
rm -r proyectos/web
```

> [!success] Prueba de dominio
> Sin ratón: creas 3 carpetas, mueves archivos entre ellas, renombras uno, lees el `man` de `cp` para averiguar cómo copiar conservando fechas (`-a`) y recuperas una orden anterior con `Ctrl+R`.

## Nivel 1 · Tuberías y redirecciones

**Ejercicio 1.1** — ¿Cuántos paquetes explícitos tienes instalados?

```bash
pacman -Qe | wc -l
```

**Ejercicio 1.2** — Las 5 carpetas más pesadas de tu home, guardadas en un archivo.

```bash
du -h -d1 ~ 2>/dev/null | sort -h | tail -5 | tee pesadas.txt
```

**Ejercicio 1.3** — ¿Qué clases de ventanas tienes abiertas y cuántas de cada una?

```bash
hyprctl clients -j | jq -r '.[].class' | sort | uniq -c | sort -rn
```

**Ejercicio 1.4** — Errores del arranque actual agrupados por programa.

```bash
journalctl -b -p err -o cat --output-fields=SYSLOG_IDENTIFIER | sort | uniq -c | sort -rn
```

> [!success] Prueba de dominio
> Explicas qué hacen `>`, `>>`, `2>`, `|`, `&&` y `||`, y encadenas 4 herramientas para responder una pregunta sobre tu sistema.

## Nivel 2 · Primeros scripts

**Ejercicio 2.1** — `hola.sh` que salude al argumento o a «mundo».

```bash
#!/usr/bin/env bash
nombre=${1:-mundo}
printf 'Hola, %s\n' "$nombre"
```

`chmod +x hola.sh && ./hola.sh Ana`

**Ejercicio 2.2** — `ordenar-descargas.sh`: mover imágenes, PDF y otros de `~/Downloads` a subcarpetas.

```bash
#!/usr/bin/env bash
set -euo pipefail
cd ~/Downloads
shopt -s nullglob nocaseglob
mkdir -p Imagenes PDF Otros
for f in *; do
  [[ -f $f ]] || continue
  case $f in
    *.png|*.jpg|*.jpeg|*.webp) mv -n -- "$f" Imagenes/ ;;
    *.pdf)                     mv -n -- "$f" PDF/ ;;
    *)                         mv -n -- "$f" Otros/ ;;
  esac
done
```

**Ejercicio 2.3** — Aviso si el disco raíz supera el 80 %.

```bash
#!/usr/bin/env bash
uso=$(df --output=pcent / | tail -1 | tr -dc '0-9')
if (( uso > 80 )); then
  notify-send -u critical "Disco casi lleno" "La raíz está al ${uso}%"
fi
```

> [!success] Prueba de dominio
> Escribes sin copiar un script con `if`, `for` y `case` que funciona con nombres de archivo con espacios.

## Nivel 3 · Scripts robustos

Aplica la [[04 Bash/Scripts y expansión#Plantilla de script|plantilla]]: `set -euo pipefail`, `getopts`, `trap`, funciones con `local`, mensajes de error a `stderr` y códigos de salida.

**Ejercicio 3.1** — `copia-config.sh [-d destino]`: empaqueta tus configuraciones con fecha.

```bash
#!/usr/bin/env bash
set -euo pipefail
destino=~/Backups
while getopts ":d:h" o; do
  case $o in
    d) destino=$OPTARG ;;
    h) echo "Uso: ${0##*/} [-d destino]"; exit 0 ;;
    *) echo "Opción inválida" >&2; exit 2 ;;
  esac
done
mkdir -p "$destino"
archivo="$destino/config-$(date +%F-%H%M).tar.gz"
tar czf "$archivo" -C ~ .config/hypr .config/omarchy .config/nvim .config/tmux .config/herdr .bashrc
printf 'Copia creada: %s (%s)\n' "$archivo" "$(du -h "$archivo" | cut -f1)"
```

**Ejercicio 3.2** — Instala ShellCheck (`omarchy pkg add shellcheck`) y pasa todos tus scripts: `shellcheck *.sh`. Corrige cada aviso entendiendo el motivo (cada código `SCxxxx` tiene su página en [shellcheck.net](https://www.shellcheck.net/wiki/)).

> [!success] Prueba de dominio
> Tu script acepta opciones, valida entradas, limpia temporales con `trap`, devuelve códigos de salida con sentido y ShellCheck no da avisos.

## Nivel 4 · Automatizar tu equipo

Guarda los scripts en `~/.local/bin` (ya está en el `PATH`) y conéctalos con Omarchy:

| Integración | Cómo |
|---|---|
| Atajo de teclado | `o.bind("SUPER + ALT + C", "Copia config", "copia-config.sh")` en `bindings.lua` |
| Entrada de menú | `~/.config/omarchy/extensions/omarchy-menu.jsonc` |
| Evento | `omarchy hook install post-update ~/.local/bin/mi-script` |
| Periódico | temporizador `systemd --user` |
| Notificación | `notify-send "Título" "Texto"` o `omarchy notification send` |
| Interfaz | `gum choose`, `gum input`, `omarchy menu select prompt …` |

**Ejercicio 4.1** — Temporizador semanal para `copia-config.sh`:

```ini
# ~/.config/systemd/user/copia-config.service
[Service]
Type=oneshot
ExecStart=%h/.local/bin/copia-config.sh

# ~/.config/systemd/user/copia-config.timer
[Timer]
OnCalendar=weekly
Persistent=true

[Install]
WantedBy=timers.target
```

```bash
systemctl --user daemon-reload
systemctl --user enable --now copia-config.timer
systemctl --user list-timers
```

**Ejercicio 4.2** — Selector de proyectos que abre Herdr en el elegido:

```bash
#!/usr/bin/env bash
dir=$(fd -t d -d 1 . ~/Work | gum filter --placeholder "Proyecto…") || exit 0
cd "$dir" && exec herdr
```

> [!success] Prueba de dominio
> Tienes al menos tres scripts propios en `~/.local/bin`, uno con atajo, uno periódico y uno en el menú de Omarchy, todos versionados en git.

## Siguientes pasos

- Leer el [manual de GNU Bash](https://www.gnu.org/software/bash/manual/) de principio a fin (es corto).
- Estudiar los scripts de Omarchy en `/usr/share/omarchy/bin/`: buen Bash de producción.
- Para scripts de más de ~200 líneas o con estructuras de datos complejas, considera Python (`uv run script.py`).

Fuentes: [GNU Bash manual](https://www.gnu.org/software/bash/manual/), [Devhints Bash](https://devhints.io/bash), [ShellCheck](https://www.shellcheck.net/), [ArchWiki: systemd/Timers](https://wiki.archlinux.org/title/Systemd/Timers), [gum](https://github.com/charmbracelet/gum).
