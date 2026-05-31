# Informe – Taller 06: Normatividad y Cumplimiento

## Integrantes
- Andrés Ricaurte
- Juan Restrepo
- Juan Jaime

---

## 1. Descripción general
El objetivo del taller fue evaluar el cumplimiento legal y normativo asociado al tratamiento de datos personales y la seguridad de la información, usando un checklist basado en Ley 1581 (Colombia), principios de Habeas Data y buenas prácticas alineables con ISO/IEC 27001.

Se trabajó en dos fases:
1) **Caso base (GobData)**: portal estatal con tratamiento masivo de datos personales y sensibles.
2) **Cliente real (KOOLT)**: sistema de pedidos y pagos en tienda (kiosco/POS), con datos personales limitados pero con riesgos operativos y de terceros (pagos, nube).

---

## 2. Caso base (GobData): resumen de resultados
GobData opera en un contexto de alto impacto: gestiona identidad, documentos, trámites y potencialmente datos de salud. Por esto, la exigencia de cumplimiento es alta.

Principales hallazgos:
- Hay cumplimiento básico en aviso de privacidad, roles generales y cifrado en tránsito.
- Se identificaron brechas relevantes en:
  - consentimiento reforzado para datos sensibles,
  - retención/eliminación por tipo de trámite,
  - DPIA (evaluación de impacto) para tratamientos de alto riesgo,
  - plan formal de respuesta a incidentes,
  - trazabilidad detallada de consultas a datos sensibles.

Estas brechas tienen impacto legal y reputacional, y aumentan la superficie de riesgo ante incidentes.

---

## 3. Cliente real (KOOLT): evaluación y adaptación
KOOLT, a diferencia de GobData, no procesa grandes volúmenes de datos sensibles; sin embargo, sí trata:
- datos de contacto (si se habilitan notificaciones),
- datos transaccionales (pedidos, devoluciones, anulaciones),
- integraciones con terceros (pasarela de pago, nube, mensajería),
- y controles internos que afectan fraude (panel de precios/promos).

Hallazgos principales:
- Cumple mejor en **minimización** y en **no almacenamiento de datos de tarjeta** (pasarela).
- Presenta brechas típicas de operación pequeña:
  - consentimiento explícito no siempre registrado,
  - ausencia de política de retención (se guarda “por defecto”),
  - MFA ausente en panel administrativo,
  - auditoría incompleta en cambios de precios/anulaciones,
  - manejo de incidentes sin playbooks.

---

## 4. Brechas con mayor prioridad (KOOLT)
1) **Retención**: definir tiempos por tipo de dato (contable/operativo/logs) y automatizar purga.
2) **Seguridad de admin**: MFA obligatorio + doble aprobación en cambios de precios/promos.
3) **Auditoría**: registrar cambios y excepciones (anulaciones, descuentos, promociones).
4) **Consentimiento**: captura explícita para notificaciones (y opción de revocatoria).
5) **Incidentes**: playbooks cortos para pagos, caída de POS y fuga de datos.

---

## 5. Recomendaciones concretas (accionables)
### Para GobData
- Implementar consentimiento explícito para datos sensibles y registrar evidencia.
- Definir retención/eliminación por trámite (tabla y automatización).
- DPIA por trámites de alto riesgo; comité de revisión.
- Plan de respuesta a incidentes con simulacros y criterios de notificación.
- Centralizar auditoría (quién consultó qué, cuándo, desde dónde) y alertas.

### Para KOOLT
- Publicar política completa en tienda/kiosco (QR) y registrar consentimiento para notificaciones.
- Retención por categorías (contable/operativo) y purga automática.
- MFA para panel admin y controles de cambios (aprobación + registro).
- Auditoría obligatoria para cambios de precio y anulaciones.
- Playbooks simples de incidentes y responsables por turno.

---

## 6. Distribución de responsabilidades (sugerida)
- **Andrés Ricaurte:** consentimiento, transparencia y derechos del titular.
- **Juan Restrepo:** seguridad técnica, controles de acceso y auditoría.
- **Juan Jaime:** retención, gestión de incidentes y gestión de proveedores/transferencias.

---

## 7. Conclusión
El checklist permitió comparar dos realidades: un portal gubernamental de alta criticidad (GobData) y un sistema operativo en tienda (KOOLT).  
En ambos casos, los puntos clave para “cerrar brechas” son: evidencia (trazabilidad), control de acceso, retención, incidentes y gestión de terceros.
