---
aliases: [Arch, Arch Linux]
tags: [arch, moc]
actualizado: 2026-09-27
verificado_en: "Arch Linux (rolling) bajo Omarchy 4.0.4-1"
---
# Arch Linux: la base de Omarchy

Arch es una distribución de **actualización continua** (*rolling release*) guiada por cinco principios: simplicidad (sin modificaciones innecesarias de upstream), modernidad, pragmatismo, centrada en el usuario y versátil. El kernel, las bibliotecas, systemd y las aplicaciones llegan como paquetes; Omarchy añade sus propios paquetes, defaults y migraciones encima.

| Concepto | Significado |
|---|---|
| **Paquete** | archivos instalados + metadatos + dependencias |
| **Repositorio** | origen firmado de paquetes binarios (`core`, `extra`, `multilib`, repos de Omarchy) |
| **AUR** | recetas comunitarias (PKGBUILD) que se compilan localmente |
| **Servicio** | proceso gestionado por systemd |
| **Configuración** | datos del sistema (`/etc`) o del usuario (`~/.config`) |

## Jerarquía de archivos esencial

| Ruta | Contenido |
|---|---|
| `/usr/bin`, `/usr/lib` | ejecutables y bibliotecas de paquetes (en Arch, `/bin` y `/lib` son enlaces a `/usr`) |
| `/etc` | configuración global del sistema |
| `/var/log/journal` | registros persistentes de systemd |
| `/var/cache/pacman/pkg` | caché de paquetes descargados |
| `/boot` | kernel, initramfs y bootloader (permisos restringidos) |
| `/home/castarrillo` (`~`) | tus archivos |
| `~/.config` · `~/.local/share` · `~/.cache` | configuración · datos · caché de aplicaciones (XDG) |
| `~/.local/bin` | ejecutables propios (en el PATH) |

Los permisos (`ls -l`) definen quién lee, escribe o ejecuta; `sudo` ejecuta una orden como administrador. Para editar un archivo de `/etc` sin abrir el editor como root, usa `sudoedit /etc/archivo`.

## Consultas que no modifican nada

```bash
uname -r                     # kernel en ejecución
cat /etc/os-release          # identificación de la distribución
pacman -Q omarchy hyprland   # versiones instaladas
pacman -Qi bash              # información del paquete
pacman -Ql bash | head       # archivos del paquete
pacman -Qo /usr/bin/nvim     # ¿a qué paquete pertenece?
df -h                        # espacio por sistema de archivos
lsblk -f                     # discos, particiones, LUKS, Btrfs
findmnt /                    # cómo está montada la raíz
systemctl --failed           # unidades con error
```

## Arch y Omarchy: quién manda

La ArchWiki es la referencia técnica general; en Omarchy hay tres diferencias importantes:

1. **Actualizaciones**: `omarchy update`, no `pacman -Syu` (ver [[02 Arch Linux/Paquetes y actualizaciones]]).
2. **Escritorio**: la configuración de Hyprland es Lua modular gestionada por Omarchy.
3. **Repositorios y espejos**: Omarchy fija sus propios espejos por canal (`omarchy refresh pacman` los restablece).

Continúa en [[02 Arch Linux/Paquetes y actualizaciones]], [[02 Arch Linux/Servicios red y diagnóstico]], [[02 Arch Linux/Arranque kernel y hardware]] y, para emergencias, [[02 Arch Linux/Rescate desde USB]]. Conceptos previos: [[00 Fundamentos/Conceptos de Linux]].

Fuentes: [ArchWiki (portada)](https://wiki.archlinux.org/title/Main_page), [ArchWiki en español](https://wiki.archlinux.org/title/Main_page_%28Espa%C3%B1ol%29), [Arch Linux](https://wiki.archlinux.org/title/Arch_Linux), [Arch boot process](https://wiki.archlinux.org/title/Arch_boot_process), [jerarquía de archivos](https://wiki.archlinux.org/title/Filesystem_Hierarchy_Standard), [XDG Base Directory](https://wiki.archlinux.org/title/XDG_Base_Directory), [General recommendations](https://wiki.archlinux.org/title/General_recommendations).
