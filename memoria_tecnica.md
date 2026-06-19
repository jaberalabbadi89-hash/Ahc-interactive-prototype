# Memòria Tècnica: Rediseño Digital y Objetos de Aprendizaje Interactivos (AHC)

**Autor:** Equipo de Desarrollo Técnico / Estudiante en Prácticas  
**Institución:** Asociación Huella de Carbono (AHC)  
**Duración del Proyecto:** 60 Horas  

---

## 1. Introducción y Contexto Institucional

La Asociación Huella de Carbono (AHC) es una organización comprometida con la concienciación climática y la formación en sostenibilidad. Para canalizar su oferta educativa, la asociación cuenta con un ecosistema digital compuesto por un portal web principal desarrollado en WordPress (maquetador Divi) y un entorno virtual de aprendizaje (EVA) basado en Moodle (con el tema Edwiser RemUI). 

En este contexto, el presente proyecto de 60 horas se centra en resolver la desconexión visual y la falta de interactividad dinámica dentro de sus recursos formativos. El objetivo principal ha sido conceptualizar, diseñar y programar un conjunto de prototipos interactivos funcionales para cuatro Objetos de Aprendizaje (OA) de alta prioridad pedagógica, garantizando una experiencia de usuario (UX) unificada, accesible y de alto rendimiento.

---

## 2. Alineación Estratégica con el "Plan Rediseño Digital 2026"

Durante la fase de análisis técnico, se identificó que realizar modificaciones directamente sobre las plantillas PHP del núcleo de Moodle o los archivos estructurales de WordPress presentaba graves riesgos de escalabilidad, incompatibilidad y alto coste de mantenimiento ante futuras actualizaciones de software.

Para mitigar este riesgo, la estrategia adoptada se alinea con el "Plan Rediseño Digital 2026" de la asociación a través de dos decisiones de arquitectura clave:
1. **Centralización de Estilos mediante Capa de Sobreescritura (`h5p-overrides.css`):** En lugar de editar los temas base, se diseñó una hoja de estilos de sobreescritura específica para el motor H5P. Esto permite que cualquier módulo interactivo creado de forma nativa en H5P herede automáticamente la identidad visual de la asociación, sin importar si está embebido en WordPress o Moodle.
2. **Prototipado Desacoplado de Micro-Frontends:** La lógica y la estructura de las interacciones se programaron de forma aislada e independiente en el lado del cliente (Client-Side). Esto asegura que los desarrolladores puedan testear las interacciones de forma local y ágil, reduciendo a cero la dependencia del servidor durante la fase de control de calidad (QA).

---

## 3. Tecnologías Utilizadas y Filosofía de Diseño Visual

El desarrollo técnico se ha ejecutado estrictamente bajo el estándar de desarrollo nativo web, garantizando ligereza, rendimiento y compatibilidad sin librerías externas:

* **HTML5 Semántico:** Utilizado para estructurar las secciones, optimizar el posicionamiento (SEO) y cumplir con las directrices de accesibilidad (A11y), implementando elementos nativos de interfaz de usuario como `<dialog>` para el control de ventanas modales.
* **CSS3 (Grid & Flexbox):** Uso intensivo de CSS Grid para construir la estructura modular de la página de control (Bento Box) y Flexbox para alinear de forma fluida los controles multimedia, botones de interacción y elementos arrastrables.
* **Vanilla JavaScript (ES6):** Implementación de JavaScript nativo para gestionar el estado de las interacciones, temporizadores, lógica de puntuación y el control del ciclo de vida de los componentes multimedia.

### Filosofía de Diseño Visual:
* **Diseño Bento Box:** Estructuración de la interfaz general mediante un sistema de rejilla modular y limpio, facilitando la lectura secuencial de los datos de progreso del curso y el acceso a los módulos didácticos.
* **Efecto Glassmorphic:** Aplicación de estilos de "vidrio esmerilado" mediante la propiedad `backdrop-filter: blur()`, sombras sutiles y bordes semi-transparentes para conferir una estética visual moderna y premium a las cabeceras y popups interactivos.
* **Diseño Adaptativo (Responsive Design):** Configuración de las dimensiones internas, coordenadas de hotspots y márgenes mediante unidades flexibles y porcentajes (`%`) para garantizar que la experiencia sea idéntica en dispositivos móviles, tabletas y ordenadores de escritorio.

---

## 4. Arquitectura del Proyecto e Inyección de Código

La arquitectura del proyecto está diseñada de forma modular y desacoplada, facilitando una inyección por fases que previene conflictos en el DOM y reduce la carga del servidor. La estructura de archivos del prototipo se compone de:

* **`index.html`:** Documento contenedor principal que actúa como portal de aprendizaje interactivo.
* **`custom-style.css`:** Contiene los tokens de diseño de la marca (colores oficiales como el verde `#62a144`, tipografías corporativas, etc.), las reglas estructurales de la Bento Box y los estilos específicos de cada objeto de aprendizaje.
* **`script.js`:** Núcleo de la lógica interactiva. Controla el lanzamiento de las modales y el ciclo de vida de los datos de cada actividad (volteo de tarjetas, zonas de drop, sistema de penalización temporal en video y control selectivo de tooltips).
* **`custom-interactions.js`:** Archivo aislado que ejecuta una instancia de `Intersection Observer` para aplicar animaciones de entrada (`ahc-reveal`) a los elementos de la interfaz solo cuando entran en el viewport del navegador, optimizando el uso de CPU.
* **`h5p-overrides.css`:** Hoja de estilos externa para inyectar sobre los iframes nativos de H5P. Este archivo aplica selectores de alta especificidad (ej. `.h5p-joubelui-button`) para redefinir el color de los botones del sistema de H5P al verde de la asociación, redondear esquinas y unificar el muelle visual de los diálogos interactivos, todo de manera aislada y sin interferir con las hojas de estilo nativas de Moodle (Edwiser).
