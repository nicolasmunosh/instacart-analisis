# Reporte de calidad de datos · Instacart

(exploración de datos de Instacart con Pandas). Diagnóstico de los cinco archivos CSV (separador `;`) **antes** de limpiarlos. El detalle del código está en `notebooks/01_exploracion_instacart.ipynb`.

## 1. Inventario

| Archivo                | Filas     | Columnas | Contenido             |
| ---------------------- | --------- | -------- | --------------------- |
| `instacart_orders.csv` | 478.967   | 6        | Un pedido por fila    |
| `order_products.csv`   | 4.545.007 | 4        | Un artículo por fila  |
| `products.csv`         | 49.694    | 4        | Catálogo de productos |
| `aisles.csv`           | 134       | 2        | Pasillos              |
| `departments.csv`      | 21        | 2        | Departamentos         |

## 2. Hallazgos

| Tabla                       | Hallazgo                                                         | Cantidad                                                                          | Gravedad        |
| --------------------------- | ---------------------------------------------------------------- | --------------------------------------------------------------------------------- | --------------- |
| `orders`                    | Filas duplicadas exactas (mismo `order_id`)                      | 15 pedidos repetidos (30 filas), todos miércoles a las 2 h                        | Alta            |
| `orders`                    | `days_since_prior_order` nulo                                    | 28.819 (28.817 tras quitar duplicados); corresponden a primeros pedidos           | Baja (esperado) |
| `orders`                    | Rangos                                                           | `order_dow` 0–6, `order_hour_of_day` 0–23, `order_number` 1–100, espera 0–30 días | Sin problema    |
| `products`                  | `product_name` nulo                                              | 1.258, todos del pasillo y departamento `missing`                                 | Media           |
| `products`                  | Espacios dobles en nombres                                       | 403                                                                               | Media           |
| `products`                  | Espacio invisible (no separable) en nombres                      | 16                                                                                | Media           |
| `products`                  | Nombres repetidos con `product_id` distintos (tras estandarizar) | 141                                                                               | Media           |
| `order_products`            | `add_to_cart_order` nulo                                         | 836, en pedidos de más de 64 productos (límite del registro)                      | Baja            |
| `order_products` ↔ `orders` | Pedidos sin productos registrados                                | 28.906 de 478.952                                                                 | Media           |
| `aisles`, `departments`     | Nulos, duplicados o espacios                                     | 0                                                                                 | Sin problema    |

## 3. Impacto si no se corrige

- Los duplicados inflan el conteo de pedidos y el de la hora pico.
- Los nombres con espacios o mayúsculas distintas hacen que un mismo producto se cuente como varios.
- Los tipos de 64 bits desperdician memoria en una tabla de 4,5 millones de filas.

## 4. Decisiones propuestas (se aplican en `pandas_labs.ipynb`)

| Problema                      | Decisión                               |
| ----------------------------- | -------------------------------------- |
| Pedidos duplicados            | Eliminar                               |
| `days_since_prior_order` nulo | Conservar (primer pedido)              |
| `product_name` nulo           | Rellenar con `Unknown`                 |
| Espacios y mayúsculas         | Estandarizar con `.str`                |
| Nombres repetidos             | Conservar y agrupar por `product_id`   |
| `add_to_cart_order` nulo      | Conservar                              |
| Pedidos sin productos         | Conservar y usar `left` en las uniones |

## 5. Limitaciones

- No hay fechas calendario ni precios, solo día de la semana y hora.
- Este reporte solo describe el estado inicial; la limpieza está en `reports/processing_report.md`.
