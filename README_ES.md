# VHVD: Metodología Personal de Auditoría de Accesibilidad Web

> Una metodología personal de auditoría de accesibilidad web en tres niveles, basada en WCAG 2.1 AA, Section 508 y EN 301 549.
> Construida y refinada con auditorías prácticas de mis propios proyectos, no solo desde el estudio teórico.

---

## Descripción general

Este repositorio documenta mi proceso personal para auditar accesibilidad web. Cubre cómo defino el alcance, el orden en que ejecuto pruebas manuales y automatizadas, y cómo documento y priorizo hallazgos.

La metodología está estructurada en tres niveles de auditoría según la profundidad requerida. Cada nivel construye sobre el anterior. La plantilla de auditoría (`.xlsx`) incluida en este repositorio refleja la misma estructura.

---

## Niveles de auditoría

| Nivel | Nombre | Herramientas | Cobertura WCAG | Alineación legal |
|-------|--------|--------------|----------------|------------------|
| **N1** | Revisión rápida | WAVE + Lighthouse + revisión manual | Solo issues de alto impacto (no confirma conformidad) | — |
| **N2** | Auditoría estándar | WAVE + axe DevTools + navegación con teclado + Chrome DevTools | WCAG 2.1 AA | Section 508 (EE.UU.) · EN 301 549 (UE) |
| **N3** | Auditoría completa | Todo lo anterior + lector de pantalla (VoiceOver / NVDA) | WCAG 2.1 AA (cobertura completa) | Section 508 (EE.UU.) · EN 301 549 (UE) |

> **Sobre la alineación legal:** EN 301 549 (v3.2.1) referencia WCAG 2.1 AA, mientras que Section 508 referencia WCAG 2.0 AA (actualización de 2017). Como WCAG 2.1 AA incluye todos los criterios de WCAG 2.0 AA, auditar contra WCAG 2.1 AA cubre los requisitos web basados en WCAG de ambos estándares. Los dos incluyen además requisitos que van más allá de WCAG (por ejemplo, para software, documentos y documentación de soporte) que una auditoría web no cubre. Esto no es una certificación legal. Significa que los hallazgos están expresados en términos que esos marcos reconocen y sobre los que pueden actuar.

> **N2 vs N3:** La diferencia es la profundidad de cobertura con lector de pantalla. N2 evalúa la mayoría de los criterios de WCAG 2.1 AA con herramientas automáticas y pruebas de teclado, pero algunos criterios solo se pueden confirmar con tecnología asistiva. N3 agrega pruebas manuales con lector de pantalla (VoiceOver / NVDA), necesarias para verificar criterios que las herramientas automáticas no pueden evaluar completamente, especialmente regiones dinámicas, contenido actualizado en tiempo real y patrones ARIA complejos.

---

## Mi proceso

### 1. Alcance

Antes de tocar cualquier herramienta, defino qué voy a evaluar y contra qué estándar.

- **Qué:** ¿Una sola página, un flujo de usuario o un sitio completo? Mapeo cada página o flujo en alcance antes de comenzar.
- **Contra qué:** Siempre WCAG, pero aclaro el nivel de conformidad objetivo (A, AA o AAA) con quien solicita la auditoría. Si no hay un brief, asumo WCAG 2.1 AA.
- **Restricciones:** ¿Hay tecnologías asistivas conocidas en uso? ¿Algún requisito de plataforma o navegador? Esto afecta cómo peso los hallazgos más adelante.

Saltarse este paso provoca deriva en la auditoría. Terminas evaluando cosas que no importan y pasando por alto las que sí.

---

### 2. Pruebas

#### 2a. Primera revisión (sin herramientas automáticas)

Comienzo sin ninguna herramienta automática activa. Quiero formarme mi propia opinión de la página antes de que un algoritmo me diga dónde mirar. Esto me resulta natural por revisar código con mis alumnos. Estoy acostumbrado a leer una página e identificar problemas estructurales antes de ejecutar cualquier cosa.

En la práctica, tengo dos cosas abiertas: la página y el **panel Elements de Chrome DevTools**. La capa visual me dice lo que ve el usuario; el DOM me dice lo que realmente está ahí. Un heading que visualmente parece un `h2` podría ser un `div` con estilos, y el inspector lo detecta de inmediato.

En esta revisión busco:

- Imágenes, iconos y elementos no textuales (¿son significativos o decorativos?)
- Menús desplegables, modales y componentes interactivos
- Formularios y estados de error (¿los errores están escritos en texto o solo marcados en rojo?)
- Contraste de color (fallas evidentes a simple vista)
- Secciones de la página y jerarquía de headings (¿la estructura tiene sentido en el DOM?)
- Flujos de usuario visibles (login, compra, búsqueda) que requieran pruebas de extremo a extremo

También tengo una **libreta y pluma** a la mano. Antes de que algo entre a la hoja de cálculo, pasa por papel. Me mantiene enfocado y me permite esbozar relaciones entre hallazgos sin comprometerme a una estructura demasiado pronto.

#### 2b. Checklist manual (preguntas rápidas de sí/no)

Antes de las herramientas automáticas, recorro un checklist corto de preguntas de sí/no. Esto construye una línea base que después comparo contra los resultados automatizados. El archivo separado en este repositorio contiene la lista completa de preguntas mapeadas a sus criterios WCAG. Ver [`manual-checklist_ES.md`](./manual-checklist_ES.md).

Algunos ejemplos:

- ¿Las imágenes e iconos tienen alt text?
- ¿Se usan elementos semánticos o hay divitis?
- ¿Los inputs del formulario tienen labels visibles asociados?
- ¿Los errores se identifican en texto, no solo por color o ícono?
- ¿Hay una jerarquía de headings lógica (h1 → h2 → h3)?
- ¿Se puede identificar el idioma de la página en el HTML?
- ¿Hay trampas de teclado evidentes a primera vista?

#### 2c. Herramientas automáticas

Después de tener mi propia línea base, ejecuto las herramientas en este orden:

1. **WAVE:** superposición visual amplia, rápida de leer, útil para detectar alt text faltante, errores de contraste y problemas estructurales de un vistazo
2. **Lighthouse** (auditoría de accesibilidad): da un puntaje y marca issues con referencias a WCAG, útil para priorización rápida
3. **axe DevTools:** el más preciso de los tres, menor tasa de falsos positivos, y los hallazgos se mapean directamente a criterios WCAG

Los ejecuto en este orden porque WAVE es el más rápido de leer visualmente, Lighthouse da contexto, y axe es donde profundizo en los detalles. Comparo su output contra mis notas manuales. Los desacuerdos siempre vale la pena investigar.

#### 2d. Navegación con teclado

Recorro toda la página con Tab, sin mouse. Verifico:

- Que todos los elementos interactivos sean alcanzables con teclado
- Que el indicador de foco siempre sea visible
- Que el orden de Tab siga una secuencia lógica de lectura
- Que no haya trampas de teclado

#### 2e. Lector de pantalla (solo N3)

VoiceOver en macOS o NVDA en Windows. Escucho cómo se anuncia la página, no solo si los elementos existen, sino si tienen sentido fuera de contexto visual.

---

### 3. Documentación

Cada hallazgo entra al registro de auditoría con:

- **ID:** secuencial (F-001, F-002...)
- **Página / Componente:** dónde exactamente encontramos el hallazgo
- **Criterio WCAG:** número y nombre corto (ej. 1.1.1 Non-text Content)
- **Nivel:** A, AA o AAA
- **Severidad:** Crítico / Serio / Moderado / Menor
- **Herramienta que lo encontró:** o "Manual" si lo detecté en la primera revisión
- **Descripción del issue:** qué está roto y por qué falla el criterio
- **Impacto:** quién se ve afectado y cómo
- **Corrección recomendada:** una sugerencia concreta, HTML o CSS primero, JS solo cuando sea necesario

#### Lógica de priorización

Una vez documentados, ordeno los hallazgos así:

1. **Fallas de Nivel A primero.** Son el piso mínimo. Nada más importa si el Nivel A está roto.
2. **Correcciones que solo requieren HTML o CSS.** Alto impacto, bajo costo de implementación. Estas deberían ser lo primero que ataca el equipo de desarrollo.
3. **Fallas de Nivel AA,** después de que la base estructural esté sólida.
4. **Correcciones que requieren JavaScript.** Válidas pero de mayor esfuerzo, marcadas claramente para que el equipo pueda planificarlas.

Soy transparente sobre este orden en mis reportes. Remediar la accesibilidad no es mi rol como auditor, pero sí proveo suficiente contexto para que un equipo de desarrollo entienda qué atacar primero y por qué.

---

## Plantilla de auditoría

La plantilla `.xlsx` en este repositorio refleja directamente este proceso. Incluye:

- **Registro de hallazgos:** una fila por hallazgo, con desplegables para severidad, principio POUR, nivel WCAG y estatus
- **Alcance de auditoría:** hoja de metadatos (sitio, fecha, estándares, herramientas, páginas en alcance)
- **Dashboard de resumen:** conteos calculados automáticamente por severidad y estatus
- **Referencia rápida WCAG:** los 16 criterios más comunes en auditorías, con la pregunta de auditoría que mapea a cada criterio

[Descargar la plantilla](./accessibility_audit_template_ES.xlsx)

---

## Herramientas

| Herramienta | Para qué la uso |
|-------------|-----------------|
| **Chrome DevTools (paneles Elements y Accessibility)** | Mi punto de partida. Leo el DOM junto con la capa visual desde la primera revisión. |
| **WAVE** | Superposición visual rápida, primer paso automatizado, bueno para alt text y contraste |
| **axe DevTools** | Hallazgos precisos mapeados a criterios WCAG, menor tasa de falsos positivos |
| **Lighthouse** | Puntaje rápido y lista de issues priorizados con referencias a WCAG |
| **Solo teclado** | Orden de Tab, visibilidad del foco, trampas de teclado. Ninguna herramienta reemplaza esto. |
| **VoiceOver / NVDA** | Pruebas con lector de pantalla para auditorías N3 |

---

## Referencia de estándares

| Estándar | Jurisdicción | Notas |
|----------|-------------|-------|
| [**WCAG 2.1 AA**](https://www.w3.org/TR/WCAG21/) | Base global | Fundamento de los tres niveles de auditoría |
| [**Section 508**](https://www.section508.gov/) | Estados Unidos | ICT federal; referencia WCAG 2.0 AA desde la actualización de 2017 |
| [**EN 301 549**](https://www.etsi.org/deliver/etsi_en/301500_302000/301549/03.02.01_60/en_301549v030201p.pdf) | Unión Europea | Referencia WCAG 2.1 AA; norma armonizada para el sector público bajo la Directiva de Accesibilidad Web, y principal referencia técnica de la Ley Europea de Accesibilidad (aplica desde junio de 2025) |

---

## Aviso legal

Esta metodología refleja mi práctica actual de auditoría y evoluciona con cada auditoría realizada. No es un marco de certificación, y completar una auditoría con este proceso no garantiza cumplimiento legal con ninguna regulación específica. Los hallazgos están expresados en términos alineados con WCAG 2.1 AA, Section 508 y EN 301 549, pero el cumplimiento legal de accesibilidad siempre debe evaluarse en el contexto de la legislación aplicable.

---

*Última actualización: abril 2026 — Victor Vigueras*
