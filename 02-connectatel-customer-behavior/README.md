# Comportamiento de clientes y segmentación — ConnectaTel

Análisis del comportamiento de clientes de ConnectaTel, una empresa de telecomunicaciones en América Latina, con datos registrados hasta 2024. El objetivo: entender cómo se agrupan los clientes por edad y por nivel de uso, detectar patrones atípicos, y traducir todo eso en recomendaciones concretas de producto.

**Herramientas:** Python (pandas, numpy, seaborn, matplotlib)
**Notebook:** [`connectatel-customer-analysis.ipynb`](./connectatel-customer-analysis.ipynb)

## Contexto de negocio

ConnectaTel opera con tres fuentes de datos que hay que combinar antes de poder analizar nada:

- **plans** — precio, minutos incluidos, GB incluidos y costo por extra de cada plan.
- **users** — edad, ciudad, fecha de registro, plan contratado y fecha de baja (si aplica).
- **usage** — el detalle real de consumo: llamadas y mensajes por usuario.

## Metodología

1. **Carga y exploración** de los tres datasets (~4,000 usuarios, ~40,000 registros de uso).
2. **Limpieza de calidad de datos** — ver hallazgos abajo, cada decisión de limpieza está justificada en el notebook.
3. **Construcción de un perfil de usuario** combinando consumo agregado (llamadas, mensajes, minutos) con los datos demográficos.
4. **Detección de outliers** con el método IQR, con una decisión explícita de qué hacer con cada uno.
5. **Segmentación** por nivel de uso (Bajo / Medio / Alto) y por grupo de edad (Joven / Adulto / Adulto Mayor).
6. **Traducción a recomendaciones de negocio.**

## Hallazgos clave

### Calidad de los datos

- `city`: 11.7% de valores nulos (algunos venían como `"?"`) — se dejaron como nulos por representar menos del umbral que justificaría eliminar la columna.
- `churn_date`: 88.35% de valores nulos — se eliminó la columna por su altísima proporción de vacíos, dado que ya se cuenta con la fecha de registro.
- `date` (en usage): apenas 0.125% de nulos sobre 40,000 registros — impacto mínimo.
- `duration` y `length` dependían del **tipo de registro**: los mensajes de texto no tienen duración de llamada, y las llamadas no tienen longitud de mensaje. Es decir, sus nulos son estructurales (MAR — *missing at random condicionado al tipo de registro*), no errores de captura.
- La columna de edad tenía valores centinela negativos, corregidos imputando con la mediana.

### Segmentación por edad

- El 50% de los usuarios (2,000 de 4,000) están en el rango adulto (30–60 años).
- Los adultos mayores (+60 años) son el segundo grupo más numeroso, con 1,250 usuarios (31.25%).

### Segmentación por uso

- La mayoría de los usuarios tiene un uso medio (entre 5 y 10 llamadas y mensajes al mes).
- Menos del 5% de los usuarios cae en la categoría de uso alto.
- Los valores atípicos de uso alto (usuarios con muchas más llamadas, mensajes o minutos que el resto) se conservaron deliberadamente en el análisis — no se trataron como errores, sino como comportamiento real que vale la pena entender antes de decidir qué hacer con él.

### El cruce que importa

Los adultos mayores con un patrón de uso medio-neutro son el segmento más numeroso y estable de la base de clientes — el perfil de cliente potencial más claro para ConnectaTel.

## Recomendaciones

- Aumentar la cantidad de llamadas incluidas y el precio del plan Premium, para capturar a los usuarios de uso alto que hoy ya exceden lo que su plan actual contempla.
- Crear un plan intermedio (400 llamadas, 300 mensajes) con un precio entre el plan Básico y el nuevo Premium, para no perder a los usuarios de uso medio que aún no justifican el salto al plan más caro.

## Cómo explorar este proyecto

El notebook se puede leer directamente en GitHub (renderiza el código, el texto y las gráficas sin necesidad de instalar nada). Si quieres ejecutarlo:

1. Clona el repositorio o descarga este notebook.
2. Necesitarás los tres archivos de datos (`plans.csv`, `users_latam.csv`, `usage.csv`) — no están incluidos en este repositorio porque forman parte del entorno del bootcamp; si te interesa reproducir el análisis con datos propios, la estructura de columnas está documentada en el notebook.
3. Abre el notebook en Jupyter Notebook, JupyterLab o Google Colab.
4. Instala las dependencias: `pip install pandas numpy seaborn matplotlib`.

## Limitaciones

- El análisis cubre solo hasta 2024; no hay datos posteriores para validar si las recomendaciones tuvieron efecto.
- La segmentación usa umbrales fijos (5 y 10 llamadas/mensajes) definidos para este ejercicio — en producción, valdría la pena validarlos contra la distribución real de churn por segmento.
