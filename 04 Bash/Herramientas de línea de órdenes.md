---
aliases: [Coreutils, grep sed awk, find, jq, Herramientas CLI]
tags: [bash, herramientas, referencia]
actualizado: 2026-09-27
verificado_en: "instaladas: rg fd fzf bat eza zoxide jq tldr btop lazygit gum curl rsync gawk sed uv"
---
# Herramientas de línea de órdenes

La potencia de la terminal viene de **combinar herramientas pequeñas** con tuberías (`|`). Esta nota reúne las esenciales, con la versión clásica (presente en cualquier Linux) y la moderna que trae Omarchy. Conceptos previos: [[00 Fundamentos/Conceptos de Linux]] y [[04 Bash/Bash]].

## Archivos y directorios

| Tarea | Orden | Notas |
|---|---|---|
| Listar | `ls -la` · `lt` (árbol, alias Omarchy) | `ls` aquí es `eza` con iconos |
| Moverse | `cd ruta` · `cd -` · `cd` (a `~`) | `cd proy` salta con zoxide |
| Crear carpeta | `mkdir -p a/b/c` | `-p` crea las intermedias |
| Copiar | `cp -r origen destino` · `cp -a` (conserva permisos y fechas) | |
| Mover / renombrar | `mv viejo nuevo` | `-i` pregunta antes de sobrescribir |
| Borrar | `rm archivo` · `rm -r carpeta` | **no hay papelera**; usa `gio trash archivo` si quieres una |
| Enlace simbólico | `ln -s destino nombre` | |
| Ver contenido | `cat`, `bat archivo` (con colores), `less archivo` | en `less`: `/` busca, `q` sale |
| Principio / final | `head -n 20`, `tail -n 50`, `tail -f registro.log` | `-f` sigue en vivo |
| Tamaño | `du -sh carpeta`, `df -h` | |
| Tipo de archivo | `file archivo` | |
| Comprimir | `compress carpeta` / `decompress x.tar.gz` (Omarchy) · `tar czf x.tar.gz carpeta` / `tar xzf x.tar.gz` · `unzip x.zip` | |

## Buscar archivos: `find` y `fd`

```bash
fd informe                       # por nombre (ignora .gitignore y ocultos)
fd -e md                         # por extensión
fd -H -t d config                # incluir ocultos, sólo directorios
fd -e log -x rm {}               # ejecutar algo sobre cada resultado

find . -name "*.md" -mtime -7    # clásico: .md modificados en 7 días
find ~ -size +500M               # archivos grandes
find . -type f -name "*.tmp" -delete
```

## Buscar dentro de archivos: `grep` y `rg`

```bash
rg "TODO"                        # recursivo, respeta .gitignore, muy rápido
rg -i "error" -g "*.log"         # sin mayúsculas, sólo .log
rg -l "hl.bind" ~/.config/hypr   # sólo nombres de archivo
rg -C 2 "función"                # 2 líneas de contexto
rg -w "id"                       # palabra completa

grep -rn "patrón" carpeta        # clásico: recursivo con número de línea
journalctl -b | grep -iE "error|fail"
```

> [!tip] ¿`grep` o `rg`?
> `rg` es más rápido y respeta `.gitignore`; `grep` está en cualquier Linux, así que úsalo en scripts que vayan a otros equipos.

## Filtrar y transformar texto

| Herramienta | Uso típico |
|---|---|
| `sort` / `sort -n` / `sort -h` | ordenar alfabético / numérico / tamaños (`2K`, `1G`) |
| `uniq -c` | contar repeticiones (tras `sort`) |
| `wc -l` | contar líneas |
| `cut -d: -f1` | columna 1 separada por `:` |
| `tr 'a-z' 'A-Z'` · `tr -d '\r'` | sustituir o borrar caracteres |
| `sed 's/viejo/nuevo/g'` | reemplazar (`-i` edita el archivo **in situ**) |
| `awk '{print $2}'` | columna 2 separada por espacios |
| `column -t` | alinear en tabla |
| `tee archivo` | ver la salida y guardarla a la vez |
| `xargs` | convertir líneas en argumentos |

```bash
# Los 10 comandos que más usas
history | awk '{print $2}' | sort | uniq -c | sort -rn | head

# Qué carpetas ocupan más en tu home
du -h -d1 ~ 2>/dev/null | sort -h | tail

# Reemplazar en varios archivos (revisa antes sin -i)
rg -l "viejo" | xargs sed -i 's/viejo/nuevo/g'

# Suma de una columna
awk '{s += $3} END {print s}' datos.txt

# Usuarios del sistema con shell de login
awk -F: '$7 ~ /bash/ {print $1}' /etc/passwd
```

> [!warning] `sed -i` y `rm` con comodines
> Prueba siempre primero sin `-i` (o con `ls` en vez de `rm`) para ver qué cambiaría.

## JSON: `jq`

Imprescindible con Omarchy y Hyprland, que devuelven JSON (`-j`, `--json`).

```bash
hyprctl clients -j | jq '.[] | {class, title}'          # objetos resumidos
hyprctl monitors -j | jq -r '.[].name'                  # -r: texto sin comillas
omarchy plugin list --json | jq -r '.[] | select(.enabled) | .id'
jq '.idle' ~/.config/omarchy/shell.json                 # leer una clave
jq '.idle.lock = 600' shell.json > /tmp/s && mv /tmp/s shell.json   # modificar (haz copia antes)
```

## Selección interactiva: `fzf` y `gum`

```bash
ff                                  # alias Omarchy: fzf con vista previa
nvim "$(fd -e md | fzf)"            # elegir y abrir
kill -9 "$(ps -eo pid,comm | fzf | awk '{print $1}')"
git switch "$(git branch --format='%(refname:short)' | fzf)"
gum confirm "¿Continuar?" && echo sí    # preguntas bonitas en scripts
gum choose rojo verde azul
```

`Ctrl+R` busca en el historial y `Ctrl+T` inserta una ruta, ambos con fzf.

## Red y transferencias

```bash
curl -fsSL https://ejemplo.com/api | jq .     # -f falla en errores HTTP, -L sigue redirecciones
curl -O https://ejemplo.com/archivo.zip       # descargar con el nombre original
rsync -avh --progress origen/ destino/        # copiar/sincronizar (la / final importa)
rsync -avh --delete ~/Documents/ usb:/copia/  # espejo exacto (borra lo sobrante: ¡cuidado!)
ssh usuario@host                              # ver [[08 Recetas del equipo/Recetario de tareas comunes#SSH]]
scp archivo usuario@host:/ruta/
ss -tulpn                                     # puertos abiertos
```

## Procesos y sistema

```bash
btop                         # monitor interactivo
pgrep -a nombre; pkill nombre
nohup orden &> log.txt &     # que siga al cerrar la terminal (mejor: tmux/Herdr)
watch -n 2 'df -h /'         # repetir cada 2 s
time orden                   # cuánto tarda
```

## Ayuda rápida

| Orden | Ofrece |
|---|---|
| `tldr tar` | ejemplos prácticos |
| `man tar` | manual completo |
| `orden --help \| less` | opciones |
| [Devhints](https://devhints.io/bash) | chuletas |

Fuentes: [ArchWiki: Core utilities](https://wiki.archlinux.org/title/Core_utilities), [ripgrep](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md), [fd](https://github.com/sharkdp/fd), [fzf](https://github.com/junegunn/fzf), [jq manual](https://jqlang.org/manual/), [GNU sed](https://www.gnu.org/software/sed/manual/sed.html), [GNU awk](https://www.gnu.org/software/gawk/manual/gawk.html), [rsync](https://wiki.archlinux.org/title/Rsync), [Omarchy: Shell tools](https://omarchy.org/manual/shell-tools/).
