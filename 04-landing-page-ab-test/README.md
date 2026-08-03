# Experimento A/B en página de inicio

Evaluación estadística de un experimento A/B entre dos versiones de una landing page (A y B), con el objetivo de decidir con evidencia —no con intuición— cuál versión debe convertirse en la página estándar.

**Herramientas:** Python (pandas, seaborn, matplotlib, scipy.stats, statsmodels)
**Notebook:** [`landing-page-ab-test.ipynb`](./landing-page-ab-test.ipynb)

## Contexto de negocio

El dataset `landing_experiment.csv` registra usuarios expuestos a la versión A o B de la página, con su región, dispositivo, fuente de tráfico, tipo de usuario (nuevo/recurrente), si convirtieron y cuánto gastaron. El análisis responde tres preguntas en orden:

1. ¿La página B genera más gasto promedio por usuario que la A?
2. ¿La página B convierte a una tasa más alta que la A?
3. ¿El canal de tráfico o el tipo de usuario influyen en la conversión?

## Metodología

Cada pregunta se respondió con la prueba estadística correcta para el tipo de dato y la hipótesis en juego:

- **Prueba de Levene** para verificar si las varianzas de gasto eran iguales entre A y B (no lo eran), lo que definió usar un **t-test de Student con `equal_var=False`** para comparar el gasto promedio.
- **Z-test de proporciones** para comparar tasas de conversión entre A y B — la prueba correcta para comparar tasas, no medias.
- **Chi-cuadrado de independencia** para evaluar si el canal de tráfico y el tipo de usuario influyen en la probabilidad de conversión.

## Hallazgos clave

### La página B gana en las dos métricas que importan

| Métrica | Página A | Página B | Diferencia |
|---|---|---|---|
| Gasto promedio por usuario convertido | 61.09 | 68.75 | +12.5% |
| Tasa de conversión | 12.57% | 15.96% | +27% relativo (+3.39 pp) |

- **Gasto:** Levene p = 6.88 × 10⁻⁸ (varianzas distintas, confirma usar `equal_var=False`); t-test p = 3.63 × 10⁻²¹. La diferencia es estadísticamente contundente, no es azar.
- **Conversión:** z-test de proporciones, p = 3.76 × 10⁻²². Sobre los 20,018 usuarios expuestos a la versión B, esto se tradujo en **682 conversiones adicionales** frente a lo que hubiera generado la versión A.
- **Efecto combinado:** mayor conversión × mayor gasto por conversión se traduce en un incremento estimado de **+43% en ingresos totales**.

**Recomendación:** implementar la página B como landing page única y definitiva. No hay incertidumbre estadística relevante en ninguna de las dos métricas.

### Los canales de tráfico rinden distinto

| Canal | Tasa de conversión | % del tráfico |
|---|---|---|
| Email | 15.0% | ~15% |
| Ads | 14.7% | — |
| Referral | 13.9% | — |
| Organic | 13.8% | ~45% |

Chi-cuadrado, p = 0.034 — el canal sí influye en la conversión.

- **Email es el canal más eficiente pero está infraexplotado** — apenas representa el 15% del tráfico total.
- **Organic es el canal más grande (45% del tráfico) pero el de menor conversión** — sugiere un desajuste entre lo que buscan los usuarios que llegan por SEO y lo que la página les ofrece.
- **Ads** se mantiene como canal sólido, con buen balance entre volumen y conversión.

**Recomendación:** aumentar presupuesto en Email, auditar la estrategia orgánica/SEO, y mantener Ads con optimización continua.

### El tipo de usuario no importa

Usuarios nuevos (14.36%) y recurrentes (14.09%) convierten prácticamente igual — chi-cuadrado, p = 0.47, sin diferencia estadísticamente significativa.

**Recomendación:** no vale la pena segmentar campañas por tipo de usuario; ese esfuerzo se puede redirigir a los canales y a la optimización de la página, donde sí hay señal.

## En una frase

Implementar la página B, redirigir inversión hacia Email y auditar el tráfico orgánico produce el mayor retorno con la menor complejidad operativa.

## Cómo explorar este proyecto

El notebook se puede leer directamente en GitHub. Para ejecutarlo:

1. Clona o descarga este notebook.
2. El archivo `landing_experiment.csv` no está incluido — forma parte del entorno del bootcamp.
3. Instala dependencias: `pip install pandas seaborn matplotlib scipy statsmodels`.

## Limitaciones

- El +43% de ingresos es una estimación que combina dos efectos (conversión y gasto); no es una medición directa de ingresos incrementales en producción.
- El análisis no controla por posibles factores de confusión entre canal de tráfico y versión de página (por ejemplo, si un canal estuvo desproporcionadamente más expuesto a B que a A).
- Es una fotografía del periodo del experimento — no evalúa si el efecto se sostiene en el tiempo.
