---
aliases: [LSP Neovim, Autocompletado, Formateo, Linting, Diagnósticos]
tags: [neovim, lazyvim, ide, lsp]
actualizado: 2026-09-27
verificado_en: "Neovim 0.12.5 (:help lsp, diagnostic, treesitter) · LazyVim (nvim-lspconfig, mason, blink.cmp, conform, nvim-lint)"
---
# IDE: LSP, completado, formato y *lint*

Nivel 3 de la [[05 Neovim/Ruta de cero a pro]]. Un IDE es la suma de varias piezas independientes; conocer cuál hace qué es lo que te permite diagnosticar cuando algo falla.

## Arquitectura

```text
             ┌───────────── Neovim ─────────────┐
archivo.py → │ Treesitter  → resaltado, pliegues, objetos de texto (analiza la sintaxis)
             │ Cliente LSP → habla JSON-RPC con ──→ servidor (pyright, vtsls, lua_ls…)
             │                                      ↳ definición, referencias, errores, renombrar
             │ blink.cmp   → menú de completado (fuentes: LSP, rutas, buffer, snippets)
             │ conform     → formateadores externos (ruff, prettier, stylua, shfmt)
             │ nvim-lint   → linters externos (eslint, shellcheck, markdownlint)
             │ diagnostic  → muestra errores de LSP y linters (signos, texto virtual, listas)
             └──────────────────────────────────┘
Mason → descarga los servidores, formateadores y linters en ~/.local/share/nvim/mason/
```

| Pieza | Responsable | Comprobar |
|---|---|---|
| Resaltado y estructura | Treesitter (integrado + nvim-treesitter) | `:InspectTree`, `:checkhealth nvim-treesitter` |
| Inteligencia del lenguaje | cliente LSP integrado + nvim-lspconfig | `Espacio c l`, `:checkhealth vim.lsp` |
| Instalación de herramientas | Mason | `:Mason` (`Espacio c m`) |
| Completado | blink.cmp | `:checkhealth blink.cmp` |
| Formato | conform.nvim | `:ConformInfo` |
| Lint | nvim-lint | `:lua print(vim.inspect(require("lint").linters_by_ft))` |

> [!info] En tu equipo
> Mason tiene instalados `lua-language-server`, `stylua` y `shfmt` (sólo lo necesario para Lua y shell). Analizadores Treesitter: bash, c, diff, html, javascript, jsdoc, json, lua, markdown, python, regex, toml, tsx, typescript, vim, xml, yaml, entre otros. **Ningún extra de lenguaje está activo**: activa los tuyos siguiendo [[05 Neovim/IDE - Lenguajes y extras]].

## LSP: lo que obtienes

| Acción | LazyVim | Nativo de Neovim 0.12 |
|---|---|---|
| Ir a definición | `gd` | `Ctrl+]` (usa `tagfunc` del LSP) |
| Declaración / implementación / tipo | `gD` / `gI` / `gy` | — / `gri` / `grt` |
| Referencias | `gr` | `grr` |
| Documentación flotante | `K` (dos veces para entrar en la ventana) | `K` |
| Ayuda de firma | `gK`, `Ctrl+k` en Insertar | `Ctrl+s` en Insertar |
| Renombrar símbolo | `Espacio c r` | `grn` |
| Acción de código (arreglos rápidos, imports) | `Espacio c a` | `gra` |
| Organizar imports | `Espacio c o` | — |
| Símbolos del documento | `Espacio s s` | `gO` |
| Codelens | `Espacio c c` | `grx` |
| Llamadas entrantes / salientes | `gai` / `gao` | — |
| Ampliar selección por sintaxis | `an` / `in` en Visual | `an` / `in` |

> [!tip] Conflicto de `gr`
> Neovim 0.12 crea atajos que empiezan por `gr` (`grn`, `gra`, `grr`…) y LazyVim define `gr` = referencias. Si pulsas `gr` y esperas, Neovim aguarda la siguiente tecla (`timeoutlen`). Ambos funcionan; usa el que prefieras.

**Gestión del cliente** (0.12): `:lsp restart` reinicia los servidores del buffer, `:lsp stop`, `:lsp disable pyright`, `:lsp enable pyright`. Información: `Espacio c l` o `:checkhealth vim.lsp`.

**Ayudas visuales**: *inlay hints* (tipos y nombres de parámetros en línea) `Espacio u h`; resaltado de otras apariciones del símbolo bajo el cursor (automático); `Espacio c d` diagnóstico de la línea.

## Diagnósticos (`:help diagnostic`)

| Tecla | Acción |
|---|---|
| `]d` / `[d` | siguiente / anterior (cualquier gravedad) |
| `]e` / `[e` · `]w` / `[w` | siguiente / anterior error · advertencia |
| `Espacio c d` · `Ctrl+w d` | diagnósticos de la línea en ventana flotante |
| `Espacio x x` / `Espacio x X` | lista en Trouble (proyecto / buffer) |
| `Espacio s d` | buscar diagnósticos con el picker |
| `Espacio u d` | activar/desactivar diagnósticos |

Personalizar (en `lua/plugins/lsp.lua`):

```lua
return {
  "neovim/nvim-lspconfig",
  opts = {
    diagnostics = {
      virtual_text = false,            -- sin texto al final de la línea
      virtual_lines = { current_line = true },  -- 0.11+: detalle debajo de la línea actual
      severity_sort = true,
    },
    inlay_hints = { enabled = false },
  },
}
```

## Configurar servidores

### Con LazyVim (recomendado aquí)

Casi siempre basta con activar el extra del lenguaje. Para un servidor sin extra, o para ajustar uno:

```lua
-- ~/.config/nvim/lua/plugins/lsp.lua
return {
  "neovim/nvim-lspconfig",
  opts = {
    servers = {
      bashls = {},                         -- activar (Mason lo instala)
      pyright = {                          -- ajustar un servidor existente
        settings = { python = { analysis = { typeCheckingMode = "strict" } } },
      },
      lua_ls = {
        settings = { Lua = { hint = { enable = true } } },
      },
      tsserver = { enabled = false },      -- desactivar
      clangd = { mason = false },          -- usar el binario del sistema, no el de Mason
      ["*"] = {                            -- opciones para todos los servidores
        keys = { { "gK", false } },        -- quitar un atajo LSP de LazyVim
      },
    },
  },
}
```

LazyVim traduce esto a las API nativas `vim.lsp.config()` y `vim.lsp.enable()` y le pide a Mason que instale lo que falte.

### Nativo, sin plugins (para entender qué pasa)

Neovim trae el cliente; sólo necesitas el servidor instalado y dos llamadas (`:help lsp-quickstart`):

```lua
vim.lsp.config("lua_ls", {
  cmd = { "lua-language-server" },
  filetypes = { "lua" },
  root_markers = { { ".luarc.json", ".luarc.jsonc" }, ".git" },
  settings = { Lua = { runtime = { version = "LuaJIT" } } },
})
vim.lsp.enable("lua_ls")
```

También puedes poner cada configuración en `~/.config/nvim/lsp/<nombre>.lua` (devuelve una tabla) y llamar sólo a `vim.lsp.enable("<nombre>")`. nvim-lspconfig aporta cientos de estas configuraciones listas.

## Mason (`Espacio c m`)

| Tecla en `:Mason` | Acción |
|---|---|
| `i` / `u` / `X` | instalar / actualizar / desinstalar el paquete bajo el cursor |
| `U` | actualizar todo |
| `/` o `Ctrl+f` | filtrar por lenguaje |
| `g?` | ayuda |

Para instalar herramientas siempre (reproducible en otro equipo):

```lua
{ "mason-org/mason.nvim", opts = { ensure_installed = { "shellcheck", "markdownlint-cli2", "prettier" } } }
```

> [!warning] Mason frente a mise/pacman
> Mason instala en su propio directorio. Si un servidor necesita Node o Python, usa el que encuentre en el `PATH` (aquí, el de **mise**). Si algo falla al instalar, revisa `:Mason` → `g?` → registro, y `:checkhealth mason`.

## Autocompletado (blink.cmp)

| Tecla (Insertar) | Acción |
|---|---|
| (escribir) | el menú aparece solo |
| `Ctrl+n` / `Ctrl+p` · flechas | siguiente / anterior |
| `Enter` · `Ctrl+y` | aceptar |
| `Ctrl+e` | cerrar el menú |
| `Ctrl+Espacio` | abrir menú / documentación |
| `Ctrl+b` / `Ctrl+f` | desplazar la documentación |
| `Tab` / `Shift+Tab` | saltar entre campos de un *snippet* |

Fuentes: LSP, rutas, palabras del buffer y *snippets* (friendly-snippets). Para *snippets* propios, crea `~/.config/nvim/snippets/<lenguaje>.json` en formato VS Code. La línea de órdenes (`:`) también autocompleta.

## Formato (conform.nvim)

- **Autoformato al guardar**: activado por defecto en LazyVim, pero **desactivado en este equipo** (`vim.g.autoformat = false`). Actívalo por sesión con `Espacio u f` (global) o `Espacio u F` (buffer).
- **Formatear a mano**: `Espacio c f` (archivo o selección).
- **Ver qué formateador se usa**: `:ConformInfo`.

```lua
-- lua/plugins/formatting.lua
return {
  "stevearc/conform.nvim",
  opts = {
    formatters_by_ft = {
      python = { "ruff_format" },
      javascript = { "prettier" }, typescript = { "prettier" },
      markdown = { "prettier" },
      sh = { "shfmt" },
    },
    formatters = { shfmt = { prepend_args = { "-i", "2", "-ci" } } },
  },
}
```

Si hay LSP con capacidad de formato y ningún formateador configurado, conform usa el del LSP.

## Lint (nvim-lint)

Complementa al LSP con herramientas como `eslint`, `shellcheck`, `markdownlint`. Se ejecuta al guardar/abrir y sus avisos aparecen como diagnósticos.

```lua
return {
  "mfussenegger/nvim-lint",
  opts = { linters_by_ft = { sh = { "shellcheck" }, markdown = { "markdownlint-cli2" } } },
}
```

## Treesitter (`:help treesitter`)

Analiza la sintaxis para resaltado, sangría, pliegues, objetos de texto (`af`, `ic`…) y selección incremental (`an`/`in`). Los analizadores se instalan con `:TSInstall <lenguaje>` y se actualizan con `:TSUpdate`. Diagnóstico: `:InspectTree` (árbol del archivo), `:Inspect` (grupo de resaltado bajo el cursor), `:checkhealth nvim-treesitter`.

## Cuando algo no funciona

1. ¿El tipo de archivo es el esperado? `:set filetype?`
2. ¿Hay servidor adjunto? `Espacio c l`. Si no: ¿está el extra activo? ¿Mason lo instaló? ¿la raíz del proyecto es correcta (`.git`, `pyproject.toml`, `package.json`)?
3. ¿Arranca con errores? `:checkhealth vim.lsp` y el registro del cliente: `:lua vim.cmd('tabnew ' .. vim.lsp.log.get_filename())` (más detalle con `:lua vim.lsp.log.set_level('debug')`).
4. Reinicia: `:lsp restart`, y si hace falta `:restart`.
5. Prueba sin tu configuración: [[05 Neovim/Configuración y diagnóstico#Diagnóstico]].

Fuentes: [lsp](https://neovim.io/doc/user/lsp/), [diagnostic](https://neovim.io/doc/user/diagnostic/), [treesitter](https://neovim.io/doc/user/treesitter/), [insert (completado)](https://neovim.io/doc/user/insert/), [news 0.12](https://neovim.io/doc/user/news/), [LazyVim: LSP](https://www.lazyvim.org/plugins/lsp), [LazyVim: formatting](https://www.lazyvim.org/plugins/formatting), [LazyVim: linting](https://www.lazyvim.org/plugins/linting), [nvim-lspconfig](https://github.com/neovim/nvim-lspconfig), [mason.nvim](https://github.com/mason-org/mason.nvim), [blink.cmp](https://cmp.saghen.dev/), [conform.nvim](https://github.com/stevearc/conform.nvim), [nvim-lint](https://github.com/mfussenegger/nvim-lint).
