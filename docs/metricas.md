# Metricas Del Dashboard

Catalogo de medidas DAX extraidas desde `powerbi/EFICIENCIA_DE_VENTAS.pbit`.

Total de medidas extraidas: 194

## DimFormaPago

### Lista de valores de DesFormaPago

Descripcion:  VAR __DISTINCT_VALUES_COUNT = DISTINCTCOUNT('DimFormaPago'[DesFormaPago]) VAR __MAX_VALUES_TO_SHOW = 3 RETURN 	IF( 		__DISTINCT_VALUES_COUNT > __MAX_VALUES_TO_SHOW, 		CONCATENATE( 			CONCATENATEX( 				TOPN( 					__MAX_VALUES_TO_SHOW, 					VALUES('DimFormaPago'[DesFormaPago]), 					'DimFormaPago'[DesFormaPago], 					ASC 				), 				'DimFormaPago'[DesFormaPago], 				", ", 				'DimFormaPago'[DesFormaPago], 				ASC 			), 			",  etc." 		), 		CONCATENATEX( 			VALUES('DimFormaPago'[DesFormaPago]), 			'DimFormaPago'[DesFormaPago], 			", ", 			'DimFormaPago'[DesFormaPago], 			ASC 		) 	)

```DAX

VAR __DISTINCT_VALUES_COUNT = DISTINCTCOUNT('DimFormaPago'[DesFormaPago])
VAR __MAX_VALUES_TO_SHOW = 3
RETURN
	IF(
		__DISTINCT_VALUES_COUNT > __MAX_VALUES_TO_SHOW,
		CONCATENATE(
			CONCATENATEX(
				TOPN(
					__MAX_VALUES_TO_SHOW,
					VALUES('DimFormaPago'[DesFormaPago]),
					'DimFormaPago'[DesFormaPago],
					ASC
				),
				'DimFormaPago'[DesFormaPago],
				", ",
				'DimFormaPago'[DesFormaPago],
				ASC
			),
			",  etc."
		),
		CONCATENATEX(
			VALUES('DimFormaPago'[DesFormaPago]),
			'DimFormaPago'[DesFormaPago],
			", ",
			'DimFormaPago'[DesFormaPago],
			ASC
		)
	)
```

## MedidasClientes

### % cobertura x zona

Formato: `0.00\ %;-0.00\ %;0.00\ %`

Descripcion: IF(ISBLANK(DIVIDE([Cobertura],[Cobertura zona],0)),0,DIVIDE([Cobertura],[Cobertura zona],0))

```DAX
IF(ISBLANK(DIVIDE([Cobertura],[Cobertura zona],0)),0,DIVIDE([Cobertura],[Cobertura zona],0))
```

### % YTD

Formato: `0\ %;-0\ %;0\ %`

Descripcion: DIVIDE(([Cobertura YTD]-[Cobertura YTD -1]),[Cobertura YTD -1],0)

```DAX
DIVIDE(([Cobertura YTD]-[Cobertura YTD -1]),[Cobertura YTD -1],0)
```

### Clientes ALL

Formato: `#,0`

Descripcion: CALCULATE([Total clientes Maestro],ALL(DimCalendario[Mes text]))

```DAX
CALCULATE([Total clientes Maestro],ALL(DimCalendario[Mes text]))
```

### Clientes coberturados barrekov

Formato: `0`

Descripcion: CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]), ALL(DimCalendario))

```DAX
CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]), ALL(DimCalendario))
```

### Clientes CodigoRapido YTD CBH

Formato: `#,0`

Descripcion: CALCULATE([Total clientes Maestro],DATESYTD(DimCalendario[Date]))

```DAX
CALCULATE([Total clientes Maestro],DATESYTD(DimCalendario[Date]))
```

### Clientes Recurrencia

Formato: `0`

Descripcion: DISTINCTCOUNT(DimClientePrimeraCompra[kCliente])

```DAX
DISTINCTCOUNT(DimClientePrimeraCompra[kCliente])
```

### ClientesNuevos

Formato: `#,0`

Descripcion:           /*IF(ISBLANK(CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),VW_PrimeraCompraClientesCBH[StatusRecurrencia] = "Nuevo cliente",Fact_Ventas[ClienteVendido])),0,CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),VW_PrimeraCompraClientesCBH[StatusRecurrencia] = "Nuevo cliente",Fact_Ventas[ClienteVendido]))*/     CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),DimClientePrimeraCompra[StatusRecurrencia] = "Nuevo cliente",Fact_Ventas[ClienteVendido])

```DAX

    
    /*IF(ISBLANK(CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),VW_PrimeraCompraClientesCBH[StatusRecurrencia] = "Nuevo cliente",Fact_Ventas[ClienteVendido])),0,CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),VW_PrimeraCompraClientesCBH[StatusRecurrencia] = "Nuevo cliente",Fact_Ventas[ClienteVendido]))*/
    CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),DimClientePrimeraCompra[StatusRecurrencia] = "Nuevo cliente",Fact_Ventas[ClienteVendido])
```

### ClientesNuevos mes anterior

Formato: `0`

Descripcion:  var ultimafecha = MAX(Fact_Ventas[fecha]) return CALCULATE(                     CALCULATE(MedidasClientes[ClientesNuevos],                             Filter(DATEADD(DimCalendario[Date],-1,MONTH),DAY(DimCalendario[Date])<=DAY(ultimafecha))              ),FILTER(DimCalendario,                     DimCalendario[Date] <= TODAY()              ) )

```DAX

var ultimafecha = MAX(Fact_Ventas[fecha])
return CALCULATE(
                    CALCULATE(MedidasClientes[ClientesNuevos],
                            Filter(DATEADD(DimCalendario[Date],-1,MONTH),DAY(DimCalendario[Date])<=DAY(ultimafecha))
             ),FILTER(DimCalendario,
                    DimCalendario[Date] <= TODAY()
             )
)
```

### ClientesNuevosRucUnico

Formato: `0`

Descripcion:           CALCULATE(DISTINCTCOUNT(DimCliente[Documento]),DimClientePrimeraCompra[StatusRecurrencia] = "Nuevo cliente",Fact_Ventas[ClienteVendido])

```DAX

    
    CALCULATE(DISTINCTCOUNT(DimCliente[Documento]),DimClientePrimeraCompra[StatusRecurrencia] = "Nuevo cliente",Fact_Ventas[ClienteVendido])
```

### ClientesRecurrentes

Formato: `#,0`

Descripcion:  CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),Fact_Ventas[ClienteNuevo] = "Cliente Recurrente")

```DAX

CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),Fact_Ventas[ClienteNuevo] = "Cliente Recurrente")
```

### Cobertura

Formato: `#,0`

Descripcion:  IF(ISBLANK(CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),Fact_Ventas[ClienteVendido])),0,CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),Fact_Ventas[ClienteVendido]))

```DAX

IF(ISBLANK(CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),Fact_Ventas[ClienteVendido])),BLANK(),CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),Fact_Ventas[ClienteVendido]))
```

### Cobertura mes anterior

Formato: `#,0`

Descripcion:  CALCULATE(     CALCULATE([% cobertura x zona],DATEADD(FILTER(DATESMTD(DimCalendario[Date]),DimCalendario[Date] <= TODAY()),         -1,MONTH)     ),FILTER(DimCalendario,DimCalendario[Date] <= TODAY()) )

```DAX

CALCULATE(
    CALCULATE([% cobertura x zona],DATEADD(FILTER(DATESMTD(DimCalendario[Date]),DimCalendario[Date] <= TODAY()),
        -1,MONTH)
    ),FILTER(DimCalendario,DimCalendario[Date] <= TODAY())
)
```

### Cobertura Ruc Unico

Formato: `#,0`

Descripcion:  CALCULATE(DISTINCTCOUNT(Fact_Ventas[DocumentoUnico]),Fact_Ventas[ClienteVendido])

```DAX

CALCULATE(DISTINCTCOUNT(Fact_Ventas[DocumentoUnico]),Fact_Ventas[ClienteVendido])
```

### Cobertura ruc unico mes anterior

Formato: `0`

Descripcion:  CALCULATE(                     CALCULATE(MedidasClientes[Cobertura Ruc Unico],                             DATEADD(FILTER(DATESMTD(DimCalendario[Date]),                                 DimCalendario[Date] <= TODAY()                                     ),                                     -1,MONTH                 )              ),FILTER(DimCalendario,                     DimCalendario[Date] <= TODAY()              ) )

```DAX

CALCULATE(
                    CALCULATE(MedidasClientes[Cobertura Ruc Unico],
                            DATEADD(FILTER(DATESMTD(DimCalendario[Date]),
                                DimCalendario[Date] <= TODAY()
                                    ),
                                    -1,MONTH
                )
             ),FILTER(DimCalendario,
                    DimCalendario[Date] <= TODAY()
             )
)
```

### Cobertura sin repre

Formato: `#,0`

Descripcion:  CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),Fact_Ventas[ClienteVendido],ALL(DimAgente[DesAgente]))

```DAX

CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),Fact_Ventas[ClienteVendido],ALL(DimAgente[DesAgente]))
```

### Cobertura YTD

Formato: `0`

Descripcion: CALCULATE( sumX(VALUES(DimCalendario[Mes text]),[Cobertura]),DATESYTD(dateadd(DimCalendario[Date],0,MONTH)))//DATESYTD(DimCalendario[Date]))

```DAX
CALCULATE( sumX(VALUES(DimCalendario[Mes text]),[Cobertura]),DATESYTD(dateadd(DimCalendario[Date],0,MONTH)))//DATESYTD(DimCalendario[Date]))
```

### Cobertura YTD -1

Formato: `0`

Descripcion:  //CALCULATE( sumX(VALUES(DimCalendario[Mes text]),[Cobertura]),DATESYTD(DimCalendario[Date])) //CALCULATE(mVentas[Venta],DATESYTD(DimCalendario[Date])) CALCULATE(                     CALCULATE([Cobertura YTD],                             DATEADD(FILTER(DATESMTD(DimCalendario[Date]),DimCalendario[Date] <= TODAY()),-1,YEAR)              ),FILTER(DimCalendario,                     DimCalendario[Date] <= TODAY()              ) )

```DAX

//CALCULATE( sumX(VALUES(DimCalendario[Mes text]),[Cobertura]),DATESYTD(DimCalendario[Date]))
//CALCULATE(mVentas[Venta],DATESYTD(DimCalendario[Date]))
CALCULATE(
                    CALCULATE([Cobertura YTD],
                            DATEADD(FILTER(DATESMTD(DimCalendario[Date]),DimCalendario[Date] <= TODAY()),-1,YEAR)
             ),FILTER(DimCalendario,
                    DimCalendario[Date] <= TODAY()
             )
)
```

### Cobertura zona

Formato: `#,0`

Descripcion: CALCULATE(DISTINCTCOUNT(DimCliente[kCliente]), ALL(DimCalendario))

```DAX
CALCULATE(DISTINCTCOUNT(DimCliente[kCliente]), ALL(DimCalendario))
```

### Cobertura zona prueba

Formato: `0`

Descripcion: DISTINCTCOUNT(DimCliente[kCliente])

```DAX
DISTINCTCOUNT(DimCliente[kCliente])
```

### CoberturaEvolucion

Formato: `0`

Descripcion:  CALCULATE(     CALCULATE([Cobertura],DATEADD(DimCalendario[Date],-1,MONTH) ))- CALCULATE(     CALCULATE([Cobertura],DATEADD(DimCalendario[Date],-MONTH(TODAY())+1,MONTH) ))

```DAX

CALCULATE(
    CALCULATE([Cobertura],DATEADD(DimCalendario[Date],-1,MONTH)
))-
CALCULATE(
    CALCULATE([Cobertura],DATEADD(DimCalendario[Date],-MONTH(TODAY())+1,MONTH)
))
```

### CoberturaLinea

Formato: `0`

Descripcion: CALCULATE(DISTINCTCOUNT(DimArticulo[Nivel2]),Fact_Ventas[ClienteVendido])

```DAX
CALCULATE(DISTINCTCOUNT(DimArticulo[Nivel2]),Fact_Ventas[ClienteVendido])
```

### End value_Vclientesnuevos

Descripcion: MedidasClientes[ClientesNuevos mes anterior]*2.2

```DAX
MedidasClientes[ClientesNuevos mes anterior]*2.2
```

### End value_Vcobertura

Descripcion: [Tasa de Cobertura mes anterior]*2.2

```DAX
[Tasa de Cobertura mes anterior]*2.2
```

### Franja1_Vclientesnuevos

Descripcion: MedidasClientes[ClientesNuevos mes anterior]*0.5

```DAX
MedidasClientes[ClientesNuevos mes anterior]*0.5
```

### Franja1_Vcobertura

Descripcion: [Tasa de Cobertura mes anterior]*0.5

```DAX
[Tasa de Cobertura mes anterior]*0.5
```

### Franja2_Vclientesnuevos

Formato: `0`

Descripcion: MedidasClientes[ClientesNuevos mes anterior]*1

```DAX
MedidasClientes[ClientesNuevos mes anterior]*1
```

### Franja2_Vcobertura

Formato: `0`

Descripcion: [Tasa de Cobertura mes anterior]*1

```DAX
[Tasa de Cobertura mes anterior]*1
```

### ImporteCobranza

Formato: `"S/"\ #,0.00;-"S/"\ #,0.00;"S/"\ #,0.00`

Descripcion: SUM(Fact_PlanCobranza[importe])

```DAX
SUM(Fact_PlanCobranza[importe])
```

### ImporteCobranzaM

Descripcion: IF([ImporteCobranza]=0,BLANK(),[ImporteCobranza])

```DAX
IF([ImporteCobranza]=0,BLANK(),[ImporteCobranza])
```

### Maximo valor cobertura zona

Descripcion: [Cobertura zona]*1.7

```DAX
[Cobertura zona]*1.7
```

### Tasa de cobertura

Formato: `0.0\ %;-0.0\ %;0.0\ %`

Descripcion: DIVIDE([Cobertura],[Cobertura zona],0)

```DAX
DIVIDE([Cobertura],[Cobertura zona],0)
```

### Tasa de Cobertura mes anterior

Formato: `0.00\ %;-0.00\ %;0.00\ %`

Descripcion:  var ultimafecha = MAX(Fact_Ventas[fecha]) return CALCULATE(                     CALCULATE([Tasa de cobertura],                             Filter(DATEADD(DimCalendario[Date],-1,MONTH),DAY(DimCalendario[Date])<=DAY(ultimafecha))              ),FILTER(DimCalendario,                     DimCalendario[Date] <= TODAY()              ) )

```DAX

var ultimafecha = MAX(Fact_Ventas[fecha])
return CALCULATE(
                    CALCULATE([Tasa de cobertura],
                            Filter(DATEADD(DimCalendario[Date],-1,MONTH),DAY(DimCalendario[Date])<=DAY(ultimafecha))
             ),FILTER(DimCalendario,
                    DimCalendario[Date] <= TODAY()
             )
)
```

### Total clientes Maestro

Formato: `#,0`

Descripcion: DISTINCTCOUNT(DimClientePrimeraCompra[kCliente])

```DAX
DISTINCTCOUNT(DimClientePrimeraCompra[kCliente])
```

### Total clientes unicos

Formato: `#,0`

Descripcion: DISTINCTCOUNT(DimCliente[Documento])

```DAX
DISTINCTCOUNT(DimCliente[Documento])
```

### Total clientes zona

Formato: `0`

Descripcion: DISTINCTCOUNT(DimCliente[kCliente])

```DAX
DISTINCTCOUNT(DimCliente[kCliente])
```

## MedidasCuotaCobranza

### % CumplimientoCuotaCobranza

Formato: `0.00\ %;-0.00\ %;0.00\ %`

Descripcion: DIVIDE(MedidasCuotaCobranza[Cobrado2],[Cuota cobranza],0)

```DAX
DIVIDE(MedidasCuotaCobranza[Cobrado2],[Cuota cobranza],0)
```

### % CumplimientoCuotaCobranza/2

Descripcion: DIVIDE(MedidasCuotaCobranza[Cobrado2],[Cuota cobranza],0)/1.5

```DAX
DIVIDE(MedidasCuotaCobranza[Cobrado2],[Cuota cobranza],0)/1.5
```

### Cobrado mes actual

Descripcion: CALCULATE  ([Cobrado2] , DATESINPERIOD  (  DimCalendario[Date],  MAX  (  DimCalendario[Date] ) ,  -1 ,  MONTH )) 

```DAX
CALCULATE  ([Cobrado2] , DATESINPERIOD  (  DimCalendario[Date],  MAX  (  DimCalendario[Date] ) ,  -1 ,  MONTH )) 
```

### Cobrado mes actual P

Descripcion: CALCULATE  ([Cobrado] , DATESINPERIOD  (  DimCalendario[Date],  MAX  (  DimCalendario[Date] ) ,  -1 ,  MONTH )) 

```DAX
CALCULATE  ([Cobrado] , DATESINPERIOD  (  DimCalendario[Date],  MAX  (  DimCalendario[Date] ) ,  -1 ,  MONTH )) 
```

### Cobrado mes anterior

Descripcion:  var ultimafecha = MAX(Fact_Ventas[fecha]) return CALCULATE(                     CALCULATE(MedidasCuotaCobranza[Cobrado2],                             Filter(DATEADD(DimCalendario[Date],-1,MONTH),DAY(DimCalendario[Date])<=DAY(ultimafecha))              ),FILTER(DimCalendario,                     DimCalendario[Date] <= TODAY()              ) )

```DAX

var ultimafecha = MAX(Fact_Ventas[fecha])
return CALCULATE(
                    CALCULATE(MedidasCuotaCobranza[Cobrado2],
                            Filter(DATEADD(DimCalendario[Date],-1,MONTH),DAY(DimCalendario[Date])<=DAY(ultimafecha))
             ),FILTER(DimCalendario,
                    DimCalendario[Date] <= TODAY()
             )
)
```

### Cobrado Mes Anterior Crec %

Formato: `0.00\ %;-0.00\ %;0.00\ %`

Descripcion: DIVIDE(([Cobrado2]-[Cobrado Mes Anterior Mismos dias]),[Cobrado Mes Anterior Mismos dias])

```DAX
DIVIDE(([Cobrado2]-[Cobrado Mes Anterior Mismos dias]),[Cobrado Mes Anterior Mismos dias])
```

### Cobrado Mes Anterior Mismos dias

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion:  var ultimafecha = MAX(Fact_CuotaCobranza[fecha]) return CALCULATE(                     CALCULATE([Cobrado2],                             Filter(DATEADD(DimCalendario[Date],-1,MONTH),DAY(DimCalendario[Date])<=DAY(ultimafecha))              ),FILTER(DimCalendario,                     DimCalendario[Date] <= TODAY()              ) ) --Fact_CuotaCobranza[cobranza_cuota]

```DAX

var ultimafecha = MAX(Fact_CuotaCobranza[fecha])
return CALCULATE(
                    CALCULATE([Cobrado2],
                            Filter(DATEADD(DimCalendario[Date],-1,MONTH),DAY(DimCalendario[Date])<=DAY(ultimafecha))
             ),FILTER(DimCalendario,
                    DimCalendario[Date] <= TODAY()
             )
)
--Fact_CuotaCobranza[cobranza_cuota]
```

### Cobrado mes anterior P

Descripcion:  CALCULATE(                     CALCULATE([Cobrado],                             DATEADD(FILTER(DATESMTD(DimCalendario[Date]),                                 DimCalendario[Date] <= TODAY()                                     ),                                     -1,MONTH                 )              ),FILTER(DimCalendario,                     DimCalendario[Date] <= TODAY()              ) )

```DAX

CALCULATE(
                    CALCULATE([Cobrado],
                            DATEADD(FILTER(DATESMTD(DimCalendario[Date]),
                                DimCalendario[Date] <= TODAY()
                                    ),
                                    -1,MONTH
                )
             ),FILTER(DimCalendario,
                    DimCalendario[Date] <= TODAY()
             )
)
```

### Cobrado YTD F

Descripcion: CALCULATE(CALCULATE([Cobrado],DATESYTD(DimCalendario[Date])),ALLEXCEPT(DimCalendario,DimCalendario[Last Business Date]))

```DAX
CALCULATE(CALCULATE([Cobrado],DATESYTD(DimCalendario[Date])),ALLEXCEPT(DimCalendario,DimCalendario[Last Business Date]))
```

### Cobrado2

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: IF(SUM(Fact_CuotaCobranza[cobranza_cuota])=0,BLANK(),SUM(Fact_CuotaCobranza[cobranza_cuota]))

```DAX
IF(SUM(Fact_CuotaCobranza[cobranza_cuota])=0,BLANK(),SUM(Fact_CuotaCobranza[cobranza_cuota]))
```

### CobradoAcumulado

Descripcion: CALCULATE([Cobrado], DATESYTD(DimCalendario[Date]))

```DAX
CALCULATE([Cobrado], DATESYTD(DimCalendario[Date]))
```

### CobradoHace1Año

Descripcion:  CALCULATE(                     CALCULATE([Cobrado YTD F],                             DATEADD(FILTER(DATESMTD(DimCalendario[Date]),                                 DimCalendario[Date] <= TODAY()                                     ),                                     -1,YEAR                 )              ),FILTER(DimCalendario,                     DimCalendario[Date] <= TODAY()              ) )

```DAX

CALCULATE(
                    CALCULATE([Cobrado YTD F],
                            DATEADD(FILTER(DATESMTD(DimCalendario[Date]),
                                DimCalendario[Date] <= TODAY()
                                    ),
                                    -1,YEAR
                )
             ),FILTER(DimCalendario,
                    DimCalendario[Date] <= TODAY()
             )
)
```

### CobradoMesHace1AñoF

Descripcion:  CALCULATE(                     CALCULATE([Cobrado2],                             DATEADD(FILTER(DATESMTD(DimCalendario[Date]),                                 DimCalendario[Date] <= TODAY()                                     ),                                     -1,YEAR                 )              ),FILTER(DimCalendario,                     DimCalendario[Date] <= TODAY()              ) )

```DAX

CALCULATE(
                    CALCULATE([Cobrado2],
                            DATEADD(FILTER(DATESMTD(DimCalendario[Date]),
                                DimCalendario[Date] <= TODAY()
                                    ),
                                    -1,YEAR
                )
             ),FILTER(DimCalendario,
                    DimCalendario[Date] <= TODAY()
             )
)
```

### CobradoRepre

Descripcion: SUM(Fact_CuotaCobranza[cobranza_cuota])

```DAX
SUM(Fact_CuotaCobranza[cobranza_cuota])
```

### Cuota cobranza

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: SUM(Fact_CuotaCobranza[cuota])

```DAX
SUM(Fact_CuotaCobranza[cuota])
```

### CuotaCobranzaRepre

Descripcion: SUM(Fact_CuotaCobranza[cuota])

```DAX
SUM(Fact_CuotaCobranza[cuota])
```

### DiasGiro

Descripcion: ([Cobrado2]/[Venta SL])*360 

```DAX
([Cobrado2]/[Venta SL])*360 
```

### End value_Vcuotacobranza

Descripcion: [Cuota cobranza]*1.70

```DAX
[Cuota cobranza]*1.70
```

### F1 Ccobranza

Formato: `0.00\ %;-0.00\ %;0.00\ %`

Descripcion: 0.69

```DAX
0.69
```

### F2 Ccobranza

Formato: `0\ %;-0\ %;0\ %`

Descripcion: 0.85

```DAX
0.85
```

### F3 Ccobranza

Descripcion: 0.94

```DAX
0.94
```

### F4 Ccobranza

Formato: `0`

Descripcion: 1

```DAX
1
```

### FechaVencimiento

Formato: `0`

Descripcion: DATEDIFF(MAX(DimCalendario[Date]),MAX(Fact_PlanCobranza[fecha_vencimiento]),DAY)

```DAX
DATEDIFF(MAX(DimCalendario[Date]),MAX(Fact_PlanCobranza[fecha_vencimiento]),DAY)
```

### Franja1_CCOBRANZA_FC

Descripcion: [Cuota cobranza]*0.5

```DAX
[Cuota cobranza]*0.5
```

### Franja2_CCOBRANZA_FC

Descripcion: [Cuota cobranza]*1

```DAX
[Cuota cobranza]*1
```

### icono

Descripcion: [Cobrado YTD F] - [CobradoHace1Año]

```DAX
[Cobrado YTD F] - [CobradoHace1Año]
```

### iconoCobradohace1añoF

Descripcion: [Cobrado mes actual] - [CobradoHace1Año]

```DAX
[Cobrado mes actual] - [CobradoHace1Año]
```

### IconoCobradoMes

Descripcion: [Cobrado mes actual] - [Cobrado mes anterior]

```DAX
[Cobrado mes actual] - [Cobrado mes anterior]
```

### Meta Ccobranza

Descripcion: 0.85

```DAX
0.85
```

### Por cobrar mes anterior

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion:  var ultimafecha = MAX(Fact_Ventas[fecha]) return CALCULATE(                     CALCULATE([PorCobrar],                             Filter(DATEADD(DimCalendario[Date],-1,MONTH),DAY(DimCalendario[Date])<=DAY(ultimafecha))              ),FILTER(DimCalendario,                     DimCalendario[Date] <= TODAY()              ) )

```DAX

var ultimafecha = MAX(Fact_Ventas[fecha])
return CALCULATE(
                    CALCULATE([PorCobrar],
                            Filter(DATEADD(DimCalendario[Date],-1,MONTH),DAY(DimCalendario[Date])<=DAY(ultimafecha))
             ),FILTER(DimCalendario,
                    DimCalendario[Date] <= TODAY()
             )
)
```

### PorCobrar

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: --VAR _COB = SUM(Fact_CuotaCobranza[Por cobrar]) --RETURN IF(_COB = 0 || ISBLANK(_COB) , BLANK(),_COB) SUM(Fact_CuotaCobranza[Por cobrar])

```DAX
--VAR _COB = SUM(Fact_CuotaCobranza[Por cobrar])
--RETURN IF(_COB = 0 || ISBLANK(_COB) , BLANK(),_COB)
SUM(Fact_CuotaCobranza[Por cobrar])
```

### PorCobrar PorVencer

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: CALCULATE([PorCobrar],Fact_CuotaCobranza[Dias Vencidos]<0)

```DAX
CALCULATE([PorCobrar],Fact_CuotaCobranza[Dias Vencidos]<0)
```

### PorCobrar Vencido

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: CALCULATE([PorCobrar],Fact_CuotaCobranza[Dias Vencidos]>=0)

```DAX
CALCULATE([PorCobrar],Fact_CuotaCobranza[Dias Vencidos]>=0)
```

### PorCobrar Vencido Dias Retraso Ponderado

Descripcion:  //[PorCobrar Vencido Ratio%]*AVERAGE(Fact_CuotaCobranza[Dias Vencidos]) var _t = CALCULATETABLE(Fact_CuotaCobranza,FILTER(Fact_CuotaCobranza,Fact_CuotaCobranza[Dias Vencidos]>0))  var _table = SUMMARIZE(_t,DimAgenteZona[Nivel1],DimAgenteZona[Nivel2],DimAgenteZona[Nivel3],DimAgenteZona[Nivel4],DimCliente[codCliente_RazonSocial],Fact_CuotaCobranza[DOCUMENTO],                         "NDD",AVERAGE(Fact_CuotaCobranza[Dias Vencidos]),"Ratio",AVERAGE(Fact_CuotaCobranza[Dias Vencidos])*[PorCobrar Vencido Ratio%])  var _res = GROUPBY( _table,"Promedio",AVERAGEX(CURRENTGROUP(),[NDD]))*[PorCobrar Vencido Ratio%] var _res1 = GROUPBY( _table,"Promedio",SUMX(CURRENTGROUP(),[Ratio]))*[PorCobrar Vencido Ratio%] return IF(ISBLANK([PorCobrar Vencido]),BLANK(),if(HASONEVALUE(Fact_CuotaCobranza[DOCUMENTO]),_res,_res1)) //SUMX(ALLSELECTED(Fact_CuotaCobranza),_res)  //var _aa = [PorCobrar Vencido Ratio%]*AVERAGE(Fact_CuotaCobranza[Dias Vencidos]) //var _bb = IF(ISBLANK([Cuota cobranza]),BLANK(), if(HASONEVALUE(DimCliente[codCliente_RazonSocial]),_aa,SUMX(Fact_CuotaCobranza,_aa)))

```DAX

//[PorCobrar Vencido Ratio%]*AVERAGE(Fact_CuotaCobranza[Dias Vencidos])
var _t = CALCULATETABLE(Fact_CuotaCobranza,FILTER(Fact_CuotaCobranza,Fact_CuotaCobranza[Dias Vencidos]>0))

var _table = SUMMARIZE(_t,DimAgenteZona[Nivel1],DimAgenteZona[Nivel2],DimAgenteZona[Nivel3],DimAgenteZona[Nivel4],DimCliente[codCliente_RazonSocial],Fact_CuotaCobranza[DOCUMENTO],
                        "NDD",AVERAGE(Fact_CuotaCobranza[Dias Vencidos]),"Ratio",AVERAGE(Fact_CuotaCobranza[Dias Vencidos])*[PorCobrar Vencido Ratio%])

var _res = GROUPBY( _table,"Promedio",AVERAGEX(CURRENTGROUP(),[NDD]))*[PorCobrar Vencido Ratio%]
var _res1 = GROUPBY( _table,"Promedio",SUMX(CURRENTGROUP(),[Ratio]))*[PorCobrar Vencido Ratio%]
return
IF(ISBLANK([PorCobrar Vencido]),BLANK(),if(HASONEVALUE(Fact_CuotaCobranza[DOCUMENTO]),_res,_res1))
//SUMX(ALLSELECTED(Fact_CuotaCobranza),_res)

//var _aa = [PorCobrar Vencido Ratio%]*AVERAGE(Fact_CuotaCobranza[Dias Vencidos])
//var _bb = IF(ISBLANK([Cuota cobranza]),BLANK(), if(HASONEVALUE(DimCliente[codCliente_RazonSocial]),_aa,SUMX(Fact_CuotaCobranza,_aa)))
```

### PorCobrar Vencido Ratio%

Formato: `0.00\ %;-0.00\ %;0.00\ %`

Descripcion:   var _num = [PorCobrar Vencido]   var _table = SUMMARIZE(ALLSELECTED(Fact_CuotaCobranza),"Suma",[PorCobrar Vencido])  var _den = GROUPBY(_table,"Sumaa",SUMX(CURRENTGROUP(),[Suma]))   Return    DIVIDE(_num,_den,BLANK()) 

```DAX

 var _num = [PorCobrar Vencido]

 var _table = SUMMARIZE(ALLSELECTED(Fact_CuotaCobranza),"Suma",[PorCobrar Vencido])
 var _den = GROUPBY(_table,"Sumaa",SUMX(CURRENTGROUP(),[Suma]))

 Return 

 DIVIDE(_num,_den,BLANK())

```

### PorCobrar Vencido USERELATIONSHIP Cliente

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: //CALCULATE([PorCobrar],Fact_CuotaCobranza[Dias Vencidos]>=0,USERELATIONSHIP(DimCliente[Zona],DimAgenteZona[CodZona])) [PorCobrar]

```DAX
//CALCULATE([PorCobrar],Fact_CuotaCobranza[Dias Vencidos]>=0,USERELATIONSHIP(DimCliente[Zona],DimAgenteZona[CodZona]))
[PorCobrar]
```

### SeleRepreCuota

Descripcion: IF(ISBLANK(SELECTEDVALUE(DimAgente[DesAgente])),[Cuota cobranza],[CuotaCobranzaRepre])

```DAX
IF(ISBLANK(SELECTEDVALUE(DimAgente[DesAgente])),[Cuota cobranza],[CuotaCobranzaRepre])
```

### Tasa de morosidad Cuota

Descripcion: DIVIDE([PorCobrar Vencido],[PorCobrar],0)

```DAX
DIVIDE([PorCobrar Vencido],[PorCobrar],0)
```

### Tex cum ccobranza_fc

Descripcion: "Cumplimiento actual:"

```DAX
"Cumplimiento actual:"
```

### Text Cuota Cobranza

Descripcion: "<Al corte de hoy el avance de la cobranza es de " & FORMAT( [% CumplimientoCuotaCobranza],"Percent") & ", tenemos Vencidos -> S/. " & SUBSTITUTE(FORMAT([PorCobrar Vencido],"#,0"),".",",") & "  y en el tramo de cuentas morosas > 60 días -> S/. " & SUBSTITUTE(format(CALCULATE([PorCobrar Vencido],FILTER(Fact_CuotaCobranza,[Dias Vencidos]>=60)),"#,0"),".",",") & ">"

```DAX
"<Al corte de hoy el avance de la cobranza es de " & FORMAT( [% CumplimientoCuotaCobranza],"Percent") & ", tenemos Vencidos -> S/. " & SUBSTITUTE(FORMAT([PorCobrar Vencido],"#,0"),".",",") & "  y en el tramo de cuentas morosas > 60 días -> S/. " & SUBSTITUTE(format(CALCULATE([PorCobrar Vencido],FILTER(Fact_CuotaCobranza,[Dias Vencidos]>=60)),"#,0"),".",",") & ">"
```

### Total clientes por cobrar

Formato: `#,0`

Descripcion: CALCULATE(DISTINCTCOUNT(Fact_CuotaCobranza[kCliente]),Fact_CuotaCobranza[Por cobrar]>0)

```DAX
CALCULATE(DISTINCTCOUNT(Fact_CuotaCobranza[kCliente]),Fact_CuotaCobranza[Por cobrar]>0)
```

### Total clientes por cobrar mes anterior

Formato: `#,0`

Descripcion:  var ultimafecha = MAX(Fact_Ventas[fecha]) return CALCULATE(                     CALCULATE([Total clientes por cobrar],                             Filter(DATEADD(DimCalendario[Date],-1,MONTH),DAY(DimCalendario[Date])<=DAY(ultimafecha))              ),FILTER(DimCalendario,                     DimCalendario[Date] <= TODAY()              ) )

```DAX

var ultimafecha = MAX(Fact_Ventas[fecha])
return CALCULATE(
                    CALCULATE([Total clientes por cobrar],
                            Filter(DATEADD(DimCalendario[Date],-1,MONTH),DAY(DimCalendario[Date])<=DAY(ultimafecha))
             ),FILTER(DimCalendario,
                    DimCalendario[Date] <= TODAY()
             )
)
```

## MedidasMargen

### % Avance con respecto al mes anterior

Formato: `0\ %;-0\ %;0\ %`

Descripcion: DIVIDE(MedidasMargen[Margen],MedidasMargen[Margen mes anterior],0)

```DAX
DIVIDE(MedidasMargen[Margen],MedidasMargen[Margen mes anterior],0)
```

### % Margen

Descripcion: "Margen vs venta neta: " & format(DIVIDE((MedidasVentas[Venta SL]-[Costo]),[Venta SL],0),"0.00%")

```DAX
"Margen vs venta neta: " & format(DIVIDE((MedidasVentas[Venta SL]-[Costo]),[Venta SL],0),"0.00%")
```

### % Margen mes anterior

Formato: `0.0\ %;-0.0\ %;0.0\ %`

Descripcion:  CALCULATE(                     CALCULATE(MedidasMargen[% Margen],                             DATEADD(FILTER(DATESMTD(DimCalendario[Date]),                                 DimCalendario[Date] <= TODAY()                                     ),                                     -1,MONTH                                     )                              ),FILTER(DimCalendario,                                     DimCalendario[Date]<=TODAY()         ) )

```DAX

CALCULATE(
                    CALCULATE(MedidasMargen[% Margen],
                            DATEADD(FILTER(DATESMTD(DimCalendario[Date]),
                                DimCalendario[Date] <= TODAY()
                                    ),
                                    -1,MONTH
                                    )
                             ),FILTER(DimCalendario,
                                    DimCalendario[Date]<=TODAY()
        )
)
```

### Costo

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: SUM(Fact_Ventas[costo_bruto])

```DAX
SUM(Fact_Ventas[costo_bruto])
```

### End value_Vmargen

Descripcion: MedidasMargen[Margen mes anterior]*1.5

```DAX
MedidasMargen[Margen mes anterior]*1.5
```

### Franja1_Vmargen

Descripcion: MedidasMargen[Margen mes anterior]*0.5

```DAX
MedidasMargen[Margen mes anterior]*0.5
```

### Franja2_Vmargen

Descripcion: MedidasMargen[Margen mes anterior]*1

```DAX
MedidasMargen[Margen mes anterior]*1
```

### Margen

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: MedidasVentas[Venta SL]-[Costo]

```DAX
MedidasVentas[Venta SL]-[Costo]
```

### Margen mes actual

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: CALCULATE  ( MedidasMargen[Margen] , DATESINPERIOD  (  DimCalendario[Date],  MAX  (  DimCalendario[Date] ) ,  -1 ,  MONTH )) 

```DAX
CALCULATE  ( MedidasMargen[Margen] , DATESINPERIOD  (  DimCalendario[Date],  MAX  (  DimCalendario[Date] ) ,  -1 ,  MONTH )) 
```

### Margen mes anterior

Descripcion:   var ultimafecha = MAX(Fact_Ventas[fecha]) return CALCULATE(                     CALCULATE(MedidasMargen[Margen],                             Filter(DATEADD(DimCalendario[Date],-1,MONTH),DAY(DimCalendario[Date])<=DAY(ultimafecha))              ),FILTER(DimCalendario,                     DimCalendario[Date] <= TODAY()              ) )

```DAX


var ultimafecha = MAX(Fact_Ventas[fecha])
return CALCULATE(
                    CALCULATE(MedidasMargen[Margen],
                            Filter(DATEADD(DimCalendario[Date],-1,MONTH),DAY(DimCalendario[Date])<=DAY(ultimafecha))
             ),FILTER(DimCalendario,
                    DimCalendario[Date] <= TODAY()
             )
)
```

### Prueba today

Formato: `General Date`

Descripcion: MAX(Fact_Ventas[fecha])

```DAX
MAX(Fact_Ventas[fecha])
```

## MedidasPlanCobranza

### %vencido

Formato: `0\ %;-0\ %;0\ %`

Descripcion: DIVIDE([DeudaVencida],[Total saldo],0)

```DAX
DIVIDE([DeudaVencida],[Total saldo],0)
```

### Cobrado

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: CALCULATE(SUM(Fact_PlanCobranza[importe]),Fact_PlanCobranza[Status]="Al Día")

```DAX
CALCULATE(SUM(Fact_PlanCobranza[importe]),Fact_PlanCobranza[Status]="Al Día")
```

### Deuda mes actual

Descripcion: CALCULATE  ( [Total saldo] , DATESINPERIOD  (  DimCalendario[Date],  MAX  (  DimCalendario[Date] ) ,  -1 ,  MONTH )) 

```DAX
CALCULATE  ( [Total saldo] , DATESINPERIOD  (  DimCalendario[Date],  MAX  (  DimCalendario[Date] ) ,  -1 ,  MONTH )) 
```

### DeudaVencida

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: CALCULATE([Total saldo],Fact_PlanCobranza[Status]="Vencido")

```DAX
CALCULATE([Total saldo],Fact_PlanCobranza[Status]="Vencido")
```

### End Value Morosidad

Formato: `0.00\ %;-0.00\ %;0.00\ %`

Descripcion: 1

```DAX
1
```

### F1 Tmoro

Formato: `0.00\ %;-0.00\ %;0.00\ %`

Descripcion: 0.2

```DAX
0.2
```

### F2 Tmoro

Formato: `0.00\ %;-0.00\ %;0.00\ %`

Descripcion: 0.3

```DAX
0.3
```

### Importe vencido

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: CALCULATE(SUM(Fact_PlanCobranza[importe]),Fact_PlanCobranza[Status]="Vencido")

```DAX
CALCULATE(SUM(Fact_PlanCobranza[importe]),Fact_PlanCobranza[Status]="Vencido")
```

### Importe vencido mes anterior

Descripcion:  var ultimafecha = MAX(Fact_Ventas[fecha]) return CALCULATE(                     CALCULATE([Importe vencido],                             Filter(DATEADD(DimCalendario[Date],-1,MONTH),DAY(DimCalendario[Date])<=DAY(ultimafecha))              ),FILTER(DimCalendario,                     DimCalendario[Date] <= TODAY()              ) )

```DAX

var ultimafecha = MAX(Fact_Ventas[fecha])
return CALCULATE(
                    CALCULATE([Importe vencido],
                            Filter(DATEADD(DimCalendario[Date],-1,MONTH),DAY(DimCalendario[Date])<=DAY(ultimafecha))
             ),FILTER(DimCalendario,
                    DimCalendario[Date] <= TODAY()
             )
)
```

### Nro de documentos

Formato: `#,0`

Descripcion: DISTINCTCOUNT(Fact_PlanCobranza[documento])

```DAX
DISTINCTCOUNT(Fact_PlanCobranza[documento])
```

### Ratio vencido de por cobrar

Formato: `0.0\ %;-0.0\ %;0.0\ %`

Descripcion: DIVIDE([DeudaVencida],[Total saldo],0)

```DAX
DIVIDE([DeudaVencida],[Total saldo],0)
```

### Saldo por vencer

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion:  IF(ISBLANK(CALCULATE([Total saldo],Fact_PlanCobranza[Status]="Por vencer")),0,CALCULATE([Total saldo],Fact_PlanCobranza[Status]="Por vencer"))

```DAX

IF(ISBLANK(CALCULATE([Total saldo],Fact_PlanCobranza[Status]="Por vencer")),BLANK(),CALCULATE([Total saldo],Fact_PlanCobranza[Status]="Por vencer"))
```

### Saldo por vencer mes anterior

Descripcion:  IF(ISBLANK(CALCULATE([Saldo por vencer],DATEADD(DimCalendario[Date],-1,MONTH))),0,CALCULATE([Saldo por vencer],DATEADD(DimCalendario[Date],-1,MONTH)))

```DAX

IF(ISBLANK(CALCULATE([Saldo por vencer],DATEADD(DimCalendario[Date],-1,MONTH))),BLANK(),CALCULATE([Saldo por vencer],DATEADD(DimCalendario[Date],-1,MONTH)))
```

### Saldo vencido mes anterior

Descripcion: CALCULATE([DeudaVencida],DATEADD(DimCalendario[Date],-1,MONTH))

```DAX
CALCULATE([DeudaVencida],DATEADD(DimCalendario[Date],-1,MONTH))
```

### Sele mes año

Descripcion: "De inicios del "&SELECTEDVALUE(DimCalendario[Año])&" hasta "& SELECTEDVALUE(DimCalendario[Mes text completo])&" del "&SELECTEDVALUE(DimCalendario[Año])

```DAX
"De inicios del "&SELECTEDVALUE(DimCalendario[Año])&" hasta "& SELECTEDVALUE(DimCalendario[Mes text completo])&" del "&SELECTEDVALUE(DimCalendario[Año])
```

### SeleCanal

Descripcion: "DETALLE DE VENCIDOS DE "&IF(ISBLANK(SELECTEDVALUE(DimAgenteZona[Nivel1])),"TODOS LOS CANALES",UPPER(SELECTEDVALUE(DimAgenteZona[Nivel1])))

```DAX
"DETALLE DE VENCIDOS DE "&IF(ISBLANK(SELECTEDVALUE(DimAgenteZona[Nivel1])),"TODOS LOS CANALES",UPPER(SELECTEDVALUE(DimAgenteZona[Nivel1])))
```

### Tasa de morosidad

Formato: `0.00\ %;-0.00\ %;0.00\ %`

Descripcion: DIVIDE([DeudaVencida],[Total saldo],0)

```DAX
DIVIDE([DeudaVencida],[Total saldo],0)
```

### Total Cliente YTD

Formato: `#,0`

Descripcion: CALCULATE(DISTINCTCOUNT(Fact_PlanCobranza[kCliente]),DATESYTD(DimCalendario[Date]))

```DAX
CALCULATE(DISTINCTCOUNT(Fact_PlanCobranza[kCliente]),DATESYTD(DimCalendario[Date]))
```

### Total Clientes con Saldo REP

Formato: `#,0`

Descripcion: DISTINCTCOUNT(Fact_PlanCobranza[kCliente])

```DAX
DISTINCTCOUNT(Fact_PlanCobranza[kCliente])
```

### Total Clientes con Saldo YTD

Formato: `#,0`

Descripcion: CALCULATE(DISTINCTCOUNT(Fact_PlanCobranza[kCliente]),Fact_PlanCobranza[Status]<>"Al día",DATESYTD(DimCalendario[Date]))   

```DAX
CALCULATE(DISTINCTCOUNT(Fact_PlanCobranza[kCliente]),Fact_PlanCobranza[Status]<>"Al día",DATESYTD(DimCalendario[Date]))



```

### Total Clientes YTD

Formato: `0`

Descripcion: CALCULATE(DISTINCTCOUNT(Fact_PlanCobranza[kCliente]),DATESYTD(DimCalendario[Date]))

```DAX
CALCULATE(DISTINCTCOUNT(Fact_PlanCobranza[kCliente]),DATESYTD(DimCalendario[Date]))
```

### Total deuda CONREPRE

Formato: `"S/"\ #,0.00;-"S/"\ #,0.00;"S/"\ #,0.00`

Descripcion: SUM(Fact_PlanCobranza[importe])

```DAX
SUM(Fact_PlanCobranza[importe])
```

### Total importe

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: SUM(Fact_PlanCobranza[importe])

```DAX
SUM(Fact_PlanCobranza[importe])
```

### Total Importe AlContado

Descripcion: CALCULATE([Total importe],DimFormaPago[Tipo]="Contado")

```DAX
CALCULATE([Total importe],DimFormaPago[Tipo]="Contado")
```

### Total Importe YTD

Descripcion: CALCULATE(SUM(Fact_PlanCobranza[importe]),DATESYTD(DimCalendario[Date]))

```DAX
CALCULATE(SUM(Fact_PlanCobranza[importe]),DATESYTD(DimCalendario[Date]))
```

### Total saldo

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: var _saldo = SUM(Fact_PlanCobranza[saldo]) return if(ISBLANK(_saldo) || _saldo = 0,BLANK(),_saldo)

```DAX
var _saldo = SUM(Fact_PlanCobranza[saldo])
return if(ISBLANK(_saldo) || _saldo = 0,BLANK(),_saldo)
```

### Total Saldo Años Anteriores

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion:      var _fecMin = calculate(MIN(DimCalendario[Date]),ALL(DimCalendario))     var _fecMax = ENDOFYEAR(DATEADD(DimCalendario[Date],-1,YEAR)) return     CALCULATE([Total saldo],FILTER(DimCalendario,DimCalendario[Date] >= _fecMin && DimCalendario[Date] <= _fecMax))

```DAX

    var _fecMin = calculate(MIN(DimCalendario[Date]),ALL(DimCalendario))
    var _fecMax = ENDOFYEAR(DATEADD(DimCalendario[Date],-1,YEAR))
return
    CALCULATE([Total saldo],FILTER(DimCalendario,DimCalendario[Date] >= _fecMin && DimCalendario[Date] <= _fecMax))
```

### Total Saldo YTD

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: CALCULATE(SUM(Fact_PlanCobranza[saldo]),DATESYTD(DimCalendario[Date]))

```DAX
CALCULATE(SUM(Fact_PlanCobranza[saldo]),DATESYTD(DimCalendario[Date]))
```

### Total saldo YTD mes anterior

Descripcion:  CALCULATE([Total Saldo YTD],DATEADD(DimCalendario[Date],-1,MONTH))

```DAX

CALCULATE([Total Saldo YTD],DATEADD(DimCalendario[Date],-1,MONTH))
```

### TotalClientes

Formato: `#,0`

Descripcion: DISTINCTCOUNT(Fact_PlanCobranza[kCliente])

```DAX
DISTINCTCOUNT(Fact_PlanCobranza[kCliente])
```

### TotalClientes conSaldo mes anterior

Formato: `#,0`

Descripcion:  CALCULATE(                     CALCULATE([Total Clientes con Saldo YTD],                             DATEADD(FILTER(DATESMTD(DimCalendario[Date]),                                 DimCalendario[Date] <= TODAY()                                     ),                                     -1,MONTH                 )              ),FILTER(DimCalendario,                     DimCalendario[Date] <= TODAY()              ) )

```DAX

CALCULATE(
                    CALCULATE([Total Clientes con Saldo YTD],
                            DATEADD(FILTER(DATESMTD(DimCalendario[Date]),
                                DimCalendario[Date] <= TODAY()
                                    ),
                                    -1,MONTH
                )
             ),FILTER(DimCalendario,
                    DimCalendario[Date] <= TODAY()
             )
)
```

### TotalSaldoaCobrar

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: CALCULATE(SUM(Fact_PlanCobranza[saldo]),Fact_PlanCobranza[Status]<>"Al Día")

```DAX
CALCULATE(SUM(Fact_PlanCobranza[saldo]),Fact_PlanCobranza[Status]<>"Al Día")
```

### TotalSaldoaCobrarMesAnterior

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion:  CALCULATE(                     CALCULATE([TotalSaldoaCobrar],                             DATEADD(FILTER(DATESMTD(DimCalendario[Date]),                                 DimCalendario[Date] <= TODAY()                                     ),                                     -1,MONTH                 )              ),FILTER(DimCalendario,                     DimCalendario[Date] <= TODAY()              ) )

```DAX

CALCULATE(
                    CALCULATE([TotalSaldoaCobrar],
                            DATEADD(FILTER(DATESMTD(DimCalendario[Date]),
                                DimCalendario[Date] <= TODAY()
                                    ),
                                    -1,MONTH
                )
             ),FILTER(DimCalendario,
                    DimCalendario[Date] <= TODAY()
             )
)
```

## MedidasTiempo

### _fecMin

Formato: `General Date`

Descripcion:  var _fec = switch(true(), min(Fact_CuotaCobranza[fecha]) <= min(Fact_CuotaVenta[fecha]) &&  min(Fact_CuotaCobranza[fecha]) <= min(Fact_PlanCobranza[fecha_vencimiento]) &&  min(Fact_CuotaCobranza[fecha]) <= min(Fact_Ventas[fecha]), ( min(Fact_CuotaCobranza[fecha])),  min(Fact_CuotaVenta[fecha]) <= min(Fact_CuotaCobranza[fecha]) &&  min(Fact_CuotaVenta[fecha]) <= min(Fact_PlanCobranza[fecha_vencimiento]) &&  min(Fact_CuotaVenta[fecha]) <= min(Fact_Ventas[fecha]), min(Fact_CuotaVenta[fecha]),  min(Fact_PlanCobranza[fecha_vencimiento]) <= min(Fact_CuotaCobranza[fecha]) &&  min(Fact_PlanCobranza[fecha_vencimiento]) <= min(Fact_CuotaVenta[fecha]) &&  min(Fact_PlanCobranza[fecha_vencimiento]) <= min(Fact_Ventas[fecha]), min(Fact_PlanCobranza[fecha_vencimiento]),   min(Fact_Ventas[fecha]) <= min(Fact_CuotaCobranza[fecha]) &&  min(Fact_Ventas[fecha]) <= min(Fact_CuotaVenta[fecha]) &&  min(Fact_Ventas[fecha]) <= min(Fact_PlanCobranza[fecha_vencimiento]), min(Fact_Ventas[fecha])) var _fecMin = DATE(year(_fec),1,1) return _fecMin

```DAX

var _fec = switch(true(),
min(Fact_CuotaCobranza[fecha]) <= min(Fact_CuotaVenta[fecha]) && 
min(Fact_CuotaCobranza[fecha]) <= min(Fact_PlanCobranza[fecha_vencimiento]) && 
min(Fact_CuotaCobranza[fecha]) <= min(Fact_Ventas[fecha]),
( min(Fact_CuotaCobranza[fecha])),

min(Fact_CuotaVenta[fecha]) <= min(Fact_CuotaCobranza[fecha]) && 
min(Fact_CuotaVenta[fecha]) <= min(Fact_PlanCobranza[fecha_vencimiento]) && 
min(Fact_CuotaVenta[fecha]) <= min(Fact_Ventas[fecha]),
min(Fact_CuotaVenta[fecha]),

min(Fact_PlanCobranza[fecha_vencimiento]) <= min(Fact_CuotaCobranza[fecha]) && 
min(Fact_PlanCobranza[fecha_vencimiento]) <= min(Fact_CuotaVenta[fecha]) && 
min(Fact_PlanCobranza[fecha_vencimiento]) <= min(Fact_Ventas[fecha]),
min(Fact_PlanCobranza[fecha_vencimiento]),


min(Fact_Ventas[fecha]) <= min(Fact_CuotaCobranza[fecha]) && 
min(Fact_Ventas[fecha]) <= min(Fact_CuotaVenta[fecha]) && 
min(Fact_Ventas[fecha]) <= min(Fact_PlanCobranza[fecha_vencimiento]),
min(Fact_Ventas[fecha]))
var _fecMin = DATE(year(_fec),1,1)
return
_fecMin
```

### Año Actual

Descripcion: "Venta acumulada "&CALCULATE(MAX(DimCalendario[Año]),ALLSELECTED())&" Vs "&CALCULATE(MAX(DimCalendario[Año]),ALLSELECTED())-1

```DAX
"Venta acumulada "&CALCULATE(MAX(DimCalendario[Año]),ALLSELECTED())&" Vs "&CALCULATE(MAX(DimCalendario[Año]),ALLSELECTED())-1
```

### Año Actual cobranza

Descripcion: "Cobranza acumulada "&CALCULATE(MAX(DimCalendario[Año]),ALLSELECTED())

```DAX
"Cobranza acumulada "&CALCULATE(MAX(DimCalendario[Año]),ALLSELECTED())
```

### Año Anterior

Formato: `0`

Descripcion: [Año Actual]-1

```DAX
[Año Actual]-1
```

### Mes actual

Descripcion: MAXX(FILTER(DimCalendario,DimCalendario[Date] = MAX(DimCalendario[Date])),DimCalendario[Mes text completo]&" "&YEAR(MAX(DimCalendario[Date])))

```DAX
MAXX(FILTER(DimCalendario,DimCalendario[Date] = MAX(DimCalendario[Date])),DimCalendario[Mes text completo]&" "&YEAR(MAX(DimCalendario[Date])))
```

### Mes actual - 1 año

Descripcion: MAXX(FILTER(DimCalendario,DimCalendario[Date] = MAX(DimCalendario[Date])),DimCalendario[Mes text completo])&" "&YEAR(MAX(DimCalendario[Date]))-1

```DAX
MAXX(FILTER(DimCalendario,DimCalendario[Date] = MAX(DimCalendario[Date])),DimCalendario[Mes text completo])&" "&YEAR(MAX(DimCalendario[Date]))-1
```

### Mes anterior

Descripcion: IF(MONTH(MAX(DimCalendario[Date]))-1=1,"Enero",                 IF(MONTH(MAX(DimCalendario[Date]))-1=2,"Febrero",                     IF(MONTH(MAX(DimCalendario[Date]))-1=3,"Marzo",                         IF(MONTH(MAX(DimCalendario[Date]))-1=4,"Abril",                             IF(MONTH(MAX(DimCalendario[Date]))-1=5,"Mayo",                                 IF(MONTH(MAX(DimCalendario[Date]))-1=6,"Junio",                                     IF(MONTH(MAX(DimCalendario[Date]))-1=7,"Julio",                                         IF(MONTH(MAX(DimCalendario[Date]))-1=8,"Agosto",                                             IF(MONTH(MAX(DimCalendario[Date]))-1=9,"Setiembre",                                                 IF(MONTH(MAX(DimCalendario[Date]))-1=10,"Octubre",                                                     IF(MONTH(MAX(DimCalendario[Date]))-1=11,"Noviembre",                                                         IF(MONTH(MAX(DimCalendario[Date]))-1=12,"Diciembre")))))))))))) &" "&YEAR(MAX(DimCalendario[Date]))

```DAX
IF(MONTH(MAX(DimCalendario[Date]))-1=1,"Enero",
                IF(MONTH(MAX(DimCalendario[Date]))-1=2,"Febrero",
                    IF(MONTH(MAX(DimCalendario[Date]))-1=3,"Marzo",
                        IF(MONTH(MAX(DimCalendario[Date]))-1=4,"Abril",
                            IF(MONTH(MAX(DimCalendario[Date]))-1=5,"Mayo",
                                IF(MONTH(MAX(DimCalendario[Date]))-1=6,"Junio",
                                    IF(MONTH(MAX(DimCalendario[Date]))-1=7,"Julio",
                                        IF(MONTH(MAX(DimCalendario[Date]))-1=8,"Agosto",
                                            IF(MONTH(MAX(DimCalendario[Date]))-1=9,"Setiembre",
                                                IF(MONTH(MAX(DimCalendario[Date]))-1=10,"Octubre",
                                                    IF(MONTH(MAX(DimCalendario[Date]))-1=11,"Noviembre",
                                                        IF(MONTH(MAX(DimCalendario[Date]))-1=12,"Diciembre")))))))))))) &" "&YEAR(MAX(DimCalendario[Date]))
```

### Ultimodiamesactual

Descripcion: FORMAT(EOMONTH(TODAY(),0),"DD/MM/YYYY")

```DAX
FORMAT(EOMONTH(TODAY(),0),"DD/MM/YYYY")
```

## MedidasVentas

### % Cump Cobrado de VentaSL

Formato: `0.00\ %;-0.00\ %;0.00\ %`

Descripcion: DIVIDE(MedidasCuotaCobranza[Cobrado mes actual],MedidasVentas[Venta SL],0)

```DAX
DIVIDE(MedidasCuotaCobranza[Cobrado mes actual],MedidasVentas[Venta SL],0)
```

### % Cump cuota de venta

Formato: `0.00\ %;-0.00\ %;0.00\ %`

Descripcion: DIVIDE(MedidasVentas[Venta SL],MedidasVentas[Cuota Venta],0)

```DAX
DIVIDE(MedidasVentas[Venta SL],MedidasVentas[Cuota Venta],0)
```

### % Cump Cuota de venta sele repre

Formato: `0.00\ %;-0.00\ %;0.00\ %`

Descripcion:  IF(ISBLANK([SeleRepre]),DIVIDE(MedidasVentas[Venta SL],MedidasVentas[Cuota Venta],0),     DIVIDE(MedidasVentas[Venta SL],[Cuota Venta SeleRepre],0))

```DAX

IF(ISBLANK([SeleRepre]),DIVIDE(MedidasVentas[Venta SL],MedidasVentas[Cuota Venta],0),
    DIVIDE(MedidasVentas[Venta SL],[Cuota Venta SeleRepre],0))
```

### % Cump cuota de venta SL

Formato: `0.0\ %;-0.0\ %;0.0\ %`

Descripcion: DIVIDE(MedidasVentas[Venta SL],MedidasVentas[Cuota Venta SL],0)

```DAX
DIVIDE(MedidasVentas[Venta SL],MedidasVentas[Cuota Venta SL],0)
```

### % Cump de budget

Formato: `0.00\ %;-0.00\ %;0.00\ %`

Descripcion: DIVIDE(MedidasVentas[Venta SL],[Budget],0)

```DAX
DIVIDE(MedidasVentas[Venta SL],[Budget],0)
```

### % Cump MA

Descripcion: DIVIDE(MedidasVentas[Ventas mes actual],MedidasVentas[Cuota venta mes actual],0)

```DAX
DIVIDE(MedidasVentas[Ventas mes actual],MedidasVentas[Cuota venta mes actual],0)
```

### % Cump mes actual

Formato: `0\ %;-0\ %;0\ %`

Descripcion: CALCULATE  ( [% Cump MA] , DATESINPERIOD  (  DimCalendario[Date],  MAX  (  DimCalendario[Date] ) ,  -1 ,  MONTH )) 

```DAX
CALCULATE  ( [% Cump MA] , DATESINPERIOD  (  DimCalendario[Date],  MAX  (  DimCalendario[Date] ) ,  -1 ,  MONTH )) 
```

### % Cump. Mes anterior

Formato: `0\ %;-0\ %;0\ %`

Descripcion:  CALCULATE(                     CALCULATE([% Cump cuota de venta],                             DATEADD(FILTER(DATESMTD(DimCalendario[Date]),                                 DimCalendario[Date] <= TODAY()                                     ),                                     -1,MONTH                 )              ),FILTER(DimCalendario,                     DimCalendario[Date] <= TODAY()              ) )

```DAX

CALCULATE(
                    CALCULATE([% Cump cuota de venta],
                            DATEADD(FILTER(DATESMTD(DimCalendario[Date]),
                                DimCalendario[Date] <= TODAY()
                                    ),
                                    -1,MONTH
                )
             ),FILTER(DimCalendario,
                    DimCalendario[Date] <= TODAY()
             )
)
```

### % Variacion Acum Año

Formato: `0.00\ %;-0.00\ %;0.00\ %`

Descripcion:   (MedidasVentas[Ventas YTD CBH]-[AcumuladoHace1Año])/[AcumuladoHace1Año]

```DAX


(MedidasVentas[Ventas YTD CBH]-[AcumuladoHace1Año])/[AcumuladoHace1Año]
```

### % Variación de venta mensual solo segmento

Formato: `#,0.00\ %;-#,0.00\ %;#,0.00\ %`

Descripcion:  DIVIDE(([Venta Solo para segmento]-[Ventas mes anterior solo para segmento]),[Ventas mes anterior solo para segmento])

```DAX

DIVIDE(([Venta Solo para segmento]-[Ventas mes anterior solo para segmento]),[Ventas mes anterior solo para segmento])
```

### AcumuladoHace1Año

Formato: `"S/"\ #,0.00;-"S/"\ #,0.00;"S/"\ #,0.00`

Descripcion:  CALCULATE(                     CALCULATE([Ventas YTD CBH],                             DATEADD(FILTER(DATESMTD(DimCalendario[Date]),                                 DimCalendario[Date] <= TODAY()                                     ),                                     -1,YEAR                 )              ),FILTER(DimCalendario,                     DimCalendario[Date] <= TODAY()              ) )

```DAX

CALCULATE(
                    CALCULATE([Ventas YTD CBH],
                            DATEADD(FILTER(DATESMTD(DimCalendario[Date]),
                                DimCalendario[Date] <= TODAY()
                                    ),
                                    -1,YEAR
                )
             ),FILTER(DimCalendario,
                    DimCalendario[Date] <= TODAY()
             )
)
```

### Budget

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: SUM(BUDGET[Venta (PPTO)])

```DAX
SUM(BUDGET[Venta (PPTO)])
```

### Cuota Anual

Descripcion: CALCULATE(SUM(Fact_CuotaVenta[cuota_valor]),ALL(DimCalendario[Mes text]))

```DAX
CALCULATE(SUM(Fact_CuotaVenta[cuota_valor]),ALL(DimCalendario[Mes text]))
```

### Cuota Venta

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion:  /*CALCULATE(Sum(Fact_CuotaVenta[cuota_valor]),USERELATIONSHIP(Fact_CuotaVenta[kArticulo],DimArticulo[kArticulo]))*/ Sum(Fact_CuotaVenta[cuota_valor])

```DAX

/*CALCULATE(Sum(Fact_CuotaVenta[cuota_valor]),USERELATIONSHIP(Fact_CuotaVenta[kArticulo],DimArticulo[kArticulo]))*/
Sum(Fact_CuotaVenta[cuota_valor])
```

### Cuota venta mes actual

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: CALCULATE  ( [Cuota Venta] , DATESINPERIOD  (  DimCalendario[Date],  MAX  (  DimCalendario[Date] ) ,  -1 ,  MONTH )) 

```DAX
CALCULATE  ( [Cuota Venta] , DATESINPERIOD  (  DimCalendario[Date],  MAX  (  DimCalendario[Date] ) ,  -1 ,  MONTH )) 
```

### Cuota venta para maximo

Descripcion: [Cuota Venta]*1.8

```DAX
[Cuota Venta]*1.8
```

### Cuota Venta SeleRepre

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: IF(ISBLANK([SeleRepre]),[Cuota Venta],[Cuota Venta SL])

```DAX
IF(ISBLANK([SeleRepre]),[Cuota Venta],[Cuota Venta SL])
```

### Cuota Venta SL

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: Sum(Fact_CuotaVenta[cuota_valor])

```DAX
Sum(Fact_CuotaVenta[cuota_valor])
```

### End value

Descripcion: [Cuota Venta]*1.70

```DAX
[Cuota Venta]*1.70
```

### End value Budget

Descripcion: [Budget]*1.70

```DAX
[Budget]*1.70
```

### End value_Tpromedio

Descripcion: [Ticket promedio mes anterior]*1.7

```DAX
[Ticket promedio mes anterior]*1.7
```

### Franja 2 Budget

Descripcion: [Budget]*1

```DAX
[Budget]*1
```

### Franja1 Budget

Descripcion: [Budget]*0.5

```DAX
[Budget]*0.5
```

### Franja1_Tpromedio

Descripcion: [Ticket promedio mes anterior]*0.5

```DAX
[Ticket promedio mes anterior]*0.5
```

### Franja1_Vcuota

Descripcion: [Cuota Venta]*0.5

```DAX
[Cuota Venta]*0.5
```

### Franja2_Tpromedio

Descripcion: [Ticket promedio mes anterior]*1

```DAX
[Ticket promedio mes anterior]*1
```

### Franja2_Vcuota

Descripcion: [Cuota Venta]*1

```DAX
[Cuota Venta]*1
```

### iconoAño

Descripcion: [Ventas YTD CBH]-[AcumuladoHace1Año]

```DAX
[Ventas YTD CBH]-[AcumuladoHace1Año]
```

### iconoCumpCuotaVenta

Descripcion: MedidasVentas[Venta SL]-[Cuota Venta]

```DAX
MedidasVentas[Venta SL]-[Cuota Venta]
```

### iconoCumpCuotaVenta SL

Descripcion: MedidasVentas[Venta SL]-[Cuota Venta SL]

```DAX
MedidasVentas[Venta SL]-[Cuota Venta SL]
```

### iconoVentaActual

Descripcion: [Ventas mes actual]-MedidasVentas[Ventas mes anteriorF]

```DAX
[Ventas mes actual]-MedidasVentas[Ventas mes anteriorF]
```

### iconoVentaAnterior

Descripcion: [Ventas mes anterior]-MedidasVentas[Ventas mes anterioranterior]

```DAX
[Ventas mes anterior]-MedidasVentas[Ventas mes anterioranterior]
```

### iconoVentaAñoAnterior

Descripcion: MedidasVentas[Ventas mes actual]-[VentaMesHace1Año]

```DAX
MedidasVentas[Ventas mes actual]-[VentaMesHace1Año]
```

### Información actualizada al

Descripcion: FORMAT(CALCULATE(MAX(Fact_Ventas[fecha]),ALL(DimCalendario)),"DD/MM/YYYY")

```DAX
FORMAT(CALCULATE(MAX(Fact_Ventas[fecha]),ALL(DimCalendario)),"DD/MM/YYYY")
```

### N° Pedidos

Formato: `#,0`

Descripcion: DISTINCTCOUNT(Fact_Ventas[pedido])

```DAX
DISTINCTCOUNT(Fact_Ventas[pedido])
```

### Número de pedidos

Formato: `0`

Descripcion: DISTINCTCOUNT(Fact_Ventas[pedido])

```DAX
DISTINCTCOUNT(Fact_Ventas[pedido])
```

### Prueba

Formato: `General Date`

Descripcion: MAX(Fact_Ventas[fecha])

```DAX
MAX(Fact_Ventas[fecha])
```

### puntosventa

Formato: `0`

Descripcion: DISTINCTCOUNT(Fact_CuotaVenta[bruto])

```DAX
DISTINCTCOUNT(Fact_CuotaVenta[bruto])
```

### Rentabilidad

Formato: `#,0\ %;-#,0\ %;#,0\ %`

Descripcion: DIVIDE(MedidasMargen[Margen],MedidasVentas[Venta SL],0)

```DAX
DIVIDE(MedidasMargen[Margen],MedidasVentas[Venta SL],0)
```

### SeleRepre

Descripcion: SELECTEDVALUE(DimAgente[DesAgente])

```DAX
SELECTEDVALUE(DimAgente[DesAgente])
```

### Tex cum ven act Rep

Descripcion: "Cumplimiento actual:"

```DAX
"Cumplimiento actual:"
```

### Text budget

Descripcion: "Cumplimiento actual:"

```DAX
"Cumplimiento actual:"
```

### Ticket promedio

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: DIVIDE(MedidasVentas[Venta SL],[Número de pedidos],0)

```DAX
DIVIDE(MedidasVentas[Venta SL],[Número de pedidos],0)
```

### Ticket promedio mes anterior

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion:  var ultimafecha = MAX(Fact_Ventas[fecha]) return CALCULATE(                     CALCULATE([Ticket promedio],                             Filter(DATEADD(DimCalendario[Date],-1,MONTH),DAY(DimCalendario[Date])<=DAY(ultimafecha))              ),FILTER(DimCalendario,                     DimCalendario[Date] <= TODAY()              ) )

```DAX

var ultimafecha = MAX(Fact_Ventas[fecha])
return CALCULATE(
                    CALCULATE([Ticket promedio],
                            Filter(DATEADD(DimCalendario[Date],-1,MONTH),DAY(DimCalendario[Date])<=DAY(ultimafecha))
             ),FILTER(DimCalendario,
                    DimCalendario[Date] <= TODAY()
             )
)
```

### Total budget

Formato: `"S/"\ #,0.0;#,0.0\ -"S/";"S/"\ #,0.0`

Descripcion: SUM(BUDGET[Venta (PPTO)])

```DAX
SUM(BUDGET[Venta (PPTO)])
```

### Total unidades vendidas

Formato: `#,0.00`

Descripcion: SUM(Fact_Ventas[unidades])

```DAX
SUM(Fact_Ventas[unidades])
```

### Venta

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: CALCULATE(SUM(Fact_Ventas[Venta neta]), USERELATIONSHIP(DimArticulo[kArticulo],Fact_CuotaVenta[kArticulo]))

```DAX
CALCULATE(SUM(Fact_Ventas[Venta neta]), USERELATIONSHIP(DimArticulo[kArticulo],Fact_CuotaVenta[kArticulo]))
```

### Venta Cuota Sele Repre

Formato: `"S/"\ #,0;#,0\ -"S/";"S/"\ #,0`

Descripcion:  IF(ISBLANK([SeleRepre]),[Cuota Venta],[Cuota Venta SL]) 

```DAX

IF(ISBLANK([SeleRepre]),[Cuota Venta],[Cuota Venta SL])

```

### Venta mes anterior T

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: CALCULATE(     SUM(Fact_Ventas[Venta neta]),     DATEADD(DimCalendario[Date],-1,MONTH) )

```DAX
CALCULATE(
    SUM(Fact_Ventas[Venta neta]),
    DATEADD(DimCalendario[Date],-1,MONTH)
)
```

### Venta SL

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: IF(ISBLANK(SUM(Fact_Ventas[Venta neta])),BLANK(),SUM(Fact_Ventas[Venta neta]))

```DAX
IF(ISBLANK(SUM(Fact_Ventas[Venta neta])),BLANK(),SUM(Fact_Ventas[Venta neta]))
```

### Venta Solo para segmento

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: SUM(Fact_Ventas[Venta neta])

```DAX
SUM(Fact_Ventas[Venta neta])
```

### VentaMesAnterior

Descripcion: "Venta Mes" & FORMAT([Ventas mes anterior],"#000")

```DAX
"Venta Mes" & FORMAT([Ventas mes anterior],"#000")
```

### VentaMesHace1Año

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion:  CALCULATE(                     CALCULATE(MedidasVentas[Venta SL],                             DATEADD(FILTER(DATESMTD(DimCalendario[Date]),                                 DimCalendario[Date] <= TODAY()                                     ),                                     -1,YEAR                 )              ),FILTER(DimCalendario,                     DimCalendario[Date] <= TODAY()              ) )

```DAX

CALCULATE(
                    CALCULATE(MedidasVentas[Venta SL],
                            DATEADD(FILTER(DATESMTD(DimCalendario[Date]),
                                DimCalendario[Date] <= TODAY()
                                    ),
                                    -1,YEAR
                )
             ),FILTER(DimCalendario,
                    DimCalendario[Date] <= TODAY()
             )
)
```

### Ventas año actual

Descripcion:  VAR X = [Año Actual]    RETURN CALCULATE(MedidasVentas[Venta SL],DimCalendario[Año]=X)

```DAX

VAR X = [Año Actual]    RETURN
CALCULATE(MedidasVentas[Venta SL],DimCalendario[Año]=X)
```

### Ventas año anterior

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion:  VAR X = [Año Anterior] RETURN CALCULATE(MedidasVentas[Venta SL],DimCalendario[Año]=X)

```DAX

VAR X = [Año Anterior] RETURN
CALCULATE(MedidasVentas[Venta SL],DimCalendario[Año]=X)
```

### Ventas mes actual

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: CALCULATE  ( MedidasVentas[Venta SL] , DATESINPERIOD  (  DimCalendario[Date],  MAX  (  DimCalendario[Date] ) ,  -1 ,  MONTH )) 

```DAX
CALCULATE  ( MedidasVentas[Venta SL] , DATESINPERIOD  (  DimCalendario[Date],  MAX  (  DimCalendario[Date] ) ,  -1 ,  MONTH )) 
```

### Ventas mes anterior

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion:  var ultimafecha = MAX(Fact_Ventas[fecha]) return CALCULATE(                     CALCULATE(MedidasVentas[Venta SL],                             Filter(DATEADD(DimCalendario[Date],-1,MONTH),DAY(DimCalendario[Date])<=DAY(ultimafecha))              ),FILTER(DimCalendario,                     DimCalendario[Date] <= TODAY()              ) )

```DAX

var ultimafecha = MAX(Fact_Ventas[fecha])
return CALCULATE(
                    CALCULATE(MedidasVentas[Venta SL],
                            Filter(DATEADD(DimCalendario[Date],-1,MONTH),DAY(DimCalendario[Date])<=DAY(ultimafecha))
             ),FILTER(DimCalendario,
                    DimCalendario[Date] <= TODAY()
             )
)
```

### Ventas mes anterior solo para segmento

Formato: `"S/"\ #,0;#,0\ -"S/";"S/"\ #,0`

Descripcion:  var ultimafecha = MAX(Fact_Ventas[fecha]) return CALCULATE(                     CALCULATE(MedidasVentas[Venta SL],                             Filter(DATEADD(DimCalendario[Date],-1,MONTH),DAY(DimCalendario[Date])<=DAY(ultimafecha))              ),FILTER(DimCalendario,                     DimCalendario[Date] <= TODAY()              ))

```DAX

var ultimafecha = MAX(Fact_Ventas[fecha])
return CALCULATE(
                    CALCULATE(MedidasVentas[Venta SL],
                            Filter(DATEADD(DimCalendario[Date],-1,MONTH),DAY(DimCalendario[Date])<=DAY(ultimafecha))
             ),FILTER(DimCalendario,
                    DimCalendario[Date] <= TODAY()
             ))
```

### Ventas mes anterioranterior

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion:  CALCULATE(                     CALCULATE(MedidasVentas[Venta SL],                             DATEADD(FILTER(DATESMTD(DimCalendario[Date]),                                 DimCalendario[Date] <= TODAY()                                     ),                                     -2,MONTH                 )              ),FILTER(DimCalendario,                     DimCalendario[Date] <= TODAY()              ) )

```DAX

CALCULATE(
                    CALCULATE(MedidasVentas[Venta SL],
                            DATEADD(FILTER(DATESMTD(DimCalendario[Date]),
                                DimCalendario[Date] <= TODAY()
                                    ),
                                    -2,MONTH
                )
             ),FILTER(DimCalendario,
                    DimCalendario[Date] <= TODAY()
             )
)
```

### Ventas mes anteriorF

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion:  var ultimafecha = MAX(Fact_Ventas[fecha]) return CALCULATE(                     CALCULATE(MedidasVentas[Venta SL],                             Filter(DATEADD(DimCalendario[Date],-1,MONTH),DAY(DimCalendario[Date])<=DAY(ultimafecha))              ),FILTER(DimCalendario,                     DimCalendario[Date] <= TODAY()              ) )

```DAX

var ultimafecha = MAX(Fact_Ventas[fecha])
return CALCULATE(
                    CALCULATE(MedidasVentas[Venta SL],
                            Filter(DATEADD(DimCalendario[Date],-1,MONTH),DAY(DimCalendario[Date])<=DAY(ultimafecha))
             ),FILTER(DimCalendario,
                    DimCalendario[Date] <= TODAY()
             )
)
```

### Ventas YTD CBH

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: CALCULATE(MedidasVentas[Venta SL],DATESYTD(DimCalendario[Date]))

```DAX
CALCULATE(MedidasVentas[Venta SL],DATESYTD(DimCalendario[Date]))
```

### Ventas YTD CBH -1

Formato: `"S/"\ #,0.00;-"S/"\ #,0.00;"S/"\ #,0.00`

Descripcion:   CALCULATE ( MedidasVentas[Venta SL],     DATESYTD ( DATEADD ( DimCalendario[Date], -1, YEAR ) ) )

```DAX


CALCULATE (
MedidasVentas[Venta SL],
    DATESYTD ( DATEADD ( DimCalendario[Date], -1, YEAR ) )
)
```

### VentasTotalAñoActual

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: CALCULATE( IF( 	ISFILTERED('DimCalendario'[Date]), 	ERROR("La medida rápida de inteligencia de tiempo solo se puede agrupar o filtrar mediante la jerarquía de datos proporcionada por Power BI o por la columna de datos principal."), 	MedidasVentas[Venta SL] ),ALLEXCEPT(DimCalendario,DimCalendario[Año]))

```DAX
CALCULATE(
IF(
	ISFILTERED('DimCalendario'[Date]),
	ERROR("La medida rápida de inteligencia de tiempo solo se puede agrupar o filtrar mediante la jerarquía de datos proporcionada por Power BI o por la columna de datos principal."),
	MedidasVentas[Venta SL]
),ALLEXCEPT(DimCalendario,DimCalendario[Año]))
```

### YoY% de Venta

Formato: `0.00\ %;-0.00\ %;0.00\ %`

Descripcion:  IF( 	ISFILTERED('DimCalendario'[Date]), 	ERROR("La medida rápida de inteligencia de tiempo solo se puede agrupar o filtrar mediante la jerarquía de datos proporcionada por Power BI o por la columna de datos principal."), 	VAR __PREV_YEAR = CALCULATE([Venta SL], DATEADD('DimCalendario'[Date], -1, YEAR)) 	RETURN 		DIVIDE([Venta SL] - __PREV_YEAR, __PREV_YEAR) )

```DAX

IF(
	ISFILTERED('DimCalendario'[Date]),
	ERROR("La medida rápida de inteligencia de tiempo solo se puede agrupar o filtrar mediante la jerarquía de datos proporcionada por Power BI o por la columna de datos principal."),
	VAR __PREV_YEAR = CALCULATE([Venta SL], DATEADD('DimCalendario'[Date], -1, YEAR))
	RETURN
		DIVIDE([Venta SL] - __PREV_YEAR, __PREV_YEAR)
)
```


