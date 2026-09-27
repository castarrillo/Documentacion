---
aliases: [DAP Neovim, Depurar en Neovim, Neotest, Terminal Neovim]
tags: [neovim, lazyvim, ide, depuracion, pruebas]
actualizado: 2026-09-27
verificado_en: "LazyVim extras dap.core y test.core · Neovim 0.12.5 (:help terminal, quickfix)"
---
# IDE: depuración, pruebas y ejecución

Requiere activar los extras **`dap.core`** y **`test.core`** (`:LazyExtras`) además del extra de tu lenguaje, que aporta el adaptador concreto (debugpy para Python, js-debug para Node…). Ver [[05 Neovim/IDE - Lenguajes y extras]].

## Depuración (DAP)

**DAP** (*Debug Adapter Protocol*) es al depurador lo que el LSP al lenguaje: Neovim (nvim-dap) habla con un adaptador que controla el programa. nvim-dap-ui muestra variables, pila, *watches*, puntos de ruptura y consola.

| Tecla | Acción |
|---|---|
| `Espacio d b` | alternar punto de ruptura |
| `Espacio d B` | punto de ruptura **condicional** |
| `Espacio d c` | ejecutar / continuar (elige configuración la primera vez) |
| `Espacio d C` | ejecutar hasta el cursor |
| `Espacio d O` / `Espacio d i` / `Espacio d o` | *step over* / *into* / *out* |
| `Espacio d j` / `Espacio d k` | bajar / subir en la pila |
| `Espacio d g` | ir a la línea sin ejecutar |
| `Espacio d l` | repetir la última ejecución |
| `Espacio d P` | pausar |
| `Espacio d t` | terminar |
| `Espacio d u` | mostrar/ocultar la interfaz |
| `Espacio d e` | evaluar expresión (o selección) |
| `Espacio d w` | *widgets* (valor bajo el cursor) |
| `Espacio d r` | REPL del depurador |
| `Espacio d s` | sesión |
| Python: `Espacio d P t` / `Espacio d P c` | depurar el método / la clase de prueba |

### Sesión típica

1. Abre el archivo y pon un punto de ruptura con `Espacio d b` en la línea sospechosa.
2. `Espacio d c` → elige «Launch file» (Python) o la configuración de Node.
3. Cuando se detenga, inspecciona variables en el panel, evalúa con `Espacio d e`, avanza con `Espacio d O`.
4. `Espacio d t` para terminar; `Espacio d u` cierra la interfaz.

**Configuraciones de proyecto**: nvim-dap lee `.vscode/launch.json` si existe, así que puedes compartir configuraciones con quien use VS Code. Para argumentos o variables de entorno propios, créalo:

```json
{
  "version": "0.2.0",
  "configurations": [
    { "type": "python", "request": "launch", "name": "API local",
      "program": "${workspaceFolder}/app/main.py", "args": ["--debug"],
      "env": { "APP_ENV": "dev" } }
  ]
}
```

## Pruebas (neotest)

| Tecla | Acción |
|---|---|
| `Espacio t r` | ejecutar la prueba **más cercana** al cursor |
| `Espacio t t` | ejecutar el archivo |
| `Espacio t T` | ejecutar todos los archivos de prueba |
| `Espacio t l` | repetir la última |
| `Espacio t d` | depurar la prueba más cercana (con DAP) |
| `Espacio t s` | panel resumen (árbol de pruebas) |
| `Espacio t o` / `Espacio t O` | salida de la prueba / panel de salida |
| `Espacio t w` | modo vigilancia: re-ejecutar al guardar |
| `Espacio t S` | detener |

Los resultados aparecen como signos (✓/✗) junto a cada prueba y como diagnósticos. Adaptadores según el extra de lenguaje: pytest (Python), Jest/Vitest (añadiendo `neotest-jest` o `neotest-vitest`), Go, Rust…

## Terminal integrada (`:help terminal`)

| Tecla / orden | Acción |
|---|---|
| `Ctrl+/` · `Espacio f t` | terminal flotante en la raíz (vuelve a pulsar para ocultarla) |
| `Espacio f T` | terminal en el cwd |
| `:terminal` · `:vsplit \| term` | terminal en un buffer / en división |
| `Esc Esc` · `Ctrl+\ Ctrl+n` | pasar a modo Normal (copiar, buscar en la salida) |
| `i` / `a` | volver a escribir en la terminal |

Para procesos largos (servidores, *watchers*) es mejor un pane de Herdr/tmux al lado del editor: sobrevive a cerrar Neovim ([[07 Herdr/Workspaces y agentes]]).

## Compilar y saltar a los errores (`:make` + quickfix)

Neovim puede ejecutar cualquier orden de compilación y convertir su salida en una lista de errores navegable (`:help quickfix`):

```vim
:set makeprg=npm\ run\ build     " o: cargo build, make, ruff check .
:make                             " ejecuta y rellena la quickfix
:copen                            " ver errores; ]q / [q para recorrerlos
:compiler tsc                     " usar un analizador de salida predefinido (:help compiler)
```

El extra `editor.overseer` añade un gestor de tareas (lee `tasks.json`, `package.json`, `Makefile`) con salida en paneles.

## Ejecutar el archivo actual rápidamente

```vim
:!python3 %                      " % = archivo actual
:w !bash                         " enviar el buffer a bash sin guardarlo
:r !date                         " insertar la salida de una orden
:'<,'>!sort -u                    " filtrar la selección por una orden
```

Puedes fijarlo como atajo en `lua/config/keymaps.lua` (ver [[05 Neovim/Configuración Lua a fondo#Autocomandos]]).

Fuentes: [terminal](https://neovim.io/doc/user/terminal/), [quickfix](https://neovim.io/doc/user/quickfix/), [LazyVim: dap.core](https://www.lazyvim.org/extras/dap/core), [LazyVim: test.core](https://www.lazyvim.org/extras/test/core), [nvim-dap](https://github.com/mfussenegger/nvim-dap), [nvim-dap-ui](https://github.com/rcarriga/nvim-dap-ui), [neotest](https://github.com/nvim-neotest/neotest), [Debug Adapter Protocol](https://microsoft.github.io/debug-adapter-protocol/).
