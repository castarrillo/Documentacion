---
aliases: [Recetario, How-to, Tareas comunes]
tags: [recetas, principiantes, tutorial]
actualizado: 2026-09-27
verificado_en: "Omarchy 4.0.4-1 · configuración real de este equipo"
---
# Recetario de tareas comunes

Instrucciones paso a paso para lo que un usuario nuevo necesita hacer tarde o temprano. Cada receta indica si modifica el sistema (⚙️) o sólo tu usuario.

## Instalar y quitar software

| Tienes… | Haz |
|---|---|
| Una app conocida (repos oficiales) | menú *Install → Package* o `omarchy pkg add nombre` ⚙️ |
| Un navegador, editor, terminal, servicio o entorno de desarrollo soportado | menú *Install* (`omarchy install browser firefox`, `omarchy install dev-env python`…) ⚙️ |
| Una web que usas como app | `omarchy webapp install` (asistente) |
| Una TUI (programa de terminal) con lanzador | `omarchy tui install` |
| Sólo en AUR | `omarchy pkg aur add nombre` ⚙️ — **revisa el PKGBUILD** antes ([[02 Arch Linux/Paquetes y actualizaciones#AUR]]) |
| Una herramienta de desarrollo en versión concreta | `mise use -g node@22` o en el proyecto `mise use python@3.13` |
| Quitar | menú *Remove*, `omarchy pkg drop nombre`, `omarchy webapp remove` |

Buscar antes: `pacman -Ss palabra` (repos) · `yay -Ss palabra` (incluye AUR).

## SSH

### Crear y usar una clave

```bash
ssh-keygen -t ed25519 -C "castarrillo@portatil"      # Enter a todo; pon frase de paso
cat ~/.ssh/id_ed25519.pub                             # esta es la parte pública: cópiala al servidor/GitHub
ssh-copy-id usuario@servidor                          # instalarla en un servidor
ssh usuario@servidor
```

`~/.ssh/config` evita recordar direcciones y claves:

```sshconfig
Host mi-vps
  HostName 203.0.113.10
  User deploy
  IdentityFile ~/.ssh/id_ed25519
```

Después: `ssh mi-vps`, `scp archivo mi-vps:/tmp/`, `rsync -avh carpeta/ mi-vps:copia/`.

> [!info] En tu equipo
> Claves: `id_ed25519`, `id_rsa_github`, `id_rsa_azure`; `~/.ssh/config` tiene entradas para `github.com` y `ssh.dev.azure.com`. El **servidor SSH está activo** en este portátil con **sólo acceso por clave** (sin contraseña). Si no lo necesitas, desactívalo: `omarchy remove security sshd`. Revisa quién puede entrar en `~/.ssh/authorized_keys`.

Permisos obligatorios: `chmod 700 ~/.ssh; chmod 600 ~/.ssh/id_* ~/.ssh/config`.

## Git: primera configuración

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@correo"
gh auth login                      # GitHub CLI (instalado): autenticación y clonar repos privados
git clone git@github.com:usuario/repo.git
```

Tu configuración global ya trae buenos valores (rama inicial `master`, `pull.rebase`, `push.autoSetupRemote`, `rerere`, alias `co`, `br`, `ci`, `st`). Uso diario: `g st`, `gcm "mensaje"`, `lazygit` (`Super+Shift+D` es Docker; lazygit desde la terminal o `Espacio g g` en Neovim). Ramas en paralelo: `ga rama` / `gd`.

## Memorias USB y discos

```bash
lsblk -f                                  # identificar la USB (p. ej. /dev/sda1)
udisksctl mount -b /dev/sda1              # montar (aparece en /run/media/castarrillo/…)
udisksctl unmount -b /dev/sda1            # desmontar antes de retirar
format-drive /dev/sda "USB"               # ⚠️ formatear TODA la memoria en exFAT (Omarchy)
iso2sd imagen.iso                         # ⚠️ crear USB arrancable
```

Nautilus (`Super+Shift+F`) monta y expulsa con clic; `udiskie` monta automáticamente las memorias al conectarlas.

## Compartir archivos

| Con quién | Cómo |
|---|---|
| Móvil u otro equipo en la misma red | **LocalSend**: `Super+Ctrl+S` o `omarchy share file ruta` |
| Tus equipos en Tailscale | `omarchy tailscale send <equipo> archivo` (recepción automática aquí) |
| Un servidor | `scp` / `rsync` (arriba) |
| Portapapeles hacia el móvil | `omarchy share clipboard` |

## Redes privadas (VPN)

| VPN | Uso en este equipo |
|---|---|
| **Tailscale** (instalado) | `tailscale status`, `tailscale up`; panel en la barra |
| **FortiClient / Fortinet** | alias `f` = `sudo openfortivpn` (lee `/etc/openfortivpn/config`) ⚙️ |
| WireGuard / OpenVPN | importa el perfil en NetworkManager: `nmcli connection import type wireguard file perfil.conf` |

## Impresoras y escáneres

CUPS está activo. Lanzador → *Print Settings* → *Add*. Para impresoras de red usa IPP (`ipp://IP/ipp/print`). Estado: `lpstat -p`. Guía: [FAQ de Omarchy](https://omarchy.org/manual/faq/).

## Capturas, grabación, OCR y dictado

| Tarea | Atajo |
|---|---|
| Captura de región/ventana/pantalla | `ImprPant` |
| Grabar pantalla (con audio: menú de captura) | `Alt+ImprPant` · `Super+Ctrl+C` |
| Copiar el texto de una imagen o PDF escaneado | `Super+Ctrl+ImprPant` (OCR) |
| Leer un QR de la pantalla | `omarchy capture qr` |
| Dictado por voz | mantener `F9` · alternar `Super+Ctrl+X` (Voxtype, activo) |
| Color de un píxel | `Super+ImprPant` |

## Aplicaciones predeterminadas

```bash
omarchy default browser firefox      # chromium|chrome|brave|firefox|zen…
omarchy default terminal kitty       # alacritty|foot|ghostty|kitty
omarchy default editor nvim          # code|zed|helix|nvim…
omarchy default agent claude         # agente de IA por defecto (Super+Shift+Ctrl+A)
xdg-mime default org.gnome.Evince.desktop application/pdf   # app para un tipo de archivo
```

## Docker

Tu usuario está en el grupo `docker` y el servicio arranca bajo demanda (socket). Básico:

```bash
d ps                                   # contenedores en marcha (alias d = docker)
d run --rm -it python:3.13 python      # probar algo sin instalarlo
omarchy install docker dbs             # PostgreSQL/MySQL/Redis… listos para desarrollo
lazydocker                             # interfaz TUI (Super+Shift+D)
d system df; d system prune            # espacio usado / limpiar (⚠️ borra lo no usado)
```

## Proyectos Python modernos

```bash
uv init mi-app && cd mi-app            # proyecto con pyproject.toml
uv add requests                        # dependencias en .venv del proyecto
uv run main.py
```

En Neovim selecciona el entorno con `Espacio c v` ([[05 Neovim/IDE - Lenguajes y extras#Python]]).

## Copias de seguridad

- **Red Pill Backup**: `Super+Alt+B` (TUI); recordatorio al iniciar sesión.
- Configuraciones: script de [[04 Bash/Ruta de cero a pro#Nivel 3 · Scripts robustos]].
- Qué respaldar: [[01 Omarchy/Instalación seguridad y recuperación#Copias de seguridad]].
- Probar la restauración cada trimestre ([[08 Recetas del equipo/Mantenimiento periódico]]).

## Energía y portátil

| Tarea | Cómo |
|---|---|
| Perfil ahorro / rendimiento | `Super+Ctrl+P` · `omarchy powerprofiles set battery power-saver` |
| Ver batería restante | `Super+Ctrl+Alt+B` · `omarchy battery status` |
| No suspender durante una presentación | `omarchy toggle idle stay-awake` |
| Pantalla del portátil apagada con monitor externo | `Super+Ctrl+Supr` |
| Proyectar/duplicar pantalla | `Super+Ctrl+Alt+Supr` (espejo) |

## Máquina Windows

`omarchy windows vm install` crea una VM de Windows con carpetas compartidas; `launch`, `stop`, `status`, `remove` la gestionan. Ver el [capítulo oficial](https://omarchy.org/manual/windows-vm/).

Fuentes: [Omarchy: FAQ](https://omarchy.org/manual/faq/), [Other Packages](https://omarchy.org/manual/other-packages/), [Web Apps](https://omarchy.org/manual/web-apps/), [Screenshots & Recording](https://omarchy.org/manual/screenshots-recording/), [Text Extraction & Dictation](https://omarchy.org/manual/text-extraction-dictation/), [Development Tools](https://omarchy.org/manual/development-tools/), [Networking](https://omarchy.org/manual/networking/), [ArchWiki: SSH keys](https://wiki.archlinux.org/title/SSH_keys), [OpenSSH](https://wiki.archlinux.org/title/OpenSSH), [udisks](https://wiki.archlinux.org/title/Udisks), [CUPS](https://wiki.archlinux.org/title/CUPS), [uv](https://docs.astral.sh/uv/).
