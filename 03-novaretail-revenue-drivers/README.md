# Factores de comportamiento asociados al ingreso anual — NovaRetail+

NovaRetail+ es una plataforma de e-commerce en América Latina. El equipo de Crecimiento y Retención necesitaba responder una pregunta concreta al cierre de 2024: **¿qué factores del comportamiento del cliente están más fuertemente asociados con el ingreso anual que genera?**

Este es un análisis **correlacional y exploratorio** — el objetivo es identificar asociaciones, no probar causalidad, y esa distinción se mantiene en cada hallazgo de este proyecto.

**Herramientas:** Python (pandas, numpy, seaborn, matplotlib, scipy.stats)
**Notebook:** [`novaretail-revenue-drivers.ipynb`](./novaretail-revenue-drivers.ipynb)

## Datos

El dataset contiene una fila por cliente, con las siguientes columnas:

| Columna | Descripción |
|---|---|
| `id_cliente` | Identificador único del cliente |
| `edad` | Edad del cliente |
| `nivel_ingreso` | Ingreso anual estimado del cliente |
| `visitas_mes` | Visitas mensuales a la app/sitio |
| `compras_mes` | Compras realizadas en el mes |
| `gasto_publicidad_dirigida` | Gasto en anuncios asignado al usuario |
| `satisfaccion` | Calificación de satisfacción (1–5) |
| `miembro_premium` | Si tiene suscripción premium (1/0) |
| `abandono` | Si abandonó la plataforma (1/0) |
| `tipo_dispositivo` | Móvil, escritorio o tablet |
| `region` | Norte, sur, oeste o este |
| `ingreso_anual` | **Variable objetivo** — ingreso anual generado por el cliente |

## Metodología

Cada relación se evaluó con el método estadístico correcto para el tipo de variables involucradas, no con un solo método aplicado a todo:

- **Pearson** para relaciones numéricas lineales.
- **Punto-biserial** para relacionar una variable binaria con una continua.
- **Spearman** cuando la relación no es necesariamente lineal.
- **V de Cramér** para dos variables categóricas.

## Hallazgos clave

| # | Relación | Método | Resultado |
|---|---|---|---|
| 1 | `compras_mes` vs `ingreso_anual` | Pearson | Asociación positiva: a más compras mensuales, mayor ingreso anual |
| 2 | `miembro_premium` vs `ingreso_anual` | Punto-biserial | Asociación baja — la membresía premium **no** está relacionada con el ingreso generado |
| 3 | `gasto_publicidad_dirigida` vs `visitas_mes` | Spearman | Coeficiente de 0.56 — asociación moderada positiva |
| 4 | `tipo_dispositivo` vs `region` | V de Cramér | Coeficiente de 0.012 — sin asociación relevante |

**Lectura de negocio:**

- El volumen de compras mensuales es el predictor más claro de valor de cliente identificado en este análisis — vale la pena explorar programas de fidelidad o campañas de captación enfocadas en clientes con compras altas.
- Que la membresía premium no se asocie con mayor ingreso es un hallazgo importante en sí mismo: sugiere que el valor de un cliente no depende de si paga por premium, sino de su comportamiento de compra real.
- La relación moderada entre gasto publicitario y visitas es prometedora pero no concluyente — hace falta profundizar en qué está impulsando qué antes de escalar inversión en base a esto.
- El tipo de dispositivo y la región del cliente parecen ser dimensiones independientes entre sí — no hay evidencia de que el comportamiento por dispositivo varíe sistemáticamente por región.

## Próximos pasos

El propio análisis identificó su siguiente iteración:

- Correlacionar (Spearman) `abandono`, `satisfacción` y `visitas_mes` contra `ingreso_anual`, que quedaron fuera de esta primera pasada.
- Profundizar en el ~25% de datos faltantes en `ingreso_anual` antes de confiar plenamente en las conclusiones.
- Validar las cuatro relaciones de esta tabla con una muestra más completa.

## Cómo explorar este proyecto

El notebook se puede leer directamente en GitHub. Para ejecutarlo:

1. Clona o descarga este notebook.
2. El archivo de datos (`novaretail_comportamiento_clientes_2024.csv`) no está incluido — forma parte del entorno del bootcamp. La estructura de columnas está documentada arriba si quieres reproducir el análisis con datos propios.
3. Instala dependencias: `pip install pandas numpy seaborn matplotlib scipy`.

## Limitaciones

- Correlación no implica causalidad — ninguno de estos hallazgos debe leerse como "X provoca Y".
- Aproximadamente 25% de los datos de `ingreso_anual` (la variable más importante del análisis) están ausentes.
- No hay datos sobre factores externos: competencia, campañas específicas de temporada, estacionalidad.
- El dataset solo cubre el cierre de 2024 — es una fotografía, no una serie temporal.
