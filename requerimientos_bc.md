# Especificación de Requerimientos: Herramienta de Base de Datos de Clientes (Fase 1)

## 1. Módulo: Core de Datos (Supabase/Postgres)
**Problema:** Necesidad de una fuente de verdad única y centralizada para los datos de clientes.
**Requerimiento:** Configuración de base de datos relacional con acceso vía Postgres.

* **Historia de Usuario:** Como administrador, quiero una base de datos centralizada en Supabase para asegurar que toda la información de proyectos y fincas filiales sea consistente y accesible.
* **Criterios de Aceptación:**
    * Tablas normalizadas para Clientes, Proyectos y Fincas Filiales.
    * Conexión estable vía cadena de conexión Postgres.
    * El esquema debe soportar el crecimiento modular para futuras fases.

## 2. Módulo: Gestión de Estados y Bitácora de Incidencias
**Problema:** Pérdida de trazabilidad cuando un negocio se cae o cambia de etapa.
**Requerimiento:** Flujo de estados interno con registro de motivos de salida (Finiquitos).

* **Historia de Usuario:** Como usuario de ventas, quiero actualizar el estado de una finca (Firma, Reserva, Caída) y registrar observaciones en una bitácora para mantener el historial del cliente.
* **Reglas:**
    * Si una venta se marca como "Caída", el sistema debe solicitar obligatoriamente el motivo en la bitácora.
    * Notificación interna a los interesados (Chicas/Administración) al cambiar a estado "Firma".
* **Criterios de Aceptación:**
    * Campo de "Estado" con opciones predefinidas.
    * Sección de bitácora visible por registro de cliente.

## 3. Módulo: Gobierno de Datos (Sistema de Tickets)
**Problema:** Riesgo de errores manuales en datos críticos (Cédula, Correo, Nombre).
**Requerimiento:** Flujo de aprobación interna para modificaciones de datos sensibles.

* **Historia de Usuario:** Como usuario de Formalizaciones, quiero validar y aprobar cambios en los datos críticos de los clientes antes de que se guarden permanentemente.
* **Reglas:**
    * La edición de campos críticos genera un ticket en estado "Pendiente".
    * Solo el Super Admin o Formalizaciones (Iván) pueden dar el visto bueno al ticket.
* **Criterios de Aceptación:**
    * Panel de control para visualizar y procesar tickets pendientes.
    * Actualización automática del registro principal tras la aprobación.

## 4. Módulo: Visualización y Edición Rápida
**Problema:** Lentitud en la consulta de datos específicos por proyecto o fase.
**Requerimiento:** Vista tabular con filtros avanzados y edición por celda para administradores.

* **Campos Críticos a Validar:** Correo electrónico, teléfono, nombre y cédula.
* **Historia de Usuario:** Como administrador, quiero editar rápidamente datos no críticos mediante un lápiz en la celda y filtrar la tabla por Proyecto o Fase.
* **Criterios de Aceptación:**
    * Filtros por: Fase, Proyecto, Tipología (Playa/Ciudad), Finca Filial.
    * Edición rápida (Inline editing) restringida por rol de usuario.
    * Soporte para correos y teléfonos adicionales con su respectivo parentesco.

## 5. Módulo: Automatización Documental (Cloud)
**Problema:** Inconsistencia en la nomenclatura y estructura de carpetas de expedientes.
**Requerimiento:** Generador automático de carpetas según tipología de proyecto.

* **Historia de Usuario:** Como usuario administrativo, quiero que el sistema cree automáticamente el árbol de carpetas al registrar un contrato para estandarizar el archivo digital.
* **Estructura por Tipo:**
    * **Playa:** `/ACQFLA/FINCAS/RETA/FR-01/FF[ID]-[Nombre]`
    * **Urbano:** `/URBANOS/FINCAS/BAC_GELDSTUCK/FF[ID]-[Nombre]`
* **Criterios de Aceptación:**
    * Botón de "Descargar ZIP" para obtener el expediente completo.
    * Integración con servicio de almacenamiento en la nube (definido en arquitectura).

## 6. Módulo: Aprovisionamiento de Inventario (Precarga)
**Problema:** Carga manual tediosa de fincas filiales al iniciar una nueva fase.
**Requerimiento:** Importación masiva de unidades con prefijos lógicos.

* **Historia de Usuario:** Como Super Admin, quiero precargar la lista de fincas filiales de una fase mediante un rango o prefijo para agilizar la puesta en marcha del proyecto.
* **Criterios de Aceptación:**
    * Herramienta de carga que solicita: Proyecto -> Fase -> Lista de Fincas.
    * Validación de no duplicidad de códigos de referencia.

## 7. Módulo: Seguridad, Logs y Login
**Problema:** Falta de control sobre el acceso y acciones de los usuarios.
**Requerimiento:** Autenticación personalizada y registro de auditoría.

* **Historia de Usuario:** Como auditor, quiero ver un log detallado de quién creó, actualizó o eliminó un registro para asegurar la responsabilidad sobre los datos.
* **Criterios de Aceptación:**
    * Login con gestión de contraseñas (cambio por usuario).
    * Tabla de Logs: Usuario, Fecha, Acción, Valor Anterior, Valor Nuevo.
    * Gestión de permisos por módulo y por carpetas.
