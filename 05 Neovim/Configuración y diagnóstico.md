---
aliases: [Configurar Neovim, checkhealth]
tags: [neovim, lazyvim, configuracion, diagnostico]
actualizado: 2026-09-27
verificado_en: "Neovim 0.12.5 · LazyVim"
---
# Configuración y diagnóstico de Neovim

## Estructura de archivos

```text
~/.config/nvim/
├── init.lua                     punto de entrada (carga config.lazy)
├── lazyvim.json                 extras activados y versión de LazyVim
├── lazy-lock.json               versiones fijadas de cada plugin
└── lua/
    ├── config/
    │   ├── lazy.lua             arranque de lazy.nvim e import de LazyVim
    │   ├── options.lua          opciones (vim.opt…) — se carga antes que plugins
    │   ├── keymaps.lua          atajos propios
    │   ├── autocmds.lua         autocomandos
    │   └── remote_clipboard.lua (propio de este equipo)
    └── plugins/                 un archivo = una o más especificaciones de plugin
        ├── theme.lua, all-themes.lua, omarchy-theme-hotreload.lua   (Omarchy)
        └── example.lua, disable-news-alert.lua, …
~/.local/share/nvim/lazy/        código de plugins descargados
~/.local/share/nvim/mason/       herramientas instaladas por Mason
~/.local/state/nvim/             registros, historial, sesiones
```

Qué hace cada pieza: **lazy.nvim** instala y carga plugins; **LazyVim** aporta la configuración base; **Mason** instala servidores LSP, formateadores y linters; **nvim-lspconfig** conecta los servidores; **Treesitter** analiza la sintaxis; **conform.nvim** formatea; **nvim-lint** ejecuta linters; **blink.cmp** autocompleta.

## Personalizar sin romper

Guía completa de Lua, autocomandos, órdenes y especificaciones de plugins: [[05 Neovim/Configuración Lua a fondo]].

**Opción**: en `lua/config/options.lua`

```lua
vim.opt.relativenumber = false
vim.g.autoformat = false
```

**Atajo**: en `lua/config/keymaps.lua`

```lua
vim.keymap.set("n", "<leader>ya", "ggVGy", { desc = "Copiar todo el archivo" })
vim.keymap.del("n", "<leader>l")   -- quitar un atajo de LazyVim
```

**Plugin nuevo o cambiar uno existente**: un archivo en `lua/plugins/`

```lua
-- lua/plugins/mis-plugins.lua
return {
  -- Añadir un plugin
  { "folke/zen-mode.nvim", cmd = "ZenMode" },
  -- Modificar opciones de uno que LazyVim ya trae
  { "folke/snacks.nvim", opts = { dashboard = { enabled = false } } },
  -- Desactivar uno
  { "folke/flash.nvim", enabled = false },
}
```

**Lenguajes y funciones opcionales**: `:LazyExtras` (marca con `x`), p. ej. `lang.python`, `lang.typescript`, `coding.mini-surround`. Se guardan en `lazyvim.json`.

## Mantener plugins

| Orden | Uso |
|---|---|
| `:Lazy` | panel: `U` actualizar, `S` sincronizar, `C` comprobar, `X` limpiar, `L` registro |
| `:Lazy restore` | volver a las versiones de `lazy-lock.json` (útil si una actualización rompe algo) |
| `:Mason` | instalar/actualizar herramientas (`i`, `u`, `U`, `X`) |
| `:LazyExtras` | activar/desactivar extras |
| `:TSUpdate` | actualizar analizadores Treesitter |

> [!tip] Versiona tu configuración
> Guarda `~/.config/nvim` en git (incluido `lazy-lock.json`): si una actualización de plugins falla, `git checkout lazy-lock.json` + `:Lazy restore` te devuelve al estado anterior.

## Diagnóstico

| Síntoma | Comprobación |
|---|---|
| Algo va mal en general | `:checkhealth` (o `:checkhealth lazy`, `:checkhealth vim.lsp`) |
| Error de un plugin al arrancar | `:Lazy` (pestaña de errores), `:messages`, `<leader>n` |
| `gd`/`K` no funcionan | `<leader>cl` (Lsp Info) o `:checkhealth vim.lsp`: ¿hay servidor para ese tipo de archivo? Instálalo con `:Mason` o el extra del lenguaje |
| No formatea | `:ConformInfo`; recuerda que aquí el autoformato está desactivado (`<leader>cf` a mano) |
| Un atajo hace otra cosa | `:verbose nmap <tecla>` muestra quién lo definió |
| ¿Qué opción está activa? | `:verbose set opción?` |
| ¿Es culpa de mi configuración? | `nvim --clean archivo` (sin configuración ni plugins) |
| Arranque lento | `:Lazy profile` |
| Resaltado raro | `:Inspect` / `:InspectTree` |

> [!warning] No borres `~/.config/nvim` para «arreglar»
> Contiene tu trabajo. Para empezar de cero de forma reversible: `mv ~/.config/nvim{,.bak}` y también `~/.local/share/nvim`, `~/.local/state/nvim`, `~/.cache/nvim`.

Fuentes: [LazyVim: configuración](https://www.lazyvim.org/configuration), [LazyVim: plugins](https://www.lazyvim.org/configuration/plugins), [LazyVim: extras](https://www.lazyvim.org/extras), [lazy.nvim](https://lazy.folke.io/), [Neovim: health](https://neovim.io/doc/user/health/), [Neovim: LSP](https://neovim.io/doc/user/lsp/), [Neovim: Lua guide](https://neovim.io/doc/user/lua-guide/).
