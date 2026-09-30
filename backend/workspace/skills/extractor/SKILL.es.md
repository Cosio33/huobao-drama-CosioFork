---
name: extractor
description: Reglas y métodos para extraer personajes, escenas y props
---

# Guía de Extracción de Personajes, Escenas y Props

## Reglas de extracción de personajes

Campos de personaje extraídos (correspondencia uno a uno con los parámetros de la herramienta `save_dedup_characters`):
- **name** (obligatorio): nombre completo del personaje
- **role**: posicionamiento del personaje — protagonista / secundario / extra
- **appearance**: descripción de apariencia (300-500 caracteres) — género, impresión de edad, rasgos faciales, físico, porte. **No generes los rasgos de personalidad del personaje por separado; conviértelos en porte y expresión externos integrados en la descripción de apariencia** (p. ej. "personalidad fría" debe escribirse como "mirada fría, expresión contenida, casi nunca sonríe")
- **styling**: estilismo — peinado, ropa, maquillaje, accesorios, etc.
- **description**: trasfondo y relaciones del personaje (complemento opcional)

## Reglas de extracción de escenas

Campos de escena extraídos (correspondencia uno a uno con los parámetros de la herramienta `save_dedup_scenes`):
- **location** (obligatorio): nombre del lugar específico
- **time**: franja temporal (p. ej. de día / atardecer / madrugada); la misma ubicación en una franja temporal distinta cuenta como escena nueva
- **prompt**: descripción de la escena — espacio, ambientación, textura de época, elementos visuales clave (fondo puro, sin personas)
- **lighting**: iluminación de la escena — fuentes de luz, tono de color, contraste de brillo, atmósfera

## Reglas de extracción de props

**Principio central: prefiere extraer de menos antes que de más.** Los props son assets de alto coste que se usan para generar fotos de producto sobre fondo blanco y son referenciados por primeros planos de video; solo los props críticos para la trama merecen extraerse. Un episodio suele tener **0-3** props clave; si hay más de 3, ordénalos por importancia argumental y conserva solo los 3 primeros.

Deben cumplirse AMBAS condiciones siguientes — ninguna es opcional:
1. **Impulsa directamente la trama**: la aparición, entrega, daño o descubrimiento del objeto desencadena un giro argumental (p. ej. un arma homicida, una prenda simbólica, un documento clave, un regalo romántico, una prueba decisiva).
2. **Merece generar una imagen dedicada**: los storyboards posteriores le darán primeros planos o reaparece, por lo que necesita una apariencia fija.

**Tres preguntas de autoverificación** (formúlalas y respóndelas para cada prop candidato; si alguna respuesta es "no", descártalo):
- (1) ¿La trama sigue en pie si se elimina? → Si es así, **no lo extraigas** (es solo un prop decorativo)
- (2) ¿Es solo un objeto cotidiano que el personaje usa casualmente (teléfono, palillos, vaso de agua, cigarrillos, paraguas)? → Si es así, **no lo extraigas**
- (3) ¿Forma parte de la ambientación del decorado (mesas y sillas, lámparas, puertas y ventanas, adornos de pared, vajilla)? → Si es así, **no lo extraigas** (eso pertenece a la descripción de la escena)

**Típicos no-props**: objetos ordinarios usados casualmente sin efecto en el rumbo de la trama; ambientación y mobiliario; objetos mencionados una sola vez y nunca más; la vestimenta habitual de un personaje (pertenece al estilismo del personaje).

Si ningún prop cumple los requisitos, **no fuerces una extracción** — simplemente pasa un array vacío al llamar a `save_dedup_props`.

Campos de prop extraídos (correspondencia uno a uno con los parámetros de la herramienta `save_dedup_props`):
- **name** (obligatorio): nombre del prop
- **type**: categoría — cotidiano / arma / transporte / decoración / documento, etc.
- **description**: únicamente la apariencia física del objeto (material, color, forma, tamaño, grado de desgaste, signos de daño, etc.); no describas su función argumental ni su relación con los personajes ni nada más

Los props **no necesitan un prompt de imagen** — el prompt final de un prop lo genera el Agente de generación de prompts antes de la generación de imágenes (especificación de foto de producto sobre fondo blanco).

## Pasos

1. Llama a `read_script_for_extraction` para leer el guion del episodio actual
2. Llama a `read_existing_characters` para ver los personajes existentes del proyecto y los personajes ya vinculados al episodio actual
3. Llama a `read_existing_scenes` para ver las escenas existentes del proyecto y las escenas ya vinculadas al episodio actual
4. Llama a `read_existing_props` para ver los props existentes del proyecto y los props ya vinculados al episodio actual
5. Extrae únicamente los personajes, escenas y props realmente involucrados en el episodio actual
6. Llama a `save_dedup_characters` para guardar los personajes y vincularlos automáticamente al episodio actual
7. Llama a `save_dedup_scenes` para guardar las escenas y vincularlas automáticamente al episodio actual
8. Llama a `save_dedup_props` para guardar los props y vincularlos automáticamente al episodio actual

## Reglas del episodio actual

- El objetivo es completar los personajes, escenas y props que necesita el "episodio actual", no volver a escanear todo el proyecto
- Si un asset ya existe en el proyecto pero aún no está vinculado al episodio actual, reutilízalo y vincúlalo al episodio actual
- Reglas de desduplicación: los personajes/props se cotejan exactamente por nombre, las escenas se cotejan exactamente por [ubicación + franja temporal]; ante una coincidencia, prefiere reutilizar — no crees duplicados
- Desduplicación de nombres similares: cuando un nombre lleva un calificador o alias entre paréntesis, compara por la parte principal anterior a los paréntesis (p. ej. "Lucía Fernández (protagonista)" y "Lucía Fernández" son el mismo personaje/prop — reutiliza el existente); el normalized_name devuelto por read_existing_characters / read_existing_props es el nombre normalizado, y normalized_location funciona igual para las escenas — úsalos para este juicio
