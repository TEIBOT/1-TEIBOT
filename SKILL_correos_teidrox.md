# SKILL — Correos Comerciales TEIDROX
**Versión:** 1.0  
**Repositorio:** github.com/TEIBOT/1-TEIBOT  
**Última actualización:** 2026-04-29

---

## CUÁNDO SE ACTIVA ESTA SKILL

Se activa cuando Benicio termina una visita comercial y vomita la información de lo que pasó. Claude detecta que hay que enviar un correo y activa este protocolo completo antes de redactar nada.

También se activa para los correos de seguimiento 2 al 7 del workflow de n8n.

---

## FILOSOFÍA DE REDACCIÓN

### Autores de referencia: Monje Malo + Isra Bravo (fusionados)

**De Isra Bravo:**
- Frases cortas. Nunca dos ideas en la misma frase.
- Sin adjetivos vacíos. Sin "excelente", "fantástico", "revolucionario".
- El lector es el protagonista, no TEIDROX.
- El correo no vende. El correo abre una conversación o provoca una acción concreta.
- Posdata (P.D.) siempre. Es lo segundo que más se lee tras el asunto.

**Del Monje Malo:**
- Hablar de los problemas del cliente antes de hablar de la solución.
- Autoridad sin arrogancia. Sabemos lo que hacemos, no necesitamos demostrarlo.
- Generar curiosidad, no dar todo en el primer correo.
- El silencio del cliente no es un no. Es una invitación a seguir.

**Fusión TEIDROX:**
- Tono: directo, humano, sin jerga técnica innecesaria.
- Respeto real al cliente: no se presiona, se ofrece valor.
- Nunca parecer desesperado. Nunca pedir el sí.
- Siempre hay un solo CTA (llamada a la acción) por correo. Solo uno.
- Firma siempre como: **Equipo TEIDROX** + teléfono de contacto.

---

## VARIABLES DINÁMICAS (las rellena n8n desde el CRM)

| Variable | Descripción |
|---|---|
| `{{nombre_contacto}}` | Nombre de la persona con quien se habló |
| `{{cargo_contacto}}` | Cargo (director/a, gerente, responsable...) |
| `{{nombre_establecimiento}}` | Nombre del centro / restaurante / hotel |
| `{{tipo_cliente}}` | residencia / restaurante / hotel / otro |
| `{{producto_interes}}` | Lo que se presentó (osmosis, ozone, fuente...) |
| `{{fecha_visita}}` | Fecha en que se hizo la visita |
| `{{problema_detectado}}` | El pain point específico detectado en visita |
| `{{telefono_teidrox}}` | Teléfono de contacto TEIDROX (pendiente confirmar) |
| `{{numero_seguimiento}}` | Número de correo en la secuencia (1-7) |

---

## ESTRUCTURA OBLIGATORIA DE CADA CORREO

```
ASUNTO: [ver reglas de asunto abajo]

Cuerpo línea 1: Frase de apertura. Sin saludo genérico.
Cuerpo: 3-5 párrafos cortos. Máximo 3 frases por párrafo.
CTA: Una sola acción concreta al final.
Firma: Equipo TEIDROX | {{telefono_teidrox}}
P.D.: Siempre. Una línea. Refuerza el CTA o añade un dato inesperado.
```

---

## REGLAS DE ASUNTO

- Nunca más de 7 palabras.
- Nunca usar: "Seguimiento", "Presupuesto", "Oferta", "Propuesta".
- Generar curiosidad o apelar al problema del cliente.
- Ejemplos válidos:
  - "Lo que vimos en {{nombre_establecimiento}}"
  - "Una pregunta sobre el agua de su cocina"
  - "{{nombre_contacto}}, ¿sigue pensando en ello?"
  - "Esto no suele contarse en las visitas"

---

## VARIANTES POR TIPO DE CLIENTE

### 🏥 RESIDENCIA DE ANCIANOS

**Contexto del cliente:** La directora/responsable tiene presión constante: inspecciones, normativa sanitaria, calidad de vida de los residentes, coste operativo. Toma decisiones con criterio técnico y emocional a la vez.

**Ángulo principal:** Seguridad sanitaria y calidad del agua para personas vulnerables. Cumplimiento normativo. Reducción de incidencias.

**Tono:** Profesional, cercano, con sensibilidad. No técnico en exceso. Hablar de personas, no de máquinas.

**Palabras que funcionan:** tranquilidad, seguridad, residentes, bienestar, inspección, normativa, sin complicaciones.

**Palabras a evitar:** revolucionario, innovador, oportunidad, oferta, precio.

---

### 🍽️ RESTAURANTE

**Contexto del cliente:** El responsable piensa en producto, en cliente final, en costes y en tiempo. El agua afecta al sabor, a los electrodomésticos y a la imagen del local.

**Ángulo principal:** Calidad del producto final (café, cocina, hielo). Ahorro en costes de mantenimiento. Diferenciación frente a la competencia.

**Tono:** Directo, rápido, sin rodeos. Valora su tiempo. Hablar de resultados visibles.

**Palabras que funcionan:** sabor, cliente, diferencia, mantenimiento, limpieza, coste.

**Palabras a evitar:** sinergia, solución integral, propuesta de valor, paradigma.

---

### 🏨 HOTEL

**Contexto del cliente:** Imagen de marca, experiencia del huésped, reputación online. El agua aparece en habitaciones, spa, restaurante, cocina. Decisión más lenta y con más interlocutores.

**Ángulo principal:** Experiencia del huésped. Sostenibilidad (eliminar plásticos). Imagen de marca premium.

**Tono:** Aspiracional pero sin exceso. Hablar de lo que el huésped percibe. Mencionar sostenibilidad si el hotel tiene ese perfil.

**Palabras que funcionan:** huésped, experiencia, reputación, sostenible, sin plástico, detalle.

**Palabras a evitar:** presupuesto, precio, coste (en primeros correos), barato.

---

## SECUENCIA DE 7 CORREOS — ESTRUCTURA GENERAL

| Correo | Cuándo | Objetivo | Tono |
|---|---|---|---|
| 1 | Tras la visita (mismo día o siguiente) | Abrir relación. Recordar la visita. Un solo punto de valor. | Cálido, personalizado |
| 2 | +4 días sin respuesta | Dar un dato concreto relevante para su sector | Informativo, sin presión |
| 3 | +4 días sin respuesta | Apelar al problema detectado en visita | Más directo |
| 4 | +4 días sin respuesta | Caso de éxito o resultado concreto similar | Prueba social |
| 5 | +4 días sin respuesta | El coste de no actuar (sin dramatizar) | Reflexivo |
| 6 | +4 días sin respuesta | Pregunta directa y sencilla | Muy corto, directo |
| 7 | +4 días sin respuesta | Cierre de secuencia. Dejar la puerta abierta. | Elegante, sin reproches |

---

## PROTOCOLO ANTES DE REDACTAR EL CORREO 1

Claude debe extraer de la información que Benicio vomita:

1. **Nombre y cargo** de la persona con quien habló
2. **Nombre del establecimiento**
3. **Tipo de cliente** (residencia / restaurante / hotel)
4. **Qué producto o servicio se presentó**
5. **Problema o necesidad detectada** (el pain point real)
6. **Temperatura del lead** (frío / tibio / caliente)
7. **Siguiente paso acordado** si lo hubo
8. **Algo personal o anecdótico** de la conversación (para personalizar)

Si falta alguno de estos datos, Claude pregunta antes de redactar.

---

## PROTOCOLO CONFIRMACIÓN ANTES DE ENVÍO AUTOMÁTICO (correos 2-7)

Antes de que n8n envíe cada correo automático, el workflow manda mensaje al bot de Telegram:

> "📧 Voy a enviar el correo nº {{numero_seguimiento}} a {{nombre_contacto}} de {{nombre_establecimiento}} en {{tiempo}} horas. ¿Tienes noticias de ellos? Responde: ENVIAR / PAUSAR / CERRAR"

- **ENVIAR** → n8n envía el correo y actualiza el CRM
- **PAUSAR** → n8n espera otros 4 días sin enviar
- **CERRAR** → n8n marca el lead como cerrado en el CRM y para la secuencia

---

## CAMPOS CRM QUE SE ACTUALIZAN EN CADA CORREO

- `fecha_ultimo_contacto`
- `numero_correo_enviado`
- `estado_lead` (activo / pausado / cerrado / respondido)
- `notas_seguimiento`

---

## EJEMPLO — CORREO 1 PARA RESIDENCIA

**Asunto:** Lo que vimos en {{nombre_establecimiento}}

---

{{nombre_contacto}},

El agua que consumen sus residentes cada día importa más de lo que parece en una primera visita.

No lo digo por hablar. Lo digo porque en centros como {{nombre_establecimiento}}, la calidad del agua afecta directamente al bienestar de personas que no pueden elegir lo que beben.

Lo que vimos ayer tiene solución. Y no es tan complicado como parece.

¿Tiene diez minutos esta semana para que le explique exactamente cómo lo resolvemos en centros similares al suyo?

Equipo TEIDROX  
{{telefono_teidrox}}

P.D. No le voy a pedir que decida nada todavía. Solo quiero que vea los números reales.

---

*Este es un ejemplo base. El correo real se redacta con la información específica de cada visita.*
