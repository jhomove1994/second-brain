---
id: Atajos de Teclado
aliases: []
tags: []
---

# Atajos de Teclado

# recurso #atajos #productividad #referencia

> **Tipo**: Recurso de referencia  
> **Uso**: Consulta diaria durante aprendizaje  
> **Última actualización**: 2025-07-23

## 🔗 Enlaces Relacionados

- [[Configuración Hammerspoon]] - Scripts y configuración
- [[Workflow de Productividad]] - Cómo integrar en el día a día
- [[Aprender Vim]] - Conceptos de movimiento
- [[Configuración Neovim]] - Setup del editor

---

## 🖱️ Hammerspoon - Control del Cursor

> **Estado de dominio**: 🔴 Principiante (0/25 atajos)

### Activación del Sistema

- [x] `Cmd+Alt+Ctrl+V` - **Activar/desactivar modo Vim**
  - _Nota_: Comando maestro para encender todo el sistema

### Movimiento Básico (Esencial)

- [ ] `Cmd+Alt+Ctrl+H` - **Mover cursor izquierda**
- [ ] `Cmd+Alt+Ctrl+J` - **Mover cursor abajo**
- [ ] `Cmd+Alt+Ctrl+K` - **Mover cursor arriba**
- [ ] `Cmd+Alt+Ctrl+L` - **Mover cursor derecha**

### Movimiento Preciso

- [ ] `Cmd+Alt+Ctrl+Shift+H` - **Movimiento lento izquierda**
- [ ] `Cmd+Alt+Ctrl+Shift+J` - **Movimiento lento abajo**
- [ ] `Cmd+Alt+Ctrl+Shift+K` - **Movimiento lento arriba**
- [ ] `Cmd+Alt+Ctrl+Shift+L` - **Movimiento lento derecha**

### 🎯 Sistema de Hints (EasyMotion)

> **Concepto**: Como easymotion en [[Vim]] - saltar a cualquier punto de pantalla

- [ ] `Cmd+Alt+Ctrl+Space` - **Mostrar hints en pantalla**
- [ ] `[a-z]` - **Saltar a hint específico** (ej: presionar 'k' para ir al hint K)
- [ ] `ESC` - **Cancelar hints**

**Ejemplo de uso**:

1. `Cmd+Alt+Ctrl+Space` → aparecen letras en pantalla
2. Presionar `f` → cursor salta al punto marcado con 'F'

### 🖱️ Clicks y Acciones

- [ ] `Cmd+Alt+Ctrl+Enter` - **Click izquierdo**
- [ ] `Cmd+Alt+Ctrl+.` - **Click derecho**
- [ ] `Cmd+Alt+Ctrl+D` - **Doble click**

### 📝 Selección de Texto

> **Flujo**: s → mover → e (iniciar → expandir → terminar)

- [ ] `Cmd+Alt+Ctrl+S` - **Iniciar selección**
  - _Después de esto, cualquier movimiento expande la selección_
- [ ] `H/J/K/L` (durante selección) - **Expandir selección**
- [ ] `Space + letra` (durante selección) - **Expandir con hints**
- [ ] `Cmd+Alt+Ctrl+E` - **Terminar selección**
- [ ] `Cmd+Alt+Ctrl+ESC` - **Cancelar selección**

### 🚀 Saltos Rápidos

> **Concepto**: Como en Vim - ir rápidamente a bordes de pantalla

- [ ] `Cmd+Alt+Ctrl+G` - **Saltar arriba** (como `gg` en Vim)
- [ ] `Cmd+Alt+Ctrl+Shift+G` - **Saltar abajo** (como `G` en Vim)
- [ ] `Cmd+Alt+Ctrl+0` - **Saltar extremo izquierdo** (como `0` en Vim)
- [ ] `Cmd+Alt+Ctrl+4` - **Saltar extremo derecho** (como `$` en Vim)
- [ ] `Cmd+Alt+Ctrl+M` - **Ir al centro de pantalla**

### Movimiento por "Palabras"

- [ ] `Cmd+Alt+Ctrl+W` - **Salto rápido hacia derecha**
- [ ] `Cmd+Alt+Ctrl+B` - **Salto rápido hacia izquierda**

# |## 📜 Scroll

- [ ] `Cmd+Alt+Ctrl+U` - **Scroll hacia arriba** (como `Ctrl+U` en Vim)
- [ ] `Cmd+Alt+Ctrl+I` - **Scroll hacia abajo** (como `Ctrl+D` en Vim)

### 🛠️ Utilidades

- [ ] `Cmd+Alt+Ctrl+?` - **Mostrar ayuda completa**
- [ ] `Cmd+Alt+Ctrl+R` - **Recargar configuración**

---

## 📝 Obsidian.nvim - Gestión de Notas

> **Estado de dominio**: 🔴 Principiante (0/20 atajos)

### 🗂️ Comandos Básicos

- [ ] `<leader>oo` - **Abrir nota en app Obsidian**
- [ ] `<leader>on` - **Crear nueva nota**
- [ ] `<leader>oq` - **Quick Switch** (buscar/cambiar notas)
- [ ] `<leader>of` - **Seguir enlace bajo cursor**
- [ ] `<leader>or` - **Renombrar nota actual**

### 🔍 Navegación y Búsqueda

- [ ] `<leader>ob` - **Mostrar backlinks** (qué notas enlazan aquí)
- [ ] `<leader>ol` - **Mostrar todos los enlaces**
- [ ] `<leader>os` - **Buscar en todas las notas**
- [ ] `<leader>og` - **Mostrar todos los tags**
- [ ] `<leader>ow` - **Cambiar workspace**

### 📅 Notas Diarias

- [ ] `<leader>ot` - **Abrir nota de hoy**
- [ ] `<leader>oy` - **Abrir nota de ayer**
- [ ] `<leader>od` - **Navegador de notas diarias**

### 🎨 Templates y Contenido

- [ ] `<leader>oT` - **Insertar template**
- [ ] `<leader>op` - **Pegar imagen desde clipboard**
- [ ] `<leader>ox` - **Extraer selección a nueva nota**
- [ ] `<leader>ol` (modo visual) - **Crear enlace a nueva nota**

### ⚡ Navegación Especial (en archivos markdown)

- [ ] `gf` - **Seguir enlace** (overrideado por Obsidian)
- [ ] `<leader>ch` - **Toggle checkbox** (`- [ ]` ↔ `- [x]`)
- [ ] `<cr>` - **Acción inteligente** (seguir enlace o toggle checkbox)

---

## ⚡ LazyVim - Editor

> **Estado de dominio**: 🔴 Principiante (0/20 atajos)  
> **Prioridad**: 🔴 Alta - Uso diario

### 📁 Navegación de Archivos

- [ ] `<leader>e` - **Toggle Explorer** (neo-tree)
- [ ] `<leader>ff` - **Buscar archivos** (telescope)
- [ ] `<leader>fg` - **Buscar en contenido** (live grep)
- [ ] `<leader>fb` - **Buscar en buffers abiertos**
- [ ] `<leader>fr` - **Archivos recientes**

### 🪟 Gestión de Ventanas

- [ ] `<C-h>` - **Ir a ventana izquierda**
- [ ] `<C-j>` - **Ir a ventana inferior**
- [ ] `<C-k>` - **Ir a ventana superior**
- [ ] `<C-l>` - **Ir a ventana derecha**
- [ ] `<leader>wd` - **Cerrar ventana actual**
- [ ] `<leader>w-` - **Split horizontal**
- [ ] `<leader>w|` - **Split vertical**

### 🔧 LSP y Código

- [ ] `gd` - **Ir a definición**
- [ ] `gr` - **Mostrar referencias**
- [ ] `K` - **Hover documentation**
- [ ] `<leader>ca` - **Code actions**
- [ ] `<leader>cr` - **Rename símbolo**
- [ ] `<leader>cf` - **Format documento**

### 💻 Terminal

- [ ] `<C-/>` - **Toggle terminal flotante**
- [ ] `<leader>ft` - **Terminal en directorio raíz**
- [ ] `<leader>fT` - **Terminal en directorio actual**

### 🔀 Git

- [ ] `<leader>gg` - **Abrir Lazygit**
- [ ] `<leader>gb` - **Git blame**
- [ ] `<leader>gf` - **Git files**

---

## 🖥️ Sistema macOS

> **Estado de dominio**: 🔴 Principiante (0/10 atajos)  
> **Prioridad**: 🟢 Baja - Nice to have

### 🪟 Gestión de Ventanas (Rectangle/Magnet)

- [ ] `Cmd+Opt+Left` - **Ventana mitad izquierda**
- [ ] `Cmd+Opt+Right` - **Ventana mitad derecha**
- [ ] `Cmd+Opt+Up` - **Maximizar ventana**
- [ ] `Cmd+Opt+Down` - **Restaurar tamaño**

### 🔍 Spotlight y Búsqueda

- [ ] `Cmd+Space` - **Spotlight search**
- [ ] `Cmd+Opt+Space` - **Finder search**

### 📱 Aplicaciones

- [ ] `Cmd+Tab` - **Cambiar entre aplicaciones**
- [ ] `` Cmd+` `` - **Cambiar ventanas de la misma app**
- [ ] `Cmd+Q` - **Cerrar aplicación**
- [ ] `Cmd+W` - **Cerrar ventana**

### 📕 Texto

- [ ] Cmd+<- - Ir al inicio de linea
- [ ] Cmd+ -> - Ir al final de linea
- [ ] Cmd + ⬆️ - Ir a el inicio de documento
- [ ] Cmd + ⬇️ - Ir al final de documento
- [ ] Opt + <- - Ir una palabra a la izquierda
- [ ] Opt + -> - In una palabra a la derecha
- [ ] Shift + Cmd + <- - Seleccionar a inicio de linea
- [ ] Shift + Cmd + -> - Seleccionar a final de linea
- [ ] Shift + Opt + <- - Seleccionar palabra anterior
- [ ] Shift + Opt + -> - Seleccionar palabra siguiente
- [ ] Cmd + A - Seleccionar todo
- [ ] Shift + ⬆️/⬇️ - Seleccionar arriba/abajo

### 🛜 Navegador - Vimium C

- [ ] f -> para abrir un link en el tab
- [ ] F -> para abrir un link en otro tab

---

## 📊 Progreso de Aprendizaje

### 🎯 Objetivos por Semana

#### Semana 1: Hammerspoon Básico

**Objetivo**: Eliminar mouse para navegación básica

- [ ] Dominar activación (`Cmd+Alt+Ctrl+V`)
- [ ] Movimiento básico (`H/J/K/L`) hasta ser natural
- [ ] Clicks básicos (`Enter`, `.`, `D`)
- [ ] **Meta**: 30 min sin tocar mouse

#### Semana 2: Hammerspoon Intermedio

**Objetivo**: Velocidad y eficiencia

- [ ] Sistema de hints (`Space` + letras)
- [ ] Selección básica (`S` → mover → `E`)
- [ ] Saltos rápidos (`G`, `0`, `4`, `M`)
- [ ] **Meta**: Navegar cualquier interfaz en <5 segundos

#### Semana 3: Obsidian Workflow

**Objetivo**: Gestión fluida de notas

- [ ] Creación de notas (`<leader>on`)
- [ ] Navegación (`<leader>oq`, `gf`)
- [ ] Enlaces y backlinks (`<leader>ob`, `<leader>ol`)
- [ ] **Meta**: Crear 5 notas conectadas por día

#### Semana 4: LazyVim Esencial

**Objetivo**: Edición eficiente de código

- [ ] Navegación de archivos (`<leader>ff`, `<leader>fg`)
- [ ] Ventanas (`C-hjkl`, splits)
- [ ] LSP básico (`gd`, `gr`, `K`)
- [ ] **Meta**: Editar código sin mouse

---

## 🎲 Ejercicios Diarios

### 🏃‍♂️ Rutina de 15 Minutos

1. **Hammerspoon (5 min)**: Navegar sistema operativo sin mouse
2. **Obsidian (5 min)**: Crear/conectar 1 nota nueva usando solo atajos
3. **LazyVim (5 min)**: Editar archivo de código usando solo teclado

### 🎯 Desafíos Semanales

- **Semana 1**: Día completo sin mouse para navegación
- **Semana 2**: Crear presentación usando solo atajos
- **Semana 3**: Organizar 20 notas en Obsidian
- **Semana 4**: Refactorizar código usando solo LSP + atajos

---

## 💡 Tips y Trucos

### 🧠 Memoria Muscular

- **Practica 1 atajo nuevo cada día** hasta que sea automático
- **Usa post-its** con atajos en monitor durante primera semana
- **No vuelvas al mouse** - fuerza el uso de atajos

### 🔄 Workflow Efectivo

1. **Hammerspoon** para navegación del sistema
2. **LazyVim** para edición de archivos
3. **Obsidian** para documentar y conectar conocimiento
4. **Repetir** hasta que sea natural

### ⚡ Shortcuts de Shortcuts

- Usa `<leader>?` en LazyVim para ver todos los atajos disponibles
- `Cmd+Alt+Ctrl+?` en Hammerspoon para ayuda rápida
- Mantén esta nota siempre accesible con `<leader>oq` → "Atajos"

---

## 🔗 Proyectos Relacionados

- [[100-projects/Dominar Hammerspoon]] - Proyecto activo de aprendizaje
- [[200-areas/Productividad]] - Área de mejora continua
- [[300-resources/Configuración Hammerspoon]] - Scripts y setup
- [[300-resources/Configuración Neovim]] - Setup del editor

---

**🎯 Próxima revisión**: Cada viernes  
**📈 Progreso actual**: 0/75 atajos dominados  
**⏰ Tiempo estimado para dominio**: 4 semanas con práctica diaria
