
Todo el programa —interfaz, motor de cálculo, gráficos y persistencia— vive en un único archivo, pensado para ser portable y fácil de auditar.

---

## ⚠️ Limitaciones y avisos

- Herramienta **educativa y de predimensionado**. No sustituye un análisis estructural firmado por un profesional competente ni la verificación con el código local aplicable.
- El análisis es **lineal elástico** de primer orden.
- La losa se resuelve con teoría de placa delgada (Kirchhoff); no se cubre corte por flexión ni placas gruesas.
- El método modal no incorpora todavía la torsión accidental (sí lo hace el ELF).
- Las verificaciones de zapata siguen criterios ACI 318 simplificados; confirma con tu código nacional.
- Revisa siempre el **equilibrio** mostrado en la pestaña de resultados antes de interpretar las salidas.

---

## 📜 Licencia

Este proyecto se distribuye bajo **Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)**.

Puedes compartir y adaptar el material para cualquier propósito, incluso comercial, siempre que:

- **Atribución**: des crédito adecuado a los autores originales, enlaces a la licencia e indiques si hiciste cambios.
- **Compartir Igual**: si remezclas, transformas o creas a partir del material, debes distribuir tus contribuciones bajo la misma licencia.

Texto completo: <https://creativecommons.org/licenses/by-sa/4.0/>

[![CC BY-SA 4.0](https://licensebuttons.net/l/by-sa/4.0/88x31.png)](https://creativecommons.org/licenses/by-sa/4.0/)

---

## 👥 Autoría

- **Autor original**: Dr. Bernardo Deschapelles — base del programa ZAPEL77P.
- **Actualización y desarrollo 3D**: Prof. Raymond Rosa Ávila.

Si usas o adaptas este programa, por favor cita a sus autores y conserva esta licencia.

---

## 🤝 Contribuciones

Las contribuciones son bienvenidas bajo los términos de la CC BY-SA 4.0. Si detectas errores de cálculo, sugerencias de nuevas verificaciones o mejoras de interfaz, abre un *issue* o envía un *pull request* describiendo el cambio y su justificación técnica.
