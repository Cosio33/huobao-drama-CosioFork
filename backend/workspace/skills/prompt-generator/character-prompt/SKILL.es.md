---
name: character-prompt
description: Especificación del prompt final de personaje — primer plano frontal de rostro + hoja de tres vistas (frente / perfil a 90 grados / espalda), que sirve como ancla de apariencia para toda la generación posterior
---

# Prompt final de personaje (izquierda: primer plano frontal de rostro + derecha: hoja de tres vistas)

Lo que se genera es una hoja de referencia de personaje con una composición fija:

- **Izquierda: primer plano frontal de rostro** — un primer plano frontal de cabeza y hombros, con rasgos faciales, peinado y textura de piel claramente visibles, que sirve como ancla de reconocibilidad facial
- **Derecha: tres vistas de cuerpo completo de igual altura, lado a lado — frente, perfil a 90 grados y espalda** — tres vistas de cuerpo completo del mismo personaje a igual altura, con las coronillas y las plantas de los pies alineadas

**Principio central: coherencia > belleza.** Esta imagen es el ancla de apariencia de todas las imágenes de personaje y referencias de video posteriores; debe ser neutra, clara y reutilizable — no persigas la artisticidad de una imagen única.

## Estructura de salida (ensambla un único pasaje coherente en este orden, siguiendo la directiva de idioma de la sesión)

```
Hoja de referencia de personaje, a la izquierda un primer plano frontal de rostro, a la derecha tres vistas
de cuerpo completo de igual altura lado a lado mostrando frente, perfil a 90 grados y espalda;
el primer plano y las vistas de cuerpo completo son el mismo personaje, cuerpo completo en cuadro, pose neutra en A,
las tres vistas de cuerpo completo de igual altura, lado a lado, coronillas y plantas de los pies alineadas;
[impresión de edad + impresión de género + físico], [rasgos faciales], [peinado], [ropa + accesorios];
el rostro, el peinado y la ropa del primer plano frontal y de las tres vistas son completamente idénticos;
fondo blanco puro, iluminación suave y uniforme, calidad cinematográfica
```

## Reglas de orden de descripción

Pon **las características más reconocibles primero**, cubriendo todos los elementos clave de `appearance` (looks) y `styling` (pelo/ropa/maquillaje) en este orden, sin omisiones:

1. Anclas de identidad: impresión de edad (p. ej. "veintitantos años"), impresión de género, físico (altura y complexión, hábitos de postura)
2. Rasgos faciales: forma del rostro, ojos, otros rasgos notables (cicatrices, lunares, gafas, etc.) — el primer plano frontal depende especialmente de esta parte
3. Peinado: color, longitud, estilo
4. Ropa: estilo, color, material, estado (p. ej. "un uniforme de trabajo arrugado con marcas de soldadura en los puños")
5. Accesorios: escribe solo los reconocibles; no los amontones

Convierte los rasgos de personalidad del personaje en descripciones de porte y expresión externos (p. ej. "demacrado" → "ojos cansados, hombros ligeramente caídos"); las palabras de personalidad no deben aparecer directamente.

## Composición y coherencia

- Primer plano frontal a la izquierda: mirando directamente a cámara, expresión neutra, cabeza y hombros completamente en cuadro
- Tres vistas de cuerpo completo a la derecha: frente, perfil a 90 grados y espalda del mismo personaje, **de igual altura, lado a lado, con espaciado uniforme**, coronillas y plantas de los pies en las mismas líneas horizontales
- El primer plano y las tres vistas de cuerpo completo deben tener el mismo rostro, el mismo peinado y la misma ropa — declara explícitamente "el rostro, el peinado y la ropa del primer plano frontal y de las tres vistas de cuerpo completo son completamente idénticos"
- Postura neutra, expresión natural — fácil de reutilizar como imagen de referencia
- Iluminación de estudio suave y uniforme; nada de luces y sombras dramáticas (la imagen de referencia debe funcionar en todo tipo de escenas)
- No mezcles palabras ajenas en la salida

## Prohibiciones

- Poses dinámicas, expresiones exageradas, props en las manos, estar en cuadro con otras personas
- Recortar el cuerpo (las vistas de cuerpo completo deben ser de cuerpo completo, de la cabeza a las plantas completamente en cuadro; el primer plano debe tener cabeza y hombros completamente en cuadro)
- Texto, etiquetas, marcas de agua, firmas
- Sombras densas, iluminación de fondo de color, props en el fondo

## Guardado

Llama a `save_character_final_prompt`: el parámetro prompt no contiene palabras de estilo — **el estilo visual del proyecto es inyectado automáticamente por la herramienta al principio del prompt final**.
