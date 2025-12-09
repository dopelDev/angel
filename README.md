# 🎨 HSL Color Picker — Rust + OpenGL + NeoVim Integration

**Un selector de colores cromático minimalista, rápido y escrito en Rust.**

Este proyecto es una herramienta gráfica independiente que renderiza un círculo cromático (color wheel) y permite seleccionar colores en formato **HSL** con el mouse.
La herramienta está diseñada para integrarse cómodamente con **NeoVim**, permitiendo insertar colores directamente en archivos de código mientras programas.

El objetivo principal no es solo su utilidad, sino también ser un proyecto interesante para aprender Rust, OpenGL y técnicas de integración con NeoVim.

---

## 📌 Objetivos del Proyecto

- Crear un **selector cromático visual** usando Rust y OpenGL.
- Experimentar con:
  - **Winit** (ventanas y eventos)
  - **Glium** (wrapper seguro de OpenGL)
  - **Rust seguro y moderno**

- Permitir integración simple con NeoVim:
  - Tool externa → NeoVim recibe un string con el color HSL.

- Mantener el proyecto **ligero, portable y sin dependencias pesadas**.

---

## 🧱 Stack Tecnológico

### **Core del Proyecto**

| Componente                | Tecnología                    |
| ------------------------- | ----------------------------- |
| Lenguaje                  | Rust                          |
| Renderizado               | OpenGL (via Glium)            |
| Manejo de ventana/eventos | Winit                         |
| Backend matemático        | Rust std + utilidades propias |
| Integración con NeoVim    | Lua (opcional)                |

---

## 🚀 Funcionalidades

- Renderiza un **círculo cromático** interpolado en HSL.
- Detecta posición del cursor sobre el círculo.
- Convierte coordenadas → HSL en tiempo real.
- Al hacer clic:
  - Imprime el valor seleccionado en **stdout** (ejemplo: `hsl(210, 72%, 54%)`).

- Se puede lanzar desde NeoVim y capturar automáticamente el resultado.
- Ventana minimalista y rápida.

---

## 🗂️ Estructura del Repositorio

```
hsl-color-picker/
│
├── src/
│   ├── main.rs                  # Entry point
│   ├── graphics/                # Render loop, shaders, OpenGL buffers
│   ├── color/                   # Utilidades para HSL, HSV, RGB
│   └── ui/                      # Lógica de interacción con mouse
│
├── shaders/
│   ├── wheel.frag               # Shader para generar el círculo cromático
│   └── basic.vert
│
├── scripts/
│   └── nvim.lua                 # Plugin opcional para integrar NeoVim
│
├── README.md
└── Cargo.toml
```

> _Esta estructura es sugerida. Puedes reacomodarla según lo que descubras mientras desarrollas._

---

## 📦 Instalación

### 1. Instalar Rust

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

### 2. Clonar el repositorio

```bash
git clone https://github.com/tuusuario/hsl-color-picker.git
cd hsl-color-picker
```

### 3. Instalar dependencias (automático con cargo)

```bash
cargo build --release
```

---

## ▶️ Uso desde la Terminal

Ejecuta la app:

```bash
./target/release/hsl-color-picker
```

Selecciona un color en la ventana → al hacer clic se imprimirá en el terminal algo como:

```
hsl(128, 64%, 52%)
```

---

## ✨ Integración con NeoVim (opcional)

### 1. Crea un comando Lua

En `~/.config/nvim/lua/colorpicker.lua`:

```lua
local M = {}

M.pick = function()
  local cmd = "path/to/hsl-color-picker"
  local handle = io.popen(cmd)
  local result = handle:read("*a")
  handle:close()

  result = result:gsub("%s+", "")  -- limpiar espacios y saltos
  vim.api.nvim_put({result}, "c", true, true)
end

return M
```

### 2. Añade un keymap en `init.lua`

```lua
vim.keymap.set("n", "<leader>cp", function()
  require("colorpicker").pick()
end, { desc = "Open Rust HSL Picker" })
```

Ahora puedes presionar:

```
<leader>cp
```

Y automáticamente:

1. Se abrirá tu herramienta Rust.
2. Elegirás un color.
3. Se insertará el valor HSL en tu archivo actual.

---

## 🧪 Roadmap / Próximos Pasos

- [ ] Dibujar la rueda cromática vía fragment shader
- [ ] Sistema de selección suave (hover y clic)
- [ ] Transición interactiva de luminosidad
- [ ] Vista secundaria para RGB y hex
- [ ] Integración bidireccional NeoVim ↔ Picker
- [ ] Convertirlo en plugin de NeoVim instalable con Lazy.nvim
- [ ] Compilación multiplataforma (Linux/Windows/Mac)

---

## 🤝 Contribuciones

Este proyecto es experimental, pero contribuciones son bienvenidas: issues, PRs, ideas, optimizaciones o incluso arte del shader de la rueda.

---

## 📄 Licencia

MIT — libre para modificar, estudiar y usar en tus propios proyectos.

---
