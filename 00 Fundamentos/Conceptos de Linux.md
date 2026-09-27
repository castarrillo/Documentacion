---
aliases: [Linux básico, Permisos, Procesos, Usuarios y grupos, Variables de entorno]
tags: [fundamentos, linux, principiantes]
actualizado: 2026-09-27
verificado_en: "Arch Linux bajo Omarchy 4.0.4-1 · usuario castarrillo"
---
# Conceptos de Linux para empezar

Lo mínimo que conviene entender antes de tocar la terminal en serio. Cada sección termina con órdenes **de sólo lectura** para practicar sin riesgo. Vocabulario: [[00 Fundamentos/Glosario]]. Sintaxis de Bash: [[04 Bash/Bash]].

## 1. Todo es un archivo, en un único árbol

No hay «unidades» `C:` o `D:`: todo cuelga de la raíz `/`. Los discos, particiones y memorias USB se **montan** en carpetas del árbol.

```text
/                    raíz
├── boot/            arranque (kernel, Limine) — sólo root
├── etc/             configuración del sistema
├── home/castarrillo/  tu carpeta personal (~)
├── usr/bin/         programas instalados por paquetes
├── var/log/         registros
├── tmp/             temporales (se borran al reiniciar)
├── run/media/…      memorias USB montadas
└── dev/  proc/  sys/  dispositivos y datos del kernel (virtuales)
```

| Ruta | Significado |
|---|---|
| `/home/castarrillo/Documents` | **absoluta** (empieza por `/`) |
| `Documents/Documentacion` | **relativa** al directorio actual |
| `~` | tu carpeta personal |
| `.` / `..` | directorio actual / padre |
| `-` (en `cd -`) | directorio anterior |

Los archivos cuyo nombre empieza por `.` están **ocultos** (`~/.config`, `~/.bashrc`); `ls -a` los muestra. Linux distingue mayúsculas: `Foto.png` ≠ `foto.png`. Evita espacios en nombres que uses desde la terminal (o entrecomíllalos).

```bash
pwd; ls -la ~; tree -L 1 / 2>/dev/null || ls /
findmnt -t btrfs,vfat          # qué hay montado y dónde
```

> [!info] En tu equipo
> La raíz es un volumen **Btrfs** cifrado con **LUKS**, dividido en subvolúmenes: `@` → `/`, `@home` → `/home`, `@log` → `/var/log`, `@pkg` → `/var/cache/pacman/pkg`. Por eso los snapshots de `/` no incluyen `/home` ([[01 Omarchy/Instalación seguridad y recuperación]]).

## 2. Usuarios, grupos y `sudo`

Cada archivo pertenece a un **usuario** y a un **grupo**. Tú eres `castarrillo` (UID 1000). **root** (UID 0) puede hacer cualquier cosa.

```bash
id                 # tu usuario y grupos
whoami; groups
```

| Grupo | Qué permite |
|---|---|
| `wheel` | usar `sudo` (administrar el sistema con tu contraseña) |
| `docker` | usar Docker sin `sudo` — **equivale a ser root** |
| `vboxusers` | usar VirtualBox con USB |

`sudo orden` ejecuta **una** orden como root tras pedir tu contraseña (queda recordada unos minutos). Úsalo sólo para administrar el sistema (`/etc`, paquetes, servicios), **nunca** para tus propios archivos: crearías archivos de root en tu carpeta que luego no podrás editar. Para editar un archivo del sistema: `sudoedit /etc/archivo`.

> [!warning] Si una orden «necesita sudo» sin motivo claro
> Detente y averigua por qué. Copiar órdenes con `sudo` de internet sin entenderlas es la forma más común de romper (o comprometer) un sistema.

## 3. Permisos

```text
$ ls -l script.sh
-rwxr-x---  1 castarrillo castarrillo  512 sep 27 10:00 script.sh
│└┬┘└┬┘└┬┘    └── dueño ──┘ └─ grupo ─┘
│ │  │  └── otros:  ---  (nada)
│ │  └───── grupo:  r-x  (leer y ejecutar)
│ └──────── dueño:  rwx  (leer, escribir, ejecutar)
└────────── tipo: - archivo, d directorio, l enlace
```

| Permiso | En un archivo | En un directorio |
|---|---|---|
| `r` (4) | leer el contenido | listar su contenido |
| `w` (2) | modificarlo | crear/borrar archivos dentro |
| `x` (1) | ejecutarlo | entrar (`cd`) |

```bash
chmod +x script.sh            # hacer ejecutable
chmod 600 ~/.ssh/id_ed25519   # sólo el dueño lee/escribe (números: r4+w2+x1)
chmod 755 carpeta             # rwxr-xr-x
sudo chown castarrillo:castarrillo archivo   # cambiar dueño (requiere root)
stat archivo                  # todos los detalles
```

Para ejecutar un script del directorio actual: `./script.sh` (el directorio actual **no** está en el `PATH` por seguridad).

## 4. Procesos y señales

Un **proceso** es un programa en ejecución con un número (**PID**), un dueño y un proceso padre.

```bash
ps aux | head                 # todos los procesos
pgrep -a kitty                # buscar por nombre
btop                          # monitor interactivo (Super+Ctrl+T)
```

| Señal | Número | Significado | Enviar |
|---|---|---|---|
| `SIGTERM` | 15 | «termina, por favor» (permite guardar/limpiar) | `kill PID` · `pkill nombre` |
| `SIGINT` | 2 | interrumpir (lo que hace `Ctrl+C`) | `kill -INT PID` |
| `SIGKILL` | 9 | matar sin opción a limpiar (último recurso) | `kill -9 PID` |
| `SIGHUP` | 1 | «se cerró la terminal» / muchos demonios recargan su config | `kill -HUP PID` |
| `SIGSTOP`/`SIGCONT` | — | pausar / reanudar (`Ctrl+Z`, `fg`) | `kill -STOP PID` |

Una ventana gráfica colgada: `hyprctl kill` y clic sobre ella, o `omarchy restart app <nombre>`. Los procesos de larga duración gestionados por systemd son **servicios** ([[02 Arch Linux/Servicios red y diagnóstico]]).

## 5. Variables de entorno

Pares `NOMBRE=valor` que cada proceso hereda de su padre y que configuran el comportamiento de los programas.

```bash
printenv | sort | less
echo "$HOME $USER $SHELL $EDITOR $XDG_CONFIG_HOME"
printf '%s\n' "$PATH" | tr ':' '\n'      # dónde se buscan los programas
MI_VAR=1 programa                        # sólo para esa ejecución
export MI_VAR=1                          # para esta shell y sus hijos
```

| Dónde definirla | Alcance |
|---|---|
| `~/.bashrc` | terminales Bash interactivas |
| `~/.config/uwsm/env` | **toda la sesión gráfica** (apps lanzadas desde el escritorio); reinicia sesión para aplicarla |
| `hl.env("VAR", "valor")` en Hyprland | procesos lanzados por Hyprland |
| `/etc/environment` | todo el sistema (evítalo salvo necesidad) |

> [!info] En tu equipo
> `~/.config/uwsm/env` define `OMARCHY_SCREENSHOT_DIR` y `OMARCHY_SCREENRECORD_DIR`, que es como se fijan las carpetas de capturas.

## 6. Entrada, salida y códigos de salida

Todo programa tiene **stdin** (0, entrada), **stdout** (1, salida) y **stderr** (2, errores), y termina con un **código de salida** (`0` = éxito). Con eso se encadenan herramientas pequeñas: `journalctl -b | grep -i error | wc -l`. Detalle: [[04 Bash/Bash#Redirecciones, tuberías y códigos de salida]] y [[04 Bash/Herramientas de línea de órdenes]].

## 7. Paquetes, servicios y configuración

| Si quieres… | Es un… | Se gestiona con |
|---|---|---|
| tener un programa | **paquete** | `omarchy pkg add`, `pacman` |
| que algo corra en segundo plano | **servicio** | `systemctl` |
| cambiar cómo se comporta | **archivo de configuración** | `~/.config/…` (tuyo) o `/etc/…` (sistema) |

## 8. Dispositivos y discos

```bash
lsblk -f                     # discos, particiones, sistemas de archivos
udisksctl mount -b /dev/sda1 # montar una USB como usuario (Nautilus lo hace solo)
udisksctl unmount -b /dev/sda1
lspci -k                     # tarjetas y su controlador (driver)
sensors                      # temperaturas
```

Los dispositivos aparecen en `/dev` (`/dev/nvme0n1` = tu SSD, `/dev/sda` = una USB). **Nunca** escribas directamente en `/dev/…` sin estar seguro del nombre: `dd` o `iso2sd` al dispositivo equivocado borra un disco entero.

## 9. Pedir ayuda al propio sistema

| Orden | Da |
|---|---|
| `orden --help` | resumen de opciones |
| `man orden` | manual completo (`/` busca, `q` sale) |
| `tldr orden` | ejemplos prácticos (instalado) |
| `help cd` | ayuda de órdenes internas de Bash |
| `apropos palabra` | buscar manuales por tema |
| [ArchWiki](https://wiki.archlinux.org/title/Main_page) | referencia general, excelente para principiantes |

Fuentes: [ArchWiki: Users and groups](https://wiki.archlinux.org/title/Users_and_groups), [File permissions and attributes](https://wiki.archlinux.org/title/File_permissions_and_attributes), [Sudo](https://wiki.archlinux.org/title/Sudo), [Environment variables](https://wiki.archlinux.org/title/Environment_variables), [Filesystem Hierarchy Standard](https://wiki.archlinux.org/title/Filesystem_Hierarchy_Standard), [Core utilities](https://wiki.archlinux.org/title/Core_utilities), `man 7 signal`, `man hier`.
