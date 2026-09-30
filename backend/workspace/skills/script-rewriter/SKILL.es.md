---
name: script-rewriter
description: Metodología y reglas para reescribir una novela en un guion formateado
---

# Guía de Reescritura de Guion

## Principios de reescritura

1. **Preservar la trama central**: no cambies la línea argumental principal ni las relaciones entre personajes
2. **Reforzar la calidad visual**: convierte la prosa narrativa en descripciones de escenas visualizables
3. **Impulsado por el diálogo**: usa el diálogo para hacer avanzar la trama y reduce la narración
4. **Control del ritmo**: mantén cada escena en 30-60 segundos, adecuado para video corto
5. **Sin lenguaje de cámara**: nada de tamaños de plano, ángulos ni movimientos de cámara — eso pertenece al paso de desglose de storyboard

## Formato de guion formateado

```
## S01 | INT · Cafetería | Atardecer

La luz del atardecer se vierte por los ventanales hacia el interior de la cafetería; del mostrador sube el vapor de las tazas de café.

Marcos está sentado solo en un reservado de la esquina, cabizbajo sobre su teléfono, con gesto algo ansioso.

Suena la campanilla de la puerta cuando Lucía la abre de un empujón. Ve a Marcos y se acerca con una sonrisa.

Lucía: (sonriendo) ¿Llevas mucho esperando?
Marcos: (levantando la vista) No, acabo de llegar.
```

### Reglas de formato

- `## S<número> | INT/EXT · Ubicación | Franja temporal` — encabezado de escena
- Descripción de acción en párrafos naturales — sin lenguaje de cámara de ningún tipo
- `NombrePersonaje: (estado/expresión) contenido de la línea` — formato de diálogo

### Referencia de volumen de contenido

El guion formateado es aproximadamente un 20-30% más largo que el contenido original; el aumento proviene principalmente de los marcadores de encabezado de escena y del formato de diálogo, no de una escritura expansiva.

## Pasos de reescritura

1. Primero llama a `read_episode_script` para leer el contenido original
2. Analiza la estructura del contenido (las proporciones de diálogo, narración y monólogo interior)
3. Llama a `rewrite_to_screenplay` para realizar la reescritura
4. Revisa el resultado reescrito y confirma que se ajusta al formato de guion formateado
5. Llama a `save_script` para guardar el resultado final

## Notas

- El monólogo interior puede convertirse en expresiones/acciones del personaje o en voz en off
- Divide los pasajes narrativos largos en varias escenas cortas
- Asegúrate de que cada escena tenga un punto de giro emocional claro
- Mantén coherente el estilo de lenguaje de cada personaje
- Los números de escena aumentan de forma consecutiva (S01, S02, S03...)
- Las franjas temporales deben ser específicas (atardecer, madrugada, primera hora de la mañana) — no escribas un vago "de día"
