---
name: video-prompt
description: Especificación del prompt de video — genera un prompt de generación de video segmentado en el tiempo a partir del contenido de un segmento de storyboard, con cortes permitidos dentro del segmento
---

# Prompt de video (segmento de storyboard → video_prompt)

A partir de la descripción de un único segmento de storyboard (que contiene la estructura de subplanos 【镜头N】 y el diálogo/narración) / atmósfera / duración, genera el `video_prompt` que impulsa la generación de video con IA. **Un segmento de storyboard = un video de 8-15 segundos, con cortes permitidos en su interior**: los segmentos consecutivos pueden ser planos distintos (cambio de tamaño de plano/ángulo/sujeto), unidos con cortes secos; pero el segmento completo **nunca cruza escenas** y nunca usa flashbacks.

## Formato

La **primera línea del `video_prompt` es la cabecera**: presenta primero qué personajes y qué escena aparecen en este video, y luego continúa con los segmentos temporales. Los personajes y las escenas siempre se referencian con @ (durante la generación se reemplazan por los marcadores de imagen de referencia correspondientes, para que el modelo de video fije primero "quién" y "dónde").

```
Personajes: @Marcos, @Lucía; Escena: @Cafetería.
0-3s: @Cafetería, plano cercano, cámara estática; @Marcos mira su teléfono cabizbajo, los dedos tamborilean repetidamente sobre la mesa, expresión ansiosa.
3-6s: Corte a un plano general de la entrada; suena la campanilla cuando @Lucía abre la puerta de un empujón y entra, trayendo consigo una ráfaga de aire frío.
6-9s: Corte de vuelta a un plano medio; @Lucía se acerca con una sonrisa y se sienta frente a Marcos; Marcos dice: "Por fin llegaste."
```

Reglas de la cabecera:
- Lista únicamente los personajes que realmente aparecen en este segmento de storyboard y la escena vinculada — no incluyas los que no aparecen
- Cuando un prop tiene una aparición destacada, puede añadirse a la cabecera (p. ej. `; Props: @Carta`)
- La cabecera es su propia línea, termina con un punto, y después van los segmentos temporales

Divídelo en segmentos de 3 segundos, cada segmento en su propia línea separada por saltos de línea, con rangos de tiempo continuos y contiguos (sin solapamientos, sin huecos).

## Mapeo con la descripción del storyboard

La `description` es la única fuente de contenido del video_prompt (lo visual, las acciones, el diálogo y la narración están todos en ella). Reglas de conversión:

- Cada `【镜头N】` de la `description` se mapea a **1-2 segmentos consecutivos de 3 segundos** — mismo orden, sin omisiones, sin fusiones, sin subplanos nuevos
- El diálogo/narración se extrae de las entradas "NombrePersonaje dice: "..."" / "Narración: ..." dentro del `【镜头N】` correspondiente y se asigna a los segmentos mapeados de ese subplano; **no inventes diálogo nuevo fuera de la descripción**
- Las acciones visuales siguen la `description`; `atmosphere` solo se usa para complementar las descripciones de iluminación, tono de color y estado de ánimo de cada segmento

## Estructura dentro del segmento

Organiza el contenido de cada segmento en este orden (los elementos sin contenido pueden omitirse, pero la acción/lo visual es obligatorio):

**Rango de tiempo + @referencia de escena + tamaño de plano/movimiento de cámara + @referencia de personaje + acción principal·expresión + diálogo/narración + estado de ánimo e iluminación**

- **El primer segmento debe establecer el espacio**: escena + posición de cámara + posición y estado de cada personaje, para que el público sepa de un vistazo dónde estamos y a quién mirar
- **Cortes**: inicia un segmento posterior a un corte con una palabra de transición como "corte a / corte de vuelta", y reafirma el tamaño de plano y el sujeto; los puntos de corte deben alinearse con la estructura `【镜头N】` de la `description` del storyboard
- **Tamaño de plano/movimiento de cámara**: un estado de cámara por segmento (plano cercano / plano medio / plano general / primer plano; estático / acercamiento / alejamiento / panorámica / travelling); el movimiento de cámara es continuo dentro de un solo subplano y puede cambiar tras un corte
- **Acción**: una acción principal por segmento, con verbos concretos y visibles (caminar, girarse, levantar la vista, apretar, hacer una pausa)
- **Toda emoción debe convertirse en descripción visible**: nada de palabras abstractas como "está muy triste / el ambiente es tenso" — escríbelo como "baja la cabeza, los dedos se aferran al borde de la taza, la respiración se vuelve más pesada"
- **Diálogo/narración**: escribe "NombrePersonaje dice: "línea""; la narración como "Narración: contenido"; una línea larga que no pueda decirse en 3 segundos se reparte entre varios segmentos; un segmento sin diálogo puede anotar sonidos ambientales/de acción (p. ej. "las máquinas siguen rugiendo")

## Reglas de referencia

- `@NombreEscena` — referencia de escena; el nombre debe coincidir exactamente con la ubicación de la lista de escenas
- `@NombrePersonaje` — referencia de personaje; el nombre debe coincidir exactamente con el nombre de la lista de personajes
- `@NombreProp` — referencia de prop; el nombre debe coincidir exactamente con el nombre de la lista de props; referencia un prop cuando sea claramente visible en cuadro, se use o se muestre en primer plano
- Durante la generación, cada `@nombre` se reemplaza automáticamente por el marcador de imagen de referencia correspondiente (p. ej. `@Marcos` → `@Imagen1Marcos`), por lo que los nombres deben coincidir exactamente — no los abrevies ni añadas símbolos extra
- **Cada segmento debe tener al menos una referencia @ anclando el encuadre**; cualquier segmento en el que aparezca un personaje debe @referenciar a ese personaje; solo referencia escenas/personajes/props ya vinculados a este segmento de storyboard

## Reglas de la línea temporal

- Número de segmentos = duración del segmento de storyboard ÷ 3 segundos (redondeado hacia arriba); los rangos de tiempo de los segmentos deben sumar exactamente la duración total del segmento
- Ritmo del contenido: el primer segmento establece → los segmentos intermedios hacen avanzar la acción/el conflicto → el segmento final aterriza en el resultado o en el beat emocional

## Prohibiciones

- Cambios entre escenas, flashbacks (un segmento transcurre en una única escena)
- Referenciar nombres de escenas/personajes fuera de las listas
- Descripción psicológica abstracta, metáfora literaria (el modelo solo reconoce imágenes visibles)
- Lenguaje que no coincida con la directiva de idioma de la sesión

## Guardado

Llama a `update_storyboard` para actualizar únicamente el campo `video_prompt` de este segmento de storyboard; no modifiques ningún otro campo ni rehagas el desglose de todo el episodio.
