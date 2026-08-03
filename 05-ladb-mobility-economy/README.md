# Movilidad urbana y productividad económica en América Latina

¿En qué ciudades latinoamericanas conviene invertir en infraestructura de transporte? Este proyecto cruza datos reales de tráfico (TomTom Traffic Index) con datos económicos (OECD Cities) para 15 ciudades de 8 países, y prioriza dónde una inversión en movilidad tendría más impacto.

**Herramientas:** Python (pandas, numpy, seaborn, matplotlib)
**Notebook:** [`ladb-mobility-economy.ipynb`](./ladb-mobility-economy.ipynb)

## Datos

Dos fuentes que había que limpiar y combinar antes de poder analizar nada:

- **TomTom Traffic Index** — minutos de congestión, índice de tráfico, kilómetros de embotellamiento, tiempos de viaje por ciudad.
- **OECD Cities** — PIB per cápita, tasa de desempleo, PM2.5, población por ciudad.

Ambos datasets llegaron con nombres de columnas inconsistentes, tipos de dato incorrectos (números como texto, con separadores europeos) y sin una columna de año explícita — todo eso se corrigió antes del análisis.

## Metodología

1. Estandarización de nombres de columna a `snake_case` en ambos datasets.
2. Corrección de tipos de dato y limpieza de valores numéricos que venían como texto.
3. Extracción del año a partir de las fechas, para poder filtrar y agrupar por periodo.
4. Agregación del tráfico por ciudad, país y año (promedios de las métricas clave).
5. Unión de tráfico y economía con un `inner join` sobre ciudad y año — se descartó cualquier ciudad sin datos completos en ambas fuentes para no distorsionar el análisis de correlación. Esto redujo la tabla original de 1,004,464 registros de tráfico a 387 combinaciones ciudad-año únicas, y finalmente a las 15 ciudades con datos completos en ambas fuentes para 2024.
6. Visualización de relaciones (boxplot, histograma, gráfico de barras comparativo).

## Hallazgos clave

- **CDMX es la ciudad con el mayor tiempo promedio de congestión** de las 15 analizadas.
- **No existe una correlación lineal simple entre PIB per cápita y congestión.** Montevideo, por ejemplo, tiene el PIB per cápita más alto de la muestra pero eso no se traduce en menor (ni mayor) movilidad relativa al resto — es una excepción a la tendencia que sí se observa en la mayoría de las otras ciudades, donde SÍ existe una asociación positiva entre actividad económica y congestión.
- **Priorización de inversión** — combinando alta congestión con PIB per cápita moderado (las ciudades donde la congestión más limita la productividad, sin que ya exista la riqueza para haberla resuelto por su cuenta):

| Ciudad | Congestión promedio | PIB per cápita |
|---|---|---|
| Bogotá | 1,141 min | $11,442 |
| Lima | 1,052 min | $13,472 |
| Buenos Aires | 571 min | $18,117 |

- **Montevideo** es el caso opuesto y vale la pena estudiarlo como referencia: PIB per cápita más alto de la muestra ($26,176) con apenas 50.2 minutos de congestión — la mejor relación PIB/congestión de las 15 ciudades.

## Recomendaciones

- **Bogotá y Lima:** invertir con urgencia en sistemas de metro y transporte masivo — son las ciudades donde la congestión limita más la productividad y donde el retorno de una inversión en infraestructura sería mayor.
- **Buenos Aires:** priorizar la optimización de rutas existentes y tecnología inteligente de tráfico, antes que obra nueva.
- **Montevideo:** documentar y estudiar sus políticas de movilidad como posible modelo a replicar en otras ciudades de la región.

## Cómo explorar este proyecto

El notebook se puede leer directamente en GitHub. Para ejecutarlo:

1. Clona o descarga este notebook.
2. Los archivos de datos (`tomtom_traffic.csv`, `oecd_city_economy.csv`) no están incluidos — forman parte del entorno del bootcamp.
3. Instala dependencias: `pip install pandas numpy seaborn matplotlib`.

## Limitaciones

- El análisis es correlacional, no causal — una alta congestión coincidiendo con PIB moderado no prueba que una cause a la otra.
- La muestra final se limita a las 15 ciudades con datos completos en ambas fuentes para 2024; ciudades con datos incompletos quedaron fuera del análisis.
- Faltan factores que probablemente importan tanto o más que el PIB: infraestructura urbana existente, políticas públicas de transporte y densidad poblacional.
