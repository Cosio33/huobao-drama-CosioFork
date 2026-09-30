---
name: prop-prompt
description: Especificación del prompt final de prop — bodegón de objeto único sobre fondo blanco, punto de vista estándar de fotografía de producto: proporciones precisas, bordes completos, el fondo no porta narrativa
---

# Prompt final de prop (objeto único sobre fondo blanco · fotografía de producto estándar)

Lo que se genera es una foto de producto sobre fondo blanco: **usando un punto de vista estándar de fotografía de producto**, el encuadre contiene únicamente el prop en sí, colocado de forma aislada sobre un fondo blanco puro, **sin ningún otro elemento mezclado** — sin otros objetos, sin personas, sin entorno de escena, sin manos que lo sostengan.

Tres requisitos estrictos:
1. **Proporciones precisas de todas las partes del objeto** — sin exageración, distorsión ni estiramiento estilizado; las relaciones de tamaño relativo del prop deben ser verdaderas
2. **Bordes completos** — el prop está completamente en cuadro como un todo, con márgenes en todos los lados; ninguna parte puede quedar recortada por el borde del encuadre
3. **El fondo no porta contenido narrativo** — el fondo blanco puro es solo un soporte, sin sensación de lugar, sin insinuaciones argumentales, sin elementos decorativos

## Estructura de salida (ensambla un único pasaje coherente en este orden, siguiendo la directiva de idioma de la sesión)

```
Foto de producto de objeto único, punto de vista estándar de fotografía de producto,
[nombre del prop + material/color/forma/tamaño + grado de desgaste y detalles de daños];
proporciones precisas de todas las partes, colocado de forma aislada sobre un fondo blanco puro,
centrado y completamente en cuadro, bordes completos sin recortes;
fondo limpio y sin contenido narrativo, sin otros objetos, sin personas, sin escena;
luz de estudio suave y uniforme, sombras tenues, alto nivel de detalle
```

## Reglas de generación

- Construye en torno al `name` y la `description` (apariencia física) del prop: material, color, forma, tamaño, grado de desgaste, signos de daño y demás detalles físicos deben **trasladarse uno por uno** — son la fuente de la reconocibilidad del prop
- Punto de vista estándar de fotografía de producto: una vista 3/4 ligeramente picada (mostrando a la vez la parte superior y un lateral para máxima dimensionalidad); los props planos (papel, carnés, fotos) usan un flat lay cenital directo
- Presenta el objeto único centrado y completo, con márgenes en todos los lados, proporciones precisas, bordes completos — no recortes el cuerpo del prop
- Luz de estudio suave y uniforme, sombras tenues, alto nivel de detalle
- Describe únicamente el objeto en sí; no menciones la trama, los personajes ni el uso (ni el fondo ni el encuadre portan contenido narrativo)
- No mezcles palabras ajenas en la salida; **no** uses palabras del tipo "calidad cinematográfica" (la imagen de un prop es una foto de producto, no un fotograma de película)

## Prohibiciones

- Manos que lo sostienen, personas, otros objetos o entorno de escena en cuadro
- Embalajes, bases, soportes de exhibición (salvo que sean parte del propio prop)
- Texto, marcas de agua, firmas (el texto y los gráficos impresos en el propio prop pueden conservarse y describirse)
- Reflejos ambientales, luz de color
- Perspectiva exagerada, distorsión, errores de proporción, recorte de bordes

## Guardado

Llama a `save_prop_final_prompt`: el parámetro prompt no contiene palabras de estilo — **el estilo visual del proyecto es inyectado automáticamente por la herramienta al principio del prompt final**.
