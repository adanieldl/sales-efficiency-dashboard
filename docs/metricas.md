# Metricas Del Dashboard

Catalogo de medidas DAX extraidas desde `powerbi/EFICIENCIA_DE_VENTAS.pbit`.

Las medidas estan agrupadas por grupo de medidas de Power BI y por carpeta de visualizacion. Cuando una medida no tiene carpeta asignada, se clasifica en `General`.

Total de medidas extraidas: 194

## Resumen

| Grupo de medidas | Carpeta | Cantidad |
|---|---:|---:|
| DimFormaPago | General | 1 |
| MedidasClientes | Cobertura | 15 |
| MedidasClientes | General | 20 |
| MedidasCuotaCobranza | General | 42 |
| MedidasMargen | General | 11 |
| MedidasPlanCobranza | General | 33 |
| MedidasTiempo | General | 8 |
| MedidasVentas | General | 64 |

## DimFormaPago

### Carpeta: General

#### Lista de valores de DesFormaPago

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

### Carpeta: Cobertura

#### % cobertura x zona

Formato: `0.00\ %;-0.00\ %;0.00\ %`

```DAX
IF(ISBLANK(DIVIDE([Cobertura],[Cobertura zona],0)),0,DIVIDE([Cobertura],[Cobertura zona],0))
```

#### Clientes coberturados barrekov

Formato: `0`

```DAX
CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]), ALL(DimCalendario))
```

#### Cobertura

Formato: `#,0`

Descripcion:  IF(ISBLANK(CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),Fact_Ventas[ClienteVendido])),0,CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),Fact_Ventas[ClienteVendido]))

```DAX

IF(ISBLANK(CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),Fact_Ventas[ClienteVendido])),BLANK(),CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),Fact_Ventas[ClienteVendido]))
```

#### Cobertura mes anterior

Formato: `#,0`

Descripcion:  CALCULATE(     CALCULATE([% cobertura x zona],DATEADD(FILTER(DATESMTD(DimCalendario[Date]),DimCalendario[Date] <= TODAY()),         -1,MONTH)     ),FILTER(DimCalendario,DimCalendario[Date] <= TODAY()) )

```DAX

CALCULATE(
    CALCULATE([% cobertura x zona],DATEADD(FILTER(DATESMTD(DimCalendario[Date]),DimCalendario[Date] <= TODAY()),
        -1,MONTH)
    ),FILTER(DimCalendario,DimCalendario[Date] <= TODAY())
)
```

#### Cobertura Ruc Unico

Formato: `#,0`

Descripcion:  CALCULATE(DISTINCTCOUNT(Fact_Ventas[DocumentoUnico]),Fact_Ventas[ClienteVendido])

```DAX

CALCULATE(DISTINCTCOUNT(Fact_Ventas[DocumentoUnico]),Fact_Ventas[ClienteVendido])
```

#### Cobertura ruc unico mes anterior

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

#### Cobertura sin repre

Formato: `#,0`

Descripcion:  CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),Fact_Ventas[ClienteVendido],ALL(DimAgente[DesAgente]))

```DAX

CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),Fact_Ventas[ClienteVendido],ALL(DimAgente[DesAgente]))
```

#### Cobertura zona

Formato: `#,0`

```DAX
CALCULATE(DISTINCTCOUNT(DimCliente[kCliente]), ALL(DimCalendario))
```

#### Cobertura zona prueba

Formato: `0`

```DAX
DISTINCTCOUNT(DimCliente[kCliente])
```

#### End value_Vcobertura

```DAX
[Tasa de Cobertura mes anterior]*2.2
```

#### Franja1_Vcobertura

```DAX
[Tasa de Cobertura mes anterior]*0.5
```

#### Franja2_Vcobertura

Formato: `0`

```DAX
[Tasa de Cobertura mes anterior]*1
```

#### Maximo valor cobertura zona

```DAX
[Cobertura zona]*1.7
```

#### Tasa de cobertura

Formato: `0.0\ %;-0.0\ %;0.0\ %`

```DAX
DIVIDE([Cobertura],[Cobertura zona],0)
```

#### Tasa de Cobertura mes anterior

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

### Carpeta: General

#### % YTD

Formato: `0\ %;-0\ %;0\ %`

```DAX
DIVIDE(([Cobertura YTD]-[Cobertura YTD -1]),[Cobertura YTD -1],0)
```

#### Clientes ALL

Formato: `#,0`

```DAX
CALCULATE([Total clientes Maestro],ALL(DimCalendario[Mes text]))
```

#### Clientes CodigoRapido YTD CBH

Formato: `#,0`

```DAX
CALCULATE([Total clientes Maestro],DATESYTD(DimCalendario[Date]))
```

#### Clientes Recurrencia

Formato: `0`

```DAX
DISTINCTCOUNT(DimClientePrimeraCompra[kCliente])
```

#### ClientesNuevos

Formato: `#,0`

Descripcion:           /*IF(ISBLANK(CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),VW_PrimeraCompraClientesCBH[StatusRecurrencia] = "Nuevo cliente",Fact_Ventas[ClienteVendido])),0,CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),VW_PrimeraCompraClientesCBH[StatusRecurrencia] = "Nuevo cliente",Fact_Ventas[ClienteVendido]))*/     CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),DimClientePrimeraCompra[StatusRecurrencia] = "Nuevo cliente",Fact_Ventas[ClienteVendido])

```DAX

    
    /*IF(ISBLANK(CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),VW_PrimeraCompraClientesCBH[StatusRecurrencia] = "Nuevo cliente",Fact_Ventas[ClienteVendido])),0,CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),VW_PrimeraCompraClientesCBH[StatusRecurrencia] = "Nuevo cliente",Fact_Ventas[ClienteVendido]))*/
    CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),DimClientePrimeraCompra[StatusRecurrencia] = "Nuevo cliente",Fact_Ventas[ClienteVendido])
```

#### ClientesNuevos mes anterior

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

#### ClientesNuevosRucUnico

Formato: `0`

Descripcion:           CALCULATE(DISTINCTCOUNT(DimCliente[Documento]),DimClientePrimeraCompra[StatusRecurrencia] = "Nuevo cliente",Fact_Ventas[ClienteVendido])

```DAX

    
    CALCULATE(DISTINCTCOUNT(DimCliente[Documento]),DimClientePrimeraCompra[StatusRecurrencia] = "Nuevo cliente",Fact_Ventas[ClienteVendido])
```

#### ClientesRecurrentes

Formato: `#,0`

Descripcion:  CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),Fact_Ventas[ClienteNuevo] = "Cliente Recurrente")

```DAX

CALCULATE(DISTINCTCOUNT(Fact_Ventas[kCliente]),Fact_Ventas[ClienteNuevo] = "Cliente Recurrente")
```

#### Cobertura YTD

Formato: `0`

```DAX
CALCULATE( sumX(VALUES(DimCalendario[Mes text]),[Cobertura]),DATESYTD(dateadd(DimCalendario[Date],0,MONTH)))//DATESYTD(DimCalendario[Date]))
```

#### Cobertura YTD -1

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

#### CoberturaEvolucion

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

#### CoberturaLinea

Formato: `0`

```DAX
CALCULATE(DISTINCTCOUNT(DimArticulo[Nivel2]),Fact_Ventas[ClienteVendido])
```

#### End value_Vclientesnuevos

```DAX
MedidasClientes[ClientesNuevos mes anterior]*2.2
```

#### Franja1_Vclientesnuevos

```DAX
MedidasClientes[ClientesNuevos mes anterior]*0.5
```

#### Franja2_Vclientesnuevos

Formato: `0`

```DAX
MedidasClientes[ClientesNuevos mes anterior]*1
```

#### ImporteCobranza

Formato: `"S/"\ #,0.00;-"S/"\ #,0.00;"S/"\ #,0.00`

```DAX
SUM(Fact_PlanCobranza[importe])
```

#### ImporteCobranzaM

```DAX
IF([ImporteCobranza]=0,BLANK(),[ImporteCobranza])
```

#### Total clientes Maestro

Formato: `#,0`

```DAX
DISTINCTCOUNT(DimClientePrimeraCompra[kCliente])
```

#### Total clientes unicos

Formato: `#,0`

```DAX
DISTINCTCOUNT(DimCliente[Documento])
```

#### Total clientes zona

Formato: `0`

```DAX
DISTINCTCOUNT(DimCliente[kCliente])
```

## MedidasCuotaCobranza

### Carpeta: General

#### % CumplimientoCuotaCobranza

Formato: `0.00\ %;-0.00\ %;0.00\ %`

```DAX
DIVIDE(MedidasCuotaCobranza[Cobrado2],[Cuota cobranza],0)
```

#### % CumplimientoCuotaCobranza/2

```DAX
DIVIDE(MedidasCuotaCobranza[Cobrado2],[Cuota cobranza],0)/1.5
```

#### Cobrado mes actual

```DAX
CALCULATE  ([Cobrado2] , DATESINPERIOD  (  DimCalendario[Date],  MAX  (  DimCalendario[Date] ) ,  -1 ,  MONTH )) 
```

#### Cobrado mes actual P

```DAX
CALCULATE  ([Cobrado] , DATESINPERIOD  (  DimCalendario[Date],  MAX  (  DimCalendario[Date] ) ,  -1 ,  MONTH )) 
```

#### Cobrado mes anterior

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

#### Cobrado Mes Anterior Crec %

Formato: `0.00\ %;-0.00\ %;0.00\ %`

```DAX
DIVIDE(([Cobrado2]-[Cobrado Mes Anterior Mismos dias]),[Cobrado Mes Anterior Mismos dias])
```

#### Cobrado Mes Anterior Mismos dias

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

#### Cobrado mes anterior P

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

#### Cobrado YTD F

```DAX
CALCULATE(CALCULATE([Cobrado],DATESYTD(DimCalendario[Date])),ALLEXCEPT(DimCalendario,DimCalendario[Last Business Date]))
```

#### Cobrado2

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

```DAX
IF(SUM(Fact_CuotaCobranza[cobranza_cuota])=0,BLANK(),SUM(Fact_CuotaCobranza[cobranza_cuota]))
```

#### CobradoAcumulado

```DAX
CALCULATE([Cobrado], DATESYTD(DimCalendario[Date]))
```

#### CobradoHace1Año

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

#### CobradoMesHace1AñoF

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

#### CobradoRepre

```DAX
SUM(Fact_CuotaCobranza[cobranza_cuota])
```

#### Cuota cobranza

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

```DAX
SUM(Fact_CuotaCobranza[cuota])
```

#### CuotaCobranzaRepre

```DAX
SUM(Fact_CuotaCobranza[cuota])
```

#### DiasGiro

```DAX
([Cobrado2]/[Venta SL])*360 
```

#### End value_Vcuotacobranza

```DAX
[Cuota cobranza]*1.70
```

#### F1 Ccobranza

Formato: `0.00\ %;-0.00\ %;0.00\ %`

```DAX
0.69
```

#### F2 Ccobranza

Formato: `0\ %;-0\ %;0\ %`

```DAX
0.85
```

#### F3 Ccobranza

```DAX
0.94
```

#### F4 Ccobranza

Formato: `0`

```DAX
1
```

#### FechaVencimiento

Formato: `0`

```DAX
DATEDIFF(MAX(DimCalendario[Date]),MAX(Fact_PlanCobranza[fecha_vencimiento]),DAY)
```

#### Franja1_CCOBRANZA_FC

```DAX
[Cuota cobranza]*0.5
```

#### Franja2_CCOBRANZA_FC

```DAX
[Cuota cobranza]*1
```

#### icono

```DAX
[Cobrado YTD F] - [CobradoHace1Año]
```

#### iconoCobradohace1añoF

```DAX
[Cobrado mes actual] - [CobradoHace1Año]
```

#### IconoCobradoMes

```DAX
[Cobrado mes actual] - [Cobrado mes anterior]
```

#### Meta Ccobranza

```DAX
0.85
```

#### Por cobrar mes anterior

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

#### PorCobrar

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: --VAR _COB = SUM(Fact_CuotaCobranza[Por cobrar]) --RETURN IF(_COB = 0 || ISBLANK(_COB) , BLANK(),_COB) SUM(Fact_CuotaCobranza[Por cobrar])

```DAX
--VAR _COB = SUM(Fact_CuotaCobranza[Por cobrar])
--RETURN IF(_COB = 0 || ISBLANK(_COB) , BLANK(),_COB)
SUM(Fact_CuotaCobranza[Por cobrar])
```

#### PorCobrar PorVencer

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

```DAX
CALCULATE([PorCobrar],Fact_CuotaCobranza[Dias Vencidos]<0)
```

#### PorCobrar Vencido

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

```DAX
CALCULATE([PorCobrar],Fact_CuotaCobranza[Dias Vencidos]>=0)
```

#### PorCobrar Vencido Dias Retraso Ponderado

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

#### PorCobrar Vencido Ratio%

Formato: `0.00\ %;-0.00\ %;0.00\ %`

Descripcion:   var _num = [PorCobrar Vencido]   var _table = SUMMARIZE(ALLSELECTED(Fact_CuotaCobranza),"Suma",[PorCobrar Vencido])  var _den = GROUPBY(_table,"Sumaa",SUMX(CURRENTGROUP(),[Suma]))   Return    DIVIDE(_num,_den,BLANK()) 

```DAX

 var _num = [PorCobrar Vencido]

 var _table = SUMMARIZE(ALLSELECTED(Fact_CuotaCobranza),"Suma",[PorCobrar Vencido])
 var _den = GROUPBY(_table,"Sumaa",SUMX(CURRENTGROUP(),[Suma]))

 Return 

 DIVIDE(_num,_den,BLANK())

```

#### PorCobrar Vencido USERELATIONSHIP Cliente

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: //CALCULATE([PorCobrar],Fact_CuotaCobranza[Dias Vencidos]>=0,USERELATIONSHIP(DimCliente[Zona],DimAgenteZona[CodZona])) [PorCobrar]

```DAX
//CALCULATE([PorCobrar],Fact_CuotaCobranza[Dias Vencidos]>=0,USERELATIONSHIP(DimCliente[Zona],DimAgenteZona[CodZona]))
[PorCobrar]
```

#### SeleRepreCuota

```DAX
IF(ISBLANK(SELECTEDVALUE(DimAgente[DesAgente])),[Cuota cobranza],[CuotaCobranzaRepre])
```

#### Tasa de morosidad Cuota

```DAX
DIVIDE([PorCobrar Vencido],[PorCobrar],0)
```

#### Tex cum ccobranza_fc

```DAX
"Cumplimiento actual:"
```

#### Text Cuota Cobranza

```DAX
"<Al corte de hoy el avance de la cobranza es de " & FORMAT( [% CumplimientoCuotaCobranza],"Percent") & ", tenemos Vencidos -> S/. " & SUBSTITUTE(FORMAT([PorCobrar Vencido],"#,0"),".",",") & "  y en el tramo de cuentas morosas > 60 días -> S/. " & SUBSTITUTE(format(CALCULATE([PorCobrar Vencido],FILTER(Fact_CuotaCobranza,[Dias Vencidos]>=60)),"#,0"),".",",") & ">"
```

#### Total clientes por cobrar

Formato: `#,0`

```DAX
CALCULATE(DISTINCTCOUNT(Fact_CuotaCobranza[kCliente]),Fact_CuotaCobranza[Por cobrar]>0)
```

#### Total clientes por cobrar mes anterior

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

### Carpeta: General

#### % Avance con respecto al mes anterior

Formato: `0\ %;-0\ %;0\ %`

```DAX
DIVIDE(MedidasMargen[Margen],MedidasMargen[Margen mes anterior],0)
```

#### % Margen

```DAX
"Margen vs venta neta: " & format(DIVIDE((MedidasVentas[Venta SL]-[Costo]),[Venta SL],0),"0.00%")
```

#### % Margen mes anterior

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

#### Costo

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

```DAX
SUM(Fact_Ventas[costo_bruto])
```

#### End value_Vmargen

```DAX
MedidasMargen[Margen mes anterior]*1.5
```

#### Franja1_Vmargen

```DAX
MedidasMargen[Margen mes anterior]*0.5
```

#### Franja2_Vmargen

```DAX
MedidasMargen[Margen mes anterior]*1
```

#### Margen

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

```DAX
MedidasVentas[Venta SL]-[Costo]
```

#### Margen mes actual

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

```DAX
CALCULATE  ( MedidasMargen[Margen] , DATESINPERIOD  (  DimCalendario[Date],  MAX  (  DimCalendario[Date] ) ,  -1 ,  MONTH )) 
```

#### Margen mes anterior

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

#### Prueba today

Formato: `General Date`

```DAX
MAX(Fact_Ventas[fecha])
```

## MedidasPlanCobranza

### Carpeta: General

#### %vencido

Formato: `0\ %;-0\ %;0\ %`

```DAX
DIVIDE([DeudaVencida],[Total saldo],0)
```

#### Cobrado

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

```DAX
CALCULATE(SUM(Fact_PlanCobranza[importe]),Fact_PlanCobranza[Status]="Al Día")
```

#### Deuda mes actual

```DAX
CALCULATE  ( [Total saldo] , DATESINPERIOD  (  DimCalendario[Date],  MAX  (  DimCalendario[Date] ) ,  -1 ,  MONTH )) 
```

#### DeudaVencida

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

```DAX
CALCULATE([Total saldo],Fact_PlanCobranza[Status]="Vencido")
```

#### End Value Morosidad

Formato: `0.00\ %;-0.00\ %;0.00\ %`

```DAX
1
```

#### F1 Tmoro

Formato: `0.00\ %;-0.00\ %;0.00\ %`

```DAX
0.2
```

#### F2 Tmoro

Formato: `0.00\ %;-0.00\ %;0.00\ %`

```DAX
0.3
```

#### Importe vencido

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

```DAX
CALCULATE(SUM(Fact_PlanCobranza[importe]),Fact_PlanCobranza[Status]="Vencido")
```

#### Importe vencido mes anterior

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

#### Nro de documentos

Formato: `#,0`

```DAX
DISTINCTCOUNT(Fact_PlanCobranza[documento])
```

#### Ratio vencido de por cobrar

Formato: `0.0\ %;-0.0\ %;0.0\ %`

```DAX
DIVIDE([DeudaVencida],[Total saldo],0)
```

#### Saldo por vencer

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion:  IF(ISBLANK(CALCULATE([Total saldo],Fact_PlanCobranza[Status]="Por vencer")),0,CALCULATE([Total saldo],Fact_PlanCobranza[Status]="Por vencer"))

```DAX

IF(ISBLANK(CALCULATE([Total saldo],Fact_PlanCobranza[Status]="Por vencer")),BLANK(),CALCULATE([Total saldo],Fact_PlanCobranza[Status]="Por vencer"))
```

#### Saldo por vencer mes anterior

Descripcion:  IF(ISBLANK(CALCULATE([Saldo por vencer],DATEADD(DimCalendario[Date],-1,MONTH))),0,CALCULATE([Saldo por vencer],DATEADD(DimCalendario[Date],-1,MONTH)))

```DAX

IF(ISBLANK(CALCULATE([Saldo por vencer],DATEADD(DimCalendario[Date],-1,MONTH))),BLANK(),CALCULATE([Saldo por vencer],DATEADD(DimCalendario[Date],-1,MONTH)))
```

#### Saldo vencido mes anterior

```DAX
CALCULATE([DeudaVencida],DATEADD(DimCalendario[Date],-1,MONTH))
```

#### Sele mes año

```DAX
"De inicios del "&SELECTEDVALUE(DimCalendario[Año])&" hasta "& SELECTEDVALUE(DimCalendario[Mes text completo])&" del "&SELECTEDVALUE(DimCalendario[Año])
```

#### SeleCanal

```DAX
"DETALLE DE VENCIDOS DE "&IF(ISBLANK(SELECTEDVALUE(DimAgenteZona[Nivel1])),"TODOS LOS CANALES",UPPER(SELECTEDVALUE(DimAgenteZona[Nivel1])))
```

#### Tasa de morosidad

Formato: `0.00\ %;-0.00\ %;0.00\ %`

```DAX
DIVIDE([DeudaVencida],[Total saldo],0)
```

#### Total Cliente YTD

Formato: `#,0`

```DAX
CALCULATE(DISTINCTCOUNT(Fact_PlanCobranza[kCliente]),DATESYTD(DimCalendario[Date]))
```

#### Total Clientes con Saldo REP

Formato: `#,0`

```DAX
DISTINCTCOUNT(Fact_PlanCobranza[kCliente])
```

#### Total Clientes con Saldo YTD

Formato: `#,0`

Descripcion: CALCULATE(DISTINCTCOUNT(Fact_PlanCobranza[kCliente]),Fact_PlanCobranza[Status]<>"Al día",DATESYTD(DimCalendario[Date]))   

```DAX
CALCULATE(DISTINCTCOUNT(Fact_PlanCobranza[kCliente]),Fact_PlanCobranza[Status]<>"Al día",DATESYTD(DimCalendario[Date]))



```

#### Total Clientes YTD

Formato: `0`

```DAX
CALCULATE(DISTINCTCOUNT(Fact_PlanCobranza[kCliente]),DATESYTD(DimCalendario[Date]))
```

#### Total deuda CONREPRE

Formato: `"S/"\ #,0.00;-"S/"\ #,0.00;"S/"\ #,0.00`

```DAX
SUM(Fact_PlanCobranza[importe])
```

#### Total importe

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

```DAX
SUM(Fact_PlanCobranza[importe])
```

#### Total Importe AlContado

```DAX
CALCULATE([Total importe],DimFormaPago[Tipo]="Contado")
```

#### Total Importe YTD

```DAX
CALCULATE(SUM(Fact_PlanCobranza[importe]),DATESYTD(DimCalendario[Date]))
```

#### Total saldo

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: var _saldo = SUM(Fact_PlanCobranza[saldo]) return if(ISBLANK(_saldo) || _saldo = 0,BLANK(),_saldo)

```DAX
var _saldo = SUM(Fact_PlanCobranza[saldo])
return if(ISBLANK(_saldo) || _saldo = 0,BLANK(),_saldo)
```

#### Total Saldo Años Anteriores

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion:      var _fecMin = calculate(MIN(DimCalendario[Date]),ALL(DimCalendario))     var _fecMax = ENDOFYEAR(DATEADD(DimCalendario[Date],-1,YEAR)) return     CALCULATE([Total saldo],FILTER(DimCalendario,DimCalendario[Date] >= _fecMin && DimCalendario[Date] <= _fecMax))

```DAX

    var _fecMin = calculate(MIN(DimCalendario[Date]),ALL(DimCalendario))
    var _fecMax = ENDOFYEAR(DATEADD(DimCalendario[Date],-1,YEAR))
return
    CALCULATE([Total saldo],FILTER(DimCalendario,DimCalendario[Date] >= _fecMin && DimCalendario[Date] <= _fecMax))
```

#### Total Saldo YTD

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

```DAX
CALCULATE(SUM(Fact_PlanCobranza[saldo]),DATESYTD(DimCalendario[Date]))
```

#### Total saldo YTD mes anterior

Descripcion:  CALCULATE([Total Saldo YTD],DATEADD(DimCalendario[Date],-1,MONTH))

```DAX

CALCULATE([Total Saldo YTD],DATEADD(DimCalendario[Date],-1,MONTH))
```

#### TotalClientes

Formato: `#,0`

```DAX
DISTINCTCOUNT(Fact_PlanCobranza[kCliente])
```

#### TotalClientes conSaldo mes anterior

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

#### TotalSaldoaCobrar

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

```DAX
CALCULATE(SUM(Fact_PlanCobranza[saldo]),Fact_PlanCobranza[Status]<>"Al Día")
```

#### TotalSaldoaCobrarMesAnterior

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

### Carpeta: General

#### _fecMin

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

#### Año Actual

```DAX
"Venta acumulada "&CALCULATE(MAX(DimCalendario[Año]),ALLSELECTED())&" Vs "&CALCULATE(MAX(DimCalendario[Año]),ALLSELECTED())-1
```

#### Año Actual cobranza

```DAX
"Cobranza acumulada "&CALCULATE(MAX(DimCalendario[Año]),ALLSELECTED())
```

#### Año Anterior

Formato: `0`

```DAX
[Año Actual]-1
```

#### Mes actual

```DAX
MAXX(FILTER(DimCalendario,DimCalendario[Date] = MAX(DimCalendario[Date])),DimCalendario[Mes text completo]&" "&YEAR(MAX(DimCalendario[Date])))
```

#### Mes actual - 1 año

```DAX
MAXX(FILTER(DimCalendario,DimCalendario[Date] = MAX(DimCalendario[Date])),DimCalendario[Mes text completo])&" "&YEAR(MAX(DimCalendario[Date]))-1
```

#### Mes anterior

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

#### Ultimodiamesactual

```DAX
FORMAT(EOMONTH(TODAY(),0),"DD/MM/YYYY")
```

## MedidasVentas

### Carpeta: General

#### % Cump Cobrado de VentaSL

Formato: `0.00\ %;-0.00\ %;0.00\ %`

```DAX
DIVIDE(MedidasCuotaCobranza[Cobrado mes actual],MedidasVentas[Venta SL],0)
```

#### % Cump cuota de venta

Formato: `0.00\ %;-0.00\ %;0.00\ %`

```DAX
DIVIDE(MedidasVentas[Venta SL],MedidasVentas[Cuota Venta],0)
```

#### % Cump Cuota de venta sele repre

Formato: `0.00\ %;-0.00\ %;0.00\ %`

Descripcion:  IF(ISBLANK([SeleRepre]),DIVIDE(MedidasVentas[Venta SL],MedidasVentas[Cuota Venta],0),     DIVIDE(MedidasVentas[Venta SL],[Cuota Venta SeleRepre],0))

```DAX

IF(ISBLANK([SeleRepre]),DIVIDE(MedidasVentas[Venta SL],MedidasVentas[Cuota Venta],0),
    DIVIDE(MedidasVentas[Venta SL],[Cuota Venta SeleRepre],0))
```

#### % Cump cuota de venta SL

Formato: `0.0\ %;-0.0\ %;0.0\ %`

```DAX
DIVIDE(MedidasVentas[Venta SL],MedidasVentas[Cuota Venta SL],0)
```

#### % Cump de budget

Formato: `0.00\ %;-0.00\ %;0.00\ %`

```DAX
DIVIDE(MedidasVentas[Venta SL],[Budget],0)
```

#### % Cump MA

```DAX
DIVIDE(MedidasVentas[Ventas mes actual],MedidasVentas[Cuota venta mes actual],0)
```

#### % Cump mes actual

Formato: `0\ %;-0\ %;0\ %`

```DAX
CALCULATE  ( [% Cump MA] , DATESINPERIOD  (  DimCalendario[Date],  MAX  (  DimCalendario[Date] ) ,  -1 ,  MONTH )) 
```

#### % Cump. Mes anterior

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

#### % Variacion Acum Año

Formato: `0.00\ %;-0.00\ %;0.00\ %`

Descripcion:   (MedidasVentas[Ventas YTD CBH]-[AcumuladoHace1Año])/[AcumuladoHace1Año]

```DAX


(MedidasVentas[Ventas YTD CBH]-[AcumuladoHace1Año])/[AcumuladoHace1Año]
```

#### % Variación de venta mensual solo segmento

Formato: `#,0.00\ %;-#,0.00\ %;#,0.00\ %`

Descripcion:  DIVIDE(([Venta Solo para segmento]-[Ventas mes anterior solo para segmento]),[Ventas mes anterior solo para segmento])

```DAX

DIVIDE(([Venta Solo para segmento]-[Ventas mes anterior solo para segmento]),[Ventas mes anterior solo para segmento])
```

#### AcumuladoHace1Año

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

#### Budget

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

```DAX
SUM(BUDGET[Venta (PPTO)])
```

#### Cuota Anual

```DAX
CALCULATE(SUM(Fact_CuotaVenta[cuota_valor]),ALL(DimCalendario[Mes text]))
```

#### Cuota Venta

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion:  /*CALCULATE(Sum(Fact_CuotaVenta[cuota_valor]),USERELATIONSHIP(Fact_CuotaVenta[kArticulo],DimArticulo[kArticulo]))*/ Sum(Fact_CuotaVenta[cuota_valor])

```DAX

/*CALCULATE(Sum(Fact_CuotaVenta[cuota_valor]),USERELATIONSHIP(Fact_CuotaVenta[kArticulo],DimArticulo[kArticulo]))*/
Sum(Fact_CuotaVenta[cuota_valor])
```

#### Cuota venta mes actual

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

```DAX
CALCULATE  ( [Cuota Venta] , DATESINPERIOD  (  DimCalendario[Date],  MAX  (  DimCalendario[Date] ) ,  -1 ,  MONTH )) 
```

#### Cuota venta para maximo

```DAX
[Cuota Venta]*1.8
```

#### Cuota Venta SeleRepre

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

```DAX
IF(ISBLANK([SeleRepre]),[Cuota Venta],[Cuota Venta SL])
```

#### Cuota Venta SL

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

```DAX
Sum(Fact_CuotaVenta[cuota_valor])
```

#### End value

```DAX
[Cuota Venta]*1.70
```

#### End value Budget

```DAX
[Budget]*1.70
```

#### End value_Tpromedio

```DAX
[Ticket promedio mes anterior]*1.7
```

#### Franja 2 Budget

```DAX
[Budget]*1
```

#### Franja1 Budget

```DAX
[Budget]*0.5
```

#### Franja1_Tpromedio

```DAX
[Ticket promedio mes anterior]*0.5
```

#### Franja1_Vcuota

```DAX
[Cuota Venta]*0.5
```

#### Franja2_Tpromedio

```DAX
[Ticket promedio mes anterior]*1
```

#### Franja2_Vcuota

```DAX
[Cuota Venta]*1
```

#### iconoAño

```DAX
[Ventas YTD CBH]-[AcumuladoHace1Año]
```

#### iconoCumpCuotaVenta

```DAX
MedidasVentas[Venta SL]-[Cuota Venta]
```

#### iconoCumpCuotaVenta SL

```DAX
MedidasVentas[Venta SL]-[Cuota Venta SL]
```

#### iconoVentaActual

```DAX
[Ventas mes actual]-MedidasVentas[Ventas mes anteriorF]
```

#### iconoVentaAnterior

```DAX
[Ventas mes anterior]-MedidasVentas[Ventas mes anterioranterior]
```

#### iconoVentaAñoAnterior

```DAX
MedidasVentas[Ventas mes actual]-[VentaMesHace1Año]
```

#### Información actualizada al

```DAX
FORMAT(CALCULATE(MAX(Fact_Ventas[fecha]),ALL(DimCalendario)),"DD/MM/YYYY")
```

#### N° Pedidos

Formato: `#,0`

```DAX
DISTINCTCOUNT(Fact_Ventas[pedido])
```

#### Número de pedidos

Formato: `0`

```DAX
DISTINCTCOUNT(Fact_Ventas[pedido])
```

#### Prueba

Formato: `General Date`

```DAX
MAX(Fact_Ventas[fecha])
```

#### puntosventa

Formato: `0`

```DAX
DISTINCTCOUNT(Fact_CuotaVenta[bruto])
```

#### Rentabilidad

Formato: `#,0\ %;-#,0\ %;#,0\ %`

```DAX
DIVIDE(MedidasMargen[Margen],MedidasVentas[Venta SL],0)
```

#### SeleRepre

```DAX
SELECTEDVALUE(DimAgente[DesAgente])
```

#### Tex cum ven act Rep

```DAX
"Cumplimiento actual:"
```

#### Text budget

```DAX
"Cumplimiento actual:"
```

#### Ticket promedio

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

```DAX
DIVIDE(MedidasVentas[Venta SL],[Número de pedidos],0)
```

#### Ticket promedio mes anterior

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

#### Total budget

Formato: `"S/"\ #,0.0;#,0.0\ -"S/";"S/"\ #,0.0`

```DAX
SUM(BUDGET[Venta (PPTO)])
```

#### Total unidades vendidas

Formato: `#,0.00`

```DAX
SUM(Fact_Ventas[unidades])
```

#### Venta

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

```DAX
CALCULATE(SUM(Fact_Ventas[Venta neta]), USERELATIONSHIP(DimArticulo[kArticulo],Fact_CuotaVenta[kArticulo]))
```

#### Venta Cuota Sele Repre

Formato: `"S/"\ #,0;#,0\ -"S/";"S/"\ #,0`

Descripcion:  IF(ISBLANK([SeleRepre]),[Cuota Venta],[Cuota Venta SL]) 

```DAX

IF(ISBLANK([SeleRepre]),[Cuota Venta],[Cuota Venta SL])

```

#### Venta mes anterior T

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion: CALCULATE(     SUM(Fact_Ventas[Venta neta]),     DATEADD(DimCalendario[Date],-1,MONTH) )

```DAX
CALCULATE(
    SUM(Fact_Ventas[Venta neta]),
    DATEADD(DimCalendario[Date],-1,MONTH)
)
```

#### Venta SL

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

```DAX
IF(ISBLANK(SUM(Fact_Ventas[Venta neta])),BLANK(),SUM(Fact_Ventas[Venta neta]))
```

#### Venta Solo para segmento

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

```DAX
SUM(Fact_Ventas[Venta neta])
```

#### VentaMesAnterior

```DAX
"Venta Mes" & FORMAT([Ventas mes anterior],"#000")
```

#### VentaMesHace1Año

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

#### Ventas año actual

Descripcion:  VAR X = [Año Actual]    RETURN CALCULATE(MedidasVentas[Venta SL],DimCalendario[Año]=X)

```DAX

VAR X = [Año Actual]    RETURN
CALCULATE(MedidasVentas[Venta SL],DimCalendario[Año]=X)
```

#### Ventas año anterior

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

Descripcion:  VAR X = [Año Anterior] RETURN CALCULATE(MedidasVentas[Venta SL],DimCalendario[Año]=X)

```DAX

VAR X = [Año Anterior] RETURN
CALCULATE(MedidasVentas[Venta SL],DimCalendario[Año]=X)
```

#### Ventas mes actual

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

```DAX
CALCULATE  ( MedidasVentas[Venta SL] , DATESINPERIOD  (  DimCalendario[Date],  MAX  (  DimCalendario[Date] ) ,  -1 ,  MONTH )) 
```

#### Ventas mes anterior

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

#### Ventas mes anterior solo para segmento

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

#### Ventas mes anterioranterior

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

#### Ventas mes anteriorF

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

#### Ventas YTD CBH

Formato: `"S/"\ #,0;-"S/"\ #,0;"S/"\ #,0`

```DAX
CALCULATE(MedidasVentas[Venta SL],DATESYTD(DimCalendario[Date]))
```

#### Ventas YTD CBH -1

Formato: `"S/"\ #,0.00;-"S/"\ #,0.00;"S/"\ #,0.00`

Descripcion:   CALCULATE ( MedidasVentas[Venta SL],     DATESYTD ( DATEADD ( DimCalendario[Date], -1, YEAR ) ) )

```DAX


CALCULATE (
MedidasVentas[Venta SL],
    DATESYTD ( DATEADD ( DimCalendario[Date], -1, YEAR ) )
)
```

#### VentasTotalAñoActual

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

#### YoY% de Venta

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


