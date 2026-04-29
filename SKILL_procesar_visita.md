# SKILL — Procesar Visita Comercial TEIDROX
**Versión:** 1.0  
**Repositorio:** github.com/TEIBOT/1-TEIBOT  
**Última actualización:** 2026-04-29

---

## CUÁNDO SE ACTIVA

Cuando Benicio termina una visita y vomita la información de lo que pasó. Puede ser texto libre, desordenado, en jerga, con voz transcrita. Claude extrae todo lo relevante y decide qué acción tomar.

---

## PASO 1 — EXTRACCIÓN DE DATOS

De lo que Benicio cuente, Claude extrae obligatoriamente:

| Campo | Descripción | Obligatorio |
|---|---|---|
| `nombre_contacto` | Nombre de la persona con quien se habló | Sí |
| `cargo_contacto` | Cargo o rol en el establecimiento | Si se sabe |
| `nombre_establecimiento` | Nombre del centro | Sí |
| `tipo_cliente` | residencia / restaurante / hotel / otro | Sí |
| `producto_interes` | Qué se presentó o qué necesitan | Sí |
| `problema_detectado` | Pain point real detectado | Sí |
| `temperatura_lead` | frío / tibio / caliente | Sí |
| `siguiente_paso` | Lo que se acordó (si algo) | Si lo hay |
| `fecha_visita` | Hoy por defecto salvo que indique otra | Sí |
| `email_contacto` | Email para la secuencia | Si se tiene |
| `telefono_contacto` | Teléfono del contacto | Si se tiene |
| `notas_libres` | Cualquier dato personal o anecdótico útil | Si lo hay |

Si falta `email_contacto` y la acción es enviar correo → preguntar antes de continuar.

---

## PASO 2 — ÁRBOL DE DECISIÓN

Tras extraer los datos, Claude decide la acción o combinación de acciones:

```
¿Se acordó enviar correo?
├── SÍ → Activar SKILL_correos_teidrox.md + registrar en CRM
└── NO → Continuar árbol

¿Hay que llamar en X días?
├── SÍ → Crear evento en Google Calendar (gestion@teidrox.com) + registrar en CRM
└── NO → Continuar árbol

¿Hay que mandar algo por WhatsApp o material?
├── SÍ → Anotar en CRM como tarea pendiente + avisar a Benicio
└── NO → Continuar árbol

¿No hay siguiente paso claro?
└── Registrar en CRM como "visita realizada, sin acción acordada" + preguntar a Benicio qué quiere hacer
```

---

## PASO 3 — REGISTRO EN CRM (Google Sheet gestion@teidrox.com)

Siempre, independientemente de la acción, se registra en la hoja CRM:

**Hoja:** `CRM_Leads`  
**Acción:** Añadir fila nueva o actualizar si el contacto ya existe

Columnas que se rellenan:

| Columna | Valor |
|---|---|
| fecha_visita | Fecha de la visita |
| nombre_contacto | Nombre |
| cargo_contacto | Cargo |
| nombre_establecimiento | Nombre del centro |
| tipo_cliente | residencia / restaurante / hotel |
| email_contacto | Email |
| telefono_contacto | Teléfono |
| producto_interes | Producto presentado |
| problema_detectado | Pain point |
| temperatura_lead | frío / tibio / caliente |
| siguiente_paso | Acción acordada |
| estado_lead | nuevo / activo / pausado / cerrado / respondido |
| numero_correo_enviado | 0 (se actualiza con cada envío) |
| fecha_ultimo_contacto | Fecha del último correo/llamada |
| notas | Notas libres |

---

## PASO 4 — CONFIRMACIÓN A BENICIO

Tras registrar y activar las acciones, Claude muestra un resumen claro:

```
✅ VISITA REGISTRADA — {{nombre_establecimiento}}

👤 Contacto: {{nombre_contacto}} ({{cargo_contacto}})
🌡️ Temperatura: {{temperatura_lead}}
📧 Correo 1: [REDACTADO / PENDIENTE DE REDACTAR]
📅 Próxima acción: {{siguiente_paso}}
📋 CRM: actualizado

¿Revisamos el correo antes de enviarlo?
```

---

## NOTAS IMPORTANTES

- Claude nunca asume el email si no se ha dicho. Pregunta.
- Si el lead ya existía en el CRM (misma empresa), Claude lo indica y pregunta si actualizar o crear entrada nueva.
- La temperatura del lead la decide Benicio, no Claude. Claude puede sugerir pero no imponer.
- Si Benicio no sabe el email en ese momento, se registra sin email y se marca como "pendiente email" en el CRM.
