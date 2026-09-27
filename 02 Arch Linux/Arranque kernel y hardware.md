---
aliases: [Arranque, Kernel, Limine, initramfs, Drivers]
tags: [arch, arranque, kernel, hardware]
actualizado: 2026-09-27
verificado_en: "linux-omarchy 7.2.5 · Limine 12.8 · limine-mkinitcpio-hook · mesa 26.2 · AMD Ryzen 7 7730U"
---
# Arranque, kernel y hardware

Cómo arranca este equipo en detalle, qué kernel usa, cómo cambiar parámetros de arranque y cómo inspeccionar el hardware. Visión general: [[00 Fundamentos/Mapa del sistema]].

## La cadena de arranque

| Etapa | Componente | Dónde se configura |
|---|---|---|
| 1. Firmware | UEFI del portátil (menú con `F2`/`F12` en Lenovo) | BIOS/UEFI |
| 2. Cargador | **Limine** (`/boot/EFI/limine/`) con menú de kernels y **snapshots** | `/etc/default/limine` → genera `/boot/limine.conf` |
| 3. Kernel + initramfs | `linux-omarchy` y su imagen inicial | `/etc/mkinitcpio.conf` + `/etc/mkinitcpio.conf.d/*.conf` |
| 4. Descifrado | Plymouth pide la clave LUKS (hook `encrypt`) | `cryptdevice=` en la línea de órdenes del kernel |
| 5. Sistema | systemd monta Btrfs (`rootflags=subvol=@`) e inicia servicios | unidades systemd |
| 6. Sesión | SDDM → uwsm → Hyprland | [[01 Omarchy/Instalación seguridad y recuperación]] |

> [!info] En tu equipo
> Kernel en uso: **`7.2.5-3-omarchy`** del paquete `linux-omarchy` (Omarchy compila su propio kernel). También está instalado el kernel `linux` de Arch (7.2.3), que aparece en Limine como alternativa: es tu **plan B** si un kernel nuevo falla. Orden de arranque (`BOOT_ORDER`): `linux-omarchy`, variantes, el resto, *fallback*, *Snapshots*.

## Kernel

```bash
uname -r                           # kernel en ejecución
pacman -Q linux-omarchy linux      # kernels instalados
cat /proc/cmdline                  # parámetros con los que arrancó
journalctl -k -b -p warning        # avisos del kernel en este arranque
lsmod | less                       # módulos (controladores) cargados
modinfo amdgpu | head              # información de un módulo
```

Tras actualizar el kernel **hay que reiniciar** para usarlo; mientras tanto, algunos módulos nuevos (USB, sistemas de archivos) pueden no cargar. `omarchy update` avisa cuando conviene reiniciar.

**Microcódigo**: `amd-ucode` corrige errores del procesador; se carga en el initramfs (hook `microcode`).

## initramfs y mkinitcpio

El **initramfs** es un mini-sistema que el kernel carga primero para poder descifrar el disco y montar la raíz. Lo genera `mkinitcpio` con los *hooks* de `/etc/mkinitcpio.conf.d/omarchy_hooks.conf`:

```text
base udev plymouth keyboard autodetect microcode modconf kms keymap consolefont block encrypt filesystems fsck btrfs-overlayfs
```

> [!important] Regenerar en este equipo
> Usa **`sudo limine-mkinitcpio`** (o `sudo limine-update`), **no** `mkinitcpio -P`: el envoltorio `/usr/local/bin/mkinitcpio` avisa de que ese método **no actualiza las entradas de Limine**. Normalmente no hace falta hacerlo a mano: los hooks de pacman lo ejecutan al actualizar el kernel.

## Parámetros del kernel y Limine

Los parámetros se definen en **`/etc/default/limine`** (variable `KERNEL_CMDLINE[default]`), no en `/boot/limine.conf`, que se regenera.

```bash
sudoedit /etc/default/limine       # p. ej. añadir amdgpu.dcdebugmask=0x10 al final de la línea
sudo limine-update                 # regenerar entradas
```

Para probar un parámetro **una sola vez**: en el menú de Limine selecciona la entrada, pulsa `e`, añade el parámetro al final de la línea `cmdline` y arranca con `F10`. Si funciona, hazlo permanente.

Snapshots en el menú: los gestiona `limine-snapper-sync` a partir de snapper ([[01 Omarchy/Instalación seguridad y recuperación#Snapshots y reversión]]).

> [!warning] Cuidado con la línea de órdenes del kernel
> Contiene `cryptdevice=PARTUUID=…:root root=/dev/mapper/root rootflags=subvol=@ …`. Si la rompes, el equipo no encontrará el disco. Copia el archivo antes y ten a mano [[02 Arch Linux/Rescate desde USB]].

## Hardware de este equipo

| Componente | Controlador / paquete | Comprobar |
|---|---|---|
| GPU AMD Radeon (Barcelo) | `amdgpu` (kernel) + `mesa`, `vulkan-radeon` | `lspci -k \| grep -A3 VGA` · `glxinfo -B` (paquete `mesa-utils`) |
| Firmware de dispositivos | `linux-firmware` | `journalctl -k \| grep -i firmware` |
| Wi-Fi / Bluetooth | módulos del kernel + NetworkManager / bluez | `nmcli device`, `bluetoothctl show` |
| Audio | SOF/HDA + PipeWire | `wpctl status` |
| Energía | `power-profiles-daemon` | `powerprofilesctl`, `omarchy powerprofiles list` |
| Brillo | backlight | `brightnessctl`, `omarchy brightness display` |
| Temperaturas | lm_sensors | `sensors` |
| Resumen de todo | inxi | `inxi -Fxz` (el `z` oculta datos privados) |

> [!tip] Informes de hardware para pedir ayuda
> `inxi -Fxz` y `fastfetch` dan un resumen compartible sin números de serie. `lsusb` requiere el paquete `usbutils` (no instalado).

## Actualizar firmware (BIOS, SSD)

`omarchy update firmware` instala `fwupd` la primera vez y consulta LVFS. Lenovo publica muchas actualizaciones por ahí. Enchufa el cargador antes y no apagues durante el proceso.

## Suspensión e hibernación

- **Suspender** (RAM): menú `Super+Escape`. Si al volver algo falla (Wi-Fi, pantalla negra), mira `journalctl -b -1 -e` tras reiniciar.
- **Hibernar** (disco): requiere swap y parámetro `resume`; en este equipo ya está configurado (`resume=/dev/mapper/root resume_offset=…`, archivo `omarchy_resume.conf`). Gestiona con `omarchy hibernation available|setup|remove`.

Fuentes: [ArchWiki: Arch boot process](https://wiki.archlinux.org/title/Arch_boot_process), [Limine](https://wiki.archlinux.org/title/Limine), [Kernel parameters](https://wiki.archlinux.org/title/Kernel_parameters), [mkinitcpio](https://wiki.archlinux.org/title/Mkinitcpio), [Microcode](https://wiki.archlinux.org/title/Microcode), [AMDGPU](https://wiki.archlinux.org/title/AMDGPU), [Kernel module](https://wiki.archlinux.org/title/Kernel_module), [fwupd](https://wiki.archlinux.org/title/Fwupd), [Power management/Suspend and hibernate](https://wiki.archlinux.org/title/Power_management/Suspend_and_hibernate), [Omarchy: System sleep](https://omarchy.org/manual/system-sleep/).
