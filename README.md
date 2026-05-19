# Executive Sales Efficiency Dashboard

## Overview

Dashboard ejecutivo desarrollado en Power BI para monitorear el desempeno comercial, financiero y de cobertura de clientes.

El reporte permite analizar:

- Ventas
- Cobranzas
- Cobertura
- Cumplimiento de cuota
- Clientes nuevos
- Ticket promedio
- Analisis geografico

## Objetivo Del Negocio

Optimizar la toma de decisiones comerciales mediante indicadores de desempeno en tiempo real, permitiendo identificar oportunidades de crecimiento, riesgos de cobranza y variaciones en la cobertura comercial.

## KPIs Principales

- Venta mensual
- Cobranza acumulada
- % cumplimiento
- Ticket promedio
- Cobertura de clientes
- Clientes nuevos
- Saldo vencido

## Tecnologias Utilizadas

- Power BI
- SQL Server
- DAX
- Power Query
- Modelado estrella

## Cobertura Geografica

El dashboard permite analizar la informacion comercial por:

- Departamento
- Provincia
- Distrito
- Canal comercial

## Dashboard Preview

| Portada | Ventas |
|---|---|
| ![Portada](images/portada.png) | ![Ventas](images/ventas.png) |

| Cobranza | Cobertura |
|---|---|
| ![Cobranza](images/cobranza.png) | ![Cobertura](images/cobertura.png) |

| Analisis geografico |
|---|
| ![Geografia](images/geografia.png) |

## Insights Encontrados

- Identificacion de canales con menor cobranza
- Deteccion de caida en clientes nuevos
- Comparativo historico 2021-2024
- Seguimiento de morosidad

## Estructura Del Repositorio

```text
sales-efficiency-dashboard/
|
|-- README.md
|-- images/
|   |-- portada.png
|   |-- ventas.png
|   |-- cobranza.png
|   |-- cobertura.png
|   `-- geografia.png
|
|-- docs/
|   |-- modelo-datos.png
|   |-- arquitectura.png
|   `-- metricas.md
|
|-- powerbi/
|   `-- EFICIENCIA_DE_VENTAS.pbit
|
`-- sql/
    `-- queries.sql
```

## Archivo Power BI

El template principal del proyecto se encuentra en:

```text
powerbi/EFICIENCIA_DE_VENTAS.pbit
```

## Documentacion

- `docs/metricas.md`: definicion de KPIs y medidas principales.
- `docs/modelo-datos.png`: modelo de datos utilizado en Power BI.
- `docs/arquitectura.png`: arquitectura general de la solucion.
- `sql/queries.sql`: consultas SQL utilizadas para la preparacion de datos.
