# Resumen Técnico: Modelo y Tablero de Logística y Ventas en Power BI

Este documento sintetiza la estructura del modelo relacional y la composición del tablero ejecutivo de control logístico y comercial.

---

## 1. Modelo de Datos (Data Model)

El modelo implementa un **esquema de constelación de hechos** (o estrella múltiple) que vincula dos procesos de negocio principales a través de dimensiones compartidas.

### Tablas de Hechos (Fact Tables)
* **`pedidosFinal`**: Registra la demanda solicitada (pedidos, artículos, cantidades requeridas y fechas).
* **`despachosFinal`**: Registra la ejecución logística y entregas efectivas de los productos.

### Tablas de Dimensiones Conformed (Shared Dimensions)
* **`articulos`**: Catálogo de productos (`k_sc_codigo_articulo`, `sc_detalle_articulo`), con relaciones de uno a varios ($1:*$) hacia ambas tablas de hechos.
* **`terceros`**: Maestro de clientes/proveedores (`ka_nl_tercero`, `sc_nombre`), conectado a ambas tablas de hechos ($1:*$) y a su vez relacionado con `ciudades`.
* **`ciudades`**: Ubicación geográfica (`ka_ni_ciudad`, `sc_nombre_ciudad`).
* **`Calendario`**: Dimensión temporal estructurada por fecha, año y mes para análisis de tendencias y cálculos de Time Intelligence.

### Soporte y Gobernanza
* **`medidasDAX`**: Tabla desconectada destinada exclusivamente al almacenamiento centralizado de métricas y cálculos DAX.
* **`kn_nl_art_Repetidos`**: Tabla complementaria auxiliar (tabla de control de calidad de datos o staging).

---

## 2. Tablero de Control Ejecutivo (Dashboard KPIs)

El reporte visualiza el cumplimiento del despacho frente a los pedidos solicitados, con segmentación temporal y por cliente.

### Panel Lateral de Estado (Sidebar Izquierda)
* **Clientes Activos:** Conteo actual de clientes en el periodo filtrado (1).
* **Número de despachos:** Total de órdenes de salida procesadas (10).
* **Unidades Pendientes:** Brecha o backorder por entregar (1.080 uds).
* **Participación Cliente (PD):** Concentración del cliente seleccionado (100%).
* **Acción rápida:** Botón interactivo superior para *borrar filtros*.

### Indicadores Principales (Tarjetas Superiores y Centrales)
* **Total Art. Pedidos:** Volumen total demandado (9.770 uds).
* **Tickets Promedio Pedidos:** Tamaño promedio por orden (1.628,33).
* **Cumplimiento DS/PD:** Tasa de efectividad de entrega vs. pedido (**88,95%**).
* **Variaciones Temporales:**
  * Crecimiento interanual (**YoY: 29%**).
  * Variación intertrimestral (**QoQ: -43,73%**).
* **Ratios de Operación:**
  * Relación pedidos vs. unidades (6 / 9.770).
  * Relación despachos vs. unidades (10 / 8.690).

### Detalle Analítico y Metas (Zona Inferior)
* **Tabla TOP 10 Mejores Clientes:** Ranking con desglose de artículos pedidos, conteo de órdenes y unidades despachadas.
* **Indicadores contra Metas:**
  * Cumplimiento del objetivo anual de pedidos frente al año anterior (superado ampliamente vs. meta base).
  * Gauge / Indicador trimestral de cumplimiento DS/PD frente a la meta fijada del 75%.
