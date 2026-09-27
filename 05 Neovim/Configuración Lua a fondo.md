---
aliases: [Lua en Neovim, init.lua, lazy.nvim spec, Autocomandos]
tags: [neovim, lua, configuracion]
actualizado: 2026-09-27
verificado_en: "Neovim 0.12.5 (:help lua-guide, lua, api, starting, pack) · lazy.nvim"
---
# Configuración con Lua a fondo

Nivel 4 de la [[05 Neovim/Ruta de cero a pro]]. Estructura de tu configuración y mantenimiento: [[05 Neovim/Configuración y diagnóstico]]. Referencia oficial: `:help lua-guide` y `:help lua`.

## Cómo arranca Neovim (`:help startup`)

1. Lee `~/.config/nvim/init.lua` (o `init.vim`).
2. Carga los plugins de `packpath` y los archivos de `plugin/` de cada directorio del `runtimepath`.
3. Al abrir un archivo detecta su **filetype** y carga `ftplugin/<ft>.lua`, `indent/<ft>.lua`, `syntax/…`.
4. Por último, los directorios `after/` (sirven para **sobrescribir** lo anterior).

En este equipo `init.lua` → `lua/config/lazy.lua` → lazy.nvim carga LazyVim, que carga `lua/config/options.lua` (antes de los plugins), `keymaps.lua` y `autocmds.lua` (en el evento `VeryLazy`) y todos los archivos de `lua/plugins/`.

| Ruta | Se carga |
|---|---|
| `lua/<módulo>.lua` | con `require("módulo")` (una vez; queda en caché) |
| `plugin/*.lua` | siempre al arrancar |
| `ftplugin/<ft>.lua` · `after/ftplugin/<ft>.lua` | al abrir un archivo de ese tipo (el de `after/` al final) |
| `lsp/<servidor>.lua` | al hacer `vim.lsp.enable("<servidor>")` |
| `snippets/<lenguaje>.json` | fragmentos para blink.cmp |

Comprueba el orden real con `:set runtimepath?` y los tiempos con `nvim --startuptime /tmp/t.log` o `:Lazy profile`.

## Lua mínimo para configurar

```lua
local nombre = "Ana"                       -- variable local (usa siempre local)
local lista = { "a", "b", "c" }            -- tabla como array (índices desde 1)
local conf = { tema = "oscuro", n = 2 }    -- tabla como diccionario
print(conf.tema, #lista)                   -- oscuro 3
for i, v in ipairs(lista) do print(i, v) end
for k, v in pairs(conf) do print(k, v) end
local function doble(x) return x * 2 end
if nombre ~= "" and not false then print("..") end  -- ~= es «distinto»
local texto = "hola " .. nombre            -- concatenar con ..
```

Explorar desde Neovim: `:lua print(vim.inspect(vim.opt.tabstop:get()))`, `:=vim.bo.filetype` (atajo de `:lua print(…)`), `:lua =vim.lsp.get_clients()`.

## Opciones (`:help lua-guide-options`)

```lua
vim.opt.number = true             -- vim.opt: API cómoda, admite listas (:append, :remove)
vim.opt.wildignore:append({ "*.pyc", "node_modules" })
vim.o.scrolloff = 8               -- vim.o: valor simple (equivale a :set)
vim.bo.shiftwidth = 4             -- opción local al buffer
vim.wo.wrap = false               -- opción local a la ventana
vim.g.mapleader = " "             -- variable global (g:mapleader)
vim.g.autoformat = false          -- variable que lee LazyVim
vim.env.MI_VAR = "x"              -- variable de entorno
```

Averigua qué hace una opción con `:help 'scrolloff'` (con comillas simples) y quién la cambió con `:verbose set scrolloff?`.

## Atajos (`:help vim.keymap.set()`)

```lua
local map = vim.keymap.set
map("n", "<leader>w", "<cmd>w<cr>", { desc = "Guardar" })
map({ "n", "v" }, "<leader>y", '"+y', { desc = "Copiar al sistema" })
map("n", "<leader>rr", function()
  vim.cmd("!python3 " .. vim.fn.expand("%"))
end, { desc = "Ejecutar archivo con Python" })
map("n", "<leader>q", "<cmd>copen<cr>", { buffer = 0, desc = "sólo este buffer" })
vim.keymap.del("n", "<leader>l")          -- quitar uno existente
```

| Opción | Efecto |
|---|---|
| `desc` | texto que muestran which-key y `Espacio s k` (**ponlo siempre**) |
| `buffer = 0` | atajo local al buffer actual |
| `silent = true` | no mostrar la orden |
| `expr = true` | la función devuelve las teclas a ejecutar |
| `remap = true` | permite que la parte derecha use otros atajos |

Modos: `n` Normal, `i` Insertar, `v` Visual+Selección, `x` Visual, `o` operador pendiente, `t` terminal, `c` línea de órdenes. Notación: `<leader>`, `<cr>`, `<esc>`, `<C-x>` (Ctrl), `<A-x>`/`<M-x>` (Alt), `<S-x>` (Shift), `<Tab>`, `<BS>`.

## Autocomandos

Ejecutan código ante **eventos** (`:help autocmd-events`): `BufWritePre`, `BufReadPost`, `FileType`, `InsertLeave`, `TextYankPost`, `VimResized`, `LspAttach`…

```lua
-- lua/config/autocmds.lua
local grupo = vim.api.nvim_create_augroup("mis_autocmds", { clear = true })

-- Sangría de 4 espacios en Python
vim.api.nvim_create_autocmd("FileType", {
  group = grupo,
  pattern = { "python" },
  callback = function() vim.opt_local.shiftwidth = 4 end,
})

-- Quitar espacios finales al guardar
vim.api.nvim_create_autocmd("BufWritePre", {
  group = grupo,
  pattern = "*",
  callback = function() vim.cmd([[%s/\s\+$//e]]) end,
})

-- Atajo local sólo en scripts de shell: ejecutar el archivo
vim.api.nvim_create_autocmd("FileType", {
  group = grupo,
  pattern = "sh",
  callback = function(ev)
    vim.keymap.set("n", "<leader>rr", "<cmd>!bash %<cr>", { buffer = ev.buf, desc = "Ejecutar script" })
  end,
})
```

El grupo con `clear = true` evita duplicados al recargar. Lista los activos con `:autocmd` o `Espacio s a`. Alternativa para ajustes por lenguaje: `after/ftplugin/python.lua` con `vim.opt_local.shiftwidth = 4`.

## Órdenes propias (`:help nvim_create_user_command()`)

```lua
vim.api.nvim_create_user_command("Hoy", function()
  vim.api.nvim_put({ os.date("%Y-%m-%d") }, "c", true, true)
end, { desc = "Insertar la fecha de hoy" })

vim.api.nvim_create_user_command("Grep", function(opts)
  vim.cmd("silent grep! " .. opts.args .. " | copen")
end, { nargs = "+", desc = "rg al quickfix" })
```

## API útil de Neovim

| Función | Uso |
|---|---|
| `vim.api.nvim_get_current_buf()`, `nvim_buf_get_lines(0, 0, -1, false)` | leer el buffer |
| `vim.api.nvim_buf_set_lines(0, 0, 0, false, { "línea" })` | escribir líneas |
| `vim.fn.expand("%:p")` | ruta completa del archivo (`:help expand()`) |
| `vim.fs.root(0, { ".git", "package.json" })` | raíz del proyecto |
| `vim.system({ "git", "status" }, { text = true }):wait()` | ejecutar un proceso |
| `vim.notify("Hecho", vim.log.levels.INFO)` | notificación |
| `vim.ui.select(items, {}, fn)` / `vim.ui.input({}, fn)` | menús y preguntas (LazyVim los muestra con snacks) |
| `vim.schedule(fn)` / `vim.defer_fn(fn, ms)` | diferir código |
| `vim.uv` | bucle de eventos (archivos, temporizadores) |

Referencia completa: `:help api`, `:help lua-stdlib`, `:help vim.fs`, `:help vim.system()`.

## Plugins con lazy.nvim

Cada archivo de `lua/plugins/` devuelve una **especificación** (o una lista):

```lua
return {
  "autor/plugin.nvim",             -- repositorio de GitHub
  version = "*",                   -- última versión etiquetada (estable)
  dependencies = { "nvim-lua/plenary.nvim" },
  -- Carga perezosa: sólo cuando haga falta
  event = "VeryLazy",              -- o "BufReadPost", "InsertEnter"…
  cmd = "MiOrden",                 -- al ejecutar :MiOrden
  ft = { "python" },               -- al abrir un tipo de archivo
  keys = {                         -- al pulsar uno de estos atajos
    { "<leader>xp", "<cmd>MiOrden<cr>", desc = "Mi plugin" },
  },
  opts = { opcion = true },        -- se pasa a require("plugin").setup(opts)
  -- config = function(_, opts) require("plugin").setup(opts) end,  -- si necesitas más control
  enabled = true,                  -- false para desactivarlo
}
```

**Modificar un plugin de LazyVim** sin copiar su configuración: repite su nombre y da sólo lo que cambias. `opts` como tabla **se fusiona**; como función recibe las opciones actuales y las modifica:

```lua
return {
  "nvim-lualine/lualine.nvim",
  opts = function(_, opts)
    table.insert(opts.sections.lualine_x, "encoding")
  end,
}
```

Localiza la especificación original con `Espacio s p` (buscar spec de plugin).

## Un plugin local mínimo

```lua
-- ~/.config/nvim/lua/notas/init.lua
local M = {}

function M.nueva(titulo)
  local ruta = vim.fn.expand("~/Documents/Documentacion/Bandeja/") .. titulo .. ".md"
  vim.cmd.edit(ruta)
  vim.api.nvim_buf_set_lines(0, 0, 0, false, { "# " .. titulo, "", os.date("Creada: %Y-%m-%d") })
end

function M.setup()
  vim.api.nvim_create_user_command("NotaNueva", function(o) M.nueva(o.args) end, { nargs = 1 })
  vim.keymap.set("n", "<leader>nn", function()
    vim.ui.input({ prompt = "Título: " }, function(t) if t then M.nueva(t) end end)
  end, { desc = "Nota nueva en el vault" })
end

return M
```

```lua
-- ~/.config/nvim/lua/plugins/notas.lua
return { dir = vim.fn.stdpath("config") .. "/lua/notas", name = "notas", config = function() require("notas").setup() end }
```

Con `dir` lazy.nvim carga código local; así puedes empaquetar tus utilidades como un plugin más.

## `vim.pack`: el gestor de plugins nativo (0.12)

Neovim 0.12 incluye un gestor **experimental** propio. No lo mezcles con lazy.nvim en la misma configuración; es útil para configuraciones mínimas o para entender cómo funciona un gestor:

```lua
vim.pack.add({
  "https://github.com/folke/flash.nvim",
  { src = "https://github.com/stevearc/oil.nvim", version = vim.version.range("2") },
})
require("flash").setup()
-- :lua vim.pack.update()   actualizar (muestra los cambios y pide confirmación)
-- :lua vim.pack.del({ "flash.nvim" })
```

Guarda el estado en `~/.config/nvim/nvim-pack-lock.json` (versiónalo). Ver `:help vim.pack`.

## Experimentar sin romper tu configuración

`NVIM_APPNAME` hace que Neovim use otro directorio de configuración, datos y estado:

```bash
NVIM_APPNAME=nvim-prueba nvim      # usa ~/.config/nvim-prueba, ~/.local/share/nvim-prueba…
nvim --clean                        # sin configuración ni plugins
nvim -u minimo.lua archivo          # con un init concreto (para aislar fallos)
```

Ideal para probar una configuración desde cero (p. ej. [kickstart.nvim](https://github.com/nvim-lua/kickstart.nvim), muy didáctica) sin tocar tu LazyVim.

## Buenas prácticas

- `local` siempre; un archivo por tema en `lua/plugins/`.
- `desc` en todos los atajos; grupos de autocomandos con `clear = true`.
- Prefiere `opts` a `config`, y `keys`/`cmd`/`ft`/`event` para cargar tarde.
- Tras cada cambio: `:restart`, `:checkhealth`, `:Lazy` sin errores, y commit.
- Lee el código de LazyVim cuando dudes: `~/.local/share/nvim/lazy/LazyVim/lua/lazyvim/`.

Fuentes: [lua-guide](https://neovim.io/doc/user/lua-guide/), [lua](https://neovim.io/doc/user/lua/), [api](https://neovim.io/doc/user/api/), [map](https://neovim.io/doc/user/map/), [autocmd](https://neovim.io/doc/user/autocmd/), [options](https://neovim.io/doc/user/options/), [starting](https://neovim.io/doc/user/starting/), [pack](https://neovim.io/doc/user/pack/), [usr_05](https://neovim.io/doc/user/usr_05/), [usr_40](https://neovim.io/doc/user/usr_40/), [usr_43](https://neovim.io/doc/user/usr_43/), [lazy.nvim: spec](https://lazy.folke.io/spec), [LazyVim: configuración de plugins](https://www.lazyvim.org/configuration/plugins).
