# Guía de Prompts para Google Flow (Veo) — Producción de Microdramas en Español

Esta guía te permite usar **Google Flow** (la herramienta de video con IA de Google, basada en Veo) como motor de generación de video para los proyectos creados con Huobao Drama. Sigue la misma filosofía del pipeline interno: **un segmento de storyboard = un clip de video**, con personajes, escenas y props coherentes entre planos.

---

## 1. Cómo encaja Google Flow en el flujo de trabajo

| Paso en Huobao Drama | Qué llevas a Google Flow |
|---|---|
| Guion formateado | Lo usas como fuente de diálogos y acciones |
| Assets (personajes/escenas/props) | Las imágenes de referencia generadas se suben a Flow como **"ingredientes"** (Ingredients to Video) |
| Storyboard (segmentos 8-15 s) | Cada segmento = 1 o 2 clips de Flow (los clips de Veo duran ~8 segundos) |
| Prompt de video (video_prompt) | Se adapta al formato de prompt de Flow (ver plantillas abajo) |
| Fusión y exportación | Descargas los clips de Flow y los unes en el editor de Flow ("Scenebuilder") o en tu editor de video |

**Dos modos de trabajo en Flow:**
- **Text to Video**: solo texto. Rápido para explorar, pero la coherencia de personajes entre clips es baja.
- **Ingredients to Video** (recomendado): subes las imágenes de referencia de tus personajes y escenas (las que genera Huobao Drama) y Veo mantiene su apariencia. Es el equivalente al sistema de referencias `@Personaje` / `@Escena` del pipeline interno.

---

## 2. Anatomía de un buen prompt para Veo 3

Un prompt efectivo sigue este orden:

```
[Sujeto] + [Acción concreta] + [Escena/Contexto] + [Cámara] + [Iluminación y atmósfera] + [Estilo visual] + [Audio/Diálogo]
```

Reglas de oro:
1. **Una acción principal por clip.** Veo rinde mejor con una acción clara que con tres cosas pasando a la vez.
2. **Sé literal y visible.** Nada de "se siente triste" → escribe "baja la cabeza, aprieta el borde de la taza, la respiración se acelera".
3. **Describe la cámara explícitamente**: plano general / plano medio / primer plano / cámara estática / travelling / zoom lento.
4. **Para el diálogo**, usa el formato: `El hombre dice: "línea de diálogo"`. Veo 3 genera el audio y la sincronización labial.
5. **Especifica el audio ambiental** si lo quieres: "sonido de lluvia contra la ventana, tráfico lejano".
6. **Evita**: flashbacks, cambios de escena dentro de un clip, texto en pantalla, descripciones psicológicas abstractas.
7. Escribe el prompt **en español o en inglés** — Veo 3 entiende ambos; el inglés suele dar resultados ligeramente más estables, pero el español funciona bien si es claro y directo.

---

## 3. Plantillas listas para copiar

### 3.1 Plano establecedor (escena, sin diálogo)

```
Plano general de cámara fija, establecedor: [descripción de la escena], [franja temporal].
[Elemento de primer plano] en primer término, [espacio principal] en el plano medio, [fondo] al fondo.
Iluminación: [fuentes de luz, temperatura de color, contraste]. Atmósfera: [sensación concreta].
Sin personas. Calidad cinematográfica, imagen nítida.
Audio: [sonido ambiente].
```

**Ejemplo:**
```
Plano general de cámara fija, establecedor: una cafetería de barrio al atardecer.
El marco de una ventana empañada en primer término, la barra con tazas humeantes en el plano medio,
una puerta de hierro a la izquierda del encuadre al fondo.
Iluminación: luz cálida y baja del atardecer entrando por los ventanales, sombras suaves.
Atmósfera: aire quieto, sensación de espera. Sin personas. Calidad cinematográfica.
Audio: murmullo lejano de la calle, el tintineo de una campanilla.
```

### 3.2 Plano de personaje con acción

```
[Tamaño de plano], [movimiento de cámara]: @[Personaje] [acción concreta con detalles corporales y expresión].
Escena: @[Escena]. Iluminación: [descripción]. Atmósfera: [estado de ánimo visible].
Audio: [sonidos de la acción / ambiente].
```

**Ejemplo:**
```
Plano cercano, cámara estática: Marcos mira su teléfono cabizbajo, los dedos tamborilean
repetidamente sobre la mesa, expresión ansiosa, se muerde el labio.
Escena: cafetería de barrio al atardecer. Iluminación: luz cálida lateral, fondo desenfocado.
Atmósfera: tensión contenida.
Audio: el golpeteo de los dedos sobre la madera, murmullo de la calle.
```

### 3.3 Plano con diálogo (Veo 3 genera voz y sincronización labial)

```
[Tamaño de plano]: @[Personaje] [acción/expresión] y dice: "[línea de diálogo, máximo ~3 segundos]".
Escena: @[Escena]. [Cámara]. [Iluminación].
Audio: voces claras en primer plano, [ambiente de fondo].
```

**Ejemplo:**
```
Plano medio a contraluz suave: Lucía abre la puerta de un empujón, sonríe al ver a Marcos
y dice: "¿Llevas mucho esperando?"
Escena: entrada de la cafetería al atardecer. Cámara estática.
Iluminación: luz dorada de la calle entrando tras ella.
Audio: la campanilla de la puerta, voz clara en primer plano.
```

### 3.4 Corte entre subplanos dentro de un segmento (para clips de 8 s con dos tiempos)

```
[Plano A: 0-4s] [descripción completa del primer plano].
Corte a [Plano B: 4-8s] [descripción completa del segundo plano, mismo lugar].
```

**Ejemplo:**
```
Plano medio: Marcos levanta la vista del teléfono y sonríe con alivio.
Corte a primer plano de las manos de Lucía dejando las llaves sobre la mesa;
Lucía dice: "Tenemos que hablar."
Audio: el golpe seco de las llaves sobre la madera.
```

---

## 4. Mantener la coherencia entre clips (lo más importante)

1. **Sube siempre los ingredientes**: la hoja de referencia del personaje y la imagen de la escena generadas en Huobao Drama. Repite los mismos ingredientes en todos los clips de la misma escena.
2. **Repite las descripciones verbatim**: copia la misma descripción de apariencia/estilismo del personaje en cada prompt ("hombre de unos treinta años, abrigo gris gastado, pelo negro corto..."). No la reescribas con sinónimos.
3. **Misma iluminación y misma hora del día** en todos los clips de una misma escena.
4. **Usa Frames to Video** para continuidad: toma el último fotograma del clip anterior como imagen inicial del siguiente.
5. **Genera varias tomas** (Flow lo permite por prompt) y quédate con la más coherente.

---

## 5. De storyboard de Huobao Drama a prompt de Flow — conversión

El `video_prompt` interno usa segmentos de 3 segundos. Para Flow:

1. Agrupa cada 2-3 segmentos de 3 s en **un clip de ~8 s** (mismo plano o con un corte interno).
2. Sustituye `@NombrePersonaje` y `@NombreEscena` por las descripciones físicas completas + los ingredientes subidos.
3. Mantén el orden: **tiempo → escena → cámara → personaje → acción → diálogo → atmósfera**.
4. Respeta las mismas prohibiciones: sin cambios de escena dentro del clip, sin flashbacks, sin inventar diálogo que no esté en el guion.

**Ejemplo de conversión:**

`video_prompt` interno:
```
Personajes: @Marcos, @Lucía; Escena: @Cafetería.
0-3s: @Cafetería, plano cercano, cámara estática; @Marcos mira su teléfono cabizbajo, expresión ansiosa.
3-6s: Corte a plano general de la entrada; suena la campanilla cuando @Lucía abre la puerta y entra.
6-9s: Corte de vuelta a plano medio; @Lucía se sienta frente a Marcos; Marcos dice: "Por fin llegaste."
```

Prompt para Flow (clip 1, ~8 s, con ingredientes de Marcos, Lucía y la cafetería):
```
Plano cercano, cámara estática: Marcos (hombre de unos treinta años, abrigo gris gastado,
pelo negro corto) mira su teléfono cabizbajo, tamborilea los dedos sobre la mesa, ansioso.
Cafetería de barrio al atardecer, luz cálida lateral.
Corte a plano general de la entrada: suena la campanilla, Lucía (mujer de unos veintiocho años,
abrigo rojo, pelo castaño recogido) abre la puerta de un empujón y entra con una ráfaga de aire frío.
Audio: campanilla, murmullo de la calle, golpeteo de dedos. Calidad cinematográfica.
```

---

## 6. Ajustes recomendados en Flow

- **Modelo**: Veo 3 (o el más reciente disponible) para clips con diálogo/audio; Veo sin audio para planos mudos si quieres ahorrar generaciones.
- **Relación de aspecto**: la misma del proyecto en Huobao Drama (9:16 para microdramas verticales, 16:9 para horizontal). Defínela al crear el clip y no la cambies a mitad de proyecto.
- **Duración**: ~8 s por clip; para segmentos de 12-15 s genera 2 clips consecutivos con Frames to Video.
- **Idioma del diálogo**: Veo 3 genera la voz en el idioma del diálogo escrito en el prompt — escribe las líneas en español para voces en español.

---

## 7. Problemas frecuentes

| Problema | Solución |
|---|---|
| El personaje cambia de cara entre clips | Sube la hoja de referencia como ingrediente y repite la descripción física exacta |
| El diálogo suena en inglés | Escribe la línea en español y añade "habla en español" al final del prompt |
| El clip ignora el final del prompt | Acorta el prompt: Veo da más peso al principio; pon lo esencial primero |
| Objetos o personas extra en el fondo | Añade "sin otras personas en cuadro" / "fondo limpio" |
| Movimiento de cámara errático | Una sola instrucción de cámara por clip ("cámara estática" es lo más seguro) |
| Moderación rechaza el contenido | Reformula acciones violentas/sensibles de forma implícita; evita mencionar personas reales |

---

*Documento en español para el fork Cosio33/huobao-drama-CosioFork. Basado en el pipeline de producción de Huobao Drama (chatfire-AI/huobao-drama) y las capacidades públicas de Google Flow / Veo 3.*
