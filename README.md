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

<table>
  <tr>
    <td align="center">
      <strong>Portada</strong><br>
      <img src="images/eficiencia%20de%20ventas%202-1.png" alt="Portada" width="220">
    </td>
    <td align="center">
      <strong>Ventas</strong><br>
      <img src="images/eficiencia%20de%20ventas%202-2.png" alt="Ventas" width="220">
    </td>
    <td align="center">
      <strong>Cobertura</strong><br>
      <img src="images/eficiencia%20de%20ventas%202-3.png" alt="Cobertura" width="220">
    </td>
  </tr>
  <tr>
    <td align="center">
      <strong>Cobranza</strong><br>
      <img src="images/eficiencia%20de%20ventas%202-4.png" alt="Cobranza" width="220">
    </td>
    <td align="center">
      <strong>Clientes</strong><br>
      <img src="images/eficiencia%20de%20ventas%202-5.png" alt="Clientes" width="220">
    </td>
    <td align="center">
      <strong>Geografia</strong><br>
      <img src="images/eficiencia%20de%20ventas%202-6.png" alt="Geografia" width="220">
    </td>
  </tr>
</table>

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
