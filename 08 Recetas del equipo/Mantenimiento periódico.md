---
aliases: [Mantenimiento, Checklist de mantenimiento]
tags: [mantenimiento, recetas, checklist]
actualizado: 2026-09-27
verificado_en: "Omarchy 4.0.4-1"
---
# Mantenimiento periódico

Lista de comprobación basada en [ArchWiki: System maintenance](https://wiki.archlinux.org/title/System_maintenance) y el [flujo de actualización de Omarchy](https://omarchy.org/manual/updates/), adaptada a este equipo. Copia las casillas a una nota diaria si quieres llevar registro.

## Semanal

- [ ] Leer las [noticias de Arch](https://archlinux.org/news/) por si piden intervención manual.
- [ ] `omarchy update` (o esperar al aviso del widget de actualización en la barra).
- [ ] Revisar el final de la actualización: mensajes en amarillo/rojo, `omarchy update analyze logs`.
- [ ] Reiniciar si lo pide (kernel, systemd, controladores).
- [ ] `systemctl --failed` y `systemctl --user --failed` vacíos.

## Mensual

- [ ] Archivos `.pacnew`: `pacdiff -o`; fusionar con `sudo DIFFPROG="nvim -d" pacdiff`.
- [ ] Huérfanos: `pacman -Qdt` (o `omarchy update orphan pkgs`).
- [ ] Paquetes AUR: `pacman -Qm`; ¿siguen mantenidos? ¿hay ya versión oficial?
- [ ] Espacio: `df -h`, `sudo btrfs filesystem usage /`, `du -sh ~/.cache`.
- [ ] Snapshots: `sudo snapper -c root list`; snapper limpia solo, pero revisa que no crezcan sin control.
- [ ] Neovim: `:Lazy` → `U` (actualizar) y `:Mason` → `U`; comprobar `:checkhealth`.
- [ ] Plugins de la shell y temas de git: `omarchy plugin update`, `omarchy theme update`.
- [ ] Firmware: `omarchy update firmware`.
- [ ] Errores recurrentes: `journalctl -b -p err`.

## Trimestral

- [ ] **Probar una restauración** desde la copia de seguridad (Red Pill Backup): recuperar un archivo concreto.
- [ ] Revisar puertos abiertos: `sudo ufw status verbose` y `ss -tulpn`.
- [ ] Revisar servicios de usuario activos que ya no uses: `systemctl --user list-units --type=service`.
- [ ] Revisar claves SSH y `~/.ssh/authorized_keys` si activaste `sshd`.
- [ ] Salud del disco: `sudo smartctl -a /dev/nvme0n1` (instalar `smartmontools`).
- [ ] Actualizar este vault: comparar versiones y atajos (abajo) y apuntarlo en [[09 Fuentes y enlaces/Registro de revisión]].

## Tras una actualización mayor de Omarchy

```bash
omarchy version                                   # ¿cambió?
omarchy menu keybindings --print > /tmp/atajos-nuevos.txt
omarchy commands > /tmp/cli-nueva.txt
omarchy plugin list                               # ¿widgets nuevos que tu shell.json no incluye?
hyprctl configerrors                              # ¿sintaxis obsoleta en tus .lua?
ls /usr/share/omarchy/migrations | tail           # migraciones recientes
```

Compara con [[01 Omarchy/Referencia completa de atajos]] y [[01 Omarchy/Referencia de la CLI]], y actualiza las cabeceras `verificado_en` de las notas afectadas.

## Lo que **no** hay que hacer

- `pacman -Syu` / `yay -Syu` directos (se saltan snapshot y migraciones).
- `pacman -Sy paquete` (actualización parcial).
- Editar archivos de `/usr/share/omarchy/`.
- Borrar snapshots o la caché de pacman a mano sin saber qué hay dentro.
- Ignorar `.pacnew` durante meses.

Fuentes: [ArchWiki: System maintenance](https://wiki.archlinux.org/title/System_maintenance), [Pacnew and Pacsave](https://wiki.archlinux.org/title/Pacman/Pacnew_and_Pacsave), [Snapper](https://wiki.archlinux.org/title/Snapper), [Omarchy: actualizaciones](https://omarchy.org/manual/updates/).
