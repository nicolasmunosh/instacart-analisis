# Instacart · Análisis y estructuración de datos con Pandas

Proyecto del SENA (ADSO, ficha 3223873). Continúa la exploración inicial de Instacart y aplica limpieza, análisis y combinación de datos con Python y Pandas.

**Pregunta orientadora:** ¿cómo preparar y estructurar los datos de Instacart para obtener información confiable sobre los hábitos de compra?

## Estructura

```
instacart-analisis/
├── README.md
├── .gitignore
├── requirements.txt
├── data/
│   ├── raw/          # 5 CSV originales (no se suben a GitHub)
│   └── processed/    # archivos generados por el cuaderno
├── notebooks/
│   ├── 01_exploracion_instacart.ipynb   # actividad anterior
│   └── pandas_labs.ipynb                # lecciones 2, 3 y 4 + labs + retos
└── reports/
    └── processing_report.md             # generado por el cuaderno
```

## Cómo ejecutarlo (Windows)

1. Crear y activar el entorno: `python -m venv .venv` y `.venv\Scripts\activate.bat`
2. Instalar dependencias: `python -m pip install -r requirements.txt` (incluye `pyarrow`, necesario para Parquet)
3. Copiar los cinco CSV a `data/raw/`: `instacart_orders.csv`, `order_products.csv`, `products.csv`, `aisles.csv`, `departments.csv` (separador `;`)
4. Abrir `notebooks/pandas_labs.ipynb`, elegir el kernel `.venv` y ejecutar **Run All**

## Qué contiene `pandas_labs.ipynb`

| Lección | Contenido |
|---|---|
| 2 · Limpieza | Ejemplos, Laboratorio guiado 2 (pipeline con informe de cambios) y Reto 2 (7 ejercicios) |
| 3 · Análisis | Ejemplos, Laboratorio guiado 3 (perfil de hábitos de compra) y Reto 3 (7 ejercicios) |
| 4 · Combinación | Ejemplos, Laboratorio guiado 4 (tabla por producto) y Reto 4 (7 ejercicios) |

## Archivos generados en `data/processed/`

- `instacart_maestra.parquet`: tabla maestra (4.545.007 filas × 14 columnas). No se versiona por tamaño; se regenera.
- `ficha_productos.parquet`: una fila por producto.
- `resumen_departamentos.csv`, `lideres_por_departamento.csv`.

Además se genera `reports/processing_report.md` con el resumen del procesamiento.

## Flujo de Git

`main` ← `develop` ← `feature/pandas-data-processing` (un commit por lección y uno de documentación).
