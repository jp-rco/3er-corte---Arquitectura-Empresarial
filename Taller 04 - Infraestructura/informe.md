# Informe Técnico del Taller

## Nombre del Taller
Taller 4 - Mapa de Infraestructura y Diagnóstico Técnico

## Integrantes del equipo
- Juan Pablo Restrepo Coca
- Andrés Mauricio Ricaurte
- Juan Felipe Jaime

## Descripción general del trabajo
El propósito de este taller fue construir un mapa de infraestructura tecnológica para un sistema de información y, a partir de ese modelo, desarrollar un diagnóstico técnico que permitiera identificar debilidades, cuellos de botella y oportunidades de mejora.

Como punto de partida, en clase se trabajó el caso base RedExpress, una plataforma de logística con operación híbrida, procesamiento regional y dependencia de servicios en nube. Posteriormente, el equipo adaptó el ejercicio al cliente real asignado, KOOLT, con el fin de representar de forma más precisa la infraestructura que soporta sus pedidos, pagos, preparación y operación en tienda.

El trabajo final buscó no solo dibujar componentes, sino también entender cómo interactúan entre sí, cuáles son sus dependencias técnicas y qué riesgos existen si alguno falla o se degrada.

## Proceso de desarrollo
El desarrollo del taller se hizo en dos momentos. Primero se elaboró un mapa preliminar del caso base, organizando la arquitectura por capas: acceso, servicios, datos y monitoreo. Esa primera aproximación permitió identificar desde temprano que la disponibilidad, la observabilidad y la distribución de carga eran aspectos decisivos para la estabilidad del sistema.

Después de esa fase inicial, se adaptó el ejercicio al dominio real del cliente. En lugar de trabajar con una plataforma de rastreo logístico, se modeló la infraestructura de KOOLT como una operación híbrida entre punto físico y servicios digitales. El mapa final se estructuró alrededor de los componentes que soportan la venta y preparación del producto: kiosco o tablet de pedidos, POS, pantalla de cocina, pasarela de pagos, inventario, base de datos y monitoreo.

Durante el modelado se decidió representar una arquitectura lógica y no una física de bajo nivel, ya que el objetivo del taller era visualizar dependencias funcionales, componentes críticos y riesgos operativos. Aun así, se incluyeron elementos de red, conectividad y soporte que permitieran justificar el diagnóstico técnico.

## Análisis del modelo propuesto
El modelo final de KOOLT representa una infraestructura relativamente compacta, pero con alta sensibilidad operativa. Aunque el número de componentes no es tan grande como en una plataforma logística, la continuidad del negocio depende de la coordinación correcta entre dispositivos en tienda, backend, pagos, inventario y cocina.

La arquitectura propuesta se organiza en cinco capas:

1. **Usuarios y operación**, donde interactúan cliente, cajero y personal de preparación.
2. **Dispositivos de tienda**, como kiosco, POS y KDS, que son el punto visible de la operación.
3. **Conectividad y acceso**, que concentra red local, internet y salida hacia servicios externos.
4. **Servicios de aplicación**, donde se procesa la lógica de pedidos, pagos, inventario y notificaciones.
5. **Datos y observabilidad**, que soportan persistencia, auditoría, monitoreo y reacción ante incidentes.

Este enfoque permite ver con claridad que KOOLT no depende únicamente del software de ventas, sino de una cadena completa de servicios. Si el POS falla, la operación se afecta de inmediato. Si la conexión a internet se interrumpe, los pagos electrónicos y la sincronización con inventario pueden degradarse. Si la base de datos se congestiona, la experiencia del cliente y los tiempos de preparación se ven afectados.

## Diagnóstico técnico

### 1. Dependencia del POS como componente crítico
El POS concentra varias responsabilidades: captura del pedido, validación, cálculo del total, integración con pagos, registro de transacciones y emisión de órdenes. Esta concentración lo convierte en un punto crítico. Si el POS se degrada o deja de responder, la operación de venta prácticamente se detiene.

**Impacto:** alto.  
**Riesgo:** cuello de botella funcional y operativo.  
**Oportunidad de mejora:** separar responsabilidades entre interfaz, lógica de negocio y procesamiento de pagos; mantener mecanismos de contingencia para venta básica.

### 2. Dependencia de conectividad para pagos e inventario
El modelo final muestra que la operación depende de conectividad hacia servicios externos, especialmente pasarela de pago e inventario. En escenarios de intermitencia de red, el sistema puede quedar parcialmente operativo, pero con limitaciones importantes.

**Impacto:** alto en horas pico.  
**Riesgo:** imposibilidad de cobrar o validar stock en tiempo real.  
**Oportunidad de mejora:** modos degradados de operación, colas temporales, caché local y políticas claras de reintento.

### 3. Base de datos transaccional como posible cuello de botella
La base de datos recibe operaciones de pedidos, actualizaciones de estado, pagos, inventario y auditoría. Si todo se concentra en una misma instancia sin separación de cargas, el crecimiento en simultaneidad podría afectar tiempos de respuesta.

**Impacto:** medio-alto.  
**Riesgo:** lentitud generalizada y afectación en sincronización entre venta y preparación.  
**Oportunidad de mejora:** réplicas de lectura, índices adecuados, separación entre transaccional y analítico, políticas de backup y recuperación.

### 4. Router o red local como punto único de falla en tienda
Aunque suele pasar desapercibido, la red local soporta la comunicación entre kiosco, POS, KDS e impresora, y además la salida a internet. Si ese componente falla, la tienda pierde coordinación interna y externa.

**Impacto:** alto.  
**Riesgo:** indisponibilidad parcial o total del servicio.  
**Oportunidad de mejora:** redundancia básica de conectividad, red de respaldo o procedimientos manuales de contingencia.

### 5. Sincronización entre pedido y preparación
El KDS depende de recibir correctamente las órdenes y sus cambios de estado. Un retraso o error en sincronización puede generar pedidos incompletos, duplicados o preparados fuera de secuencia.

**Impacto:** medio.  
**Riesgo:** errores operativos visibles para el cliente.  
**Oportunidad de mejora:** colas de mensajes, confirmaciones de recepción, manejo de eventos y trazabilidad por estado.

### 6. Observabilidad aún insuficiente si no se instrumenta desde el inicio
Sin monitoreo real de tiempos de respuesta, errores, disponibilidad, uso de red y comportamiento de pagos, los problemas solo se detectan cuando ya impactan la operación.

**Impacto:** alto a mediano plazo.  
**Riesgo:** diagnóstico tardío, poca trazabilidad, soporte reactivo.  
**Oportunidad de mejora:** dashboard de métricas, alertas por umbral, centralización de logs y seguimiento de eventos críticos.

## Cómo representa el modelo las necesidades del cliente
El modelo responde al funcionamiento real del cliente porque no se limita a mostrar software aislado, sino la interacción entre infraestructura física de tienda y servicios digitales. La operación de KOOLT depende de que el pedido pase correctamente por selección, validación, cobro, preparación y entrega. Por eso el mapa se construyó pensando en continuidad operativa y no solo en componentes técnicos.

También se procuró mantener un nivel de detalle suficiente para el diagnóstico sin caer en una complejidad innecesaria. Se priorizaron aquellos nodos que realmente explican riesgos del negocio: POS, red local, backend, pagos, base de datos y KDS.

## Supuestos tomados
- KOOLT opera con una arquitectura híbrida: dispositivos físicos en tienda y servicios backend conectados por internet.
- El POS actúa como nodo principal de la lógica operativa.
- La pasarela de pagos es un servicio externo.
- La validación de inventario ocurre sobre un servicio o módulo lógico separado.
- El monitoreo y los logs pueden centralizarse en un entorno cloud o en servicios administrados.
- La tienda no cuenta necesariamente con alta redundancia física, por lo que la red local representa un punto sensible.

## Tabla de componentes principales

| Nombre del elemento | Tipo | Descripción | Criticidad |
|---------------------|------|-------------|------------|
| Kiosco / Tablet | Dispositivo | Interfaz de autogestión del pedido | Media |
| POS | Aplicación / terminal | Registra y coordina pedidos, pagos y órdenes | Alta |
| KDS | Dispositivo / app | Muestra a cocina los pedidos y sus estados | Alta |
| Servicio de pedidos | Servicio backend | Orquesta la lógica principal del sistema | Alta |
| Servicio de inventario | Servicio backend | Verifica disponibilidad de insumos | Alta |
| Pasarela de pagos | Servicio externo | Autoriza o rechaza transacciones | Alta |
| Base de datos transaccional | Datos | Guarda pedidos, pagos, estados y trazabilidad | Alta |
| Router / red local | Infraestructura | Conecta los dispositivos de tienda | Alta |
| Monitoreo y alertas | Operación | Permite detectar fallas y degradaciones | Media-Alta |
| Logs centralizados | Operación | Guarda evidencias para soporte y auditoría | Media-Alta |

## Investigación complementaria

### Tema investigado
Buenas prácticas de arquitectura de infraestructura en entornos cloud e híbridos.

### Resumen
La revisión de marcos de arquitectura de proveedores cloud mostró que la confiabilidad, la eficiencia del rendimiento y la excelencia operativa son pilares recurrentes en el diseño de infraestructura. En AWS, Azure y Google Cloud aparece de forma consistente la necesidad de diseñar sistemas resilientes, observables y preparados para escalar horizontalmente cuando crece la demanda. Esto implica evitar dependencias innecesarias de componentes únicos, distribuir la carga y diseñar mecanismos de recuperación ante falla.

En el contexto de KOOLT, estas prácticas se traducen en decisiones concretas: reducir la dependencia de un solo punto operativo, instrumentar métricas desde el inicio, separar cargas sobre la base de datos y considerar escenarios degradados cuando fallen pagos o conectividad. Aunque KOOLT no requiere una arquitectura masiva, sí necesita una infraestructura estable y trazable, porque cualquier interrupción impacta directamente la experiencia del cliente y la operación en tienda.

También se revisaron lineamientos de NIST relacionados con respuesta a incidentes y gestión de logs. Estos documentos resaltan que monitoreo, registro de eventos y capacidad de respuesta no deben añadirse al final, sino formar parte del diseño inicial. Esto fortalece el criterio usado en el mapa final, donde observabilidad y logs aparecen como componentes explícitos y no como elementos secundarios.

## Conclusión
El mapa de infraestructura permitió aterrizar el sistema de KOOLT desde una perspectiva técnica y operativa al mismo tiempo. Más allá de representar componentes, el ejercicio hizo visible dónde están las mayores dependencias del servicio y cuáles son los puntos con mayor riesgo de falla o degradación.

El diagnóstico muestra que los mayores desafíos de KOOLT no están en la complejidad de su arquitectura, sino en la concentración funcional de algunos componentes, la dependencia de conectividad y la necesidad de contar con observabilidad real. Con mejoras graduales en redundancia, monitoreo y desacoplamiento, la infraestructura puede volverse más confiable, escalable y preparada para crecimiento futuro.