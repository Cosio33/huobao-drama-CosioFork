---
name: scene-prompt
description: Especificación del prompt final de escena — un plano establecedor de gran angular nítido: posiciones relativas fijas de primer plano/plano medio/fondo/entradas/suelo/paredes/ambientación clave; espacialmente continuo, autoconsistente, reutilizable, sin personas
---

# Prompt final de escena (plano establecedor de gran angular · escena vacía sin personas)

Lo que se genera es una imagen de escena de **plano establecedor de gran angular nítido**: un plano vacío puro de la escena con **absolutamente ninguna persona**, que muestra por completo las **posiciones relativas fijas del primer plano, plano medio, fondo, entradas/salidas, suelo, paredes y ambientación clave** — espacialmente continua, autoconsistente y reutilizable.

Esta imagen sirve como ancla de referencia de fondo para cada plano de esta escena: tanto el público como el modelo deben poder leer de ella toda la disposición espacial — por dónde se entra y se sale, cómo son el suelo y las paredes, y dónde está fijada cada pieza clave de la ambientación. El punto de vista debe ser estable y de uso general.

## Estructura de salida (ensambla un único pasaje coherente en este orden, siguiendo la directiva de idioma de la sesión)

```
Plano general de cámara fija, un plano establecedor nítido, [ubicación + textura de época], [franja temporal];
composición en tres capas de primer plano ([elementos del primer plano]), plano medio ([espacio principal del plano medio])
y fondo ([profundidad del fondo]);
entradas/salidas ([posición y estilo de puertas/pasillos]), suelo ([material y estado del suelo]),
paredes ([material y color de las paredes]);
[ambientación clave y sus posiciones relativas fijas];
estructura espacial continua y autoconsistente;
[fuentes de luz + temperatura de color + contraste de brillo], [atmósfera];
Sin personas en la escena, escena vacía, calidad cinematográfica
```

## Reglas de estructura espacial

El espacio debe ser **legible, coherente y reutilizable**:

- **Primer plano**: elementos de encuadre/oclusión (marcos de puertas, esquinas de mesas, plantas, bordes de equipos) que crean profundidad — escribe 1-2 elementos concretos
- **Plano medio**: el espacio principal de la escena y la ambientación central (cadena de montaje, camas, mostrador)
- **Fondo**: la extensión del espacio (paredes lejanas, ventanas, pasillos, skyline de la ciudad)
- **Entradas/salidas**: la posición y el estilo de puertas, escaleras y pasillos deben ser explícitos (p. ej. "una puerta de hierro a la izquierda del encuadre") — esta es la base para disponer las entradas y salidas de personajes en planos posteriores
- **Suelo y paredes**: concreta material, color y estado (p. ej. "manchas de aceite sobre el suelo de hormigón", "enlucido de cal desconchado en las paredes")
- **Ambientación clave**: escribe 2-4 piezas centrales y sus **posiciones relativas fijas** (p. ej. "la cadena de montaje recorre la pared y termina en la barra"); las relaciones izquierda-derecha/cerca-lejos entre piezas deben ser autoconsistentes — no te limites a listar nombres de objetos

## Personas (Regla estricta · Máxima prioridad)

**No puede aparecer ninguna persona en la imagen de la escena — conserva únicamente la escena en sí.**

- El prompt no debe describir personas ni mencionar nada relacionado con personas
- Cualquier información humana que aparezca en la descripción de la escena (prompt) debe ignorarse y no escribirse en el prompt
- El prompt debe terminar con: "Sin personas en la escena, escena vacía"

Toda la ambientación, textura de época y elementos visuales clave de `prompt` (descripción de la escena) deben trasladarse; `lighting` (iluminación de la escena) debe concretarse: dirección de las fuentes de luz, temperatura de color cálida/fría, contraste de brillo (p. ej. "los tubos del techo emiten luz blanca fría, proyectando sombras duras bajo las máquinas").

## Punto de vista y atmósfera

- Un plano general estable a nivel de los ojos o ligeramente picado; nada de ángulos extremos picados/contrapicados, ojo de pez ni composición inclinada (se reutilizará repetidamente como escena fija)
- Determina la franja temporal y el esquema general de iluminación a partir de `location` + `time` (día / noche / atardecer son esquemas de iluminación completamente distintos)
- Concreta las palabras de atmósfera: "opresivo" → "aire asfixiante, luz escasa y baja"; no escribas solo palabras de emoción abstractas
- No mezcles palabras ajenas en la salida

## Prohibiciones

- Cualquier persona — **no puede aparecer ninguna persona en la imagen de la escena; conserva únicamente la escena en sí**
- Texto, texto legible en carteles, marcas de agua, firmas
- Desenfoque de movimiento, objetos en movimiento (la imagen de referencia de la escena debe ser quieta y estable)
- Limitarse a listar ambientación sin dar posiciones relativas (la estructura espacial debe ser continua y autoconsistente)

## Guardado

Llama a `save_scene_final_prompt`: el parámetro prompt no contiene palabras de estilo — **el estilo visual del proyecto es inyectado automáticamente por la herramienta al principio del prompt final**.
