# Insumos públicos del análisis

El cuaderno `2_Entorno_analisis.ipynb` lee de esta carpeta los tres archivos que exporta el Paso 5 de `1_Entorno_seguro.ipynb`:

| Archivo | Contenido |
|---|---|
| `base_sintetica.csv` | Base sintética cliente–línea–mes (147.507 filas): códigos seudónimos, venta neta, unidades y atributos comerciales agrupados (categorías con menos de 5 clientes → «Otros»). |
| `exogenas_alineadas.csv` | Series macroeconómicas públicas alineadas al mes en que se publican. |
| `exogenas_admitidas.json` | Series admitidas como predictores (8 de 10). |

Ninguno contiene registros reales. Aun así, la base sintética reduce el riesgo de exposición, pero **no garantiza privacidad por sí sola**: en la validación, el 3,4 % de sus registros quedó más cerca de uno real que el percentil 5 de las distancias entre registros reales (umbral: 10 %), y ninguno es una copia.

Por eso la base sintética se comparte según las reglas de la empresa: si su publicación no está autorizada, esta carpeta queda vacía y el repositorio publica el código y los resultados agregados (`resultados/`).
