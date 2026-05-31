# Informe – Taller 5: Evaluación de Seguridad con STRIDE

## 1. Alcance y metodología
Se realizó un análisis de amenazas usando el marco **STRIDE** sobre un flujo crítico del caso base (EduKIT) y luego se adaptó el ejercicio al sistema del cliente real (KOOLT).  
Para cada amenaza se documentó: componente afectado, tipo STRIDE, escenario, impacto, probabilidad, nivel de riesgo y mitigación recomendada.

La escala usada fue:
- **Impacto (1–5):** efecto en confidencialidad, integridad, disponibilidad, reputación o cumplimiento.
- **Probabilidad (1–5):** factibilidad (exposición, facilidad, precedentes, controles existentes).
- **Nivel de riesgo:** Impacto × Probabilidad.

---

## 2. Caso base (EduKIT) – Flujo crítico analizado
**Acceso a cursos + pagos + emisión de certificados**

### 2.1 Resumen de hallazgos
Los riesgos más relevantes se concentran en:
- **Autenticación y gestión de sesiones** (spoofing / secuestro de sesión).
- **Disponibilidad** del gateway y la base de datos en picos de demanda.
- **Integridad** de notas y certificados si se permite lógica crítica en el cliente o hay autorización débil.
- **Trazabilidad** de acciones (repudiación) sin auditoría central e inmutable.

### 2.2 Priorización (top 5)
1) Suplantación por credenciales filtradas en login (alto).  
2) Saturación del API Gateway / WAF en inscripciones (alto).  
3) Alteración de calificaciones o certificados (medio-alto).  
4) Filtración de contenido premium (IDOR / configuración incorrecta) (medio).  
5) Disputas de pagos sin correlación completa (medio).

---

## 3. Cliente real (KOOLT) – Adaptación al dominio
**Pedido + pago + preparación + entrega**

### 3.1 Resumen de hallazgos
En KOOLT los riesgos cambian de “historial académico” a “operación en tienda y pagos”. Los puntos sensibles son:
- **POS y red local** como piezas críticas (si caen, la operación se detiene).
- **Pagos** y trazabilidad (repudiación y disputas).
- **Manipulación de pedidos/precios** (tampering) si no se valida server-side.
- **Accesos administrativos** para promociones y precios (elevación de privilegios).
- **Disponibilidad** en horas pico (promociones).

### 3.2 Priorización (top 5)
1) Falla o DoS en red local/router (alto).  
2) Suplantación de POS o dispositivos no autorizados (medio-alto).  
3) Manipulación de precios/descuentos desde kiosco o admin (medio-alto).  
4) Exposición o mala gestión de tokens de pago/logs sensibles (medio).  
5) Saturación del backend en promociones (medio).

---

## 4. Diferencias relevantes entre EduKIT y KOOLT
- **EduKIT** prioriza protección de identidad, datos personales, notas y certificados (confidencialidad/integridad).
- **KOOLT** prioriza continuidad operativa en tienda, pagos confiables, inventario y control de fraude interno (disponibilidad/integridad).

En ambos casos, los controles más costo-efectivos fueron:
- RBAC/ABAC bien definidos, MFA para roles privilegiados, auditoría central, rate limiting/WAF y hardening de endpoints.

---

## 5. Recomendaciones concretas (ejecutables)
### EduKIT
- MFA (al menos para administradores/docentes), protección contra credential stuffing, rotación de tokens.
- Control de acceso por objeto (evitar IDOR), URLs firmadas para contenido.
- Auditoría central e inmutable para notas/certificados y pagos.
- Separación lectura/escritura en BD y límites a consultas pesadas.

### KOOLT
- Enrolamiento de dispositivos, **mTLS** POS↔backend, segmentación de red (POS/KDS separado de Wi‑Fi invitados).
- Modo degradado offline con cola local (cuando falle internet), y reconciliación posterior.
- Validación server-side de precios, promociones y reglas; doble aprobación para cambios críticos.
- Observabilidad mínima: métricas de POS, red, pagos, latencia backend; alertas por fallas repetidas.

---

## 6. Conclusión
Las tablas STRIDE entregadas permiten identificar y justificar amenazas plausibles, priorizar por riesgo y proponer mitigaciones aterrizadas al contexto.  
En EduKIT el foco está en identidad y datos académicos; en KOOLT el foco está en continuidad operativa y control de fraude/pagos.  
El siguiente paso recomendado sería validar estas amenazas con pruebas de seguridad (revisión de autorizaciones, pruebas de carga y verificación de logging/auditoría).
