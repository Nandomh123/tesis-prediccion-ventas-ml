# Predicción de ventas con machine learning en una distribuidora farmacéutica

Código y resultados de la tesis de maestría:

> **Modelos de predicción de ventas mediante técnicas de machine learning en una empresa retail dedicada a la distribución de productos farmacéuticos**
> Jonathan Fernando Pallasco Catota · Tutor: Manuel Eugenio Morocho Cayamcela
> Maestría en Inteligencia de Negocios y Ciencia de Datos · Universidad de las Américas (UDLA), Ecuador · 2026

## Resumen

La empresa fija hoy la meta mensual de cada cliente en cada línea con el promedio de los últimos seis meses. Este proyecto compara ese método con cinco algoritmos de machine learning (Random Forest, perceptrón multicapa, XGBoost, LightGBM y CatBoost) y con Croston–SBA, para pronosticar la venta neta mensual por cliente y línea a uno a seis meses.

Como los datos son confidenciales, el trabajo se divide en dos entornos:

```
┌──────────────── Entorno seguro (empresa) ────────────────┐      ┌──────────── Entorno de análisis ────────────┐
│ Base real: 865.900 registros (ene 2022 – sep 2026)        │      │ Base sintética + 10 series públicas          │
│ → seudónimos HMAC-SHA256 (la clave no sale de aquí)       │ ───► │ → pronóstico directo multihorizonte (h=1…6)  │
│ → panel cliente–línea–mes                                 │ solo │ → 5 algoritmos + Croston–SBA + método actual │
│ → base sintética (cadena de Markov + CTGAN) y validación  │ base │ → evaluación, combinación, interpretabilidad │
└───────────────────────────────────────────────────────────┘ sint.└──────────────────────────────────────────────┘
```

## Resultados principales (prueba: abril – septiembre de 2026, base sintética)

| Método | WAPE |
|---|---|
| **Combinación Random Forest + Croston–SBA** | **0,643** |
| Croston–SBA | 0,656 |
| Random Forest | 0,669 |
| Método actual de la empresa | 0,727 |
| CatBoost | 0,742 |
| LightGBM | 0,752 |
| XGBoost | 0,769 |
| Perceptrón multicapa | 0,937 |

- La combinación reduce el WAPE en 8,4 puntos frente al método actual (IC 95 % bootstrap: −10,8 a −6,2).
- Su intervalo del 80 % contuvo el 78,7 % de las ventas observadas.
- Todos los métodos subestimaron el total del semestre; los resultados se obtuvieron con la base sintética y deben validarse con datos reales antes de usarse en la empresa.

## Estructura

```
├── 1_Entorno_seguro.ipynb         # seudonimización, panel, base sintética y su validación (solo en la empresa)
├── 2_Entorno_analisis.ipynb       # todo el análisis de los capítulos IV y V (14 pasos)
├── Data/
│   ├── exogenas_completadas.csv   # valores de series públicas tomados de boletines oficiales, con su fuente
│   └── publico/                   # insumos del análisis (ver Data/publico/LEEME.md)
├── resultados/
│   ├── entorno_seguro/            # tablas y figuras agregadas de auditoría y validación de la base sintética
│   └── analisis/                  # tablas (R00–R31) y figuras de la tesis
├── requirements.txt
└── requirements_entorno_seguro.txt
```

## Cómo reproducir el análisis

1. Python 3.10 o superior: `pip install -r requirements.txt`
2. Coloca en `Data/publico/` los tres archivos que exporta el entorno seguro (ver `Data/publico/LEEME.md`).
3. Abre `2_Entorno_analisis.ipynb` (las rutas son relativas a la carpeta del cuaderno) y ejecuta todas las celdas. La semilla es `20240601`.
4. Las tablas y figuras se guardan en `Data/Analisis/outputs/` y se pueden comparar con las de `resultados/analisis/`.

## Datos y privacidad

- **No se publican** la base real, la clave HMAC, la consulta de extracción ni el cuaderno seguro ejecutado.
- La seudonimización no equivale a anonimización, y la generación con CTGAN tampoco garantiza privacidad por sí sola. La base sintética se comparte solo según las reglas de la empresa.
- Las tablas de `resultados/` son agregadas; no incluyen pronósticos por cliente.
- El tratamiento de datos sigue la Ley Orgánica de Protección de Datos Personales del Ecuador (2021).

## Licencia

Código bajo licencia MIT (ver `LICENSE`). Las series macroeconómicas provienen de fuentes oficiales citadas en `Data/exogenas_completadas.csv` y en la tesis.
