# 🚀 GUÍA DE IMPLEMENTACIÓN PASO A PASO
## Sistema de Control Global de Correspondencia SCARE

---

## 📋 PRERREQUISITOS

### Cuentas y Accesos Necesarios

✅ **Office 365**
- Cuenta: infraestructura@scare.org.co
- Permisos de administrador
- Acceso a Exchange Online (Email)
- Acceso a OneDrive/SharePoint

✅ **N8N**
- Opción 1: N8N Cloud (recomendado para empezar) - https://n8n.cloud
- Opción 2: N8N Self-hosted (servidor propio)
- Plan mínimo: Starter (soporta 5 workflows)

✅ **Google Forms**
- Cuenta de Google (puede ser gratuita)
- Para crear formulario de registro

✅ **Microsoft Forms**
- Incluido en Office 365
- Para panel de feedback

✅ **Active Directory**
- Acceso de lectura al AD de SCARE
- Para consultar datos de usuarios

---

## 📅 CRONOGRAMA DE IMPLEMENTACIÓN

### **SEMANA 1: Preparación y Configuración Base**

#### Día 1-2: Configuración de N8N
1. Crear cuenta en N8N Cloud o instalar N8N self-hosted
2. Configurar credenciales de Office 365
3. Configurar credenciales de Google
4. Probar conectividad

#### Día 3-4: Crear Base de Datos Excel
1. Crear archivo `BD_Correspondencia_SCARE.xlsx` en OneDrive
2. Configurar estructura de hojas
3. Compartir con permisos adecuados
4. Configurar SharePoint para evidencias

#### Día 5: Crear Formularios
1. Crear Google Form de registro
2. Crear Microsoft Form de feedback
3. Configurar campos según especificaciones

---

### **SEMANA 2: Implementación de Flujos Básicos**

#### Día 6-8: Flujo 1 - Registro de Correspondencia
1. Importar `FLUJO_1_Registro_Correspondencia.json` en N8N
2. Configurar variables de entorno
3. Conectar Google Forms
4. Probar registro básico
5. Validar generación de radicados

#### Día 9-10: Flujo 2 - Notificaciones
1. Importar `FLUJO_2_Notificaciones.json`
2. Personalizar plantilla de email
3. Configurar cuenta de envío
4. Realizar pruebas de notificación
5. Validar recordatorios automáticos

---

### **SEMANA 3: Funcionalidades Avanzadas**

#### Día 11-12: Flujo 3 - Feedback de Destinatarios
1. Importar `FLUJO_3_Feedback_Destinatarios.json`
2. Conectar Microsoft Forms
3. Configurar lógica de delegación
4. Probar diferentes escenarios
5. Validar actualizaciones de estado

#### Día 13-14: Integración Active Directory
1. Configurar Microsoft Graph API
2. Probar consultas de usuarios
3. Validar obtención de área y cargo
4. Implementar búsquedas

#### Día 15: Carga de Evidencias
1. Configurar carpeta SharePoint
2. Probar subida de archivos
3. Validar links en notificaciones
4. Organización por fecha

---

### **SEMANA 4: Gestión y Reportes**

#### Día 16-17: Flujo 4 - Gestión de Estados
1. Importar `FLUJO_4_Gestion_Estados.json`
2. Configurar alertas automáticas
3. Probar cálculo de días hábiles
4. Validar marcado de vencidos
5. Configurar reporte diario

#### Día 18-19: Flujo 5 - Reportes
1. Importar `FLUJO_5_Reportes_Consultas.json`
2. Configurar webhooks
3. Probar generación de Excel
4. Validar estadísticas
5. Personalizar plantillas de reporte

#### Día 20: Integración y Pruebas
1. Pruebas end-to-end completas
2. Validar todos los flujos integrados
3. Corregir errores encontrados
4. Optimizar tiempos de ejecución

---

### **SEMANA 5: Piloto y Despliegue**

#### Día 21-23: Prueba Piloto
1. Seleccionar 3 usuarios piloto
2. Capacitar usuarios piloto
3. Monitorear registros de prueba
4. Recopilar feedback
5. Realizar ajustes

#### Día 24-25: Migración de Datos Históricos
1. Exportar datos del sistema antiguo
2. Limpiar y formatear datos
3. Importar a Excel nuevo
4. Validar integridad de datos
5. Backup completo

#### Día 26-28: Capacitación
1. Crear material de capacitación
2. Grabar videos tutoriales
3. Realizar sesiones de capacitación:
   - Usuarios nivel central (3 personas)
   - Usuarios nivel nacional (21 personas)
4. Distribuir documentación

#### Día 29-30: Despliegue y Cierre
1. Activar sistema para todos los usuarios
2. Monitoreo intensivo primeros días
3. Soporte en vivo
4. Desactivar sistema antiguo
5. Documentar lecciones aprendidas

---

## 🔧 CONFIGURACIÓN DETALLADA

### 1. Crear Archivo Excel en OneDrive

**Paso 1:** Ir a OneDrive Business
```
https://scarecolombia-my.sharepoint.com
```

**Paso 2:** Crear carpeta compartida
```
Nombre: Sistema_Correspondencia_SCARE
Permisos: Edición para infraestructura@scare.org.co
```

**Paso 3:** Crear archivo Excel
```
Nombre: BD_Correspondencia_SCARE.xlsx
```

**Paso 4:** Crear hojas con esta estructura:

#### **Hoja: Registros_2024**
```
| ID | No_Radicado | Fecha_Recepcion_Fisica | Hora_Recepcion_Fisica | ... |
```
Ver archivo `ESTRUCTURA_EXCEL.xlsx` adjunto para columnas completas

#### **Hoja: Configuracion**
```
| Campo | Valor |
|-------|-------|
| Año_Actual | 2024 |
| Ultimo_Consecutivo | 0 |
| Email_Notificaciones | infraestructura@scare.org.co |
| Dias_Custodia_Max | 8 |
```

#### **Hoja: Sedes**
```
| Codigo_Sede | Nombre_Sede | Ciudad | Tiene_CURF |
|-------------|-------------|---------|------------|
| BOG-01 | Bogotá - Central | Bogotá | Sí |
| MED-01 | Medellín | Medellín | No |
```
Ver archivo `LISTA_SEDES.xlsx` para completar

#### **Hoja: Log_Auditoria**
```
| Fecha_Hora | Usuario | Accion | No_Radicado | Detalle |
```

**Paso 5:** Obtener ID del archivo
1. Abrir archivo en OneDrive
2. Hacer clic en "Compartir"
3. Copiar enlace
4. El ID está en la URL: `https://scarecolombia.sharepoint.com/personal/.../Documents/[FILE_ID]`

---

### 2. Configurar N8N

**Paso 1:** Crear cuenta en N8N
```
Ir a: https://n8n.cloud
Registrarse con: infraestructura@scare.org.co
Plan: Starter ($20/mes)
```

**Paso 2:** Configurar Credenciales

#### Microsoft Excel OAuth2
```
1. Ir a Settings > Credentials
2. Añadir nueva credencial: "Microsoft Excel OAuth2 API"
3. Nombre: Microsoft Excel SCARE
4. Client ID: [Obtener de Azure AD]
5. Client Secret: [Obtener de Azure AD]
6. Autorizar con cuenta: infraestructura@scare.org.co
```

#### Microsoft Outlook OAuth2
```
1. Añadir credencial: "Microsoft Outlook OAuth2 API"
2. Nombre: Outlook SCARE
3. Usar mismas credenciales de Azure AD
4. Autorizar con cuenta: infraestructura@scare.org.co
```

#### Microsoft Graph API
```
1. Añadir credencial: "Microsoft Graph API"
2. Nombre: Graph API SCARE
3. Permisos necesarios:
   - User.Read.All
   - Directory.Read.All
4. Autorizar
```

#### Google Forms
```
1. Añadir credencial: "Google OAuth2 API"
2. Nombre: Google Forms SCARE
3. Scopes: forms.readonly, drive.readonly
4. Autorizar
```

**Paso 3:** Configurar Variables de Entorno
```
Settings > Environment Variables:

EXCEL_FILE_ID: [ID del archivo Excel copiado antes]
SHAREPOINT_FOLDER_ID: [ID de carpeta SharePoint para evidencias]
ONEDRIVE_FOLDER_ID: [ID de carpeta OneDrive para reportes]
FORMS_FEEDBACK_URL: [URL del Microsoft Form de feedback]
N8N_WEBHOOK_URL: [URL base de tu instancia N8N]
```

---

### 3. Crear Google Form de Registro

**Estructura del Formulario:**

**Sección 1: Información de Recepción**
```
- Fecha de recepción física *
  Tipo: Fecha
  Requerido: Sí
  
- Hora de recepción física *
  Tipo: Hora
  Requerido: Sí
  
- Sede donde se recibe *
  Tipo: Lista desplegable
  Opciones: [Cargar desde lista de sedes]
  Requerido: Sí
```

**Sección 2: Datos del Remitente**
```
- Nombre del remitente *
  Tipo: Texto corto
  Requerido: Sí
  
- ¿Tiene número de guía? *
  Tipo: Opción múltiple
  Opciones: Sí, No
  Requerido: Sí
  
- Número de guía
  Tipo: Texto corto
  Requerido: No (Condicional si "Sí" en anterior)
```

**Sección 3: Datos del Destinatario**
```
- Nombre del destinatario *
  Tipo: Texto corto
  Requerido: Sí
  
- A quién se le notifica (email) *
  Tipo: Texto corto
  Validación: Formato email
  Requerido: Sí
  
- Descripción de la correspondencia *
  Tipo: Texto párrafo
  Requerido: Sí
```

**Sección 4: Evidencias**
```
- Adjuntar fotografías/archivos
  Tipo: Subida de archivo
  Permitir múltiples archivos: Sí
  Tipos aceptados: JPG, PNG, PDF
  Tamaño máximo: 10MB por archivo
```

**Configuración:**
```
- Recopilar direcciones de correo: Sí
- Límite de 1 respuesta por persona: No
- Enviar copia de respuestas: Sí
- Confirmación después de enviar: "Registro creado exitosamente"
```

**Conectar con N8N:**
1. Ir a respuestas del formulario
2. Hacer clic en los tres puntos
3. Seleccionar "Obtener ID del formulario"
4. Copiar ID y guardar para configuración en N8N

---

### 4. Crear Microsoft Form de Feedback

**Estructura del Formulario:**

```
Título: Panel de Gestión de Correspondencia SCARE
Descripción: Por favor indica qué acción deseas realizar con tu correspondencia
```

**Pregunta 1:**
```
Título: Número de radicado *
Tipo: Texto corto
Requerido: Sí
Placeholder: Ejemplo: 2024-0001
```

**Pregunta 2:**
```
Título: ¿Qué deseas hacer con esta correspondencia? *
Tipo: Elección
Opciones:
  ○ Almacenar temporalmente en sede (pasaré a recogerla)
  ○ Solicitar entrega a otro usuario
  ○ Autorizo eliminación de correspondencia física
Requerido: Sí
```

**Pregunta 3 (Condicional):**
```
Mostrar si: "Solicitar entrega a otro usuario"
Título: Email del usuario delegado *
Tipo: Texto corto
Validación: Email
Requerido: Sí
```

**Pregunta 4 (Condicional):**
```
Mostrar si: "Solicitar entrega a otro usuario"
Título: Observaciones
Tipo: Texto largo
Requerido: No
```

**Pregunta 5 (Condicional):**
```
Mostrar si: "Autorizo eliminación"
Título: Justificación de eliminación *
Tipo: Texto largo
Requerido: Sí
Placeholder: Explica por qué solicitas la eliminación del documento físico
```

**Pregunta 6 (Condicional):**
```
Mostrar si: "Autorizo eliminación"
Título: Confirmo que autorizo la eliminación *
Tipo: Casilla de verificación
Opciones: [✓] Confirmo mi autorización
Requerido: Sí
```

**Configuración:**
```
- Aceptar respuestas: Sí
- Una respuesta por persona: No
- Mostrar barra de progreso: Sí
- Mensaje de agradecimiento: "Tu respuesta ha sido registrada exitosamente"
```

---

### 5. Importar Flujos en N8N

**Para cada flujo (1 a 5):**

1. Ir a N8N Dashboard
2. Click en "Add workflow" o "+"
3. Click en los tres puntos (⋮)
4. Seleccionar "Import from File"
5. Seleccionar archivo JSON correspondiente
6. El flujo se importará con todos los nodos
7. Revisar configuración de cada nodo
8. Conectar credenciales
9. Activar el workflow

**Orden de importación recomendado:**
1. FLUJO_1_Registro_Correspondencia.json
2. FLUJO_2_Notificaciones.json
3. FLUJO_3_Feedback_Destinatarios.json
4. FLUJO_4_Gestion_Estados.json
5. FLUJO_5_Reportes_Consultas.json

---

### 6. Configurar Azure AD para APIs

**Paso 1:** Ir a Azure Portal
```
https://portal.azure.com
Iniciar sesión con cuenta de administrador
```

**Paso 2:** Crear App Registration
```
1. Ir a "Azure Active Directory"
2. Ir a "App registrations"
3. Click "New registration"
4. Nombre: "N8N Sistema Correspondencia"
5. Supported account types: "Accounts in this organizational directory only"
6. Redirect URI: [URL de N8N] + /rest/oauth2-credential/callback
7. Click "Register"
```

**Paso 3:** Configurar Permisos
```
1. Ir a "API permissions"
2. Click "Add a permission"
3. Seleccionar "Microsoft Graph"
4. Agregar permisos:
   Delegated permissions:
   - User.Read
   - User.Read.All
   - Directory.Read.All
   - Mail.Send
   - Files.ReadWrite
   - Sites.ReadWrite.All
   
5. Click "Grant admin consent"
```

**Paso 4:** Crear Client Secret
```
1. Ir a "Certificates & secrets"
2. Click "New client secret"
3. Description: "N8N Integration"
4. Expires: 24 months
5. Click "Add"
6. COPIAR EL VALOR INMEDIATAMENTE (no se mostrará después)
```

**Paso 5:** Copiar Credenciales
```
Application (client) ID: [copiar]
Directory (tenant) ID: [copiar]
Client secret value: [copiar]
```

Estas credenciales se usarán en N8N para todas las integraciones de Microsoft.

---

### 7. Configurar SharePoint para Evidencias

**Paso 1:** Crear biblioteca de documentos
```
1. Ir a SharePoint
2. Crear sitio: "Sistema Correspondencia"
3. Crear biblioteca: "Evidencias_Correspondencia"
```

**Paso 2:** Organizar estructura de carpetas
```
Evidencias_Correspondencia/
├── 2024/
│   ├── 01_Enero/
│   ├── 02_Febrero/
│   ├── ...
│   └── 12_Diciembre/
├── 2025/
│   └── ...
```

**Paso 3:** Configurar permisos
```
- infraestructura@scare.org.co: Control total
- N8N Service Account: Editar
- Otros usuarios: Lectura
```

**Paso 4:** Obtener ID de la carpeta
```
1. Navegar a la carpeta raíz
2. Click en "..." > "Details"
3. El ID estará en la URL o en las propiedades
```

---

## ✅ CHECKLIST DE VALIDACIÓN

Antes de pasar a producción, verificar:

### Configuración Base
- [ ] Excel creado en OneDrive con todas las hojas
- [ ] SharePoint configurado para evidencias
- [ ] N8N instalado y accesible
- [ ] Credenciales de Office 365 configuradas
- [ ] Variables de entorno configuradas

### Formularios
- [ ] Google Form de registro creado y probado
- [ ] Microsoft Form de feedback creado y probado
- [ ] Ambos formularios conectados a N8N

### Flujos N8N
- [ ] Flujo 1 importado y activo
- [ ] Flujo 2 importado y activo
- [ ] Flujo 3 importado y activo
- [ ] Flujo 4 importado y activo
- [ ] Flujo 5 importado y activo

### Pruebas Funcionales
- [ ] Registro de correspondencia funciona
- [ ] Radicados se generan correctamente
- [ ] Notificaciones llegan al destinatario
- [ ] Feedback se procesa correctamente
- [ ] Evidencias se suben a SharePoint
- [ ] Alertas automáticas funcionan
- [ ] Reportes se generan correctamente

### Integración
- [ ] Active Directory responde consultas
- [ ] Emails se envían desde cuenta correcta
- [ ] Links en emails funcionan
- [ ] Delegación de usuarios funciona
- [ ] Log de auditoría se actualiza

---

## 🆘 SOLUCIÓN DE PROBLEMAS COMUNES

### Error: "No se puede conectar a Excel"
**Solución:**
1. Verificar credenciales de Microsoft Excel OAuth2
2. Volver a autorizar con la cuenta correcta
3. Verificar que el ID del archivo es correcto
4. Verificar permisos del archivo

### Error: "No se genera radicado"
**Solución:**
1. Verificar que la hoja "Configuracion" existe
2. Verificar que campo "Ultimo_Consecutivo" existe
3. Inicializar con valor 0 si está vacío

### Error: "No llegan emails"
**Solución:**
1. Verificar credenciales de Outlook
2. Verificar que infraestructura@scare.org.co tiene permisos de envío
3. Revisar logs de N8N para errores específicos
4. Verificar que no hay límites de envío alcanzados

### Error: "No se encuentran usuarios en AD"
**Solución:**
1. Verificar permisos de Microsoft Graph API
2. Verificar que User.Read.All está autorizado
3. Probar búsqueda manualmente en Graph Explorer
4. Verificar formato de email

---

## 📞 CONTACTO Y SOPORTE

**Administrador del Sistema:**
- Email: infraestructura@scare.org.co
- Responsable: [Nombre del responsable]

**Soporte Técnico N8N:**
- Documentación: https://docs.n8n.io
- Community: https://community.n8n.io
- Support (plan pago): support@n8n.io

**Microsoft Support:**
- Office 365 Admin Center
- Soporte empresarial si se tiene

---

**Versión:** 1.0  
**Última Actualización:** Febrero 2024  
**Próxima Revisión:** Marzo 2024
