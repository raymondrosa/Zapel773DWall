# VALIDACIÓN FINAL · Zapel773DWall + núcleo nativo Rust

**Documento maestro de verificación.** Fecha: 2026-11-13 · actualizado 2026-11-27 con la app **rev1**.
Zona: America/La_Paz.
Alcance: app web (`Zapel773DWall-index.html`), app de escritorio Electron (`desktop/`), núcleo
nativo Rust (`rust-solver/`), informes nativos y empaquetado. Este documento sustituye como
referencia a `Validacion-Modelo-Completo.md` (que queda como ficha resumida del modelo completo).

---

## 1. Artefactos verificados

| Artefacto | Identificador |
|---|---|
| Núcleo nativo definitivo | `rust-solver/target/release/zapel_solver.exe` · **SHA-256 `D6BEB78BF9C4BBEDEC4D39C5209A3E56F3D1F5FC26CAC65496CBF2928E087609`** · 1.034.923 B · 2026-11-13 12:56:51 |
| Solver empaquetado | `desktop/dist/Zapel773DWall-win32-x64/resources/zapel_solver.exe` · **mismo SHA-256** · 1.034.923 B |
| Copia de trabajo del solver | `desktop/resources/zapel_solver.exe` · mismo SHA-256 |
| App web | `Zapel773DWall-index.html` · SHA-256 `AE942B19E51637B5BACD46613BA382A824C42B0D63620CC0C5E10AA68F308669` · **421.414 B** (rev1, 2026-11-27) |
| Copia empaquetada de la app | `desktop/app.html` · **mismo SHA-256** (byte a byte) · el `app.asar` del paquete lleva la misma huella de integridad para `app.html`; también está sincronizada en `github-package/src/Zapel773DWall.html` |
| Paquete Electron | `desktop/dist/Zapel773DWall-win32-x64/resources/app.asar` · SHA-256 `1E255B5F0BECFC5D451E9D9166A99DCE1CEA94992296D787203D48383872BC41` · 432.819 B · 2026-11-27 11:16 |
| Ejecutable de escritorio | `desktop/dist/Zapel773DWall-win32-x64/Zapel773DWall.exe` · SHA-256 `BA9A34063DA0A2CE7A0DE76BDE51654310F9EC6597C1DBD3ECD0AC0B450143FD` · 249.270.272 B |
| ZIP de distribución | `Zapel773DWall-Escritorio.zip` · SHA-256 `897644A53DA1C2761E661BD30A45952C57D7F84D3EF104074C3995A1C46B7756` · 158.519.411 B · 74 entradas · 2026-11-27 11:17 · incluye `Zapel773DWall-win32-x64\resources\zapel_solver.exe` (1.034.923 B, mismo SHA) |
| Modelo de validación completa | `Zapel773DWall-modelo-completo.json` · SHA-256 `3B323D6FAAE542FF57AEEC1F577DDAB845B17D526885C01DE73366DE6ADDD022` · 408 nudos · 496 barras · 88 muros · 4 losas · 4 cargas de área · 12.882 GDL |
| Herramientas | `tools/oracle/run.js` (oráculo) · `tools/diff.js` (diferencial) · `tools/verify/sismo.js` + `tools/verify/render-md.js` (banco sísmico) |
| Entorno de verificación | Windows; Edge/Chrome headless para el oráculo; `cargo 1.99.1` / `rustc 1.99.1` |

---

## 2. Metodología

La validación no se apoya en “resultados de referencia” escritos a mano: **la referencia es la propia app**,
ejecutada por un oráculo real, y el núcleo Rust se compara contra ella métrica a métrica con tolerancias
explícitas. Para el modelo completo (que la app no puede resolver) se valida con el solver nativo, el
equilibrio global y los informes nativos.

### 2.1 Oráculo real de la app (`tools/oracle/run.js`)

- Copia `Zapel773DWall-index.html` a un archivo temporal y le inyecta un driver al final.
- El driver carga el modelo en los globales de la app (`N`, `B`, `MUROS`, `LOSAS`, `CARGAS`, `ZAPATAS`,
  `PLATEAS`, `ANOT`, `GRUPOS`) y llama a **las funciones de análisis de la propia app**:
  `analyze()` (estático), `sismoCalc()` (ELF), `modalCalc()` (modal espectral CQC) y `vientoCalc()` (viento).
- Fuerza `DESKTOP_MODE=true` y sube el límite denso para poder oráculo modelos medianos.
- Ejecuta en Edge/Chrome headless y devuelve un JSON con lo que la app calcula.

```powershell
node tools/oracle/run.js --model tools/models/m-02-2story.json --mode all --out tools/models/out/m-02-app.json
node tools/oracle/run.js --model modelo.json --mode elf --opts '{"fA":0.25}'
```

### 2.2 Suite diferencial estática (`tools/diff.js`)

Compara oráculo vs Rust por magnitud: `u` (nudos del modelo), `R` (GDL restringidos), `dmax`, `ΣR`,
fuerzas de barra (`N`, `Vy`, `Vz`, `My`, `Mz`), desplazamientos y fuerza en matres, etc.

```powershell
node tools/diff.js --model tools/models/m-02-2story.json --mode all
```

**Cobertura:** 54 fixtures estáticos (`tools/models/m01…m44`, `m-01…m-08`, incluidos los casos de
secciones, articulaciones/resortes, resortes de suelo, muros girados, losas planas/inclinadas/L/triangulares,
matres rotadas y peso propio on/off) → **sin regresiones**: `max|Δu| ≤ 6,3e-15` en todos salvo las
divergencias conocidas documentadas en §7.

### 2.3 Banco sísmico / modal / viento (`tools/verify/sismo.js`)

Ejecuta el oráculo (cacheado en `tools/reports/.cache/`, invalidado si cambian modelo, HTML u opciones) y el
binario Rust con **los mismos parámetros** (`--sds 0.7333333333333333 --sd1 0.4266666666666667 --tl 8 --r 8
--ie Ie --fA 0.25 --ecc 5 --cd 5.5 --riesgo R --modes 10`), y compara:

- **Tolerancias:** exactas ≤1e-6 rel (T, Ta, Cu, k, Cs, W, hn, da, VD…); numéricas ≤1e-4 rel
  (F, kz, pN, FX, V de tabla, kesc…); aceptación ≤1e-2 rel (dmax, u, dr/dd/θ).
- **Casos:** 9 (8 modelos + `m-02` con riesgo 3).
- **Resultado (binario definitivo `D6BEB78B…`): 1.066 métricas comparables en 9 casos, 0 fallos, 28 n/a** (ver `rust-solver/AUDITORIA-SISMO-VERIFICACION.md`
  y `tools/reports/sismo-verify-final.{json,md}`).

```powershell
node tools/verify/sismo.js --tag final
node tools/verify/render-md.js --tag final
```

### 2.4 Modelo completo (12.882 GDL)

La app densa **no puede** resolver este modelo: se bloquea por encima de **9.000 GDL** en escritorio
(`index.html:411`, `MAX_DOF`). Por eso:

- Estático, ELF, modal y viento se calculan con el **solver nativo** y se generan los informes nativos.
- Se comprueba el **equilibrio global** (ΣRz = −ΣFz) y la coherencia interna (dmax, peso, derivas, advertencias).
- La paridad app↔Rust se valida en los modelos medianos de las suites diferenciales (§2.2 y §2.3).
- El modelo completo se usa además como **prueba de escala**: 12.882 GDL / 2.147 nudos internos / 107.473 no-cero.

### 2.5 Pruebas unitarias Rust

`cargo test` sobre 17 módulos (`rust-solver/src/`): **16 pruebas** de sección, matriz, articulaciones,
cargas distribuidas, losas inclinadas, matres, modal, espectro/CQC y solver.

> **Estado:** `cargo test --release` sobre el árbol final → **16 passed; 0 failed** (2026-11-13).

### 2.6 Empaquetado

`desktop/package.json` compila el solver y lo incluye en el paquete de forma reproducible:

```powershell
cd desktop
pnpm run package      # cargo build --release  +  electron-packager --extra-resource=…zapel_solver.exe
```

Verificado: el `.exe` del paquete coincide en SHA-256 con el definitivo, `app.asar` contiene el
`app.html`/`main.js` actuales y el ZIP se regeneró con ambos. El script `package` ignora `dist/`, `node_modules` y
archivos de desarrollo para que reempaquetar sobre una carpeta `dist` existente sea reproducible (sin asar
gigantes).

### 2.7 Validación de la app rev1 (actualización 2026-11-27)

Se recibió `Zapel773DWall-IA-index-rev1.html` (421.414 B) y se verificó **antes de publicarla** contra la app
anterior y contra el núcleo Rust:

- **Motor de cálculo sin cambios numéricos.** El bloque `/*==NUCLEO==*/` de rev1 sólo añade una guarda de
  resultados no finitos (`analyze`) y propagación de errores de estabilidad en `sismoCalc`/`vientoCalc`
  (`despNivel` con guarda). No se quitó ninguna función; se añadieron 40 funciones de revisión/IA. Los valores
  calculados son idénticos a la app anterior (p. ej. m-02: `dmax=0.0011322765861425`, ELF `T=0.24624466197519082`,
  `V=92.2139786416477`, `W=425.76`).
- **Paridad app↔Rust con rev1:** banco sísmico completo **9 casos / 1.066 métricas / 0 fallos** sobre la app
  publicada (`tools/reports/sismo-verify-rev1-publicada.md`); diferencial estático m-02 11/11 y pipeline completo
  m-08 94/94; sin regresiones.
- **Pruebas de las funciones nuevas** (`tools/verify/rev1-smoke.js`): **50/50** comprobaciones en navegador y
  **54/54** simulando el puente de escritorio. Comprueba carga de la app, botones/paneles nuevos, revisión
  automática (modelo limpio = 100 y detección de defecto deliberado = error), construcción local de modelo por IA
  (malla regular 18 nudos/26 barras y polígono 18/25), ausencia de llamadas de red al cargar, y que los botones
  Rust llaman al puente y muestran el resultado.
- **Sin red obligatoria:** la revisión es local; la IA es opcional y solo se invoca a petición del usuario. No hay
  secretos ni claves embebidas en el HTML.
- **Despliegue:** la misma huella SHA-256 `AE942B19…F308669` quedó en `Zapel773DWall-index.html`,
  `desktop/app.html` y `github-package/src/Zapel773DWall.html`; se reempaquetó escritorio y ZIP (§1).

---

## 3. Resultados del modelo completo

Parámetros: app con sus valores por defecto (`SDS=0,733333`, `SD1=0,426667`, `TL=8 s`, `R=8`, `Ie=1`,
`fA=0,25`, excentricidad 5 %, `Cd=5,5`, riesgo 1, 10 modos, viento V=165 mph exposición C). Unidades internas
(m, kN, kN·m); los informes nativos los presentan en ft/kip/in porque el modelo declara `units:"IM"`.

### 3.1 Estático (gravedad + peso propio)

| Magnitud | Valor |
|---|---|
| GDL / elementos / nudos internos | 12.882 / 2.424 / 2.147 |
| No-cero de la matriz | 107.473 |
| **dmax** (máximo real, incluye nudos internos de muros/membranas) | **0,04722 m** (1,8591 in) |
| dmax en los 408 nudos del modelo | 0,01372 m (ver §6-A1 para la diferencia) |
| **Equilibrio vertical** | **ΣRz = −ΣFz = 104.839,03 kN** |
| **Peso sísmico (fA=0,25)** | **88.619,69 kN** (19.922,50 kip) |
| Residual relativo / residual libre máximo | 0 / 6,47e-5 |
| Advertencias | ninguna |
| Tiempo de cálculo | ≈4 s |

### 3.2 Fuerza lateral equivalente (ELF)

| Magnitud | Valor |
|---|---|
| T / Ta / T1 / Tcalc | **0,6883 s** / 0,5017 s / 0,6884 s / 0,6883 s |
| Cu / k | 1,4 / 1,0942 |
| **Cs** | **0,07749** |
| **V** | **6.867,07 kN** (1.543,78 kip) |
| **W** | **88.619,69 kN** (19.922,50 kip) |
| hn | 14,021 m (46,00 ft) |
| dmax | **0,1233 m** (4,8527 in) |
| Niveles sísmicos | **9** (incluye el nivel base z=0; 8 entrepisos) |
| Cd / categoría de riesgo | 5,50 / 1 |
| Advertencias | ninguna |

Nota de geometría: el modelo tiene **dos cotas por losa** (p. ej. 11,25 y 11,50 ft; 22,75 y 23,00 ft…). El ELF
las trata como niveles independientes, igual que la app (tolerancia 1e-3 m); por eso la tabla de derivas
muestra capas de 0,076 m con `da` muy pequeña (`0,02·h`) que salen “revisar”. **Es paridad con la app y
consecuencia de la geometría**, no un defecto del solver: en las capas de entrepiso real (3,43 m) las derivas
cumplen. Tiempo de cálculo ≈4,7 s.

### 3.3 Modal espectral (10 modos, CQC, torsión 5 %)

| Magnitud | X | Y |
|---|---|---|
| Cortante basal (escalado al 85 % del ELF) | **5.837,0 kN** | **5.837,0 kN** |
| Factor de escala `kesc` | **11,6327** | 11,5620 |
| dmax escalado | **1,8459 m** (72,6726 in) | 1,6135 m (63,5246 in) |
| dmax sin escalar (referencia) | **0,1587 m** | — |
| Derivas por entrepiso emitidas | 8 | 8 |
| Períodos de los 10 modos | 1,7566 · 1,5288 · 1,5116 · 1,4797 · 1,4051 · 1,3338 · 1,2708 · 1,2285 · 1,0325 · 0,9906 s | — |

- `analysis.total_weight` = 88.619,69 kN; `analysis.dmax` = 1,8459 m (máximo de ambas direcciones);
  `analysis.dmax_unscaled` = 0,1587 m; `analysis.meff` = **33,77 %**; `analysis.kesc` = 11,6327.
- **Advertencias del solver:** participación modal insuficiente (el cortante se escala ×11,63 para alcanzar el
  85 % del ELF, ASCE 7-16 12.9.4.2) y masa modal efectiva baja (34 % en la dirección más desfavorable).
- **Limitación documentada:** con 10 modos y 672 GDL dinámicos, el análisis modal de **este modelo** no captura
  toda la masa. Los desplazamientos/derivas escalados (1,85 m) **no son representativos**; el resultado
  desescalado (0,159 m) es el que tiene sentido físico. **Para este modelo debe usarse ELF** (o aumentar
  modos y verificar convergencia de participación). El informe `Informe-Rust-Sismo.html` incluye este aviso
  destacado. Tiempo de cálculo ≈3 s (10 modos).

### 3.4 Viento (ASCE 7-22, procedimiento direccional)

| Magnitud | Valor |
|---|---|
| V / exposición | 165 mph / C |
| **qh** | **63,6695 psf** |
| **Cortante basal X / Y** | **477,58 kN** / **612,21 kN** |
| **Momento de vuelco** | **4.243,79 kN·m** |
| Casos | 4 (±X, ±Y) |
| Niveles con viento | **8** |
| dmax de la envolvente | 0,04701 m (1,8506 in) |
| Derivas por entrepiso | 8 valores absolutos (m), **incluida la primera**, `da = dz/400` |
| Advertencias | ninguna |

`qv`, `Kz`, `pN`, `FX`, `FY` por nivel y `tab` de derivas (con `z`, `dz`, `dr`, `da`, `dir`, `ok`) se emiten en
el JSON; el informe nativo lista niveles y derivas. Tiempo de cálculo ≈2,0 s.

---

## 4. Veredicto por subsistema

| Subsistema | Evidencia | Veredicto |
|---|---|---|
| Estático (barras, secciones, div, resortes, articulaciones, cargas lineales) | 54 fixtures, `max|Δu| ≤ 6,3e-15` | **PASS** |
| Reparto de losas/cargas de área, inclinadas y L | fixtures m10…m13, m24…m44 | **PASS** (residual 7e-6 en m13, §7) |
| Membranas de diafragma (nudos libres + regularización) | m10, m12, m20, m-02, m-08 | **PASS** |
| Muros rígidos (incluido muro girado) | m14, m15, m22, m30, m31, m32, m-03 | **PASS** |
| Matres (rotadas, qR, punzonamiento) | m17, m33, m34, m42, m-06 | **PASS** (ver §7.1 sobre peso propio) |
| Peso sísmico con `fA` real | 9 casos, `W` exacto | **PASS** |
| ELF (T, Ta, Cs, V, W, derivas, Cd/riesgo) | 9 casos, 0 fallos | **PASS** |
| Modal espectral (V, kesc, dmax, derivas, CQC) | 9 casos; app patológica en m-03 (§6-A3) | **PASS** (con aviso de participación en el modelo completo) |
| Viento (qh, Vbase, Mbase, pN, FX/FY, derivas) | 9 casos, 0 fallos | **PASS** |
| Integración Electron ↔ Rust | §2.6 y `rust-solver/INTEGRACION.md` | **PASS** |
| Empaquetado reproducible | `.exe` y `app.asar` sincronizados; `package` ignora `dist/` y `node_modules` | **PASS** |
| App rev1 (revisión del modelo + IA local) | motor idéntico; 1.066 métricas PASS; smoke 50/50 web y 54/54 escritorio (§2.7) | **PASS** |
| Informes nativos | 4 informes regenerados con el binario definitivo | **PASS** |
| `cargo test` | 16 pruebas unitarias | **PASS** (16/16) |

---

## 5. Veredicto global

El núcleo nativo **reproduce a la app** dentro de tolerancia en todos los casos verificables
(**1.066/1.066 métricas** en sísmo/modal/viento y **54/54 fixtures** estáticos sin regresión), resuelve
modelos que la app densa no puede (12.882 GDL), mantiene el equilibrio global y está integrado y empaquetado
de forma reproducible. **La única limitación de fondo que queda es física/de modelado, no de implementación:
la participación modal del modelo completo es del 34 %, por lo que su análisis modal no es apto para diseño
(usar ELF).** Los defectos residuales detectados son de la app (muros rígidos en modal, losa inclinada con
diafragma, matre con vértices horarios, dmax sin brazos rígidos) y están documentados en §6.

---

## 6. Bugs y hallazgos de la app (no del núcleo Rust)

Se documentan porque las divergencias remanentes son del lado de la app; el núcleo Rust los resuelve o los
replica sólo cuando se trata de paridad de salida.

| ID | Hallazgo | Evidencia | Impacto |
|---|---|---|---|
| **A1** | El `dmax` de la app **omite los brazos rígidos** de los muros (y por tanto nudos de esquina). | `AUDITORIA-ESTATICO-FIXES.md` §5.2; m14/m30: app 4,18535e-5 m vs máximo real 7,72126e-5 m (−46 %) | La app subestima desplazamientos en modelos con muros. El núcleo Rust reporta el máximo real (en el modelo completo, 0,04722 m incluyendo nudos internos frente a 0,01372 m en los 408 nudos del modelo). |
| **A2** | **Losa inclinada con diafragma activo** → la app falla con “Estructura inestable (mecanismo)”; el nodo central de membrana no queda estabilizado. | `AUDITORIA-ESTATICO-FIXES.md` §2 y §5.3; fixtures m11, m25, m29 (`{"err":"Estructura inestable (mecanismo)…"}`) | La app no puede analizar losas inclinadas con diafragma; Rust sí (regularización diagonal robusta). |
| **A3** | **Modal con muros rígidos inestable**: la app produce `kesc ≈ 2,0·10⁴` (X) y `7,6·10⁵` (Y) y `dmax` de 11,18 m / 293,77 m. | `AUDITORIA-SISMO-VERIFICACION.md` §5; `tools/reports/final-suite.txt` (m-03) | Resultado modal de la app inservible en ese caso; el Rust da valores físicamente razonables (0,0038 m). Se excluye como referencia y se documenta. |
| **A4** | **Matre con vértices en sentido horario** → `plateaCalc0` lanza `TypeError`. | `AUDITORIA-ESTATICO-FIXES.md` §5.4; fixture m43; `index.html:1297-1300` | La app no puede calcular matres giradas con orden horario; Rust sí. |
| **A5** | **CQC de la app suma la deformada gravitatoria en cada modo** (`aplicaModal` ejecuta `analyze()` con las cargas base y `combinaCQC` combina `u` por modo). | `index.html:952` (`aplicaModal`), `index.html:1046-1056` (`modalDir`), `index.html:1060+` (`combinaCQC`) | Desplazamientos modales con un sesgo de paridad; **el núcleo Rust lo replica a propósito** para comparar 1:1. No es un bug del solver. |
| **A6** | **Límite denso**: la app no resuelve por encima de 9.000 GDL (escritorio) / 6.000 (navegador). | `index.html:411` (`MAX_DOF`) | El modelo completo (12.882 GDL) sólo se puede resolver con el núcleo nativo. |
| **A7** | **Matre con peso propio (`sw` on)**: la app suma el peso de la grilla equivalente en ambas direcciones (2,25× el peso real en m-06-mat). **El núcleo Rust replica ese peso por paridad** (`mat.rs`). | `AUDITORIA-ESTATICO-FIXES.md` §5.1; `tools/reports/mat-final.md` (15 métricas, 0 fallos: `qR`, `qRmin`, `qmax`, `qmin` exactos) | Ya no es divergencia. Queda documentado que es una **decisión de modelado de la app** (doble conteo del peso de la grilla), replicada en Rust para comparar 1:1. |
| **A8** | **Losa en L (reparto plano)**: diferencia residual de `max|Δu| = 7,0e-6 m` por desempates de muestreo tributario (`Math.hypot` vs `sqrt`). | `AUDITORIA-ESTATICO-FIXES.md` §5.5; fixture m13 | Diferencia numérica menor, no regresión. |

---

## 7. Limitaciones y divergencias conocidas

1. **Participación modal del modelo completo (34 %, kesc 11,63).** El modal con 10 modos no captura la masa;
   el cortante se escala al 85 % del ELF y los desplazamientos escalados no son representativos. **Usar ELF**
   para este modelo o aumentar modos y verificar convergencia. El informe de sismo lo advierte.
2. **`meff`** (masa modal efectiva): Rust la emite (33,77 % en el modelo completo) y coincide con la app en todos los casos del banco (0,0 %; máximo 1,3–2,1 % en `m-08` por grupos de modos casi degenerados). Es una métrica informativa, no requerida por el banco.
3. **Derivas de viento por caso:** Rust emite las derivas de **cada caso** en `cases[].drifts`, la **envolvente** (`envelope_drifts` + `drift_levels`) y la tabla `tab[]` con `dir`, `dz`, `dr`, `da` y `ok` por entrepiso; coincide exacto con la app.
4. **Modos casi degenerados:** en clusters (p. ej. ~0,0225 s) la repartición de modos entre app y Rust puede
   diferir hasta ~0,5 % en T; fuera de clusters coincide ≤1e-6 y el efecto en V/dmax es despreciable.
5. **Divergencias de modelado de la app:** A1 y A5 (§6) siguen abiertas por decisión de producto; el núcleo Rust las documenta y, cuando corresponde paridad de salida, las replica (A7 ya está replicado).
6. **`cargo test`:** 16 pruebas (`frame` 2, `loads` 3, `mat` 3, `modal` 1, `section` 3, `solver` 2, `spectrum` 2) y **todas pasan**: `cargo test --release` → `16 passed; 0 failed` (verificado en el árbol final).
7. **La web no expone el núcleo Rust:** los botones Rust sólo aparecen en la versión de escritorio
   (`window.ZAPEL_DESKTOP`); la app web sigue usando el solver denso con su límite de GDL.
8. **Timeout de integración:** 600 s por corrida (modelos grandes con modal). Tiempos del modelo completo (10 modos, 12.882 GDL) en el equipo de validación: modal ≈3 s, estático ≈4 s, ELF ≈4,7 s y viento ≈2 s.

---

## 8. Reproducción

```powershell
# 0) compilar el núcleo (genera rust-solver/target/release/zapel_solver.exe)
cd rust-solver
cargo build --release
cd ..

# 1) oráculo de la app (requiere Edge o Chrome; headless)
node tools/oracle/run.js --model tools/models/m-02-2story.json --mode all --out tools/models/out/m-02-app.json

# 2) diferencial estático app vs Rust
node tools/diff.js --model tools/models/m-02-2story.json --mode all

# 3) banco sísmico/modal/viento completo (9 casos, 1.066 métricas)
node tools/verify/sismo.js --tag final
node tools/verify/render-md.js --tag final
#    salidas: tools/reports/sismo-verify-final.json / .md

# 3b) prueba de la app candidata (revisión + IA local + puente Rust simulado)
node tools/verify/rev1-smoke.js --app Zapel773DWall-IA-index-rev1.html
node tools/verify/rev1-smoke.js --app Zapel773DWall-IA-index-rev1.html --desktop
#    salida: tools/reports/rev1-smoke.json

# 4) modelo completo (12.882 GDL) con informes nativos
.\rust-solver\target\release\zapel_solver.exe Zapel773DWall-modelo-completo.json --report Informe-Rust-Estatico.html
.\rust-solver\target\release\zapel_solver.exe Zapel773DWall-modelo-completo.json --elf --fA 0.25 --cd 5.5 --riesgo 1 --report Informe-Rust-ELF.html
.\rust-solver\target\release\zapel_solver.exe Zapel773DWall-modelo-completo.json --sismo --modes 10 --fA 0.25 --cd 5.5 --riesgo 1 --report Informe-Rust-Sismo.html
.\rust-solver\target\release\zapel_solver.exe Zapel773DWall-modelo-completo.json --viento --report Informe-Rust-Viento.html

# 5) pruebas unitarias (ver §7.6)
cd rust-solver; cargo test --release

# 6) empaquetado reproducible (compila el solver y lo incluye en el paquete)
cd ..\desktop; pnpm run package
```

Comprobaciones mínimas esperadas al repetir §8-4: `dmax = 0,04722 m`, `ΣRz = −104.839,03 kN`,
`W = 88.619,69 kN`, `ELF V = 6.867,07 kN`, `ELF T = 0,6883 s`, `modal kesc = 11,6327` con las dos
advertencias de participación, `qh = 63,6695 psf`, `Vbase = 477,58 kN`, `Mbase = 4.243,79 kN·m`.

---

## 9. Evidencia (archivos)

- Auditorías/tareas: `AUDITORIA-INTEGRACION.md`, `AUDITORIA-INTEGRACION-FIXES.md`,
  `rust-solver/AUDITORIA-ESTATICO.md`, `rust-solver/AUDITORIA-ESTATICO-FIXES.md`,
  `rust-solver/AUDITORIA-SISMO.md`, `rust-solver/AUDITORIA-SISMO-VERIFICACION.md`.
- Banco sísmico: `tools/reports/sismo-verify-final.{json,md}`, `sismo-verify-rev1-publicada.{json,md}` (app rev1) y pasadas `p1…p7`, `final2`.
- App rev1: `tools/verify/rev1-smoke.js`, `tools/reports/rev1-smoke.json`, `tools/reports/rev1-deploy-m02.md`, `tools/reports/rev1-deploy-m08.md`.
- Suite estática: `tools/models/out/*` (oráculo), `tools/models/out-fix/*` (candidato),
  `tools/reports/static-m-*.md`, `final2-all-m-02.md`, `final2-all-m-08.md`, `mat-final.md` (matre con peso propio), `walls-final.md`.
- Informes nativos: `Informe-Rust-Estatico.html`, `Informe-Rust-ELF.html`, `Informe-Rust-Sismo.html`,
  `Informe-Rust-Viento.html`.
- Ficha resumida del modelo completo: `Validacion-Modelo-Completo.md`.
- Integración: `rust-solver/INTEGRACION.md`; escritorio: `README_DESKTOP.md`.
