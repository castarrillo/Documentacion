---
aliases: [Git en Neovim, gitsigns, lazygit Neovim]
tags: [neovim, lazyvim, ide, git]
actualizado: 2026-09-27
verificado_en: "LazyVim (gitsigns.nvim, snacks lazygit/picker) · Neovim 0.12.5 (:help diff)"
---
# IDE: Git dentro de Neovim

Tres niveles, de menor a mayor: **gitsigns** (cambios en el margen, por *hunk*), **lazygit** (interfaz completa en ventana flotante) y el **modo diff** nativo de Neovim para resolver conflictos.

## Cambios en el archivo actual (gitsigns)

El margen izquierdo marca líneas añadidas, modificadas y borradas respecto al índice. Un **hunk** es un bloque de cambios contiguos.

| Tecla | Acción |
|---|---|
| `]h` / `[h` | hunk siguiente / anterior |
| `]H` / `[H` | último / primer hunk |
| `Espacio g h p` | vista previa del hunk en línea |
| `Espacio g h s` | **preparar** (*stage*) el hunk (también sobre una selección) |
| `Espacio g h r` | **descartar** el hunk (vuelve a la versión del índice) |
| `Espacio g h S` / `Espacio g h R` | preparar / descartar todo el archivo |
| `Espacio g h u` | deshacer el último *stage* |
| `Espacio g h b` / `Espacio g h B` | *blame* de la línea (completo) / de todo el buffer |
| `Espacio g h d` / `Espacio g h D` | diff del archivo contra el índice / contra el commit anterior |
| `ih` (objeto) | seleccionar el hunk: `vih`, `dih` |
| `Espacio u G` | mostrar/ocultar los signos de git |

## Git del proyecto (picker)

| Tecla | Acción |
|---|---|
| `Espacio g s` | estado (archivos modificados; `Tab` prepara) |
| `Espacio g d` / `Espacio g D` | diff por hunks / contra `origin` |
| `Espacio g l` / `Espacio g L` | log (raíz / cwd) |
| `Espacio g f` | historial del archivo actual |
| `Espacio g b` | *blame* de la línea |
| `Espacio g S` | *stashes* |
| `Espacio g B` / `Espacio g Y` | abrir la línea en GitHub / copiar su URL |
| `Espacio g i` / `Espacio g p` | issues / pull requests abiertos de GitHub (requiere `gh`, instalado aquí) |

## lazygit (`Espacio g g`)

Interfaz completa de Git en una ventana flotante (en la raíz del repositorio; `Espacio g G` en el cwd). Lo esencial:

| Tecla | Acción | Tecla | Acción |
|---|---|---|---|
| `1`–`5` | paneles: estado, archivos, ramas, commits, stash | `?` | ayuda del panel |
| `Espacio` | preparar / quitar archivo | `a` | preparar todo |
| `Enter` | entrar (ver hunks, preparar líneas sueltas) | `c` | commit |
| `A` | *amend* del último commit | `P` / `p` | *push* / *pull* |
| `n` (ramas) | nueva rama | `Espacio` (ramas) | cambiar de rama |
| `r` (commits) | reescribir mensaje | `s` (commits) | *squash* |
| `e` (archivo) | abrir en el editor | `q` | salir |

Al salir de lazygit vuelves a Neovim en el mismo punto. El tema de Omarchy también se aplica a lazygit.

## Resolver conflictos de fusión

Tras un `git merge` o `git rebase` con conflictos, el archivo contiene marcas `<<<<<<<`, `=======`, `>>>>>>>`.

- **Rápido**: en lazygit, panel de archivos → `Enter` sobre el archivo en conflicto → `Espacio` elige el bloque (`b` = ambos), `←/→` cambia de lado.
- **Manual en Neovim**: busca `/<<<<<<<`, edita dejando el resultado correcto y borra las marcas; después `Espacio g h s` o `git add`.
- **Modo diff nativo** (`:help diff`): con dos o más ventanas en diff (`nvim -d archivo1 archivo2`, o `:diffthis` en cada una):

| Tecla / orden | Acción |
|---|---|
| `]c` / `[c` | diferencia siguiente / anterior |
| `do` | *diff obtain*: traer el cambio de la otra ventana |
| `dp` | *diff put*: llevar el cambio a la otra ventana |
| `:diffupdate` | recalcular |
| `:diffoff!` | salir del modo diff |

Configura Neovim como herramienta de conflictos de git: `git config --global merge.tool nvimdiff` y usa `git mergetool`.

## Flujo diario recomendado

1. Edita; revisa cambios con `]h` + `Espacio g h p`.
2. Descarta lo que no quieras con `Espacio g h r`; prepara con `Espacio g h s`.
3. `Espacio g g` → revisa → `c` para el commit → `P` para *push*.
4. Para ramas aisladas usa worktrees de Omarchy (`ga rama` / `gd`, ver [[04 Bash/Bash#Aliases y funciones de Omarchy]]) y abre Neovim en cada una.

Fuentes: [diff](https://neovim.io/doc/user/diff/), [LazyVim keymaps: git](https://www.lazyvim.org/keymaps), [gitsigns.nvim](https://github.com/lewis6991/gitsigns.nvim), [lazygit](https://github.com/jesseduffield/lazygit/blob/master/docs/keybindings/Keybindings_en.md), [snacks.nvim lazygit](https://github.com/folke/snacks.nvim/blob/main/docs/lazygit.md).
