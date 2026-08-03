# De datos a decisiones de negocio — RappiPlus

Diagnóstico integral del negocio de RappiPlus en tres países (Argentina, Colombia y México): rentabilidad, comportamiento de compra, embudo de conversión, retención de usuarios y un experimento A/B sobre el checkout. Es el proyecto más completo de este portafolio — combina limpieza de datos en Python, consultas SQL contra una base de datos en vivo, una prueba estadística formal y un dashboard en Tableau.

**Herramientas:** Python (pandas, numpy), SQL (PostgreSQL vía SQLAlchemy), scipy/statsmodels, Tableau
**Notebook:** [`rappiplus-business-diagnosis.ipynb`](./rappiplus-business-diagnosis.ipynb)
**Dashboard en vivo:** [public.tableau.com/app/profile/alan.raygoza](https://public.tableau.com/app/profile/alan.raygoza)

## Contexto de negocio

Tres fuentes de datos (enero–junio 2025), más dos tablas en base de datos:

- **rappiplus_orders_raw.csv** — pedidos, precios, descuentos y revenue.
- **rappiplus_catalog.csv** — costo, categoría y proveedor de los 7 productos del catálogo.
- **rappiplus_marketing_spend.csv** — inversión en marketing por día, país y canal.
- **Base de datos PostgreSQL** — tablas `events` (eventos de navegación por usuario) y `users` / `user_activity` (registro y actividad), usadas para el embudo y la retención.
- **experiment_checkout_ui.csv** — resultados de un experimento A/B sobre un rediseño del checkout.

## Metodología y hallazgos, paso a paso

### 1. Calidad de datos

De 25,100 filas originales de pedidos, se detectaron y resolvieron varios problemas antes de calcular una sola métrica:

- **100 filas duplicadas** eliminadas.
- **10 filas con cantidades absurdas** (hasta 20,000 unidades en un pedido) — por sí solas inflaban el revenue en **$42.3 millones ficticios**. Se identificaron y removieron antes de que contaminaran cualquier KPI.
- 4 filas con cantidad negativa y 50 con cantidad nula o cero, también removidas.
- Inconsistencias de formato (`"mexico"` vs `"Mexico"`, `"Electrónica"` vs `"Electronica"`) unificadas.
- Categorías nulas recuperadas cruzando el catálogo por nombre de producto; lo que no se pudo recuperar se marcó explícitamente como `"Desconocido"` en vez de descartarse en silencio.
- Verificación de que `monto_total = cantidad × precio − descuento` en el 100% de las filas restantes.
- Resultado: **24,936 pedidos limpios** (99.3% de los datos originales conservados).

### 2. ¿El negocio es rentable?

| Métrica | Valor |
|---|---|
| Ingreso total (revenue) | $9,622,281.56 |
| Costo de producto | $3,828,869.01 |
| Inversión en marketing | $2,694,664.43 |
| **Profit** | **$3,098,748.12** |
| Margen neto | 32.2% |
| ROAS global | 3.57x |
| Ticket promedio | $385.88 |
| Pedidos / usuarios | 24,936 / 7,640 (3.26 pedidos por usuario) |

**Sí, el negocio es rentable** — pero esa cifra agregada esconde un problema puntual:

**Laptop-Gaming-16GB es el único producto del catálogo que pierde dinero:** costo unitario de $280.68 frente a un precio de venta promedio de $261.27 — se vende por debajo de su propio costo. Pérdida acumulada: **-$93,621.66** (margen de -8.6% sobre su propio revenue). Los otros 6 productos del catálogo tienen márgenes saludables, entre 26.3% y 96.0%.

Este problema **no se ve si solo miras la categoría**: Electrónica en su conjunto tiene un margen de 58.7%, porque el Tablet y el Phone de la misma categoría son altamente rentables (90.2% y 96.0%) y compensan contablemente la pérdida de la Laptop. Hay que bajar a nivel de producto individual para encontrarlo.

**Por país,** el negocio está sorprendentemente equilibrado: Colombia, México y Argentina generan cada uno alrededor de $1M de profit, con márgenes entre 31.4% y 32.2% — no hay un país que esté subsidiando a otro.

**Por canal de adquisición,** paid_search es el más eficiente: profit de $1.1M, margen de 34.4%, el mejor ROAS (3.72x) y el CAC más bajo ($104.53 por pedido) de los tres canales.

**Por mes,** enero 2025 tuvo el mejor margen (40.6%) — no por vender más, sino por haber invertido menos en marketing ese mes ($323K, frente a $445K–516K el resto de los meses).

### 3. ¿Dónde se pierden los usuarios? (funnel vía SQL)

Consulta directa a la tabla `events` con SQL (`LAG()` para calcular la caída paso a paso):

| Paso | Usuarios únicos | % que avanza desde el paso anterior |
|---|---|---|
| Primera visita | 7,796 | — |
| Selecciona producto | 7,582 | 97.3% |
| Agrega al carrito | 7,634 | 100.7% |
| Inicia checkout | 7,208 | 94.4% |
| Agrega método de pago | 6,250 | **86.7%** |
| Compra | 6,240 | 99.8% |

**Conversión total (primera visita → compra): 80.04%** — inusualmente alta comparada con un embudo de e-commerce típico, lo que indica que estos son usuarios con intención de compra ya establecida, no tráfico frío. El verdadero cuello de botella no está al principio del embudo (como suele pasar en otros negocios), sino en un solo paso: **agregar el método de pago**, donde se pierden 958 usuarios — la única caída que realmente importa en este funnel.

### 4. ¿Los usuarios regresan? (retención por cohortes, vía SQL)

Cinco cohortes mensuales (enero–mayo 2025), midiendo qué % de cada cohorte sigue activo en las semanas 1, 2 y 3 después de registrarse:

| Cohorte | Semana 1 | Semana 2 | Semana 3 |
|---|---|---|---|
| Promedio de las 5 cohortes | 42.01% | 41.96% | 41.92% |

La retención es notablemente **estable**: apenas cae 0.22% de la semana 1 a la semana 3, en vez del desplome pronunciado típico de otros negocios de e-commerce. La cohorte de enero 2025 es la mejor (42.84% en semana 1); la de mayo, la más baja (41.20%) — pero la diferencia entre la mejor y la peor cohorte es mínima, lo que sugiere una base de usuarios homogénea más que un problema de retención.

### 5. ¿El nuevo checkout mejora la conversión? (prueba de hipótesis)

- **H₀:** el nuevo diseño no impacta la tasa de conversión. **H₁:** sí la impacta.
- **Test:** z-test de proporciones, α = 0.05.

| Grupo | Conversiones | Usuarios | Tasa |
|---|---|---|---|
| Control | 779 | 4,965 | 15.69% |
| Tratamiento | 820 | 5,035 | 16.29% |

Diferencia observada: +0.60 puntos porcentuales (+3.82% relativo). **Z = 0.8133, p = 0.4161** — muy por encima de 0.05. **No se rechaza H₀.**

La lectura correcta de este resultado (tal como quedó documentada en el propio análisis): no rechazar H₀ no prueba que el nuevo checkout no sirva — significa que este experimento, con este tamaño de muestra, no logró detectar un efecto que sea distinguible del azar. La diferencia observada podría ser real y simplemente necesitar más datos para confirmarse, o podría no existir.

### 6. Dashboard

Los datasets limpios (`orders_clean.csv`, `catalog_clean.csv`, `marketing_clean.csv`) más los resultados de embudo, cohortes y el experimento A/B alimentan el dashboard publicado en Tableau — el mismo que se puede ver en el enlace de arriba.

## Cómo explorar este proyecto

El notebook se puede leer directamente en GitHub. Para ejecutarlo:

1. Clona o descarga este notebook.
2. Los tres CSV (`rappiplus_orders_raw.csv`, `rappiplus_catalog.csv`, `rappiplus_marketing_spend.csv`) se cargan automáticamente desde una URL pública dentro del propio notebook — no necesitas descargarlos aparte.
3. La conexión SQL requiere tus propias credenciales de base de datos: reemplaza el valor de ejemplo en la celda de conexión por las tuyas (nunca subas una contraseña real a un repositorio público — usa variables de entorno).
4. Instala dependencias: `pip install pandas numpy scipy statsmodels sqlalchemy psycopg2-binary`.

## Limitaciones

- El funnel y la retención dependen de una conexión a base de datos externa que no es pública; los números reportados aquí son el resultado ya calculado, documentado en el propio notebook.
- El experimento A/B no encontró un efecto estadísticamente significativo — no se debe interpretar como evidencia de que el nuevo checkout "no funciona", solo que este experimento no lo pudo confirmar.
- El análisis de rentabilidad por producto usa profit **bruto** (sin marketing) porque el gasto de marketing no está desglosado por SKU en los datos — solo por fecha, país y canal. El profit **neto** (con marketing) sí se calculó a nivel de categoría, país y canal.
