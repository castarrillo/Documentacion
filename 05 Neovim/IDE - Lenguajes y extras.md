---
aliases: [LazyExtras, Extras LazyVim, Lenguajes Neovim]
tags: [neovim, lazyvim, ide, lenguajes]
actualizado: 2026-09-27
verificado_en: "LazyVim (extras en ~/.local/share/nvim/lazy/LazyVim/lua/lazyvim/plugins/extras/) · mise: node 24, python 3.14"
---
# IDE: lenguajes y extras de LazyVim

Un **extra** es un módulo opcional de LazyVim que instala y configura de una vez todo lo que necesita un lenguaje o una función: servidor LSP, analizador Treesitter, formateador, linter, depurador y adaptador de pruebas. Es la forma más rápida y segura de convertir Neovim en el IDE de tu stack.

## Activarlos

1. `:LazyExtras` abre la lista. Los marcados con ★ son **recomendados** para los archivos que tienes abiertos.
2. Coloca el cursor sobre uno y pulsa `x` para activarlo o desactivarlo.
3. Reinicia Neovim (`:restart`). lazy.nvim descarga los plugins y Mason instala las herramientas (mira `:Mason`).
4. Queda guardado en `~/.config/nvim/lazyvim.json` (versiónalo).

> [!info] En tu equipo
> Sólo está activo `editor.neo-tree`. Tu entorno (mise) incluye **Node 24** y **Python 3.14**, y trabajas con Bash, Lua (configs de Omarchy/Hyprland), Markdown (este vault), JSON/YAML/TOML y Docker. La tabla siguiente es la selección recomendada.

## Selección recomendada para este equipo

| Extra | Aporta | Herramientas que instala |
|---|---|---|
| `lang.python` | LSP, formato, lint, entornos virtuales, depurador, pruebas | pyright, ruff, debugpy, venv-selector, neotest-python |
| `lang.typescript` | JS/TS/React | vtsls, js-debug-adapter |
| `lang.json` | validación con esquemas (package.json, tsconfig…) | jsonls + SchemaStore |
| `lang.yaml` | YAML con esquemas (GitHub Actions, docker-compose…) | yamlls + SchemaStore |
| `lang.toml` | TOML (`pyproject.toml`, `mise.toml`) | taplo |
| `lang.markdown` | vista previa, *lint*, índice, renderizado en el editor | marksman, markdownlint-cli2, markdown-toc, prettier, render-markdown |
| `lang.docker` | Dockerfile y compose | dockerls, docker_compose_language_service, hadolint |
| `lang.git` | resaltado de mensajes de commit y rebase | — |
| `util.dot` | **Bash y dotfiles** (Hyprland, Waybar, Kitty, Omarchy) | bashls, shellcheck |
| `dap.core` | depuración genérica (DAP) + interfaz | nvim-dap, nvim-dap-ui |
| `test.core` | ejecutar pruebas desde el editor | neotest |
| `formatting.prettier` | Prettier para web, JSON, YAML, Markdown | prettier |
| `linting.eslint` | ESLint en proyectos JS/TS | eslint-lsp |
| `coding.mini-surround` | añadir/cambiar/borrar envoltorios (`gsa`, `gsd`, `gsr`) | — |
| `editor.harpoon2` (opcional) | saltar entre 4–5 archivos fijos | — |
| `ui.treesitter-context` (opcional) | muestra la función/clase actual fija arriba | — |

Otros disponibles si los necesitas: `lang.go`, `lang.rust`, `lang.sql` (cliente de bases de datos dadbod, `Espacio D`), `lang.java`, `lang.php`, `lang.ruby`, `lang.tailwind`, `lang.vue`, `lang.svelte`, `lang.clangd`, `lang.tex`, `lang.terraform`, `util.rest` (cliente HTTP), `util.gh` / `util.octo` (GitHub).

> [!tip] Menos es más
> Activa sólo los lenguajes que usas de verdad. Cada extra añade plugins, tiempo de arranque y cosas que actualizar.

## Detalle por lenguaje

### Python

- Servidor: **pyright** (alternativa: `vim.g.lazyvim_python_lsp = "basedpyright"` en `options.lua`); **ruff** para *lint* y formato (`ruff_format`).
- **Entorno virtual**: `Espacio c v` abre *venv-selector* para elegir `.venv`, entornos de mise, etc. Sin esto, pyright no verá tus dependencias.
- Depurar: `Espacio d P t` (método) / `Espacio d P c` (clase) con debugpy.
- Pruebas: pytest vía neotest (`Espacio t r`).

> [!warning] Python de mise frente al del sistema
> El `python3` del `PATH` es el de **mise** (3.14). Crea un `.venv` por proyecto (`python -m venv .venv` o `uv venv`) y selecciónalo con `Espacio c v`, o añade `pyrightconfig.json` con `{ "venvPath": ".", "venv": ".venv" }`.

### TypeScript / JavaScript

- Servidor **vtsls** (completo, con acciones de «mover a archivo», «organizar imports» `Espacio c o`, «ir a la definición de la fuente» `gD`, «archivos que referencian» `gR`, añadir imports que faltan `Espacio c M`, corregir todos los diagnósticos `Espacio c D`, versión de TypeScript `Espacio c V`).
- Formato: Prettier (extra `formatting.prettier`) o Biome (`lang.typescript.biome`). *Lint*: `linting.eslint`.
- Depurar Node o navegador con js-debug-adapter (`Espacio d c`).

### Bash y dotfiles (`util.dot`)

Activa **bashls** + **shellcheck** (los avisos de ShellCheck aparecen al escribir) y reconoce tipos de archivo de dotfiles (Waybar, Kitty, rofi…). Formato con `shfmt`, ya instalado. Ideal para scripts de `~/.local/bin` y hooks de Omarchy.

### Lua (ya incluido)

LazyVim trae **lua_ls** y **stylua** (instalados) con *lazydev.nvim*, que entiende la API `vim.*`: completado y documentación al escribir tu configuración o módulos de Hyprland. Para los `.lua` de Hyprland (`hl.*`, `o.*`) el LSP no conoce esas globales; añade un `.luarc.json` en `~/.config/hypr` con `{ "diagnostics": { "globals": ["hl", "o"] } }` para evitar avisos.

### Markdown (para este vault)

`Espacio c p` abre la vista previa en el navegador; *render-markdown* dibuja encabezados, tablas y casillas dentro del editor (`Espacio u m` lo alterna); markdownlint señala problemas de estilo. Para Obsidian existe además el plugin comunitario *obsidian.nvim* (no incluido en LazyVim).

## IA en el editor (opcional)

| Extra | Qué hace | Atajos |
|---|---|---|
| `ai.claudecode` | integra **Claude Code** (lo tienes instalado): panel lateral, enviar selección o buffer, aceptar/rechazar diffs | `Espacio a c` alternar, `Espacio a s` enviar selección, `Espacio a b` añadir buffer, `Espacio a a` / `Espacio a d` aceptar / rechazar diff |
| `ai.sidekick` | panel para varios agentes de terminal (Claude, Codex, opencode…) + sugerencias «next edit» de Copilot | `Espacio a a`, `Espacio a s`, `Espacio a t`… |
| `ai.copilot` | completado en línea de GitHub Copilot | `Tab` acepta |

Alternativa sin extras: agentes en un pane de Herdr/tmux junto al editor ([[07 Herdr/Workspaces y agentes]]), que es el flujo de `hdl`/`tdl`.

## Comprobar que un lenguaje está bien

Abre un archivo del lenguaje y verifica:

- [ ] `:set ft?` muestra el tipo correcto.
- [ ] `Espacio c l` lista el servidor adjunto.
- [ ] `K` muestra documentación y `gd` salta a la definición.
- [ ] Un error deliberado aparece como diagnóstico.
- [ ] `Espacio c f` formatea; `:ConformInfo` muestra el formateador.
- [ ] `:InspectTree` muestra el árbol de sintaxis (Treesitter activo).
- [ ] (si aplica) `Espacio t r` ejecuta la prueba más cercana.

Fuentes: [LazyVim extras](https://www.lazyvim.org/extras), [lang.python](https://www.lazyvim.org/extras/lang/python), [lang.typescript](https://www.lazyvim.org/extras/lang/typescript), [lang.markdown](https://www.lazyvim.org/extras/lang/markdown), [util.dot](https://www.lazyvim.org/extras/util/dot), [ai.claudecode](https://www.lazyvim.org/extras/ai/claudecode), código local de los extras.
