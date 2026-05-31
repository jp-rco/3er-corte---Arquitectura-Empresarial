# Informe – Modelado BPMN del Proceso de Pedido de Helado de Yogur
## 1. Proceso seleccionado (Cliente real)

Después de la clase, el equipo seleccionó como proceso real el Proceso de Pedido de Helado de Yogur de una heladería "Koolt", el cual fue modelado digitalmente en BPMN a partir del flujo operativo real observado.

El proceso inicia cuando el cliente solicita un pedido y finaliza con la entrega del producto y confirmación del pago.

Este proceso involucra múltiples decisiones, validaciones de reglas de negocio, verificación de disponibilidad de insumos y gestión de pago, lo que lo hace más complejo que el caso base trabajado en clase.

## 2. Descripción general del proceso

El flujo comienza cuando el cliente solicita un helado de yogur. Posteriormente:

- Selecciona tipo de helado (simple o mixto).

- Selecciona tamaño.

- Selecciona toppings y salsas.

- Confirma la selección final.

- El sistema valida reglas (combinaciones, tamaños).

- Se verifica disponibilidad de insumos con la operación.

- Si hay faltantes, el sistema solicita modificación.

- Se calcula el precio total.

- Se registra el pedido y se genera orden de preparación.

- La heladería prepara el producto.

- Se solicita y procesa el pago.

Finalmente, se entrega el producto al cliente.

El proceso contempla escenarios alternos como:

- Insumos no disponibles.

- Pago rechazado.

- Cancelación.

- Intervención manual por error del sistema.

## 3. Roles del proceso

- Cliente: realiza la selección, confirma pedido, paga y recibe el producto.

- Sistema/POS: captura selecciones, valida reglas, calcula precio, procesa pago y genera órdenes.

- Heladería (Operación): valida disponibilidad de insumos y entrega el producto.

- Heladería (Preparación): prepara el helado según especificaciones.

- Caja / Operación manual: interviene en caso de error o rechazo de pago.

## 4. Diferencias con el caso base (Clínica Salud Viva)

| Aspecto                     | Clínica (Caso Base)                                | Heladería (Cliente Real)                                      |
|----------------------------|----------------------------------------------------|----------------------------------------------------------------|
| Tipo de servicio           | Servicio intangible (cita médica)                  | Producto físico personalizado (helado)                       |
| Complejidad del flujo      | Flujo relativamente lineal                         | Flujo con múltiples decisiones y validaciones                |
| Decisiones principales     | Disponibilidad de agenda                           | Tipo de producto, stock, validación de reglas y pago         |
| Validaciones               | Disponibilidad de médico y horario                 | Combinaciones permitidas, disponibilidad de insumos, pago    |
| Gestión de inventario      | No aplica                                          | Sí, control de sabores, toppings, salsas y vasos             |
| Procesamiento de pago      | No incluido en el proceso                          | Incluido (aprobado, rechazado, reintento o cancelación)      |
| Manejo de excepciones      | Reprogramación si no hay cupo                      | Faltantes, pago rechazado, cancelación, error del sistema    |
| Interacción física         | No                                                 | Sí (preparación y entrega del producto)                      |
| Nivel de riesgo operativo  | Medio                                              | Alto (errores POS, stock, pagos, cancelaciones)              |


El proceso de la heladería es significativamente más dinámico y presenta múltiples gateways (disponibilidad, pago aprobado/rechazado, modificación o cancelación), lo que aumenta la complejidad del modelado.

## 5. Justificación del modelado BPMN

Se utilizó BPMN 2.0 debido a que:

Permite separar claramente roles mediante lanes.

Facilita representar decisiones complejas.

Modela eventos alternos (rechazo de pago, cancelación).

Representa interacción entre sistema y operación física.

El uso de gateways exclusivos fue fundamental para modelar:

Validación de disponibilidad.

Pago aprobado o rechazado.

Decisión de modificar o cancelar.

## 6. Buenas prácticas BPMN aplicadas

Durante la digitalización del modelo se aplicaron las siguientes buenas prácticas:

Un único evento de inicio y uno de fin claro.

Uso consistente de gateways exclusivos (XOR).

Separación clara por lanes (Cliente, Sistema, Operación).

Evitar cruces innecesarios de flujo.

Nombrar tareas con verbo + objeto.

Modelar excepciones como flujos alternos y no como parte del flujo principal.

## 7. Ejemplos en la industria

Procesos similares se utilizan en:

Cadenas de comida rápida (McDonald's, Subway).

Sistemas POS de retail.

Plataformas de personalización de productos (Starbucks App).

Comercio electrónico con validación de inventario y pago.

En todos estos casos se combinan:

Validación de stock.

Personalización del producto.

Procesamiento de pago.

Gestión de excepciones.