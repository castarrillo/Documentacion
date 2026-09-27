---
aliases: [pacman, AUR, Actualizaciones]
tags: [arch, pacman, aur, mantenimiento]
actualizado: 2026-09-27
verificado_en: "pacman + pacman-contrib 1.13.1 · yay · Omarchy 4.0.4-1"
---
# Paquetes, pacman, AUR y actualizaciones

`pacman` instala paquetes binarios de repositorios firmados y mantiene una base de datos local que relaciona paquetes, archivos y dependencias. En Omarchy lo usarás sobre todo para **consultar**; para instalar y actualizar hay envoltorios `omarchy pkg …` y `omarchy update`.

## Chuleta de pacman

| Objetivo | Orden | Nota |
|---|---|---|
| Buscar en repositorios | `pacman -Ss término` | |
| Información de un paquete remoto | `pacman -Si paquete` | |
| Instalar | `omarchy pkg add paquete` | ejecuta `sudo pacman -S --noconfirm --needed` (sin preguntar) |
| Quitar con dependencias sin uso | `sudo pacman -Rns paquete` · `omarchy pkg drop paquete` | `omarchy pkg drop` usa `-Rns --noconfirm`; `-n` borra también configs del paquete |
| Listar instalados / explícitos | `pacman -Q` / `pacman -Qe` | |
| Paquetes externos (AUR) | `pacman -Qm` | |
| Huérfanos | `pacman -Qdt` | `omarchy update orphan pkgs` los revisa |
| Detalles instalados | `pacman -Qi paquete` | fecha, tamaño, «requerido por» |
| Archivos de un paquete | `pacman -Ql paquete` | |
| Dueño de un archivo | `pacman -Qo /ruta` | |
| Qué paquete trae un archivo no instalado | `pacman -F nombre` | requiere `sudo pacman -Fy` una vez |
| Verificar archivos instalados | `pacman -Qkk paquete` | detecta archivos modificados |
| Actualizaciones pendientes sin tocar el sistema | `checkupdates` | de `pacman-contrib` |

## Actualizar el sistema

En Arch puro, `pacman -Syu` sincroniza las bases de datos y actualiza **todo** a la vez. Nunca hagas `pacman -Sy paquete` (actualización parcial): puede dejar bibliotecas incompatibles.

En **Omarchy** usa siempre:

```bash
omarchy update          # o menú → Update → Omarchy
```

que añade snapshot, keyrings, migraciones, hooks, AUR y mise (ver [[01 Omarchy/CLI configuración y mantenimiento#Actualizar el sistema]]). Un hook de pacman bloquea `pacman -Syu`/`yay -Syu` directos.

> [!tip] Antes de actualizar
> 1. Lee las [noticias de Arch](https://archlinux.org/news/): a veces piden intervención manual.
> 2. Cierra el trabajo importante; la actualización puede requerir reinicio.
> 3. Tras actualizar, revisa `.pacnew` (abajo) y `systemctl --failed`.

## Archivos `.pacnew` y `.pacsave`

Si modificaste un archivo de `/etc` y el paquete trae una versión nueva, pacman la guarda como `archivo.pacnew` sin sobrescribir la tuya. `.pacsave` es la copia de tu archivo al desinstalar.

```bash
pacdiff -o                  # listar .pacnew/.pacsave pendientes
sudo DIFFPROG="nvim -d" pacdiff   # revisar y fusionar uno a uno
```

## Caché de paquetes

```bash
du -sh /var/cache/pacman/pkg
omarchy update pkg prune    # poda versiones antiguas (Omarchy lo hace en cada update)
paccache -dk2               # simulación: qué borraría conservando 2 versiones
```

Las versiones antiguas en caché permiten **degradar** un paquete: `sudo pacman -U /var/cache/pacman/pkg/paquete-versión.pkg.tar.zst`. En Omarchy, prefiere restaurar un snapshot.

## AUR

AUR publica **PKGBUILD** de la comunidad: recetas que descargan y compilan en tu equipo. No son binarios verificados por Arch.

```bash
omarchy pkg aur add paquete     # instalar desde AUR
omarchy pkg aur install         # buscador interactivo
yay -Qua                        # AUR con actualización pendiente (consulta)
```

> [!warning] Revisa antes de instalar
> Lee el PKGBUILD (`yay -G paquete` lo descarga sin instalar), mira la fecha, los votos y los comentarios en [aur.archlinux.org](https://aur.archlinux.org/). Prefiere siempre el paquete del repositorio oficial si existe.

## Firmas y llaveros

Si pacman se queja de firmas («invalid or corrupted package», «unknown trust»), ejecuta `omarchy update keyring` y reintenta. No desactives la verificación en `pacman.conf`.

## Saber qué cambió

```bash
omarchy version pkgs                              # fecha de la última actualización
grep -E 'upgraded|installed|removed' /var/log/pacman.log | tail -30
journalctl -b -p warning
```

Fuentes: [pacman](https://wiki.archlinux.org/title/Pacman), [Pacman/Tips and tricks](https://wiki.archlinux.org/title/Pacman/Tips_and_tricks), [Pacnew and Pacsave](https://wiki.archlinux.org/title/Pacman/Pacnew_and_Pacsave), [System maintenance](https://wiki.archlinux.org/title/System_maintenance), [AUR](https://wiki.archlinux.org/title/Arch_User_Repository), [noticias de Arch](https://archlinux.org/news/), [Omarchy: actualizaciones](https://omarchy.org/manual/updates/), [Omarchy: otros paquetes](https://omarchy.org/manual/other-packages/).
