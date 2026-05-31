# Informe – Taller 02: ERD + Diagrama de Contexto

## 1. Caso base: Clínica Salud Viva
En clase se modeló un ERD simple para el proceso de agendamiento de citas y un borrador del diagrama de contexto.

### 1.1 ERD del caso base
Entidades: Paciente, Cita, Médico, Especialidad, Factura.
Relaciones principales:
- Un paciente puede tener muchas citas (1:N).
- Cada cita está asociada a un médico y una especialidad (N:1).
- Una cita puede generar una factura (0..1).

### 1.2 Contexto del caso base
Actores: Paciente, Médico, Asistente/Recepción.
Sistemas: Agendamiento, Notificador (SMS/Email), ERP.
Se identificaron flujos de datos como: solicitud de cita, confirmación, disponibilidad, ajustes manuales y datos para registro/facturación.

## 2. Cliente real: Heladería – Pedido de Helado de Yogurt
Se adaptó el modelo de información al dominio de la heladería, manteniendo la lógica del caso base:
- “Cita” se transforma en “Pedido”.
- “Validación de disponibilidad” se transforma en “Validación de reglas + Stock de insumos”.
- “Factura/Pago” aparece como un bloque crítico por reintentos y rechazo.

### 2.1 ERD del cliente real
El modelo incorpora:
- Cliente, Pedido e Ítem de Pedido para soportar múltiples helados por pedido.
- Personalización (sabores, tamaño) y extras (toppings, salsas) mediante relaciones N:M con tablas puente.
- Inventario/stock para validación de disponibilidad.
- Pago con estados (aprobado/rechazado) y posibilidad de reintentos.
- Orden de preparación para conectar la solicitud con el flujo operativo.

### 2.2 Contexto del cliente real
Actores: Cliente, Operación/Caja, Preparación.
Sistemas: POS/Sistema de pedidos, Inventario/Stock, Pasarela de pagos, Notificador/Ticket.
Flujos clave:
- Selección y confirmación del pedido.
- Consulta de stock y gestión de faltantes.
- Cálculo del total.
- Pago aprobado/rechazado y emisión de comprobante.
- Estados de preparación y entrega.

## 3. Diferencias clave entre caso base y cliente real
- En la clínica el núcleo es la disponibilidad de agenda; en la heladería el núcleo es la personalización y el stock.
- En la heladería se necesita modelar N:M (extras) y control de inventario, aumentando complejidad del ERD.
- El contexto del cliente real incorpora pasarela de pagos y estados operativos de preparación/entrega.

## 4. Justificación de decisiones
- Se separó Pedido de ÍtemPedido para soportar escalabilidad (varios productos por pedido).
- Se usaron tablas puente para toppings y salsas porque un ítem puede tener múltiples extras y cada extra puede aparecer en muchos ítems (N:M).
- Se incluyó Pago como entidad para registrar reintentos y estados.
- Se incluyó Inventario para reflejar la validación de disponibilidad de insumos que ocurre en el proceso real.

## 5. Investigación complementaria (ERD y contexto en industria)
En sistemas reales:
- ERD se usa para garantizar consistencia de datos y reglas de negocio (p.ej., POS retail y apps de pedidos).
- Diagramas de contexto se usan para identificar integraciones críticas (pagos, inventarios, notificaciones).
Ejemplos típicos:
- Apps de comida (PedidosYa/Rappi): pedido + ítems + extras + pagos + inventario.
- Retail/POS: ticket/orden + stock + pasarela + comprobantes.
