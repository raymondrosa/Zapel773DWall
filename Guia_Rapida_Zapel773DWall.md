# Zapel773DWall — Guía rápida / Quick guide

**Zapel773DWall** es un programa de análisis estructural tridimensional para pórticos, muros, losas con diafragma, zapatas, matres y sismo. Esta guía resume el flujo de trabajo y las herramientas nuevas.

## Español

### 1. Inicio y configuración
- El programa abre en **español**. El selector **Idioma** de la barra permite cambiar a **English** y recuerda la preferencia.
- Muro inicial: **5 pulg de espesor y 8 pies de alto** (editable en la barra o en el panel del muro).
- Losa inicial: **4 pulg de espesor**. Matre inicial: **8 pulg de espesor**.
- La rejilla de dibujo cubre al menos **130 pies** alrededor del modelo, suficiente para edificios de **100 × 100 pies** y varios pisos. El espaciamiento se ajusta con **Malla**.
- Unidades: **kip · ft (Imperial)** o **kN · m (SI)**. Al cambiarlas, las medidas se convierten.

### 2. Dibujar
- **Barra:** clic en nudos; clic derecho o Esc termina la cadena.
- **Muro / Muro poli:** clics en planta; el muro poli se cierra en el primer vértice, con Esc o doble clic. El panel permite variar la altura por vértice.
- **Losa / Carga de área:** polígono en planta; clic derecho, Esc o doble clic cierra. Cambia el **Nivel** entre clics para una losa inclinada o rampa.
- **Apoyos:** Fijo, Artic., Rodillo o Libre; clic en cada nudo.
- **Zapata / Matre:** clic sobre un nudo con apoyo. La matre se define por su polígono.

### 2b. Selección, copia, grupos y mover
- **Clic** selecciona un elemento; **Ctrl+clic** añade o quita; **Alt+clic** dentro de un grupo selecciona un solo elemento.
- **Arrastrar en vacío:** de izquierda a derecha selecciona por **ventana** (todo dentro); de derecha a izquierda por **cruce** (toca el objeto).
- **Copiar / duplicar** o **Ctrl+D** duplica la selección con un desplazamiento; después puedes moverla.
- **Ctrl+G** agrupa la selección; **Ctrl+Mayús+G** desagrupa. Los grupos se guardan con el modelo.
- **Mover:** selecciona, pulsa **Mover**, haz clic en un punto base de esquina (snap **FIN**) y luego en el destino. **Esc** cancela.
- Al colocar una copia sobre nudos existentes, los nudos coincidentes se **fusionan**: se conserva el apoyo del nudo original y se descarta el apoyo de la copia en la unión, para que el nudo se desplace con la estructura original.
- Al **abrir un modelo guardado** también se fusionan los nudos coincidentes. Si la copia comparte **un solo nudo**, el resto trabaja como voladizo; conecta más nudos coincidentes si necesitas continuidad en toda la copia.
- Al fusionar nudos se eliminan **barras, muros o zapatas superpuestos**, conservando el original, para no duplicar rigidez ni peso.
- El cursor es de cruz; **botón central o Alt + arrastrar** orbita alrededor del punto bajo el cursor, como SketchUp. El botón derecho también gira y Mayús+arrastrar desplaza.

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
- El cálculo local **Mx/My** se hace para **cada losa**. En losas no rectangulares se usa el rectángulo envolvente y el contorno se recorta al polígono real; es una aproximación para revisión. Es independiente de la membrana y no incluye aberturas ni acoplamiento con la flexión fuera del plano.

### 5. Muros en tracción y compresión
- El contorno **σ en muros** usa signo: **positivo = tracción**, **negativo = compresión**.
- La escala se centra en cero cuando hay ambos signos; si todo el muro está en un solo estado, usa el rango real.
- La combinación modal conserva el signo de la tensión en el CQC y en la envolvente X/Y con torsión.

### 6. Calcular y revisar
- Pulsa **Calcular**. La barra de progreso indica la fase y el porcentaje.
- Revisa avisos, reacciones, deformada, derivas y estabilidad.
- **Resultados:** modelo, deformada, σ en muros, presiones y momentos de matres, zapatas, Mx/My de losas, σ1/σ2 de membrana y diagramas de barras.
- **Sismo:** fuerza lateral equivalente, modal espectral CQC o espectro de evento. Después, el botón **Sismo** activa la envolvente sísmica en los contornos.

### 6b. Viento
- **Viento…** aplica el procedimiento direccional de **ASCE 7-22** o **PRBC 2018 (ASCE 7-16)** para edificio cerrado.
- Calcula **Kz, qz, presiones de barlovento/sotavento, succión de techo**, cuatro casos **±X/±Y**, cortante basal, vuelco y derivas H/400.
- Después del análisis, el botón **Viento** de Resultados permite alternar entre la envolvente de viento y la gravedad; muestra **Viento ✓** cuando está activo y es excluyente con **Sismo ✓**.
- La **V básica es editable**: verifica el mapa oficial para el sitio y la categoría de riesgo. PRBC 2018 se basa en ASCE 7-16; revisa enmiendas locales.
- **Huracán María 2017:** usa V = 155 mph de viento máximo sostenido reportado por el NHC como **escenario de referencia**. No es una velocidad básica ASCE de 3 s ni una reconstrucción histórica.
- Es un método estático direccional; no sustituye túnel de viento, análisis de aberturas, presiones locales de cubierta ni combinaciones de servicio.

### 6c. Revisar el modelo
- **Revisar** abre la revisión automática: puntaje 0–100, errores, avisos y sugerencias agrupados por tema (definición, nudos, barras, secciones, apoyos, cargas, losas, muros, sismo, viento, fundaciones).
- Los filtros permiten ver solo una gravedad o un tema; **Ir** selecciona y centra el elemento afectado en el modelo.
- Incluye una **prueba de estabilidad** (análisis de prueba) y un resumen descargable para la memoria.
- La comprobación es **local y determinista**: no usa internet y no modifica el modelo.
- Opcionalmente, el botón de IA de la revisión pide una **segunda opinión** al motor configurado en **IA…** (solo se envía un resumen del modelo).

### 6d. Crear el modelo con IA
- **🤖 IA…** abre el asistente: describe la estructura con palabras (por ejemplo, «edificio de 4 niveles, 3 vanos de 20 ft en X y 2 de 24 ft en Y, entrepisos de 12 ft, muros de corte en los ejes extremos»).
- Elige el motor: **Gratis sin clave** (servicio integrado) u otro proveedor con tu clave (OpenAI, Anthropic, OpenRouter, xAI, Mistral o un servicio compatible con OpenAI).
- La IA devuelve una **especificación JSON**, no resultados de cálculo. El programa la valida, la muestra para revisión y **Construir modelo** la convierte en nudos, barras, losas, cargas y muros editables.
- Tras construirla puedes **Revisar**, calcular, corregir y guardar como cualquier otro modelo.
- Las funciones de IA requieren conexión; **el cálculo y la revisión local funcionan sin internet**.

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
- **Rueda:** zoom.
- **Botones Vista → Órbita / Pan:** elige el modo y arrastra con el botón izquierdo.
- **Botón central o Alt + arrastrar:** órbita alrededor del punto bajo el cursor, como SketchUp.
- **Botón derecho + arrastrar:** órbita alternativa.
- **Shift + arrastrar:** desplazar.

---

## English

### 1. Start-up
- The program opens in **Spanish**. The **Language** selector switches to **English** and remembers the choice.
- Default wall: **5 in thick, 8 ft high** (editable in the bar or wall panel).
- Default slab: **4 in thick**. Default mat: **8 in thick**.
- The drawing grid covers at least **130 ft** around the model, enough for **100 × 100 ft** buildings with several stories. Spacing is set with **Grid**.
- Units: **kip · ft (Imperial)** or **kN · m (SI)**; values convert when changed.

### 2. Drawing
- **Beam:** click nodes; right-click or Esc ends the chain.
- **Wall / Poly wall:** plan clicks; close at the first vertex, with Esc or double-click. The panel allows a different height per vertex.
- **Slab / Area load:** plan polygon; right-click, Esc or double-click closes. Change **Level** between clicks for a sloped slab or ramp.
- **Supports:** Fixed, Pin, Roller or Free; click each node.
- **Footing / Mat:** click a supported node. The mat is defined by its polygon.

### 2b. Selection, copy, groups and move
- **Click** selects; **Ctrl+click** adds/removes; **Alt+click** inside a group selects one element.
- **Drag on empty space:** left-to-right selects a **window** (fully inside); right-to-left selects a **crossing** (touches).
- **Copy / duplicate** or **Ctrl+D** duplicates the selection with an offset; then you can move it.
- **Ctrl+G** groups; **Ctrl+Shift+G** ungroups. Groups are saved with the model.
- **Move:** select, press **Move**, click a corner base point (FIN snap), then the destination. **Esc** cancels.
- When a copy is placed on existing nodes, coincident nodes are **merged**: the original node support is kept and the copied node support is discarded at the junction, so the node moves with the original structure.
- Coincident nodes are also merged when **opening a saved model**. If the copy shares only **one node**, the rest works as a cantilever; connect more coincident nodes if full continuity is required.
- Node fusion removes **overlapping bars, walls or footings**, keeping the original, so stiffness and weight are not counted twice.
- The cursor is a crosshair; right-drag still rotates and Shift+drag pans.

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

### 6b. Wind
- **Wind…** applies the ASCE 7-22 or PRBC 2018 (ASCE 7-16) directional procedure for enclosed buildings.
- Computes **Kz, qz, windward/leeward pressures, roof suction**, four **±X/±Y** cases, base shear, overturning and H/400 drift.
- The **basic V is editable**: verify the official site/risk-category map. PRBC 2018 is based on ASCE 7-16; check local amendments.
- **Hurricane Maria 2017:** uses V = 155 mph reported maximum sustained wind as a **reference scenario**. It is not an ASCE 3-second basic wind speed or a historical reconstruction.
- It is a static directional method; it does not replace wind-tunnel testing, opening analysis, local roof pressures or service combinations.

### 6c. Review the model
- **Review** opens the automatic review: a 0–100 score with errors, warnings and tips grouped by topic (definition, nodes, beams, sections, supports, loads, slabs, walls, seismic, wind, foundations).
- Filters show one severity or topic; **Go** selects and zooms to the affected element.
- It includes a **stability test** (trial analysis) and a downloadable summary for the calculation report.
- The check is **local and deterministic**: no internet and no model changes.
- Optionally, the review's AI button asks the engine configured in **AI…** for a **second opinion** (only a model summary is sent).

### 6d. Build the model with AI
- **🤖 AI…** opens the assistant: describe the structure in words (for example, "4-story building, 3 bays of 20 ft in X and 2 of 24 ft in Y, 12 ft stories, shear walls on the outer X axes").
- Choose the engine: **Free, no key** (integrated service) or another provider with your key (OpenAI, Anthropic, OpenRouter, xAI, Mistral or any OpenAI-compatible service).
- The AI returns a **JSON specification**, not analysis results. The program validates it, shows it for review, and **Build model** turns it into editable nodes, beams, slabs, loads and walls.
- After building it you can **Review**, analyse, correct and save it like any other model.
- AI features require an internet connection; **analysis and local review work offline**.

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
- **Wheel:** zoom.
- **View → Orbit / Pan buttons:** choose the mode and drag with the left button.
- **Middle button or Alt + drag:** orbit around the point under the cursor, SketchUp-style.
- **Right button + drag:** alternative orbit.
- **Shift + drag:** pan.

---

### Rendimiento y modelos grandes

- La barra de cálculo muestra una **actividad continua** mientras el programa trabaja; en modelos grandes el porcentaje puede no avanzar por etapas porque el cálculo ocupa el hilo del navegador.
- Si Chrome/Edge muestra **«La página no responde»**, elige **Esperar**. El cálculo continúa y no se pierde el modelo.
- En Windows puedes abrir Edge o Chrome con el parámetro `--disable-hang-monitor` para desactivar ese aviso mientras trabajas con edificios grandes.
- Para modelos muy grandes, usa **fuerza lateral equivalente** en lugar de modal espectral o reduce el número de modos; el análisis modal es el más costoso.
- Guarda el modelo antes de un cálculo muy largo.

### Performance and large models

- The progress bar shows **continuous activity** while the program works; on large models the percentage may not advance in stages because the calculation occupies the browser thread.
- If Chrome/Edge shows **"Page unresponsive"**, choose **Wait**. The calculation continues and the model is not lost.
- On Windows you can launch Edge or Chrome with `--disable-hang-monitor` to suppress that prompt while working with large buildings.
- For very large models use **equivalent lateral force** instead of modal response spectrum, or reduce the number of modes; modal analysis is the most expensive.
- Save the model before a long calculation.

**Nota / Note:** La memoria y la guía están pensadas para revisión profesional. El cálculo es lineal elástico; la membrana de losa es un elemento triangular de deformación constante y no sustituye un análisis de shell, elementos finitos detallado, diafragma con aberturas ni análisis no lineal o tiempo-historia. / The program is intended for professional review. Analysis is linear elastic; the slab membrane is a constant-strain triangular element and does not replace a detailed shell/finite-element model, openings, nonlinear or time-history analysis.

### 9. Modelos grandes y memoria
- El solver global es **denso**: la memoria crece aproximadamente con **(nudos × 6)²**. Por eso un modelo con muchos nudos o subdivisiones puede agotar la memoria del navegador.
- Reduce **div** (subdivisiones) de las barras, simplifica losas/muros y evita subdivisiones innecesarias antes de calcular.
- Cierra otras pestañas, usa Chrome/Edge de 64 bits y reinicia el navegador. Para modelos muy grandes, divide el modelo en partes.
- El programa avisa cuando el modelo supera el límite del solver denso: ~3500 GDL como advertencia y ~6000 GDL como bloqueo **en el navegador**; en la **versión de escritorio** el aviso es a ~5000 GDL y el bloqueo a ~9000 GDL, contando los nudos de subdivisión.
- Si necesitas miles de nudos, usa la **versión de escritorio** y sus botones **Rust β**: el núcleo nativo disperso no tiene el límite de 9000 GDL (resolvió el modelo de validación de 12.882 GDL).

### 9. Large models and memory
- The global solver is **dense**: memory grows roughly with **(nodes × 6)²**. A model with many nodes or subdivisions can exhaust browser memory.
- Reduce bar **div** (subdivisions), simplify slabs/walls and avoid unnecessary subdivisions before running.
- Close other tabs, use 64-bit Chrome/Edge and restart the browser. For very large models, split the model.
- The app warns above ~3500 DOF and stops above ~6000 DOF **in the browser**; the **desktop version** warns at ~5000 DOF and stops at ~9000 DOF, counting subdivision nodes.
- Thousands of nodes require the **desktop version** with its **Rust β** buttons: the native sparse core has no 9000-DOF limit (it solved the 12,882-DOF validation model).

### 10. Versión de escritorio y núcleo nativo Rust / Desktop version and native Rust core

**Español.** La **versión de escritorio** (Electron) añade cuatro botones nativos que ejecutan `zapel_solver.exe` sobre el modelo actual:

- **Rust β**: análisis estático (nudos, GDL, no-cero, dmax, peso sísmico, presiones de matres, deformada y contornos).
- **Rust sísmico β**: diálogo con norma (ASCE 7-22 / PRBC 2018), método **ELF / modal CQC / evento**, clase de sitio, riesgo, Ss, S1, TL, fA, excentricidad, Cd y nº de modos. Muestra cortante, dmax, **derivas por entrepiso** (`z, dr, dd, da, θ, estado`) y deformadas por caso.
- **Rust ELF β**: fuerza lateral equivalente con los valores de la ventana **Sismo** (T, Ta, Cs, V, W, niveles y derivas con Cd y deriva admisible según riesgo).
- **Rust viento β**: viento ASCE 7-22 (V, exposición, Kzt, Kd, Ke, G, Cp, GCpi, succión de techo), cortante basal, vuelco, presiones por nivel y derivas envolventes.

Los análisis usan **los mismos parámetros que la app** (incluida la fracción de cargas de área `fA` en la masa, el `Cd` y la categoría de riesgo). El botón **Validar Rust vs app** compara resultados en una sola unidad y avisa si el modelo o los parámetros cambiaron. El tiempo máximo por corrida es de 600 s y, si se pidió, se puede **descargar el informe HTML** generado por el núcleo.

**Aviso del modelo completo:** en modelos muy grandes con 10 modos la **participación modal puede ser insuficiente** (en el modelo de validación, 34 %); el solver lo advierte y sus desplazamientos escalados no son representativos: **usa ELF** para ese modelo. Detalles y cifras en `VALIDACION-FINAL.md`.

**English.** The **desktop version** (Electron) adds four native buttons that run `zapel_solver.exe` on the current model:

- **Rust β**: static analysis (nodes, DOF, non-zeros, dmax, seismic weight, mat pressures, deformed shape and contours).
- **Rust seismic β**: dialog with code (ASCE 7-22 / PRBC 2018), method **ELF / modal CQC / event**, site class, risk category, Ss, S1, TL, fA, eccentricity, Cd and number of modes. Shows base shear, dmax, **story drifts** (`z, dr, dd, da, θ, status`) and per-case deformed shapes.
- **Rust ELF β**: equivalent lateral force using the app's **Seismic** window values (T, Ta, Cs, V, W, levels and drifts with Cd and risk-category allowable drift).
- **Rust wind β**: ASCE 7-22 wind (V, exposure, Kzt, Kd, Ke, G, Cp, GCpi, roof suction), base shear, overturning, level pressures and envelope drifts.

The analyses use **the same parameters as the app** (including the area-load fraction `fA` in the seismic mass, `Cd` and risk category). The **Validate Rust vs app** button compares results in a single unit system and warns if the model or parameters changed. Each run is limited to 600 s and, when requested, the **native HTML report** can be downloaded.

**Full-model warning:** in very large models with 10 modes the **modal participation may be insufficient** (34 % in the validation model); the solver warns about it and its scaled displacements are not representative: **use ELF** for that model. Details and figures in `VALIDACION-FINAL.md`.
