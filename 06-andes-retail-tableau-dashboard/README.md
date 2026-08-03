# Dashboard de desempeño comercial 2024–2025 — Andes Retail Group

Andes Retail Group cerró 2025 con una caída de ingresos del 6.8% frente a 2024. Este dashboard responde la pregunta que sigue: ¿la caída es generalizada, o está concentrada en algún país, segmento o temporada específica — y qué se hace al respecto?

**Herramienta:** Tableau (workbook autocontenido, sin código)

## Contexto de negocio

Andes Retail Group opera en tres países de América Latina (Colombia, Perú y Chile), distribuidos en tres regiones y cuatro categorías de producto, acumulando cerca de **USD 5.53M en ingresos combinados** entre 2024 y 2025 (5,000 pedidos). El dashboard se construyó para responder dos preguntas en dos vistas distintas:

- **Overview:** ¿cómo evolucionó el ingreso total entre 2024 y 2025?
- **Detalle:** ¿por qué subió, bajó o se mantuvo — y dónde está concentrado el problema?

## Cómo está construido

El workbook conecta directamente a la fuente `Andes_Retail_Group_2024_2025` (embebida en el `.twbx`, por lo que es autocontenido y reproducible) y se organiza en 12 hojas consolidadas en dos dashboards:

- **KPIs principales:** Ingresos, Ticket Promedio (`SUM(Ingresos)/COUNTD(ID_Pedido)`), Margen (`Ingresos − Costo`) y Margen %, con variaciones interanuales (`Var % Ingresos`, `Var % Margen`, `Var % Unidades`).
- **Overview:** tendencia mensual de ingresos, ingresos por país, por categoría y por segmento de cliente — la portada ejecutiva.
- **Detalle:** un mapa de calor región × categoría, una vista de estacionalidad (ordenada Primavera→Invierno) y una tabla a nivel pedido, con acciones de filtro entre hojas para explorar sin perder el hilo.

## Hallazgos clave (modelo SCQA)

### Vista Overview

**Situación:** Ingresos combinados de USD 5.53M entre 2024 y 2025 (5,000 pedidos).

**Complicación:** Los ingresos cayeron de USD 2.86M en 2024 a USD 2.67M en 2025 (**-6.8%**). Los pedidos bajaron -3.9% y el ticket promedio -3.0%, mientras el margen se mantuvo estable en ~35% — es decir, **no es un problema de rentabilidad, sino de volumen de negocio.**

**Pregunta:** ¿Esta caída es generalizada, o está concentrada en algo específico?

**Respuesta:** No es generalizada. La categoría Hogar (+3.3%), la región Norte (+0.6%) y el segmento Económico (+5.9%) en realidad **crecieron**. La contracción se concentra en el segmento Premium (**-14.0%**) y en la temporada de Verano (**-16.6%**) — los dos motores que más pesan sobre el ingreso total.

### Vista Detalle

**Situación:** El segmento Premium representa cerca del 49% del ingreso 2024, y Verano es históricamente la temporada de mayor facturación.

**Complicación:** Al cruzar segmento y estación, **Premium + Verano cae -27.7%**, muy por encima del promedio general. La región Sur es la más afectada (-12.1%), impulsada por Electrónica en Sur (-21.7%); a nivel país, Perú retrocede más (-10.4%) que Chile o Colombia.

**Pregunta:** ¿La caída es de tráfico de pedidos o de precio, y qué se prioriza?

**Respuesta:** Las unidades vendidas bajaron -4.25% y los pedidos -3.9%, mientras el precio promedio se mantuvo casi plano. **Es un problema de volumen, no de precio** — no se resuelve solo con descuentos.

## Recomendaciones

- Campaña de retención dirigida al cliente Premium antes de la temporada de Verano 2026.
- Revisión de la propuesta de valor de Electrónica en la región Sur.
- Doblar la apuesta en Hogar y en el segmento Económico, que sí están creciendo.

## Mensaje ejecutivo (formato Slack)

> **📊 Actualización desempeño comercial**
>
> Equipo, cerramos 2025 con ingresos de USD 2.67M, -6.8% vs 2024. La buena noticia: no es una caída generalizada — Hogar, la región Norte y el segmento Económico crecieron. El problema está concentrado: el segmento Premium cayó -14%, y la combinación Premium + Verano se desplomó -27.7%, arrastrando gran parte del resultado global. Electrónica en la región Sur también retrocede fuerte (-21.7%).
>
> Es una caída de volumen (menos pedidos, no precios más bajos), así que no se resuelve solo con descuentos. Propongo priorizar para el próximo trimestre:
> - Campaña de retención para clientes Premium antes de la temporada de Verano 2026.
> - Revisión de la oferta de Electrónica en la región Sur.
> - Doblar la apuesta en Hogar y en el segmento Económico, que sí están creciendo.


## Limitaciones

- El análisis identifica dónde se concentra la caída, no explica causalmente por qué el segmento Premium perdió tracción en Verano — eso requeriría datos cualitativos (encuestas, entrevistas a clientes) que no forman parte de este dataset.
- Los cálculos de variación interanual comparan solo dos años (2024 vs 2025); no hay histórico suficiente para distinguir una tendencia estructural de una fluctuación puntual.
