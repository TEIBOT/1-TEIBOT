# SKILL — Rutas Comerciales TEIDROX

## Descripción
Skill para preparar rutas de visitas comerciales para TEIDROX. Se activa cuando el usuario pide buscar clientes potenciales en una zona geográfica — residencias, hoteles, lavanderías, restaurantes — y prepara una ruta optimizada con toda la información necesaria para la visita.

## Activación
Esta skill se activa cuando el usuario dice frases como:
- "búscame residencias en..."
- "prepárame una ruta por..."
- "qué hoteles hay en..."
- "busca lavanderías industriales en..."
- "quiero visitar clientes en..."
- o cualquier combinación de zona geográfica + tipo de cliente

## Contexto del negocio

**Empresa:** TEIDROX S.L. — Tenerife (mercado principal), Murcia y Alicante
**Productos:**
- Equipos de ozono (desinfección, eliminación olores, tratamiento agua) — puerta de entrada comercial
- Químicos Sallo para lavanderías industriales (línea propia, pedido mínimo 600€ al fabricante)

**Modelo de negocio:**
- Supernova Team Solutions gestiona stock, financiación, instalación y cobro de equipos de agua/ozono
- TEIDROX solo vende — cobra liquidación periódica de Supernova
- Químicos Sallo: TEIDROX compra, almacena y distribuye directamente

## Proceso obligatorio — Ejecutar en este orden

### PASO 1 — Identificar punto de partida y destino
Preguntar desde dónde sale y si hay algún cliente fijo en la ruta. Optimizar el recorrido para minimizar desplazamientos.

### PASO 2 — Buscar en tiempo real
Usar `places_search` y `web_search`. **NUNCA usar datos de memoria.** Verificar siempre en fuentes actuales.

### PASO 3 — Filtrar por criterios estrictos
**INCLUIR:**
- Privados independientes (sin central de compras)
- Concertados
- Religiosos (iglesia, congregaciones — tienen poder de decisión propio)

**EXCLUIR:**
- Cadenas nacionales con central de compras (Ballesol, DomusVi, AMMA/Amavir, Orpea, Vitalia...)
- Centros 100% públicos gestionados por administración
- Centros online sin recepción física

### PASO 4 — Datos obligatorios por cada centro
Para CADA centro encontrado obtener:
- Nombre completo y dirección exacta
- Teléfono de contacto
- **Número de plazas** (residencias) o **número de habitaciones** (hoteles) — verificado
- Si es público, privado, concertado o religioso
- **Nombre del director/a** si está disponible en fuentes públicas o reseñas
- Valoración en Google y número de reseñas
- Si hay información sobre lavandería propia o externalizada

### PASO 5 — Ordenar geográficamente
Optimizar el recorrido desde el punto de salida hasta el de llegada. Sin retrocesos innecesarios.

### PASO 6 — Presentar mapa interactivo
Usar `places_map_display_v0` con modo itinerario. Incluir en las notas de cada parada:
- Teléfono, plazas/habitaciones, director/a
- Alerta comercial (oportunidad o advertencia)

### PASO 7 — Dar enlace Google Maps
Siempre generar el enlace `https://www.google.com/maps/dir/parada1/parada2/...` para copiar y navegar desde el móvil.

### PASO 8 — Registrar en CRM Google Sheet
Añadir todos los centros encontrados al Google Sheet CRM_Rutas con estado "Pendiente visita". No duplicar centros ya visitados.

## Tipos de cliente y argumento comercial

| TIPO | CRITERIO DE INTERÉS | ARGUMENTO ENTRADA | SALLO SI... |
|---|---|---|---|
| Residencias geriátricas | +30 plazas, privada/religiosa | Ozono elimina olores y desinfecta — certificación sanitaria | Lavandería propia con volumen |
| Hoteles | Independiente sin central compras | Ozono habitaciones y zonas comunes | Lavandería propia |
| Lavanderías industriales | Cualquier tamaño | Químicos Sallo directamente | Siempre |
| Restaurantes | 3+ locales mismo dueño | Ozono cocina y superficies | No aplica |

## Speech de entrada (versión corta — recepción)

> "Buenos días, me llamo [nombre], de TEIDROX. Venimos trabajando con residencias y hoteles en [zona] en sistemas de desinfección y tratamiento de agua con tecnología de ozono. ¿Podría hablar un momento con el director o la directora? No le robo más de cinco minutos."

**Gancho especial para residencias:**
> "En residencias es especialmente relevante porque el ozono elimina olores de forma permanente y tiene efecto bactericida y virucida certificado — algo que las familias de los residentes valoran mucho."

**Si preguntan el precio:**
> "Depende del tamaño del centro y las zonas a tratar. Por eso prefiero primero hacerle la demostración — así le presento una propuesta ajustada a sus necesidades reales."

**Si no tienen tiempo hoy:**
> "Sin problema, no vengo a cerrar nada hoy. Solo a presentarme. ¿Cuándo sería buen momento para volver?"

## Tabla de seguimiento de visitas

Al final del día, cuando el usuario lo pida, generar una tabla con:

| # | CENTRO | CONTACTO | PLAZAS | RESULTADO | PRÓXIMA ACCIÓN | PRIORIDAD |

Guardar siempre en Google Sheet CRM_Rutas con:
- Nombre del centro
- Dirección
- Teléfono
- Tipo (residencia/hotel/lavandería/restaurante)
- Plazas o habitaciones
- Director/a
- Fecha de visita
- Resultado (Interesado / No interesado / Volver / Descartado)
- Próxima acción
- Fecha próxima acción

## Códigos de color Google Calendar para actividades

| TIPO | COLOR | CUÁNDO USARLO |
|---|---|---|
| Demo | 🔴 Tomate | Demostración de producto programada |
| Visita / Reunión presencial | 🔵 Arándano | Reunión presencial. SIEMPRE incluir dirección. |
| Llamada | 🟡 Banana | Llamada de seguimiento o prospección |
| Enviar correo | ⬛ Grafito | Tarea de envío de documentación |

## Reglas críticas

1. **NUNCA inventar datos** — si no se encuentra información, decirlo explícitamente
2. **SIEMPRE verificar en tiempo real** — plazas, directores, teléfonos
3. **NUNCA proponer cadenas** con central de compras aunque el usuario no lo especifique
4. **SIEMPRE incluir plazas, tipo y director/a** en la tabla — son datos obligatorios
5. **Una sola pregunta por turno** si necesitas aclaración antes de buscar
6. **Trabajar de forma autónoma** usando los conectores disponibles
7. **No repetir visitas** — consultar el Sheet CRM_Rutas antes de presentar resultados

## Notas adicionales

- Sallo solo tiene sentido si el cliente tiene lavandería propia y volumen suficiente para justificar pedido mínimo de 600€
- En Canarias los envíos desde Península son exportación (sin IVA, pagan IGIC). El flete repercute 8-15% del ticket
- Para lavanderías industriales en Tenerife: mejor stock local y reparto propio que envío directo al cliente
- Los centros religiosos (Hermanitas, Salesianos, Carmelitas...) tienen poder de decisión propio — son buenos clientes
- Preguntar siempre por el responsable de mantenimiento además del director — es el aliado interno más valioso
