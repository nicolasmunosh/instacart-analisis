# Informe de procesamiento · Instacart

Actividad GA1-220501094-01-AA1-EV06 · Limpieza, análisis y estructuración de datos con Pandas. Este informe se genera desde `notebooks/pandas_labs.ipynb`.

## 1. Resumen

- `orders`: 478.952 pedidos de 157.437 clientes; `products`: 49.694 productos en 134 pasillos y 21 departamentos; `order_prods`: 4.545.007 artículos.
- La tabla maestra combina las cinco tablas: 4.545.007 filas y 14 columnas, sin perder ni duplicar filas (`validate` y `assert`).

## 2. Lección 2 · Limpieza

### Informe de cambios del pipeline

| tabla | paso | filas | nulos | memoria_mb |
| --- | --- | --- | --- | --- |
| orders | 0 crudo | 478967 | 28819 | 23.0 |
| orders | 1 sin duplicados | 478952 | 28817 | 23.0 |
| orders | 2 tipos | 478952 | 28817 | 7.2 |
| products | 0 crudo | 49694 | 1258 | 3.1 |
| products | 1 nombres | 49694 | 0 | 3.1 |
| order_prods | 0 crudo | 4545007 | 836 | 145.4 |
| order_prods | 1 tipos | 4545007 | 836 | 54.5 |

### Decisiones

| Problema | Evidencia | Decisión |
| --- | --- | --- |
| Pedidos duplicados | 15 copias exactas, todas el mismo día y hora | Eliminar |
| Tipos de dato | Enteros de 64 bits y nulos en columnas enteras | Reducir con `astype` y usar `Int16` (nullable) |
| `days_since_prior_order` nulo | Cada nulo es un primer pedido | Conservar el nulo |
| `product_name` nulo | 1258 productos del pasillo y departamento `missing` | Rellenar con `Unknown` |
| `add_to_cart_order` nulo | 836 nulos (Reto 2, ejercicio 3) | Conservar el nulo |
| Nombres con espacios y mayúsculas | Espacios dobles y espacios invisibles | Estandarizar con `.str` |
| Duplicados implícitos | Mismo nombre con `product_id` distintos | Conservar y agrupar por `product_id` |

## 3. Lección 3 · Análisis

- Hora pico de pedidos: las 10 h (40.578 pedidos).
- Clientes por segmento (según su número de pedidos): ver tabla.

| segmento | clientes | pedidos | espera_mediana |
| --- | --- | --- | --- |
| ocasional | 55357 | 55357 | 16.0 |
| regular | 71553 | 191649 | 11.0 |
| frecuente | 30527 | 231946 | 6.0 |

Perfil por franja horaria (% de pedidos de cada segmento):

| segmento | madrugada | mañana | tarde | noche |
| --- | --- | --- | --- | --- |
| ocasional | 2.1 | 30.0 | 49.4 | 18.5 |
| regular | 1.9 | 31.6 | 48.4 | 18.1 |
| frecuente | 1.7 | 35.2 | 45.9 | 17.2 |

## 4. Lección 4 · Combinación

- Auditoría `orders` ↔ `order_prods`: 450.046 pedidos con productos y 28.906 sin productos registrados.
- Tabla por producto: 45.573 productos; se analizaron los 739 con al menos 1.000 pedidos.

Producto más fiel de cada departamento (5 primeros):

| department | product_name | veces | tasa_recompra |
| --- | --- | --- | --- |
| dairy eggs | Whole Organic Omega 3 Milk | 1286 | 0.863 |
| produce | Banana | 66050 | 0.844 |
| snacks | Extra Fancy Unsalted Mixed Nuts | 1436 | 0.822 |
| beverages | Zero Calorie Cola | 1256 | 0.818 |
| bakery | Organic Honey Sweet Whole Wheat Bread | 1607 | 0.799 |

## 5. Archivos generados en `data/processed/`

| archivo | tamaño_MB |
| --- | --- |
| ficha_productos.parquet | 1.69 |
| instacart_maestra.parquet | 103.62 |
| lideres_por_departamento.csv | 0.0 |
| resumen_departamentos.csv | 0.0 |

## 6. Limitaciones

- Los datos no traen fechas calendario ni precios; solo día de la semana y hora.
- 28.906 pedidos no tienen productos registrados.
- `add_to_cart_order` tiene 836 nulos que se conservaron.
- Los productos `Unknown` se conservan; hay que excluirlos al analizar por nombre.
- `instacart_maestra.parquet` es grande y no se sube a GitHub; se regenera ejecutando el cuaderno.