# Embudo de conversión y retención de usuarios — MercadoLibre

Análisis del recorrido de compra de MercadoLibre en 10 países de América Latina: dónde se pierden más usuarios dentro del embudo de conversión y qué tan bien retiene la plataforma a los usuarios nuevos a lo largo de sus primeras semanas.

**Herramienta:** Excel (tablas dinámicas y análisis de cohortes)
**Archivo:** [`mercadolibre-funnel-retention.xlsx`](./mercadolibre-funnel-retention.xlsx)

## Contexto de negocio

Dos preguntas de negocio distintas, en el mismo archivo:

1. **Embudo de conversión** (01/01/2025 – 31/08/2025): de todos los usuarios que ven un producto, ¿en qué paso se pierden más y cómo varía eso por país?
2. **Retención** (cohortes registradas entre 01/01/2025 – 01/06/2025): de los usuarios que se registran, ¿cuántos siguen activos en los días 7, 14, 21 y 28, y cómo varía eso por país y por mes de registro?

## Hallazgos clave

### 1. El embudo se rompe entre "ver el producto" y "agregarlo al carrito"

| Etapa | % de usuarios que la alcanza |
|---|---|
| Selecciona el producto | 76.90% |
| Agrega al carrito | 11.01% |
| Inicia el checkout | 4.00% |
| Agrega datos de envío | 2.42% |
| Agrega datos de pago | 2.09% |
| Compra | 1.25% |

La caída más grande de todo el embudo ocurre en un solo paso: de "seleccionar producto" a "agregar al carrito", se pierde **65.9 puntos porcentuales**. Ningún otro paso se acerca a esa magnitud. El producto capta la atención, pero eso no se traduce en intención de compra.

### 2. Ese mismo paso varía fuerte por país

- **Uruguay** retiene mejor en ese paso: solo pierde 59.1 puntos (81.8% → 22.7%).
- **Bolivia** es el más débil: pierde 71.0 puntos (80.6% → 9.7%).

Esto sugiere que la fricción no es puramente de producto — hay algo específico de la experiencia en Bolivia que Uruguay está resolviendo mejor.

### 3. La retención cae en picada después del día 14

Promedio a través de los 10 países:

| Día | Retención promedio |
|---|---|
| D7 | ~84% |
| D14 | ~51% |
| D21 | ~22% |
| D28 | ~2.3% |

- **Mejor retención a D28:** Perú (3.2%) y México (3.1%).
- **Peor retención a D28:** Colombia (1.6%) y Chile (1.7%).
- **Brasil** tiene la mejor retención inicial (D7: 87.2%), pero converge con el resto para D28 (2.5%).

### 4. Por cohorte mensual, marzo 2025 fue el mejor mes

La cohorte de marzo 2025 registró la mejor retención tanto en D7 (87.7%) como en D28 (3.0%, empatada con enero y mayo). Abril tuvo la segunda mejor retención en D7.

> **Nota sobre la cohorte de agosto:** el archivo incluye una cohorte de agosto 2025 con una caída aparente muy fuerte (D7: 70.8%, D28: 0.2%). No la interpreto como el peor mes real — es una cohorte censurada: los usuarios que se registraron en agosto no tuvieron tiempo de llegar al día 28 antes del corte de datos (31/08/2025), así que su retención tardía está artificialmente subestimada. La comparación válida entre meses es enero–julio.

## Recomendaciones

**Sobre el embudo:**
- Optimizar la página de producto (imágenes, descripciones, reseñas) para cerrar la brecha select→cart.
- Implementar pop-ups de abandono con descuentos del 5–10%.
- Replicar las prácticas de Uruguay en los países con peor conversión en ese paso.
- Notificaciones push recordando productos vistos.

**Sobre retención:**
- Programa de re-enganche entre los días 15–21 (emails personalizados con ofertas exclusivas), justo antes de que la curva se desplome.
- Gamificación (puntos o insignias) para sostener el engagement después del día 14.
- Replicar las estrategias de México y Perú en Colombia y Chile.
- Para Colombia y Chile específicamente: adaptar contenido, precios y métodos de pago, y explorar partnerships locales que mejoren la confianza y los tiempos de entrega.

## Cómo explorar este archivo

El Excel tiene 5 hojas:

1. **Informe Ejecutivo** — la síntesis: preguntas de negocio, hallazgo, implicación de negocio y reflexión personal.
2. **Embudo General** — la tasa de conversión global por etapa.
3. **Embudo General x País** — la misma tabla, desagregada por los 10 países.
4. **Retención x País** — retención D7/D14/D21/D28 por país.
5. **Retención x Cohorte** — retención D7/D14/D21/D28 por mes de registro.

No se necesita ninguna herramienta especial — basta con abrirlo en Excel, Google Sheets o LibreOffice Calc.

## Limitaciones

- El embudo cubre un periodo (ene–ago 2025) distinto al de las cohortes de retención (ene–jun 2025); no deben mezclarse en un mismo análisis temporal.
- La cohorte de agosto 2025 está censurada por la fecha de corte de los datos (ver nota arriba).
- El análisis es descriptivo: identifica dónde ocurre la pérdida, no explica causalmente por qué (eso requeriría datos cualitativos o un experimento controlado).
