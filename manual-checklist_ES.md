# Checklist Manual de Auditoría

> Preguntas rápidas de sí/no para ejecutar antes de cualquier herramienta automática.
> Estas apuntan a los issues más comunes encontrados consistentemente en auditorías — son un punto de partida, no el universo completo.
> Organizadas por principio WCAG 2.1 (POUR) y criterio, en el orden en que naturalmente las reviso.

---

## Perceptible

### 1.1.1 — Contenido no textual (Nivel A)
- [ ] ¿Las imágenes son decorativas o informativas? *(las decorativas deben tener `alt=""`)*
- [ ] ¿Las imágenes informativas tienen alt text?
- [ ] ¿Los iconos están usados de manera decorativa o comunican algo?
- [ ] ¿Si una imagen se comporta como botón o link, el alt describe a dónde lleva o qué hace?

### 1.3.1 — Información y relaciones (Nivel A)
- [ ] ¿Hay divitis? *(elementos `div` o `span` donde debería haber semántica)*
- [ ] ¿Se usan elementos semánticos de HTML? *(nav, main, header, footer, section, article)*
- [ ] ¿Hay más de un `h1` en la página?
- [ ] ¿Los headings están en orden descendente sin saltar niveles? *(h1 → h2 → h3…)*
- [ ] ¿Las listas están dentro de `ol` o `ul`?
- [ ] ¿Los labels de los formularios tienen suficiente información para entender qué se pide?

### 1.4.1 — Uso del color (Nivel A)
- [ ] ¿Se usa solo el color para indicar un error o campo requerido?
- [ ] ¿Los links se distinguen del texto normal por algo más que el color?
- [ ] ¿Hay información importante que desaparece si se ve en escala de grises?

### 1.4.3 — Contraste mínimo (Nivel AA)
- [ ] ¿Se nota algún texto con contraste bajo a primera vista?
- [ ] ¿El contraste bajo tiene que ver con el tamaño? *(texto normal: mínimo 4.5:1 / texto grande 18pt+: mínimo 3:1)*
- [ ] ¿El contraste bajo tiene que ver con que el texto está en negrita? *(14pt bold: mínimo 3:1)*
- [ ] ¿Hay texto sobre un degradado? *(verificar el valor de contraste en el punto más bajo)*

### 1.4.11 — Contraste de componentes no textuales (Nivel AA)
- [ ] ¿Los botones se distinguen visualmente del fondo? *(ratio mínimo: 3:1)*
- [ ] ¿Los inputs, checkboxes y radio buttons tienen borde visible con suficiente contraste?
- [ ] ¿El indicador de foco tiene contraste suficiente contra el fondo?

---

## Operable

### 2.1.1 — Teclado (Nivel A)
- [ ] ¿Puedo acceder a todos los elementos accionables con Tab? *(links, botones, inputs, selects, checkboxes, radio buttons)*
- [ ] ¿Hay algún elemento donde el foco queda atrapado y no puedo salir con teclado?
- [ ] ¿Los menús desplegables abren y cierran con teclado?
- [ ] ¿Al cerrar un menú o modal el foco regresa al elemento que lo abrió?
- [ ] ¿El foco entra al primer elemento de un menú desplegable al abrirse?

### 2.4.1 — Evitar bloques (Nivel A)
- [ ] ¿Hay un skip link visible al primer Tab que lleva al contenido principal?
- [ ] ¿El skip link funciona — o sea, realmente mueve el foco al contenido principal?

### 2.4.3 — Orden del foco (Nivel A)
- [ ] ¿El orden del Tab sigue una secuencia lógica de lectura? *(izquierda a derecha, arriba a abajo)*
- [ ] ¿Hay algún salto inesperado en el orden de foco que pueda desorientar al usuario?

### 2.4.4 — Propósito de los links (Nivel A)
- [ ] ¿El texto del link describe su propósito por sí solo?
- [ ] ¿Hay links que digan solo "click aquí", "leer más" o "ver"?
- [ ] ¿El link deja claro qué va a pasar al activarlo?

### 2.4.7 — Foco visible (Nivel AA)
- [ ] ¿El indicador de foco es visible en todos los elementos interactivos?
- [ ] ¿Hay algún elemento donde el foco desaparece o es casi imperceptible?

---

## Comprensible

### 3.1.1 — Idioma de la página (Nivel A)
- [ ] ¿El atributo `lang` en el `<html>` corresponde al idioma principal de la página?
- [ ] ¿Si hay secciones en otro idioma, están marcadas con `lang` en ese elemento?

### 3.3.1 — Identificación de errores (Nivel A)
- [ ] ¿Los errores del formulario se muestran como texto — no solo con color o ícono?
- [ ] ¿El mensaje de error identifica qué campo falló?
- [ ] ¿El error se puede leer con un lector de pantalla — está en el DOM y no solo es visual?

### 3.3.2 — Etiquetas o instrucciones (Nivel A)
- [ ] ¿Todos los campos del formulario tienen un label visible asociado?
- [ ] ¿Si el campo tiene un formato esperado, se indica antes de que el usuario intente enviarlo? *(ej. "formato: DD/MM/AAAA")*
- [ ] ¿Los campos requeridos están claramente identificados?

### 3.3.3 — Sugerencia de error (Nivel AA)
- [ ] ¿El mensaje de error explica cómo corregir el problema — no solo que existe?
- [ ] ¿Se usan `aria-describedby` o `aria-live` para anunciar los errores al lector de pantalla?
- [ ] ¿El error aparece cerca del campo que lo generó, no solo al inicio del formulario?

---

## Robusto

### 4.1.2 — Nombre, rol, valor (Nivel A)
- [ ] ¿Los botones con solo ícono tienen un nombre accesible? *(aria-label o texto oculto)*
- [ ] ¿Los elementos interactivos custom tienen el rol correcto en el árbol de accesibilidad?
- [ ] ¿Los estados de los componentes se comunican? *(expandido/colapsado, seleccionado, deshabilitado)*
- [ ] ¿En el árbol de accesibilidad hay elementos marcados como "unlabelled" o "generic" que deberían tener nombre?

### 4.1.3 — Mensajes de estado (Nivel AA)
- [ ] ¿Si aparece un mensaje de éxito o error global, se anuncia sin mover el foco?
- [ ] ¿Los mensajes dinámicos usan `aria-live` para ser leídos por el lector de pantalla?
- [ ] ¿Un spinner o indicador de carga comunica su estado al lector de pantalla?

---

## Después de este checklist — ¿qué sigue?

Estas preguntas cubren los issues más comunes y visibles. Después de completarlas:

1. **Herramientas automáticas** — WAVE, Lighthouse, axe DevTools. Comparo sus resultados contra mis notas. Los desacuerdos son los más interesantes.
2. **Navegación con teclado** — recorro toda la página sin mouse. Ninguna herramienta reemplaza esto.
3. **Juicio humano** — ¿el alt text es útil o es solo ruido? ¿Este contraste técnicamente pasa pero en contexto es ilegible? Las herramientas no tienen criterio. Yo sí.

---

*Este checklist evoluciona con cada auditoría — V.V.*
