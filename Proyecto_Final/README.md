# Proyecto Final — Econometría Financiera

**Autora:** Natali Maya Landa  
**Programa:** Maestría en Finanzas (MEF), EGADE Business School, Tecnológico de Monterrey  
**Materia:** Econometría Financiera (FZ4038)

## Descripción

El proyecto aplica herramientas de econometría financiera a un portafolio de diez acciones estadounidenses. Busca caracterizar su relación con el mercado y variables macrofinancieras, explorar relaciones en datos trimestrales, evaluar pronósticos intradía y estimar volatilidades para proponer una asignación de portafolio por paridad de riesgo.

El análisis está desarrollado en `Proyecto_Final_Econometria_Natali_Maya_1.Rmd`. El documento puede generar salidas HTML y Word. La carpeta también contiene las salidas renderizadas y los datos intradía serializados en RDS.

## Acciones analizadas

| Ticker | Empresa |
|---|---|
| NVDA | NVIDIA |
| MSFT | Microsoft |
| AAPL | Apple |
| AVGO | Broadcom |
| AMD | Advanced Micro Devices |
| JPM | JPMorgan Chase |
| WMT | Walmart |
| XOM | Exxon Mobil |
| V | Visa |
| JNJ | Johnson & Johnson |

Se usan además el S&P 500 (`^GSPC`), VIX (`^VIX`), Treasury a 10 años (`^TNX`) e índice dólar (`DX-Y.NYB`) como referencias de mercado y macrofinancieras.

## Estructura y modelos

1. **Regresión lineal simple y múltiple (datos diarios).** Descarga precios ajustados y variables de mercado de Yahoo Finance; calcula rendimientos logarítmicos/cambios; presenta descriptivos y correlaciones. Estima modelos de mercado por acción y modelos múltiples con S&P 500, cambios del VIX, TNX y DXY. Incluye pruebas de hipótesis, comparación de modelos anidados y diagnósticos (VIF, Breusch–Pagan, Durbin–Watson y errores robustos HC1), con análisis más detallado de NVDA, JNJ y WMT.
2. **Panel trimestral.** Agrega rendimientos, volatilidad y volumen a trimestres calendario y construye un panel de diez acciones. Estima modelos pooled, de efectos fijos y aleatorios; compara especificaciones con pruebas F, LM de Breusch–Pagan y Hausman, y calcula errores robustos agrupados por acción.
3. **Series intradía y pronósticos.** Analiza barras de una hora y cinco minutos mediante pruebas ADF/KPSS, selección automática de modelos ARIMA, prueba Ljung–Box y validación del 6 de noviembre de 2026 frente a un pronóstico ingenuo. También genera pronósticos para el siguiente día de operación y sus intervalos.
4. **Volatilidad y portafolio.** Ajusta modelos GARCH(1,1) a rendimientos diarios y utiliza los pronósticos de volatilidad en la estrategia de asignación por paridad de riesgo. El código contempla parámetros operativos del simulador, como valor del portafolio, porcentaje invertido, peso máximo y comisión.

## Paquetes de R

El Rmd instala automáticamente, si faltan, estos paquetes principales:

- `quantmod`, `lmtest`, `car`, `sandwich`, `knitr`
- `plm`
- `httr`, `jsonlite`, `tseries`, `forecast`
- `rugarch`

Para renderizar desde R se requiere también `rmarkdown` (y sus dependencias). Instale los paquetes antes de comenzar, por ejemplo:

```r
install.packages(c(
  "rmarkdown", "quantmod", "lmtest", "car", "sandwich", "knitr",
  "plm", "httr", "jsonlite", "tseries", "forecast", "rugarch"
))
```

Se recomienda utilizar una versión reciente de R y registrar las versiones efectivamente usadas (por ejemplo, con `sessionInfo()`), ya que el repositorio no fija un entorno mediante `renv.lock`.

## Cómo reproducir el análisis

1. Clone o descargue el repositorio y abra R/RStudio.
2. Establezca `Proyecto_Final` como directorio de trabajo, o abra el Rmd desde esa carpeta. Esto es importante porque el código busca los RDS mediante nombres relativos.
3. Confirme que estos dos archivos están junto al Rmd:
   - `intradia_1h.rds`: datos intradía de una hora para las diez acciones.
   - `intradia_5m.rds`: datos intradía de cinco minutos para las diez acciones.
4. Abra `Proyecto_Final_Econometria_Natali_Maya_1.Rmd` y use **Knit** para generar HTML o Word; alternativamente:

```r
rmarkdown::render("Proyecto_Final_Econometria_Natali_Maya_1.Rmd", output_format = "html_document")
# Para Word:
rmarkdown::render("Proyecto_Final_Econometria_Natali_Maya_1.Rmd", output_format = "word_document")
```

Los archivos intradía se leen con `readRDS()` por medio de `intradia_1h.rds` e `intradia_5m.rds`; si existen, el código no vuelve a descargarlos. Para conservar la muestra y los resultados asociados a la validación, mantenga los archivos originales. Si decide regenerarlos, el código consulta el servicio de gráficas de Yahoo Finance y guarda los resultados mediante `saveRDS()`; la disponibilidad histórica intradía depende de las ventanas que Yahoo mantenga. No elimine ni renombre esos archivos salvo que quiera forzar una nueva descarga.

Los demás datos diarios y trimestrales se descargan al ejecutar el documento desde Yahoo Finance. Por ello, la reproducción requiere conexión a internet y que Yahoo y los paquetes estén disponibles. La fecha de corte está especificada en el código; la fecha que aparece en la portada, en cambio, se genera con `Sys.Date()`.

## Datos y limitaciones de reproducción

- La fuente de datos financieros es Yahoo Finance, consultada desde `quantmod` para datos diarios y mediante el endpoint de gráficas de Yahoo para datos intradía.
- Yahoo puede modificar, corregir o dejar de servir datos históricos; también puede limitar solicitudes. Por ello, una descarga futura no garantiza una reproducción idéntica.
- Las barras intradía incluidas en los RDS son la referencia local para replicar esa parte sin depender de una descarga nueva.
- El análisis usa fechas de corte fijas para los datos, pero conviene verificar las fechas y observaciones realmente devueltas por Yahoo antes de interpretar resultados.

## Archivos

- `Proyecto_Final_Econometria_Natali_Maya_1.Rmd`: código fuente y explicación del análisis.
- `intradia_1h.rds`, `intradia_5m.rds`: muestras intradía usadas por el Rmd.
- `Proyecto_Final_Econometria_Natali_Maya_1.html`, `Proyecto_Final_Econometria_Natali_Maya_1.docx`: documentos renderizados disponibles en el repositorio.
