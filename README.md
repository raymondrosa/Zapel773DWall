ZAPEL 3D
Análisis estructural 3D de pórticos, muros, losas, zapatas y matres con acciones sísmicas (ASCE 7-22 / PRBC 2018).

ZAPEL 3D es una aplicación web autocontenida (un solo archivo HTML) para el análisis matricial de estructuras espaciales de 6 grados de libertad por nodo. Combina un modelador 3D interactivo, cálculo por el método de rigidez, reparto de cargas de superficie, contornos de resultados y análisis sísmico tanto por fuerza lateral equivalente como modal espectral.

✨ Características
Modelado
Barras espaciales con 9 tipos de sección predefinidos: rectangular, viga T, circular maciza, perfil I (con catálogo de perfiles W, IPE), tubo rectangular, tubo circular e irregular (polígono).

Muros con modelo de columna equivalente y brazos rígidos a las esquinas, o como carga lineal sin rigidez.

Losas y cargas de área con reparto por áreas tributarias (diagrama de 45°) a las vigas del nivel.

Zapatas aisladas con verificación ACI 318 (presiones de contacto, punzonamiento, cortante, flexión y acero mínimo).

Matres (losas de fundación) sobre resortes (balasto) con grilla equivalente y contornos de presión y momentos.

Cadenas de barras y polígonos con ajuste a malla, cotas en pantalla, vista isométrica/planta/frente/lateral y unidades imperiales (kip·ft) o SI (kN·m).

Análisis
Pórtico espacial de 6 GDL/nodo por el método de rigidez directa (12×12 por elemento).

Articulaciones internas y resortes verticales/laterales de suelo (kz, ky).

Diagramas de N, Vy, Vz, T, My, Mz, reacciones del suelo y deformada animada.

Contornos de tensión axial en muros (flexión compuesta) y de momentos Mx/My en losas (placa delgada por diferencias finitas).

Sismo
ASCE 7-22 (§12.8, fuerza lateral equivalente) y PRBC 2018 (IBC 2018 / ASCE 7-16).

Coeficientes de sitio Fa/Fv por clase A–F, categoría de riesgo, sistemas resistentes (R, Cd, Ω₀).

Período aproximado Ta, período fundamental por Rayleigh, distribución por nivel, torsión accidental (±5% mínimo, 4 casos).

Verificación de derivas (Δa tabla 12.12-1) y estabilidad (θ ≤ 0,10).

Análisis modal espectral con iteración inversa, combinación CQC (ζ = 5%) y escalado al 85% de V_ELF.

🚀 Uso
Descarga index.html.

Ábrelo en cualquier navegador moderno (Chrome, Edge, Firefox, Safari). No requiere servidor, instalación ni conexión a internet.

Dibuja la estructura con las herramientas de la barra superior o carga un ejemplo:

Ej. edificio — edificio de 2 niveles con muros y losas.

Ej. pórtico — pórtico espacial con carga lateral.

Ej. viga sobre suelo — viga con resortes de balasto.

Ej. viga de acero — viga IPE simplemente apoyada.

Pulsa ▶ Calcular para el caso gravitatorio, o Sismo ASCE 7-22 / PRBC 2018 para el caso sísmico.

Guarda y abre modelos en JSON con 💾 / 📂.

Controles
Acción	Control
Girar la vista	Arrastrar (o clic derecho arrastrado)
Desplazar	Mayús + arrastrar
Zoom	Rueda del ratón o botones ＋ / －
Terminar cadena / polígono	Clic derecho o Esc
Cerrar polígono	Doble clic
Eliminar selección	Delete / Backspace
Pinch-zoom	Dos dedos (táctil)
📐 Modelo de cálculo
Elemento viga 3D de 12×12 con matriz de rigidez completa (axial, torsión, flexión en dos planos) y transformación por cosenos directores.

Condensación estática de articulaciones internas.

Resortes de suelo integrados por cuadratura de Gauss a lo largo del elemento.

Losas: ecuación de placa delgada de Kirchhoff resuelta por diferencias finitas de 13 puntos y Cholesky, con condiciones de borde simplemente apoyadas.

Matres: grilla equivalente de vigas cruzadas sobre resortes, con las reacciones de los pilares como cargas.

Sismo ELF: T por Rayleigh sobre la forma de la primera iteración, k según ASCE 7-22 §12.8.3, torsión accidental con 4 casos envolventes.

Modal: iteración inversa con deflación y masa ortonormal, condensación de GDL sin masa, combinación CQC.

📁 Estructura del proyecto
text
index.html   ← aplicación completa (HTML + CSS + JavaScript)
Todo el programa —interfaz, motor de cálculo, gráficos y persistencia— vive en un único archivo, pensado para ser portable y fácil de auditar.

⚠️ Limitaciones y avisos
Herramienta educativa y de predimensionado. No sustituye un análisis estructural firmado por un profesional competente ni la verificación con el código local aplicable.

El análisis es lineal elástico de primer orden.

La losa se resuelve con teoría de placa delgada (Kirchhoff); no se cubre corte por flexión ni placas gruesas.

El método modal no incorpora todavía la torsión accidental (sí lo hace el ELF).

Las verificaciones de zapata siguen criterios ACI 318 simplificados; confirma con tu código nacional.

Revisa siempre el equilibrio mostrado en la pestaña de resultados antes de interpretar las salidas.

📜 Licencia
Este proyecto se distribuye bajo Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0).

Puedes compartir y adaptar el material para cualquier propósito, incluso comercial, siempre que:

Atribución: des crédito adecuado a los autores originales, enlaces a la licencia e indiques si hiciste cambios.

Compartir Igual: si remezclas, transformas o creas a partir del material, debes distribuir tus contribuciones bajo la misma licencia.

Texto completo: https://creativecommons.org/licenses/by-sa/4.0/

https://licensebuttons.net/l/by-sa/4.0/88x31.png

👥 Autoría
Autor original: Dr. Bernardo Deschapelles — base del programa ZAPEL77P.

Actualización y desarrollo 3D: Prof. Raymond Rosa Ávila.

Si usas o adaptas este programa, por favor cita a sus autores y conserva esta licencia.

🤝 Contribuciones
Las contribuciones son bienvenidas bajo los términos de la CC BY-SA 4.0. Si detectas errores de cálculo, sugerencias de nuevas verificaciones o mejoras de interfaz, abre un issue o envía un pull request describiendo el cambio y su justificación técnica.
