# Zapel773DWall — Guía rápida / Quick guide

**Zapel773DWall** es un programa de análisis estructural tridimensional para pórticos, muros, losas con diafragma, zapatas, matres y sismo. Esta guía resume el flujo de trabajo y las herramientas nuevas.

## Español

### 1. Inicio y configuración
- El programa abre en **español**. El selector **Idioma** de la barra permite cambiar a **English** y recuerda la preferencia.
- Muro inicial: **5 pulg de espesor y 8 pies de alto** (editable en la barra o en el panel del muro).
- La rejilla de dibujo cubre al menos **130 pies** alrededor del modelo, suficiente para edificios de **100 × 100 pies** y varios pisos. El espaciamiento se ajusta con **Malla**.
- Unidades: **kip · ft (Imperial)** o **kN · m (SI)**. Al cambiarlas, las medidas se convierten.

### 2. Dibujar
- **Barra:** clic en nudos; clic derecho o Esc termina la cadena.
- **Muro / Muro poli:** clics en planta; el muro poli se cierra en el primer vértice, con Esc o doble clic. El panel permite variar la altura por vértice.
- **Losa / Carga de área:** polígono en planta; clic derecho, Esc o doble clic cierra. Cambia el **Nivel** entre clics para una losa inclinada o rampa.
- **Apoyos:** Fijo, Artic., Rodillo o Libre; clic en cada nudo.
- **Zapata / Matre:** clic sobre un nudo con apoyo. La matre se define por su polígono.

### 3. Escuadra y dibujo asistido
- **Orto (F8):** mantiene guías, cotas y trazos horizontales o verticales.
- **Polar (F10):** pega el trazo a 45°, 30°/60° o 15°; se elige en la barra.
- **Shift:** activa Orto temporal mientras se mantiene presionado.
- **Cota diag.:** mide entre dos puntos sin forzar Orto.
- **Snaps:** al acercarse a un nudo, vértice o extremo aparece **FIN**; al acercarse a una barra o borde aparece **⊥** (perpendicular). Los snaps de nudo y la cota diagonal tienen prioridad.

### 4. Losas con diafragma
- En el panel de la losa, **Aporta rigidez de diafragma** está activado por defecto.
- La losa aporta rigidez en su plano si al menos **3 vértices coinciden con nudos** de la estructura. Si no, se omite y el programa avisa.
- Resultados nuevos: **σ1 losa (diafragma)** y **σ2 losa (diafragma)**, tensiones principales de tracción y compresión de la membrana.
- El cálculo local **Mx/My** de la losa sigue siendo independiente; la membrana no incluye aberturas ni acoplamiento con la flexión fuera del plano.

### 5. Muros en tracción y compresión
- El contorno **σ en muros** usa signo: **positivo = tracción**, **negativo = compresión**.
- La escala se centra en cero cuando hay ambos signos; si todo el muro está en un solo estado, usa el rango real.
- La combinación modal conserva el signo de la tensión en el CQC y en la envolvente X/Y con torsión.

### 6. Calcular y revisar
- Pulsa **Calcular**. La barra de progreso indica la fase y el porcentaje.
- Revisa avisos, reacciones, deformada, derivas y estabilidad.
- **Resultados:** modelo, deformada, σ en muros, presiones y momentos de matres, zapatas, Mx/My de losas, σ1/σ2 de membrana y diagramas de barras.
- **Sismo:** fuerza lateral equivalente, modal espectral CQC o espectro de evento. Después, el botón **Sismo** activa la envolvente sísmica en los contornos.

### 7. Guardar, reporte y ayuda
- **Guardar** descarga el modelo JSON; **Abrir** lo recupera.
- **Reporte** abre la memoria de cálculo para imprimir o guardar en PDF.
- **Guía** abre esta guía resumida dentro del programa, en el idioma activo.
- **?** abre la ayuda ampliada en español; en inglés abre la guía rápida.

### Atajos
- **Esc:** termina cadena, guía, polígono o muro poli.
- **Supr:** elimina el objeto seleccionado.
- **F8:** Orto.
- **F10:** Polar 45° / desactivar.
- **Shift:** Orto temporal.
- **Rueda:** zoom; **arrastrar:** girar; **Shift + arrastrar:** desplazar.

---

## English

### 1. Start-up
- The program opens in **Spanish**. The **Language** selector switches to **English** and remembers the choice.
- Default wall: **5 in thick, 8 ft high** (editable in the bar or wall panel).
- The drawing grid covers at least **130 ft** around the model, enough for **100 × 100 ft** buildings with several stories. Spacing is set with **Grid**.
- Units: **kip · ft (Imperial)** or **kN · m (SI)**; values convert when changed.

### 2. Drawing
- **Beam:** click nodes; right-click or Esc ends the chain.
- **Wall / Poly wall:** plan clicks; close at the first vertex, with Esc or double-click. The panel allows a different height per vertex.
- **Slab / Area load:** plan polygon; right-click, Esc or double-click closes. Change **Level** between clicks for a sloped slab or ramp.
- **Supports:** Fixed, Pin, Roller or Free; click each node.
- **Footing / Mat:** click a supported node. The mat is defined by its polygon.

### 3. Drafting aids
- **Ortho (F8):** keeps guides, dimensions and lines horizontal or vertical.
- **Polar (F10):** snaps to 45°, 30°/60° or 15°; choose it in the bar.
- **Shift:** temporary Ortho while held.
- **Aligned dim.:** measures between two points without forcing Ortho.
- **Snaps:** **FIN** near nodes/vertices/ends; **⊥** near beams or edges (perpendicular). Node snaps and aligned dimensions take priority.

### 4. Slab diaphragm
- In the slab panel, **Contributes diaphragm stiffness** is on by default.
- The slab contributes in-plane stiffness when at least **3 vertices coincide with structural nodes**. Otherwise it is skipped with a warning.
- New results: **Slab σ1 (diaphragm)** and **Slab σ2 (diaphragm)**, principal tension and compression membrane stresses.
- The local **Mx/My** slab analysis remains independent; the membrane has no openings or out-of-plane bending coupling.

### 5. Wall tension/compression
- The **Wall σ** contour is signed: **positive = tension**, **negative = compression**.
- The scale is zero-centered when both signs occur; if the wall is entirely in one state, it uses the actual range.
- Modal CQC and the X/Y accidental-torsion envelope preserve the stress sign.

### 6. Analysis and review
- Press **Calculate**. The progress bar shows phase and percentage.
- Check warnings, reactions, deformed shape, drift and stability.
- **Results:** model, deformed shape, wall σ, mat pressures/moments, footing pressure, slab Mx/My, membrane σ1/σ2 and beam diagrams.
- **Seismic:** equivalent lateral force, modal response spectrum (CQC) or event spectrum. Then the **Seismic** button shows the seismic envelope in the contours.

### 7. Save, report and help
- **Save** downloads the JSON model; **Open** restores it.
- **Report** opens the calculation report for printing or PDF.
- **Guide** opens this quick guide inside the program, in the active language.
- **?** opens the extended Spanish help; in English it opens the quick guide.

### Shortcuts
- **Esc:** ends chain, guide, polygon or poly wall.
- **Del:** deletes the selected object.
- **F8:** Ortho.
- **F10:** Polar 45° / off.
- **Shift:** temporary Ortho.
- **Wheel:** zoom; **drag:** rotate; **Shift + drag:** pan.

---

**Nota / Note:** La memoria y la guía están pensadas para revisión profesional. El cálculo es lineal elástico; la membrana de losa es un elemento triangular de deformación constante y no sustituye un análisis de shell, elementos finitos detallado, diafragma con aberturas ni análisis no lineal o tiempo-historia. / The program is intended for professional review. Analysis is linear elastic; the slab membrane is a constant-strain triangular element and does not replace a detailed shell/finite-element model, openings, nonlinear or time-history analysis.
