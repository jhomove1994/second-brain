---
id: Configuración Hammerspoon
aliases: []
tags: []
---

# Configuración Hammerspoon

# recurso #hammerspoon #configuración #lua #scripts

> **Tipo**: Recurso técnico  
> **Uso**: Referencia y backup de configuración  
> **Última actualización**: 2025-07-23

## 📁 Estructura de Archivos

```
~/.hammerspoon/
├── init.lua # Configuración principal
├── modules/
│ ├── cursor.lua # Control de cursor
│ ├── hints.lua # Sistema de hints
│ ├── selection.lua # Selección de texto
│ └── utils.lua # Utilidades comunes
└── README.md # Documentación

```

## ⚙️ Configuración Principal

### Archivo: `~/.hammerspoon/init.lua`

````lua
-- ~/.hammerspoon/init.lua
-- Configuración principal de Hammerspoon estilo Vim

-- Cargar módulos
local cursor = require("modules.cursor")
local hints = require("modules.hints")
local selection = require("modules.selection")
local utils = require("modules.utils")

-- Configuración global
local hyper = { "cmd", "alt", "ctrl" }
local shift_hyper = { "cmd", "alt", "ctrl", "shift" }

-- Variables globales
local vimMode = false

function showHelp()
 local help = [[
HAMMERSPOON VIM MODE - AYUDA

ACTIVACIÓN:
• Cmd+Alt+Ctrl+V : Activar/desactivar modo Vim

MOVIMIENTO:
• hjkl : Mover cursor (izq, abajo, arriba, der)
• Shift+hjkl : Movimiento lento
• w : Salto rápido derecha
• b : Salto rápido izquierda

HINTS:
• Space : Mostrar/ocultar hints en pantalla
• [a-z] : Ir a hint específico
• ESC : Cancelar hints

CLICKS:
• Enter : Click izquierdo
• . : Click derecho
• d : Doble click

SELECCIÓN:
• s : Iniciar selección
• hjkl/wb/hints : Mover mientras seleccionas
• e : Terminar selección
• ESC : Cancelar selección

SALTOS:
• g : Ir arriba de pantalla
• Shift+G : Ir abajo de pantalla
• 0 : Ir izquierda de pantalla
• 4 : Ir derecha de pantalla
• m : Ir al centro

SCROLL:
• u : Scroll arriba
• i : Scroll abajo

AYUDA:
• Shift+/ : Mostrar esta ayuda
• r : Recargar configuración
]]
 hs.dialog.blockAlert("Ayuda Vim Mode", help, "OK")
end

-- ==============================================
-- CONFIGURACIÓN DE HOTKEYS PRINCIPALES
-- ==============================================
function setupMainHotkeys()
 -- Activar/desactivar modo Vim
 hs.hotkey.bind(hyper, "v", function()
  vimMode = not vimMode
  if vimMode then
   hs.alert.show("Modo Vim activado")
  else
   hs.alert.show("Modo Vim desactivado")
  end
 end)

 -- Sistema de hints (como easymotion)
 hs.hotkey.bind(hyper, "space", function()
  if hints.isActive() then
   hints.hide()
  else
   hints.show()
  end
 end)

 -- Clicks
 hs.hotkey.bind(hyper, "return", cursor.leftClick) -- Enter para click izquierdo
 hs.hotkey.bind(hyper, ".", cursor.rightClick) -- . para click derecho
 hs.hotkey.bind(hyper, "d", cursor.doubleClick) -- d para doble click

 -- Selección
 hs.hotkey.bind(hyper, "s", selection.start) -- s para iniciar selección
 hs.hotkey.bind(hyper, "e", selection.end_) -- e para terminar selección

 -- Cancelar selección (ESC)
 hs.hotkey.bind(hyper, "escape", function()
  if selection.isActive() then
   selection.cancel()
  elseif hints.isActive() then
   hints.hide()
  end
 end)

 -- Saltos rápidos a bordes de pantalla
 hs.hotkey.bind(hyper, "g", function()
  cursor.jumpToScreenEdge("top") -- gg en Vim
 end)
 hs.hotkey.bind(shift_hyper, "g", function()
  cursor.jumpToScreenEdge("bottom") -- G en Vim
 end)
 hs.hotkey.bind(hyper, "0", function()
  cursor.jumpToScreenEdge("left") -- 0 en Vim
 end)
 hs.hotkey.bind(hyper, "4", function()
  cursor.jumpToScreenEdge("right") -- $ en Vim (shift+4)
 end)
 hs.hotkey.bind(hyper, "m", function()
  cursor.jumpToScreenEdge("center") -- centro de pantalla
 end)

 -- Movimiento por palabras (simulado con saltos más grandes)
 hs.hotkey.bind(hyper, "w", function()
  cursor.moveWithSelection(1, 0, cursor.SPEEDS.FAST) -- w para saltar hacia adelante
 end)
 hs.hotkey.bind(hyper, "b", function()
  cursor.moveWithSelection(-1, 0, cursor.SPEEDS.FAST) -- b para saltar hacia atrás
 end)

 -- Scroll
 hs.hotkey.bind(hyper, "u", function()
  hs.eventtap.scrollWheel({ 0, 0 }, {}, "line", 5) -- Ctrl+U en Vim
 end)
 hs.hotkey.bind(hyper, "i", function()
  hs.eventtap.scrollWheel({ 0, 0 }, {}, "line", -5) -- Ctrl+D en Vim (usando 'i')
 end)

 -- Ayuda (Shift+/ equivale a ? en muchos teclados)
 hs.hotkey.bind(shift_hyper, "/", showHelp)

 -- Recargar configuración
 hs.hotkey.bind(hyper, "r", function()
  hs.reload()
 end)
end

-- ==============================================
-- MOVIMIENTO ESTILO VIM
-- ==============================================
function setupVimMovement()
 local movements = {
  h = { -1, 0 }, -- izquierda
  j = { 0, 1 }, -- abajo
  k = { 0, -1 }, -- arriba
  l = { 1, 0 }, -- derecha
 }

 for key, direction in pairs(movements) do
  -- Movimiento normal
  hs.hotkey.bind(hyper, key, function()
   cursor.moveWithSelection(direction[1], direction[2], cursor.SPEEDS.NORMAL)
  end, function()
   cursor.moveWithSelection(direction[1], direction[2], cursor.SPEEDS.NORMAL)
  end)

  -- Movimiento lento (con shift)
  hs.hotkey.bind(shift_hyper, key, function()
   cursor.moveWithSelection(direction[1], direction[2], cursor.SPEEDS.SLOW)
  end, function()
   cursor.moveWithSelection(direction[1], direction[2], cursor.SPEEDS.SLOW)
  end)
 end
end

function init()
 -- Inicializar módulos
 cursor.init()
 hints.init()
 selection.init()

 -- Configurar hotkeys
 setupVimMovement()
 setupMainHotkeys()

 hs.alert.show("Configuración Vim cargada\nCmd+Alt+Ctrl+V para activar")

 -- Mostrar ayuda inicial
 hs.notify
  .new({
   title = "Hammerspoon Vim Mode",
   informativeText = "Usa Cmd+Alt+Ctrl+Space para mostrar hints\nCmd+Alt+Ctrl+hjkl para mover cursor",
   autoWithdraw = true,
   withdrawAfter = 5,
  })
  :send()
end

-- Inicializar configuración
init()```
````
