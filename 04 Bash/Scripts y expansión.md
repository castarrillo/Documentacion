---
aliases: [Scripts Bash, Expansión, Comillas]
tags: [bash, scripts]
actualizado: 2026-09-27
verificado_en: "GNU Bash 5.3.15"
---
# Bash: scripts, expansiones y errores comunes

## Orden en que Bash procesa una línea

1. **Divide en palabras** respetando comillas y operadores (`|`, `;`, `&&`, `>`).
2. **Expansiones**, en este orden: llaves `{a,b}` → tilde `~` → parámetros `$var` / aritmética `$(( ))` / sustitución de orden `$( )` → **separación de palabras** (sólo en resultados *sin comillas*) → **comodines** `*` → eliminación de comillas.
3. Aplica **redirecciones**.
4. **Ejecuta** (alias → función → builtin → `PATH`).

Por eso las comillas importan: el paso de separación de palabras y el de comodines sólo actúan sobre lo que **no** está entre comillas dobles.

```bash
archivo="Mi documento.txt"
rm $archivo          # ✗ intenta borrar «Mi» y «documento.txt»
rm "$archivo"        # ✓ un solo argumento
printf '%s\n' '${archivo}'       # comillas simples: texto literal
printf '%s\n' "${archivo%.txt}"  # comillas dobles: expande → «Mi documento»
```

| Comillas | Expande `$var` y `$( )` | Separa palabras / comodines |
|---|---|---|
| sin comillas | sí | **sí** (peligroso con espacios) |
| `"dobles"` | sí | no |
| `'simples'` | no | no |
| `$'ansi'` | secuencias `\n`, `\t` | no |

`"$@"` conserva cada argumento por separado; `"$*"` los une en uno. `read -r` evita interpretar barras invertidas.

## Plantilla de script

```bash
#!/usr/bin/env bash
# Descripción: saluda a una persona.
# Uso: saludar.sh [-v] NOMBRE
set -euo pipefail

uso() { printf 'Uso: %s [-v] NOMBRE\n' "${0##*/}" >&2; }

verbose=0
while getopts ":vh" opt; do
  case $opt in
    v) verbose=1 ;;
    h) uso; exit 0 ;;
    *) uso; exit 2 ;;
  esac
done
shift $((OPTIND - 1))

[[ $# -ge 1 ]] || { uso; exit 2; }
nombre=$1

tmp=$(mktemp)
trap 'rm -f "$tmp"' EXIT          # limpieza pase lo que pase

(( verbose )) && printf 'Saludando a %s…\n' "$nombre" >&2
printf 'Hola, %s\n' "$nombre" | tee "$tmp"
```

Guarda en `~/.local/bin/saludar` y ejecuta `chmod +x ~/.local/bin/saludar`: ya está en tu `PATH`.

### Qué hace `set -euo pipefail`

| Opción | Efecto | Advertencia |
|---|---|---|
| `-e` | termina ante una orden fallida | no actúa dentro de `if`, `&&`, `\|\|` ni en funciones llamadas desde ellos |
| `-u` | error al usar una variable no definida | usa `${1:-}` para argumentos opcionales |
| `-o pipefail` | una tubería falla si falla cualquier parte | `grep` sin coincidencias devuelve 1 |

No sustituyen a comprobar explícitamente las operaciones importantes.

## Bucles seguros sobre archivos

```bash
# Comodín: seguro con espacios
for ruta in "$HOME"/Documents/*.md; do
  [[ -e "$ruta" ]] || continue       # por si no hay coincidencias
  printf 'Archivo: %s\n' "$ruta"
done

# Resultados de find: separados por NUL
while IFS= read -r -d '' ruta; do
  printf '%s\n' "$ruta"
done < <(find . -name '*.log' -print0)
```

> [!warning] Anti-patrones
> - `for f in $(ls)` — se rompe con espacios; usa comodines.
> - `cat archivo | while read …` — el bucle corre en una subshell y sus variables se pierden; usa `while … done < archivo`.
> - `cd dir; rm -rf *` — si `cd` falla, borra el directorio actual; usa `cd dir || exit`.
> - `rm -rf "$DIR/"` con `$DIR` vacía → `rm -rf /`; usa `"${DIR:?}"`.

## Probar y depurar

```bash
bash -n script.sh          # sólo sintaxis
bash -x script.sh          # traza cada orden
shellcheck script.sh       # análisis estático (instalar: omarchy pkg add shellcheck)
```

**ShellCheck** detecta la mayoría de los errores de comillas y expansión; conviene usarlo siempre. En Neovim, LazyVim puede mostrar sus avisos con el extra de Bash.

## Scripts con privilegios

Pide `sudo` sólo en la orden que lo necesita, no ejecutes todo el script como root. Para actualizar el sistema, sigue el procedimiento de Omarchy (`omarchy update`), no un script propio con `pacman -Syu`.

Referencia de sintaxis: [[04 Bash/Referencia rápida de Bash]].

Fuentes: [GNU: Shell Expansions](https://www.gnu.org/software/bash/manual/html_node/Shell-Expansions.html), [Quoting](https://www.gnu.org/software/bash/manual/html_node/Quoting.html), [Shell Scripts](https://www.gnu.org/software/bash/manual/html_node/Shell-Scripts.html), [The Set Builtin](https://www.gnu.org/software/bash/manual/html_node/The-Set-Builtin.html), [Devhints Bash](https://devhints.io/bash), [ShellCheck](https://www.shellcheck.net/).
