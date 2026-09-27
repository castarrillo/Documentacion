---
aliases: [Todos los atajos, Keybindings]
tags: [omarchy, hyprland, atajos, referencia]
actualizado: 2026-09-27
verificado_en: "Omarchy 4.0.4-1 · generado a partir de omarchy menu keybindings --print"
---
# Referencia completa de atajos de Omarchy

Transcripción agrupada y traducida de `omarchy menu keybindings --print` en este equipo (incluye los atajos personales de `~/.config/hypr/bindings.lua`, marcados con ★). Tras actualizar Omarchy, compara con la salida actual: los defaults pueden cambiar. Resumen para memorizar: [[01 Omarchy/Atajos y flujos]].

> [!tip] Regenerar esta lista
> `omarchy menu keybindings --print > ~/atajos.txt` y compárala con `diff` contra la versión anterior.

## Menús y sistema

| Atajo | Acción |
|---|---|
| `Super+Espacio` | Lanzador / menú de Omarchy |
| `Super+Alt+Espacio` | Menú de aplicaciones |
| `Super+Escape` | Menú del sistema (bloquear, suspender, reiniciar, apagar) |
| `Super+K` | Buscador de atajos |
| `Super+Ctrl+O` | Alternar menú |
| `Super+Ctrl+H` | Menú de hardware (reiniciar Wi-Fi, audio, Bluetooth…) |
| `Super+Ctrl+P` | Energía (perfiles) |
| `Super+Ctrl+S` | Compartir (LocalSend) |
| `Super+Ctrl+T` | Actividad (monitor de sistema) |
| `Super+Ctrl+Q` | Calculadora |
| `Super+Ctrl+.` | Transcodificar |
| `Super+Ctrl+Shift+Espacio` | Menú de temas |
| `Super+Ctrl+Espacio` | Selector de fondo |
| `Super+Ctrl+L` | Bloquear |
| `Ctrl+Alt+Supr` | Cerrar todas las ventanas |

## Aplicaciones

| Atajo | Aplicación | Atajo | Aplicación |
|---|---|---|---|
| `Super+Enter` | Terminal | `Super+Shift+Enter` / `Super+Shift+B` | Navegador |
| `Super+Alt+Enter` | Terminal con tmux | `Super+Shift+Alt+B` | Navegador privado |
| `Super+Ctrl+Enter` | Terminal con Herdr | `Super+Shift+F` | Archivos |
| `Super+Shift+N` | Editor | `Super+Shift+Alt+F` | Archivos en el directorio actual |
| `Super+Shift+O` | Obsidian | `Super+Shift+W` | Omawrite |
| `Super+Shift+D` | Docker (lazydocker) | `Super+Shift+/` | Contraseñas |
| `Super+Shift+M` | Música | `Super+Shift+Alt+M` | Música (TUI) |
| `Super+Shift+Ctrl+A` | Agente de IA | `Super+Shift+A` / `Super+Shift+Alt+A` | ChatGPT / Grok |
| `Super+Shift+C` | Calendario | `Super+Shift+E` / `Super+Shift+Alt+E` | Correo / nuevo correo |
| `Super+Shift+G` | Signal | `Super+Shift+Alt+G` / `Super+Shift+Ctrl+G` | WhatsApp / Google Messages |
| `Super+Shift+P` | Google Photos | `Super+Shift+S` | Google Maps |
| `Super+Shift+Y` | YouTube | `Super+Shift+X` / `Super+Shift+Alt+X` | X / publicar en X |
| `Super+Alt+B` ★ | Red Pill Backup | | |

> [!tip] Quitar los atajos de apps preinstaladas
> En `~/.config/hypr/hyprland.lua`, antes de `require("default.hypr.omarchy")`, define `omarchy_preinstalled_bindings = false` para conservar sólo los atajos del gestor de ventanas.

## Ventanas

| Atajo | Acción |
|---|---|
| `Super+W` | Cerrar ventana |
| `Super+T` | Alternar flotante/mosaico |
| `Super+F` / `Super+Alt+F` / `Super+Ctrl+F` | Pantalla completa / ancho completo / pantalla completa en mosaico |
| `Super+J` | Alternar dirección de la división |
| `Super+P` | Pseudo-mosaico |
| `Super+O` | Sacar ventana (flotante y fijada) |
| `Super+Flechas` | Enfocar ventana en esa dirección |
| `Super+Shift+Flechas` | Intercambiar ventana en esa dirección |
| `Alt+Tab` / `Shift+Alt+Tab` | Ventana siguiente / anterior (y traerla al frente) |
| `Ctrl+Alt+Tab` / `Shift+Ctrl+Alt+Tab` | Monitor siguiente / anterior |
| `Super+clic izquierdo` / `Super+clic derecho` (arrastrar) | Mover / redimensionar |
| `Super+-` / `Super+=` | Ampliar / reducir hacia la izquierda (`Alt` = un poco, `Ctrl` = mucho) |
| `Super+Shift+-` / `Super+Shift+=` | Reducir hacia arriba / ampliar hacia abajo (`Alt` = un poco, `Ctrl` = mucho) |
| `Super+Alt+Inicio` / `Super+Inicio` | Guardar / restaurar anchura de la ventana |
| `Super+Retroceso` | Alternar transparencia |
| `Super+Shift+Retroceso` | Alternar huecos (gaps) |
| `Super+Ctrl+Retroceso` | Alternar aspecto cuadrado con una sola ventana |

## Workspaces y monitores

| Atajo | Acción |
|---|---|
| `Super+1…9`, `Super+0` | Ir al workspace 1…9, 10 |
| `Super+Shift+1…0` | Mover ventana al workspace (y seguirla) |
| `Super+Shift+Alt+1…0` | Mover ventana sin seguirla |
| `Super+Tab` / `Super+Shift+Tab` / `Super+Ctrl+Tab` | Siguiente / anterior / último usado |
| `Super+rueda` | Desplazar workspace activo |
| `Super+L` | Alternar layout *dwindle* ↔ *scrolling* |
| `Super+S` / `Super+Alt+S` | Mostrar scratchpad / enviar ventana al scratchpad |
| `Super+Shift+Alt+Flechas` | Mover el workspace al monitor en esa dirección |
| `Super+/` / `Super+Alt+/` | Subir / bajar escala del monitor |
| `Super+Ctrl+Supr` | Alternar pantalla del portátil |
| `Super+Ctrl+Alt+Supr` | Alternar espejo de la pantalla del portátil |
| `Super+Ctrl+Z` / `Super+Ctrl+Alt+Z` | Zoom / restablecer zoom |

## Grupos (ventanas con pestañas)

| Atajo | Acción |
|---|---|
| `Super+G` | Crear/deshacer grupo |
| `Super+Alt+G` | Sacar ventana del grupo |
| `Super+Alt+Flechas` | Meter ventana en el grupo de esa dirección |
| `Super+Alt+1…5` | Ir a la pestaña N del grupo |
| `Super+Alt+Tab` / `Super+Shift+Alt+Tab` | Pestaña siguiente / anterior |
| `Super+Ctrl+Izq/Der` | Mover foco dentro del grupo |
| `Super+Alt+rueda` | Pestaña siguiente / anterior |

## Portapapeles, captura y dictado

| Atajo | Acción |
|---|---|
| `Super+C` / `Super+V` / `Super+X` | Copiar / pegar / cortar universal |
| `Super+Ctrl+V` | Historial del portapapeles |
| `Super+Ctrl+E` | Emojis |
| `ImprPant` | Captura (modo inteligente) |
| `Alt+ImprPant` | Iniciar/detener grabación |
| `Super+Ctrl+C` | Menú de captura |
| `Super+Ctrl+ImprPant` | Extraer texto (OCR) |
| `Super+ImprPant` | Selector de color |
| `Super+Alt+[` / `Super+Alt+]` | Webcam superpuesta más pequeña / más grande |
| `F9` (mantener) | Dictado *push-to-talk* |
| `Super+Ctrl+X` | Alternar dictado |
| `Shift+Alt+D` / `Shift+Alt+L` | En web app: descargar vídeo / copiar URL |

## Barra, avisos y notificaciones

| Atajo | Acción |
|---|---|
| `Super+Shift+Espacio` | Mostrar/ocultar barra |
| `Super+Ctrl+1…9` | Abrir panel N de la barra |
| `Super+Ctrl+A` / `W` / `B` / `D` | Audio / red / Bluetooth / pantalla |
| `Super+Ctrl+Alt+T` / `B` / `W` / `D` | Hora / batería restante / clima / calendario |
| `Super+Ctrl+R` / `Super+Ctrl+Alt+R` / `Super+Shift+Ctrl+R` | Nuevo recordatorio / ver / borrar recordatorios |
| `Super+,` / `Super+Shift+,` | Descartar última / todas |
| `Super+Alt+,` | Ejecutar la acción de la última notificación |
| `Super+Ctrl+,` | No molestar |
| `Super+Shift+Alt+,` | Historial de notificaciones |
| `Super+Ctrl+N` | Luz nocturna |
| `Super+Ctrl+I` | Alternar bloqueo por inactividad |

## Multimedia y hardware

| Atajo | Acción |
|---|---|
| Teclas de volumen / silencio / micrófono | Volumen, silencio, silenciar micrófono |
| `Alt+` teclas de volumen o brillo | Ajuste fino |
| `Shift+` brillo | Brillo mínimo / máximo |
| `Shift+Silencio` | Cambiar salida de audio |
| `Shift+Play/Pause` | Cambiar fuente multimedia |
| `Play`, `Next`, `Prev`; `Alt+Play` / `Shift+Alt+Play` | Reproducción; pista siguiente / anterior |
| `Super+Ctrl+Arriba/Abajo` ★ | Subir / bajar volumen (repetible, con pantalla bloqueada) |
| `Super+Alt+P` ★ | Reproducir/pausar |
| Teclas de retroiluminación del teclado | Subir / bajar / ciclar |
| Tecla de touchpad | Activar / desactivar touchpad |
| Tecla de encendido | Menú de energía |

## Ayudas de multiplexores

| Atajo | Acción |
|---|---|
| `Super+Alt+K` | Chuleta de atajos de tmux |
| `Super+Ctrl+K` | Chuleta de atajos de Herdr |

Fuentes: salida local de `omarchy menu keybindings --print`, defaults en `/usr/share/omarchy/default/hypr/bindings/*.lua`, [Hotkeys oficial](https://omarchy.org/manual/hotkeys/), [Binds de Hyprland](https://wiki.hypr.land/Configuring/Basics/Binds/).
