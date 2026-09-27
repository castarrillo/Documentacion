---
aliases: [Seguridad, Recuperación, Snapshots, Copias de seguridad]
tags: [omarchy, seguridad, recuperacion, snapshots, backup]
actualizado: 2026-09-27
verificado_en: "Omarchy 4.0.4-1 · LUKS + Btrfs + snapper + Limine"
---
# Instalación, seguridad y recuperación de Omarchy

## Cómo llega el sistema al escritorio

1. El firmware **UEFI** carga **Limine** (menú de arranque y de snapshots).
2. Limine arranca el kernel; **Plymouth** pide la contraseña **LUKS** que descifra el disco.
3. **systemd** monta la raíz Btrfs e inicia servicios.
4. **SDDM** muestra el inicio de sesión; **uwsm** levanta la sesión Hyprland y Omarchy Shell.

La contraseña del disco (LUKS) y la del usuario (sesión, `sudo`) pueden ser distintas; cambia la del disco con `omarchy drive password`.

El instalador distingue **disco completo** (borra el disco elegido) y **espacio libre** para convivir con otro sistema. En *dual boot* con Windows, BitLocker y el orden de arranque requieren atención: sigue la [guía oficial](https://omarchy.org/manual/dual-boot-install/). `omarchy setup direct boot` crea una entrada EFI que salta Limine; entonces los snapshots sólo son accesibles eligiendo Limine desde el menú de arranque de la BIOS.

## Seguridad cotidiana

| Capa | Qué protege | Qué **no** protege |
|---|---|---|
| Cifrado LUKS | datos si pierdes o roban el equipo apagado | equipo encendido y desbloqueado |
| Bloqueo de pantalla | sesión abierta en tu ausencia | datos si arrancan desde otro medio (eso lo hace LUKS) |
| Cortafuegos ufw | conexiones entrantes no autorizadas | lo que tus apps envían hacia fuera |
| `sudo` | cambios al sistema | tus propios archivos: cualquier programa de tu usuario puede leerlos |
| Snapshots | la raíz del sistema tras una mala actualización | `/home`, documentos, `~/.config` |

- Activar **SSH** (`omarchy setup security sshd`) o **Sunshine** abre puertos: revisa `sudo ufw status verbose` después.

> [!info] En tu equipo
> El **servidor SSH está activo** (sólo acepta claves, no contraseñas) y tu usuario está en el grupo **docker** (equivale a root). Si no los necesitas: `omarchy remove security sshd` y `omarchy remove security sudoless docker`. Si el equipo no arranca, sigue [[02 Arch Linux/Rescate desde USB]].

- Plugins de la shell, extensiones del navegador y scripts de AUR se ejecutan con tus permisos: revisa su código.
- `omarchy setup security sudoless docker` y `omarchy sudo passwordless` reducen la seguridad; úsalos a sabiendas.
- Huella/FIDO2: [[01 Omarchy/Escritorio red y periféricos#Autenticación física]].

> [!tip] Si bloqueas tu cuenta tras varios intentos
> `Ctrl+Alt+F2` abre una TTY; entra como root y ejecuta `faillock --reset --user castarrillo`.

## Snapshots y reversión

Omarchy usa **snapper** sobre Btrfs y crea un snapshot de la raíz **antes de cada `omarchy update`**. Limine los lista en el menú de arranque con fecha y versión. Requiere Limine (predeterminado desde Omarchy 2.0); no existe con GRUB ni systemd-boot.

```bash
omarchy snapshot create        # snapshot manual antes de un cambio arriesgado
sudo snapper -c root list      # listar snapshots
omarchy snapshot restore       # restaurar (también desde la notificación al arrancar en un snapshot)
```

**Procedimiento tras una actualización fallida**

1. Reinicia y, en Limine, elige el snapshot anterior a la actualización.
2. Al arrancar aparece una notificación para **restaurar**; acéptala (o `omarchy snapshot restore`).
3. Reinicia de nuevo en la entrada normal.
4. Si alguna app se queja de su configuración, recuerda que `~/.config` **no** retrocedió: puede tener un formato más nuevo.

> [!important] Snapshots ≠ copias de seguridad
> En este equipo snapper sólo tiene configuración para `/`. `/home` queda fuera y el snapshot vive en el mismo disco: si el disco falla, se pierde todo.

## Recuperación razonada

1. Identifica la capa averiada: [[08 Recetas del equipo/Diagnóstico por síntomas]].
2. Si falla una app o servicio, lee su registro y reinícialo individualmente (`omarchy restart …`, `systemctl --user restart …`).
3. Wi-Fi, Bluetooth, audio o touchpad: *Update → Hardware* en el menú (`Super+Ctrl+H`) antes de reiniciar el equipo.
4. Si falló una actualización: snapshot (arriba) y `omarchy update analyze logs`.
5. Si dañaste un dotfile: restaura tu copia, o `omarchy refresh config <ruta>` (guarda respaldo del tuyo).
6. Último recurso: `omarchy reinstall configs` / `omarchy reinstall`, que **sobrescriben** configuraciones. Ver [[01 Omarchy/Referencia de la CLI#Restablecer (último recurso)]].

## Copias de seguridad

Respalda como mínimo, fuera del equipo:

```text
~/Documents/Documentacion      este vault
~/.config/hypr  ~/.config/omarchy  ~/.config/nvim
~/.config/tmux  ~/.config/herdr    ~/.bashrc  ~/.XCompose
~/.ssh  ~/.gnupg                   (cifrado; nunca en un repositorio público)
tus documentos, fotos y proyectos
```

> [!info] En tu equipo
> Está instalado **Red Pill Backup** (`Super+Alt+B`). Comprueba periódicamente que una restauración de prueba funciona: una copia no verificada no es una copia.

No publiques llaves privadas, contraseñas ni tokens en un repositorio de dotfiles.

Fuentes: [primeros pasos](https://omarchy.org/manual/getting-started/), [dual boot](https://omarchy.org/manual/dual-boot-install/), [seguridad](https://omarchy.org/manual/security/), [snapshots](https://omarchy.org/manual/system-snapshots/), [solución de problemas](https://omarchy.org/manual/troubleshooting/), [ArchWiki: Snapper](https://wiki.archlinux.org/title/Snapper), [ArchWiki: dm-crypt](https://wiki.archlinux.org/title/Dm-crypt), [ArchWiki: ufw](https://wiki.archlinux.org/title/Uncomplicated_Firewall).
