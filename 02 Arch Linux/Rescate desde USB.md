---
aliases: [Rescate, chroot, Recuperación avanzada, No arranca]
tags: [arch, recuperacion, emergencia]
actualizado: 2026-09-27
verificado_en: "LUKS (nvme0n1p2) + Btrfs (@, @home, @log, @pkg) + Limine en /boot"
---
# Rescate: cuando el equipo no arranca

Escalera de recuperación, de lo más sencillo a lo más drástico. **Lee esta nota antes de necesitarla** e imprímela o guárdala en el móvil: si el equipo no arranca, no podrás abrir el vault en él.

## Nivel 1 · El escritorio falla, pero el sistema arranca

| Síntoma | Qué hacer |
|---|---|
| Pantalla negra o sin barra tras iniciar sesión | `Ctrl+Alt+F2` → TTY de texto; entra con tu usuario |
| Hyprland no arranca por un error en tus `.lua` | en la TTY: `mv ~/.config/hypr/bindings.lua{,.roto}` (o el archivo que tocaste) y reinicia con `sudo reboot`; último recurso `omarchy refresh hyprland` |
| Cuenta bloqueada por intentos fallidos | en la TTY como root: `faillock --reset --user castarrillo` |
| La shell/barra no aparece | `omarchy restart shell` desde una terminal o la TTY |
| Volver a la sesión gráfica desde la TTY | `Ctrl+Alt+F1` (o la consola donde esté SDDM) |

## Nivel 2 · Falló una actualización: usa un snapshot

1. Reinicia; en **Limine** entra en *Snapshots* y elige el anterior a la actualización.
2. Si arranca, acepta la notificación de **restaurar** (o `omarchy snapshot restore`).
3. Reinicia normalmente. El registro `/tmp/omarchy-update.log` se habrá borrado al reiniciar; para ver qué falló usa `grep -iE 'error|warning' /var/log/pacman.log | tail -30`.

Detalle: [[01 Omarchy/Instalación seguridad y recuperación#Snapshots y reversión]].

## Nivel 3 · Kernel nuevo roto

En Limine elige la entrada del **kernel `linux` de Arch** (o la *fallback*). Si arranca, el problema es del kernel: espera a la siguiente actualización o pregunta en la comunidad con `journalctl -b -1 -k`.

## Nivel 4 · Nada arranca: rescate desde una memoria USB

Necesitas una USB con la **ISO de Arch** (o la de Omarchy). Prepárala **hoy**, desde este equipo:

```bash
# Descarga la ISO desde https://archlinux.org/download/ y verifica su firma
lsblk                         # identifica la USB (p. ej. /dev/sda) — ¡con cuidado!
iso2sd ~/Downloads/archlinux-*.iso   # función de Omarchy: si no indicas el dispositivo, te deja elegir entre /dev/sd*
```

### Pasos (desde el sistema en vivo)

```bash
# 0. Teclado latinoamericano y red
loadkeys la-latin1
iwctl                               # station wlan0 connect "MiWiFi"  (si hace falta red)

# 1. Descifrar el disco (pedirá la contraseña LUKS)
lsblk -f                            # la partición cifrada es nvme0n1p2 (crypto_LUKS)
cryptsetup open /dev/nvme0n1p2 root

# 2. Montar los subvolúmenes Btrfs como en el sistema real
mount -o subvol=@ /dev/mapper/root /mnt
mount -o subvol=@home /dev/mapper/root /mnt/home
mount -o subvol=@log  /dev/mapper/root /mnt/var/log
mount -o subvol=@pkg  /dev/mapper/root /mnt/var/cache/pacman/pkg
mount /dev/nvme0n1p1 /mnt/boot      # partición EFI de 2 GB (vfat); compruébalo con lsblk -f

# 3. Entrar al sistema instalado
arch-chroot /mnt
```

### Qué puedes hacer dentro del chroot

| Problema | Solución |
|---|---|
| Actualización interrumpida | `pacman -Syu` (aquí sí: estás reparando) y después `limine-mkinitcpio` |
| initramfs o entradas de Limine dañadas | `limine-mkinitcpio` / `limine-update` |
| Olvidaste la contraseña de usuario | `passwd castarrillo` |
| Rompiste un archivo de `/etc` | restáuralo desde `.pacnew`/copia o reinstala su paquete: `pacman -S paquete` |
| Recuperar datos | copia `/home/castarrillo/…` a otra USB antes de nada |
| Volver a un snapshot a mano | `snapper -c root list` y restaura con `snapper` según la [ArchWiki](https://wiki.archlinux.org/title/Snapper) |

Salir: `exit`, `umount -R /mnt`, `cryptsetup close root`, `reboot`.

> [!warning] Antes de ejecutar nada
> Verifica **dos veces** los nombres de dispositivo con `lsblk -f`: en la USB en vivo pueden cambiar (`nvme0n1`, `sda`…). Un `mkfs` o `dd` al dispositivo equivocado borra datos sin posibilidad de deshacer.

## Nivel 5 · Reinstalar

Si nada funciona: copia tus datos (Nivel 4, apartado «Recuperar datos») y reinstala con la ISO de Omarchy. Con copias de `~/.config` y del vault ([[01 Omarchy/Instalación seguridad y recuperación#Copias de seguridad]]) recuperas tu entorno en una hora. `omarchy setup factory reset` restablece el sistema sin reinstalar desde USB, pero **borra tu configuración**.

## Prepárate ahora

- [ ] USB de rescate creada y probada (arranca en el menú `F12`).
- [ ] Contraseña LUKS guardada en un gestor de contraseñas.
- [ ] Esta nota exportada a PDF o en el móvil.
- [ ] Copia reciente de `/home` fuera del equipo.

Fuentes: [ArchWiki: General troubleshooting](https://wiki.archlinux.org/title/General_troubleshooting), [chroot](https://wiki.archlinux.org/title/Chroot), [dm-crypt/Device encryption](https://wiki.archlinux.org/title/Dm-crypt/Device_encryption), [Btrfs](https://wiki.archlinux.org/title/Btrfs), [USB flash installation medium](https://wiki.archlinux.org/title/USB_flash_installation_medium), [iwd](https://wiki.archlinux.org/title/Iwd), [Omarchy: Troubleshooting](https://omarchy.org/manual/troubleshooting/), [Omarchy: System snapshots](https://omarchy.org/manual/system-snapshots/).
