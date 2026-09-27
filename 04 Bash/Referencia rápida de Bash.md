---
aliases: [Chuleta Bash, Bash cheatsheet]
tags: [bash, referencia]
actualizado: 2026-09-27
verificado_en: "GNU Bash 5.3.15"
---
# Referencia rápida de Bash

Chuleta estructurada a partir de [Devhints Bash](https://devhints.io/bash) (comunitaria) y contrastada con el [manual de GNU Bash](https://www.gnu.org/software/bash/manual/html_node/index.html) (oficial). Explicación de fondo: [[04 Bash/Scripts y expansión]].

## Variables y expansión de parámetros

```bash
nombre="Juan"
echo "$nombre"  "${nombre}"      # expansión
echo '$nombre'                   # literal, sin expansión
```

| Sintaxis | Resultado |
|---|---|
| `${var:-valor}` | `var` o `valor` si está vacía/sin definir |
| `${var:=valor}` | igual, y además asigna `valor` |
| `${var:?mensaje}` | error y salida si está vacía |
| `${var:+valor}` | `valor` sólo si `var` tiene contenido |
| `${#var}` | longitud |
| `${var:0:3}` / `${var: -2}` | subcadena desde 0, 3 caracteres / últimos 2 |
| `${var#patrón}` / `${var##patrón}` | quitar prefijo más corto / más largo |
| `${var%patrón}` / `${var%%patrón}` | quitar sufijo más corto / más largo |
| `${var/a/b}` / `${var//a/b}` | reemplazar primera / todas |
| `${var^}` `${var^^}` / `${var,,}` | mayúscula inicial, todo mayúsculas / minúsculas |
| `${!prefijo*}` | nombres de variables que empiezan por `prefijo` |

```bash
ruta="/home/yo/foto.tar.gz"
echo "${ruta##*/}"    # foto.tar.gz   (basename)
echo "${ruta%/*}"     # /home/yo      (dirname)
echo "${ruta%%.*}"    # /home/yo/foto
echo "${ruta#*.}"     # tar.gz
```

## Variables especiales

| Variable | Significado |
|---|---|
| `$0` | nombre del script |
| `$1` … `$9`, `${10}` | argumentos posicionales |
| `$#` | número de argumentos |
| `"$@"` | todos los argumentos, cada uno por separado (**la correcta casi siempre**) |
| `"$*"` | todos los argumentos en una sola cadena |
| `$?` | código de salida de la última orden |
| `$$` | PID de la shell |
| `$!` | PID del último proceso en segundo plano |
| `$_` | último argumento de la orden anterior |
| `$LINENO`, `$FUNCNAME`, `$BASH_SOURCE` | depuración |

## Condicionales

```bash
if [[ -f "$archivo" ]]; then …; elif [[ -d "$archivo" ]]; then …; else …; fi
[[ -n "$var" ]] && echo "tiene contenido"
```

| Archivos | | Cadenas | | Números (en `[[ ]]`) | |
|---|---|---|---|---|---|
| `-e` | existe | `-z s` | vacía | `-eq` | igual |
| `-f` | archivo regular | `-n s` | no vacía | `-ne` | distinto |
| `-d` | directorio | `a == b` | igual | `-lt` / `-le` | menor / menor o igual |
| `-L` | enlace simbólico | `a != b` | distinta | `-gt` / `-ge` | mayor / mayor o igual |
| `-r` `-w` `-x` | legible, escribible, ejecutable | `a < b` | orden alfabético | | |
| `-s` | tamaño > 0 | `s =~ regex` | expresión regular (`${BASH_REMATCH[1]}`) | | |
| `a -nt b` | `a` más nuevo que `b` | `s == *.txt` | patrón glob | | |

Aritmética: `(( n > 5 ))`, `(( n++ ))`, `echo $(( a * b % 7 ))`. Combinar: `&&`, `||`, `!`.

## Bucles

```bash
for f in *.md; do echo "$f"; done
for i in {1..5}; do echo "$i"; done          # 1 2 3 4 5
for i in {0..10..2}; do echo "$i"; done       # de 2 en 2
for ((i = 0; i < 3; i++)); do echo "$i"; done
while read -r linea; do echo "$linea"; done < archivo.txt
until ping -c1 host &>/dev/null; do sleep 1; done
```

`break` sale del bucle; `continue` pasa a la siguiente vuelta.

## `case`

```bash
case "$1" in
  start|iniciar) iniciar ;;
  stop)          detener ;;
  *.txt)         echo "texto" ;;
  *)             echo "uso: $0 {start|stop}" >&2; exit 2 ;;
esac
```

## Funciones

```bash
saludar() {
  local nombre=${1:?falta el nombre}   # variable local
  printf 'Hola, %s\n' "$nombre"
  return 0                             # código de salida (0-255)
}
resultado=$(saludar "Ana")             # capturar la salida
```

## Arrays

```bash
frutas=("manzana" "pera" "uva")
echo "${frutas[0]}"          # primer elemento
echo "${frutas[@]}"          # todos
echo "${#frutas[@]}"         # cantidad
echo "${!frutas[@]}"         # índices
frutas+=("kiwi")             # añadir
unset 'frutas[1]'            # quitar
for f in "${frutas[@]}"; do echo "$f"; done
mapfile -t lineas < archivo.txt   # leer un archivo en un array

declare -A color=([cielo]=azul [pasto]=verde)   # asociativo
echo "${color[cielo]}"
for k in "${!color[@]}"; do echo "$k=${color[$k]}"; done
```

## Expansiones útiles

| Sintaxis | Resultado |
|---|---|
| `{a,b,c}.txt` | `a.txt b.txt c.txt` |
| `{1..3}` / `{a..c}` | secuencias |
| `$(orden)` | sustitución de orden |
| `<(orden)` | sustitución de proceso: `diff <(ls a) <(ls b)` |
| `~` / `~usuario` | directorio personal |
| `*`, `?`, `[abc]` | comodines; `shopt -s globstar` activa `**` recursivo |
| `shopt -s nullglob` | un patrón sin coincidencias se expande a nada |

## Entrada, salida y heredocs

```bash
printf '%s tiene %d años\n' "Ana" 30
printf '%-10s|%5.2f\n' "precio" 3.14159
read -rp "Nombre: " nombre          # -r sin escapes, -p prompt
read -rsp "Clave: " clave; echo     # -s oculto
read -n1 -rp "¿Seguir? [s/N] " r

cat <<EOF                           # heredoc con expansión
Usuario: $USER
EOF
cat <<'EOF'                         # sin expansión (literal)
$USER
EOF
orden 2>&1 | tee registro.txt       # ver y guardar
exec 3> salida.log                  # descriptor propio
```

## Opciones, depuración y señales

```bash
set -euo pipefail    # salir ante error, variable sin definir, fallo en tubería
set -x               # trazar cada orden (desactivar con set +x)
bash -n script.sh    # comprobar sintaxis sin ejecutar
trap 'echo "Error en la línea $LINENO" >&2' ERR
trap 'rm -f "$tmp"' EXIT
```

## Argumentos con `getopts`

```bash
while getopts ":vo:h" opt; do
  case $opt in
    v) verbose=1 ;;
    o) salida=$OPTARG ;;
    h) uso; exit 0 ;;
    \?) echo "Opción inválida: -$OPTARG" >&2; exit 2 ;;
    :)  echo "-$OPTARG requiere un valor" >&2; exit 2 ;;
  esac
done
shift $((OPTIND - 1))     # "$@" contiene ahora los argumentos restantes
```

## Historial

| Sintaxis | Significado |
|---|---|
| `!!` | última orden (`sudo !!`) |
| `!$` / `!^` / `!*` | último / primer / todos los argumentos de la anterior |
| `!texto` | última orden que empieza por `texto` |
| `!!:s/viejo/nuevo/` | repetir sustituyendo |
| `^viejo^nuevo` | atajo de lo anterior |
| `history \| tail` / `Ctrl+R` | ver / buscar historial |

Fuentes: [Devhints Bash](https://devhints.io/bash), [GNU: Shell Parameter Expansion](https://www.gnu.org/software/bash/manual/html_node/Shell-Parameter-Expansion.html), [Special Parameters](https://www.gnu.org/software/bash/manual/html_node/Special-Parameters.html), [Conditional Expressions](https://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html), [Arrays](https://www.gnu.org/software/bash/manual/html_node/Arrays.html), [The Set Builtin](https://www.gnu.org/software/bash/manual/html_node/The-Set-Builtin.html), [History Interaction](https://www.gnu.org/software/bash/manual/html_node/History-Interaction.html).
