# Caso Empresarial — Análisis de Ventas en Power BI

Proyecto de análisis y visualización de ventas construido en **Power BI Desktop**, con un modelo de datos en estrella, transformaciones en **Power Query** y un dashboard de resumen de ventas.

## Descripción

El caso parte de dos fuentes de datos (ventas transaccionales y objetivos mensuales) y construye un modelo que permite responder preguntas de negocio como:

- ¿Cuánto se vendió en total y por año?
- ¿Quiénes son los 3 mejores clientes y los 10 SKU con más ventas?
- ¿Cómo se comparan las ventas reales contra el objetivo mensual?
- ¿Qué parte de las ventas corresponde a cada tasa de impuesto (10% y 15%)?

## Fuentes de datos

| Fuente | Contenido |
|---|---|
| Ventas (Excel) | Transacciones con ID de transacción, vendedor, cliente, factura, SKU, fecha de venta, fecha de envío, cantidad, precio unitario y tasa de impuesto. Hay 1,000 transacciones y datos de 2016 en adelante. |
| Objetivo de ventas (Excel) | Objetivo mensual por año (2016–2019), con mes, año y monto objetivo. |

## Proceso

### 1. Transformación de datos (Power Query)

Pasos aplicados sobre la tabla **Ventas**, según la configuración de consulta:

1. Conexión al origen y navegación a la hoja.
2. Promoción de encabezados y eliminación de filas superiores.
3. Cambio de tipos de datos (número entero, fechas, decimales).
4. Eliminación de columnas innecesarias.
5. Conversión de la **Tasa de Impuesto** dividiendo entre 100 (de `10` o `15` a `0.10` o `0.15`) para poder usarla como porcentaje.

La tabla **Objetivo de ventas** se cargó como segunda consulta, sin transformaciones adicionales.

### 2. Modelo de datos (esquema en estrella)

La tabla de hechos **Ventas** se relaciona con varias dimensiones:

- `dimCliente` (ID Cliente)
- `dimFactura` (ID Factura)
- `dimSKU` (ID SKU)
- `dimTransacción` (ID Transacción)
- `dimVendedor` (ID Vendedor)
- `Calendario` (tabla de fechas, con año, mes, año-trimestre, día de la semana, etc.)
- `Objetivo de ventas` (vinculada al calendario por año y mes)

Las relaciones son de uno a muchos (1 → *) desde las dimensiones hacia la tabla de ventas.

### 3. Medidas y cálculos

Medidas creadas en DAX, entre ellas:

- **Suma Subtotal**, **Suma Total**, **Ventas Totales**
- **Ventas con tasa de impuesto 10** y **Ventas con tasa de impuesto 15**, y el conteo de productos en cada tasa
- **Part. Ventas** (participación de cada cliente o SKU sobre el total)
- **Objetivo 2019**, **Suma Objetivo**, **Cump objetivo** (cumplimiento = ventas reales / objetivo)
- **Ventas semana 34 – 2018**

### 4. Dashboard "Resumen de ventas"

El reporte incluye:

- **Tarjetas KPI:** ventas totales (≈ $231.88 millones), ventas con tasa 10% (≈ $1.22 millones) y 15% (≈ $230.66 millones), y número de productos por tasa.
- **Top 3 clientes:** suma de subtotal, suma total y participación. Juntos representan ≈ 6.32% de las ventas.
- **Top 10 SKU:** con suma de subtotal, suma total y participación. Juntos representan ≈ 32.55% de las ventas. El SKU 219 es el de mayor peso (≈ 6.59%).
- **Ventas totales por año:** línea mensual comparando 2016, 2017, 2018 y 2019.
- **Cumplimiento a ventas 2019:** barras mensuales en % contra el objetivo, con colores según el nivel de cumplimiento (por ejemplo, abril 123%, febrero 83%, diciembre 88%).

## Hallazgos principales

- La gran mayoría de las ventas se concentra en la tasa de impuesto del 15%.
- El cumplimiento de objetivos en 2019 varía mucho según el mes: hay meses por encima del 100% (abril, julio, marzo) y meses claramente por debajo (febrero, diciembre).
- Los 10 SKU principales explican cerca de un tercio de las ventas, mientras que los 3 clientes principales explican una proporción pequeña, lo que indica una base de clientes diversa.

## Capturas

### Panel de datos y modelo
![Panel de datos](Creacion%20de%20medidas%20Dax.png)

### Dashboard de resumen de ventas
![Dashboard](Dashboard%20caso%20de%20estudio.png)

### Modelo de datos (esquema en estrella)
![Modelo](Modelado%20tipo%20estrella.png)

### Power Query: tabla Ventas
![Power Query](Tabla%20de%20ETL.png)

### Fuente Excel: Ventas
![Fuente ventas](Tabla%20de%20hechos.png)

### Fuente Excel: Objetivo de ventas
![Fuente objetivos](Tabla%20de%20objetivos.png)

## Estructura del repositorio

```
Caso-Empresarial/
├── README.md
└── imagenes/
    ├── 01-panel-datos-modelo.png
    ├── 02-dashboard-resumen-ventas.png
    ├── 03-modelo-estrella.png
    ├── 04-power-query-ventas.png
    ├── 05-fuente-ventas-excel.png
    └── 06-fuente-objetivos-excel.png
```

## Pendiente

El archivo `.pbix` / `.pbip` y los Excel fuente no se incluyen aquí. Si quieres subirlos, revisa primero que no contengan datos sensibles y, si el `.pbix` supera 50 MB, usa Git LFS.
