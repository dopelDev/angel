# 🛠️ ROADMAP — HSL Color Picker (Rust + OpenGL + NeoVim)

Este roadmap divide el proyecto en **etapas pequeñas**, claras y concretas.
Cada etapa tiene objetivos, subtareas, entregables y condiciones de “done”.

El propósito es ayudarte a avanzar sin estrés y con claridad total.

---

# 📅 Etapa 0 — Preparación del Entorno

### 🎯 Objetivo

Tener el entorno Rust listo, dependencias instaladas y poder ejecutar un “Hello Window”.

### 🔹 Pasos

- [ ] Instalar Rust (rustup)
- [ ] Verificar versión de Rust
- [ ] Crear un nuevo proyecto cargo:

  ```bash
  cargo new hsl-color-picker --bin
  ```

- [ ] Añadir dependencias al `Cargo.toml`:
  - `winit`
  - `glium`
  - `cgmath` (matemáticas)

- [ ] Crear estructura básica del proyecto (carpetas `src/graphics`, `src/color`, etc.)

### 📦 Entregable

- Un binario que compila y abre una ventana vacía.

### ✅ Done cuando:

- Al ejecutar `cargo run`, aparece una ventana sin contenido.

---

# 📅 Etapa 1 — Crear la Ventana Base con Winit + Glium

### 🎯 Objetivo

Tener una ventana funcional con contexto OpenGL renderizando un color plano.

### 🔹 Pasos

- [ ] Inicializar Winit (event loop + window)
- [ ] Crear el display de Glium
- [ ] Configurar el loop principal (`event_loop.run`)
- [ ] Dibujar un color sólido (p.ej. negro o gris)
- [ ] Testear eventos básicos (cerrar ventana con “X”)

### 📦 Entregable

- Ventana funcional y estable.

### ✅ Done cuando:

- La ventana se muestra correctamente y no se cierra automáticamente.
- Se puede cerrar manualmente.

---

# 📅 Etapa 2 — Render Pipeline Base

### 🎯 Objetivo

Tener un pipeline mínimo con:

- Vertex shader
- Fragment shader
- Triángulo dibujado en pantalla

### 🔹 Pasos

- [ ] Crear carpeta `shaders/`
- [ ] Escribir shader básico (`basic.vert`)
- [ ] Shader básico de color (`basic.frag`)
- [ ] Dibujar un triángulo
- [ ] Compilar shaders en tiempo de ejecución
- [ ] Manejar errores de compilación

### 📦 Entregable

- Triángulo visible en la ventana.

### ✅ Done cuando:

- El triángulo aparece centrado y puede cambiar de color.

---

# 📅 Etapa 3 — Dibujar el Círculo Cromático (Color Wheel)

### 🎯 Objetivo

Renderizar un **círculo cromático HSL** usando un fragment shader.

### 🔹 Pasos

#### Geometría del círculo

- [ ] Crear un quad (dos triángulos) que cubra toda la pantalla.
- [ ] Hacer que el fragment shader calcule color por píxel.
- [ ] Implementar función `hsl_to_rgb`.

#### Shader del wheel

- [ ] Convertir coordenadas de pantalla a coordenadas normalizadas
- [ ] Calcular ángulo del píxel → Hue
- [ ] Calcular distancia al centro → Saturación
- [ ] Luminosidad fija al 50%
- [ ] Aplicar máscara circular para cortar bordes

### 📦 Entregable

- Rueda cromática completamente visible y limpia.

### ✅ Done cuando:

- El círculo se ve suave, sin artefactos.
- El área fuera del círculo está negra o transparente.

---

# 📅 Etapa 4 — Manejo del Mouse

### 🎯 Objetivo

Obtener coordenadas del cursor y HSL correspondiente.

### 🔹 Pasos

- [ ] Capturar evento `CursorMoved`
- [ ] Convertir posición del mouse a coordenadas del shader
- [ ] Calcular:
  - [ ] hue desde ángulo
  - [ ] saturation desde distancia
  - [ ] luminance fija (por ahora)

- [ ] Mostrar valores en consola (como debug)
- [ ] Detectar clic (event `MouseInput`)

### 📦 Entregable

- Mouse moviéndose sobre la rueda y mostrando HSL en tiempo real.

### ✅ Done cuando:

- Cada movimiento imprime `hsl(H, S%, L%)` en terminal.

---

# 📅 Etapa 5 — Selección de Color

### 🎯 Objetivo

Al hacer clic, **imprimir solo 1 valor HSL limpio en stdout**.

### 🔹 Pasos

- [ ] Capturar click izquierdo
- [ ] Redondear valores HSL
- [ ] Imprimir en formato:

  ```
  hsl(123, 60%, 50%)
  ```

- [ ] Asegurar que al cerrar la ventana no imprima nada más.
- [ ] Asegurar que no imprima spam del cursor.

### 📦 Entregable

- Selección clara y exacta.

### ✅ Done cuando:

- Haces clic → termina el programa → imprime el HSL una sola vez.

---

# 📅 Etapa 6 — Mejorar la UX (Opcional pero recomendado)

### 🎯 Objetivo

Que la herramienta se sienta “bonita” y fluida.

### 🔹 Pasos

- [ ] Mostrar un pointer visual (pequeño círculo blanco)
- [ ] Suavizar edges de la rueda
- [ ] Añadir borde externo
- [ ] Anti-aliasing en el shader
- [ ] Añadir keybind para cerrar (ESC)

### 📦 Entregable

- Rueda cromática bonita y profesional.

### ✅ Done cuando:

- La rueda se ve como la de macOS, Photoshop o similares.

---

# 📅 Etapa 7 — Integración con NeoVim (Lua)

### 🎯 Objetivo

Que NeoVim pueda lanzar la herramienta, recibir el resultado e insertarlo en el archivo actual.

### 🔹 Pasos

#### Ejecutable

- [ ] Confirmar ruta del binario
- [ ] Comportamiento limpio en stdin/stdout

#### Plugin Lua

- [ ] Crear archivo `colorpicker.lua`
- [ ] Ejecutar herramienta con `io.popen`
- [ ] Leer valor retornado
- [ ] Insertar texto con `vim.api.nvim_put`
- [ ] Añadir keymap (`<leader>cp`)

### 📦 Entregable

- Comando en NeoVim que abre el picker e inserta el color.

### ✅ Done cuando:

- Presionas `<leader>cp`
- Se abre la ventana
- Haces clic
- Y aparece el `hsl(...)` en el archivo

---

# 📅 Etapa 8 — Integración con Lazy.nvim (opcional)

### 🎯 Objetivo

Convertirlo en plugin instalable por cualquiera.

### 🔹 Pasos

- [ ] Crear estructura `lua/hslpicker/`
- [ ] Añadir comando global `:HSLPicker`
- [ ] Añadir configuraciones (ruta del binario)
- [ ] Añadir documentación (`:help hslpicker`)

### 📦 Entregable

- Plugin con instalación:

```lua
{ "dopelDev/hsl-picker", lazy = true }
```

---

# 📅 Etapa 9 — Empaquetado y Release

### 🎯 Objetivo

Que cualquiera pueda usarlo sin compilar.

### 🔹 Pasos

- [ ] Compilar binarios para:
  - Linux (x86_64)
  - Linux ARM (si quieres)
  - Windows
  - macOS

- [ ] Subirlos a GitHub Releases
- [ ] Incluir instrucciones de instalación
- [ ] Crear GIF de demostración

### 📦 Entregable

- Release 1.0.0 con binarios, instrucciones y demo.

---

# 📅 Etapa 10 — Ideas Futuras

- [ ] Ajustar luminosidad con rueda externa
- [ ] Modo cuadrado HSV (tipo Photoshop)
- [ ] Copiar HEX automáticamente
- [ ] UI con egui o iced
- [ ] Exportar paletas completas
- [ ] Modo CLI con selección en terminal
- [ ] Versión WebAssembly (OpenGL ES → WebGL)

---

# 🎉 Estado Final Esperado

Cuando completes este roadmap:

- Tendrás un **selector cromático escrito desde cero**,
- usando **Rust + OpenGL** profesionalmente,
- y totalmente **integrado con NeoVim**,
- portable, ligero y extendible.

Un proyecto precioso para portafolio y también súper útil para tu propio workflow.

---
