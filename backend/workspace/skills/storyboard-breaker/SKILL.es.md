---
name: storyboard-breaker
description: Reglas profesionales de desglose de storyboard — dividir un guion en segmentos de storyboard que contienen varios subplanos cada uno
---

# Guía de Desglose de Storyboard

## Definición central: Segmento de storyboard

Un storyboard = un **segmento de storyboard** = una tarea de generación de video.

- Cada segmento dura **8-15 segundos** y contiene internamente **2-4 subplanos**
- Los cortes entre subplanos **están permitidos**: cambio de tamaño de plano, ángulo o sujeto, unidos con cortes secos
- Los subplanos **nunca cruzan escenas**: un segmento transcurre en una única escena (`scene_id` es una vinculación a nivel de segmento)
- Cada subplano dura 2-6 segundos, centrado en una unidad visual (una acción, una reacción, un primer plano)

## Proceso de desglose (cuatro pasos)

1. Llama a `read_storyboard_context` para leer el guion, los personajes, las escenas, los props y los resúmenes de storyboards existentes
2. **Identificación de beats**: identifica primero los beats narrativos del guion — marcadores como [Apertura] [Detonante] [Clímax] [Resolución], o puntos de giro narrativos (cambios de ubicación, revelaciones de reglas, erupciones emocionales, giros). **Los límites de beat fuerzan un corte de segmento**; agrupa los subplanos dentro del mismo beat en el mismo segmento siempre que sea posible, y no disperses una cadena causal (planteamiento-evento-reacción) entre segmentos distintos
3. **Anclaje del volumen total**: duración total objetivo = número de caracteres del guion ÷ 500 caracteres/minuto; número de segmentos ≈ duración total objetivo ÷ 12 segundos, con una tolerancia de ±20%. No te excedas ni te quedes corto de forma significativa
4. **Divide los subplanos dentro de cada segmento**: corta los subplanos en los puntos de cambio de acción, de punto de vista y de sujeto; tras rellenar todos los campos de cada segmento, llama a `save_storyboards` para guardarlos de una vez

## Duraciones por nivel de ritmo

Determina la duración según la función del segmento — no uses una única medida para todos:

| Tipo de segmento | Duración | Notas |
|---|---|---|
| Segmento de transición | 8-10 s | Viajes, planos vacíos, establecimiento del entorno, transiciones |
| Segmento narrativo | 10-15 s | Avance normal de la trama, diálogo |
| Segmento de clímax | 12-15 s | Primeros planos, revelaciones de reglas, erupciones emocionales, giros; ritmo de subplanos más lento, un solo subplano puede mantenerse 4-6 s |

## Piso de duración por diálogo (regla estricta)

**Duración del segmento ≥ número total de caracteres de diálogo y narración dentro del segmento (la parte escrita en description) ÷ 4,5 caracteres/segundo + 2 segundos de margen interpretativo**

El diálogo que no quepa debe trasladarse al segmento siguiente; no está permitido apiñar diálogo imposible de interpretar en un solo segmento.

## Elementos del plano

1. **Título del plano**: resumen de 3-5 caracteres del contenido central del segmento (p. ej. "Despertar")
2. **Hora**: hora específica del día + descripción de la iluminación
3. **Ubicación**: descripción completa de la escena + disposición espacial + detalles del entorno
4. **Tamaño de plano**: el tamaño de plano dominante del segmento; para segmentos con varios tamaños escribe una combinación, p. ej. "plano medio + primer plano"
5. **Ángulo**: a nivel de los ojos / contrapicado / picado / lateral / de espaldas
6. **Movimiento de cámara**: estático / acercamiento / alejamiento / panorámica / travelling / dolly (los distintos subplanos de un segmento pueden diferir)
7. **Descripción visual** `description`: describe subplano a subplano como `【镜头1】…【镜头2】…` lo que el público realmente ve y oye — lo visual (quién + acción concreta + detalles de lenguaje corporal + expresión) va primero; cuando un subplano tiene diálogo, escríbelo dentro del `【镜头N】` correspondiente como "NombrePersonaje dice: "línea"", y la narración como "Narración: contenido"
8. **Resultado visual** `result`: la consecuencia inmediata al final del segmento + detalles visuales
9. **Atmósfera** `atmosphere`: iluminación + tono de color + sonido + estado de ánimo general
10. **Duración** `duration`: duración total del segmento 8-15 segundos, y debe satisfacer el piso de duración por diálogo
11. **Vinculación de escena**: si puede corresponderse con una escena existente, debe rellenarse `scene_id`
12. **Vinculación de personajes**: rellena `character_ids`, vinculando de 0 a varios personajes involucrados en este segmento
13. **Vinculación de props**: rellena `prop_ids`, vinculando de 0 a varios props clave que aparecen en este segmento

## Reglas de vinculación de escenas

- Prefiere las `scenes` devueltas por `read_storyboard_context`
- Cuando `location + time` pueda cotejarse claramente, debe rellenarse el `scene_id` correcto
- No fabriques IDs de escena inexistentes
- Si el contenido del guion cae claramente dentro de una escena existente, no crees una descripción de escena nueva duplicada

## Reglas de vinculación de personajes

- `character_ids` debe elegirse de la lista de personajes devuelta por `read_storyboard_context`
- Un segmento puede no tener personajes, o puede vincular varios
- Cualquier personaje con una aparición clara en el segmento — visto, actuando o hablando — debe vincularse
- Los segmentos de entorno puro, planos vacíos y primeros planos de objetos pueden pasar un array vacío

## Reglas de vinculación de props

- `prop_ids` debe elegirse de la lista de props (`props`) devuelta por `read_storyboard_context`
- Cuando un prop es usado por un personaje, entregado, mostrado en primer plano, o claramente visible en cuadro y con significado narrativo, debe vincularse a ese segmento
- Los segmentos de primer plano de prop (sin personajes) también deben vincular el prop; `character_ids` puede estar vacío
- No vincules elementos de fondo ni ambientación irrelevantes para la trama; los segmentos sin props pasan un array vacío
- Los props vinculados sirven como imágenes de referencia para la generación de video (fotos de producto sobre fondo blanco), manteniendo la apariencia del prop coherente entre segmentos

## Requisitos de calidad

- `description` debe ser legible para humanos, describiendo subplano a subplano lo que el público realmente ve y oye; el diálogo/narración se escribe directamente dentro del `【镜头N】` correspondiente
- `image_prompt` debe destacar la composición del fotograma único, la apariencia del personaje, el entorno y la iluminación (correspondiente al primer subplano del segmento)
- `bgm_prompt` y `sound_effect` pueden ser frases concisas, pero no deben ser tan vagos como solo "tenso" o "triste"
- Para hacer ajustes, llama a `update_storyboard` para modificar el segmento concreto
