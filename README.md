# Artes Gráficas y Industrias — sitio web

Sitio institucional bilingüe para **Artes Gráficas y Industrias**. Presenta servicios de community management, diseño gráfico, desarrollo web, soluciones digitales y soluciones con inteligencia artificial.

## Estado del proyecto

- La web está implementada como sitio estático y no necesita un servidor de aplicaciones ni base de datos.
- El selector Español / English traduce la portada, las cinco páginas de servicios, la navegación y el formulario. La preferencia se guarda en `localStorage`.
- El formulario de contacto está preparado visualmente, pero **no envía datos**. Falta definir un correo o número de WhatsApp y conectar un mecanismo de envío.
- Este repositorio almacena el código del sitio. Publicarlo en GitHub **no actualiza automáticamente** la versión alojada actualmente.

## Arquitectura

| Archivo o carpeta | Función |
| --- | --- |
| `dist/index.html` | Documento HTML de la portada: metadatos, tipografía, favicon y carga de CSS y JavaScript. |
| `dist/servicios/*/index.html` | Documentos de entrada para las cinco rutas de servicios. Comparten el mismo contenedor y scripts. |
| `dist/app.js` | Datos de servicios, generación del contenido, navegación, cambio de idioma, formulario y animaciones. |
| `dist/translations.js` | Diccionario de traducciones al inglés del contenido y la interfaz. |
| `dist/styles.css` | Diseño visual, adaptación a distintos tamaños de pantalla, estados interactivos y animaciones CSS. |
| `scripts/dev-server.cjs` | Servidor HTTP local sin dependencias para probar todas las rutas. |

### Cómo se renderizan las páginas

Cada ruta de `servicios/` contiene un `index.html` físico para que un alojamiento estático pueda abrirla directamente. Al cargar, `app.js` lee `location.pathname`, identifica el servicio y construye la página correspondiente dentro de `#app`. La portada usa el mismo mecanismo. No hay un router, una API ni un proceso de compilación.

Los datos en español de los cinco servicios están al principio de `app.js`. `translations.js` proporciona las versiones en inglés. El idioma seleccionado se recuerda entre páginas con la clave `agi-language` de `localStorage`; también se actualizan `lang`, el título y la descripción del documento.

### Animaciones y accesibilidad

- En la portada, las etiquetas de servicios orbitan mediante `requestAnimationFrame`; el anillo y sus puntos giran con CSS.
- Las cabeceras de las páginas de servicios muestran dos satélites en órbita. Las secciones aparecen al entrar en pantalla mediante `IntersectionObserver`.
- La subnavegación marca la sección visible y los menús funcionan con enlaces y controles HTML nativos.
- Si el dispositivo solicita reducir movimiento (`prefers-reduced-motion: reduce`), se desactivan las órbitas y transiciones decorativas.

## Ejecutar en local

Requiere **Node.js 18 o superior**. No hace falta instalar paquetes.

```bash
npm start
```

Abrir `http://127.0.0.1:8765/`. Por ejemplo, la página de IA está en `http://127.0.0.1:8765/servicios/soluciones-ia/`.

También se puede servir `dist/` con cualquier servidor estático que resuelva los `index.html` de cada carpeta. Abrir los archivos directamente con `file://` no reproduce el comportamiento de las rutas y referencias absolutas.

## Estructura de navegación

```text
/
├── servicios/community-management/
├── servicios/diseno-grafico/
├── servicios/desarrollo-web/
├── servicios/soluciones-digitales/
└── servicios/soluciones-ia/
```

La portada incluye presentación, resumen de servicios, información del estudio y formulario de contacto. Cada servicio tiene secciones de desafío, valor, capacidades, proceso y una llamada a contacto.

## Cambiar contenido o diseño

1. **Textos de servicios:** editar el arreglo `servicesEs` en `dist/app.js`.
2. **Textos en inglés:** actualizar las entradas correspondientes en `dist/translations.js`.
3. **Estilos y animaciones:** editar `dist/styles.css`. La función `startOrbitAnimation` de `dist/app.js` controla la posición de las etiquetas que orbitan en la portada.
4. **Metadatos o scripts del documento:** actualizar `dist/index.html` y replicar ese documento en cada `dist/servicios/*/index.html`. `app.js` ajusta el título y la descripción según la página e idioma.
5. **Nuevo servicio:** agregar su ficha y traducciones en los archivos anteriores, crear su carpeta `dist/servicios/<slug>/index.html` y revisar la disposición de la órbita y el menú.

## Publicación

El contenido publicable es `dist/`. El sitio usa rutas absolutas como `/styles.css` y `/servicios/...`, por lo que debe alojarse en la **raíz de un dominio** o adaptarse para una subruta. Esto es relevante si se quiere utilizar GitHub Pages bajo `usuario.github.io/nombre-del-repositorio/`.

La publicación actual del sitio y este repositorio son independientes. No se incluyen aquí identificadores, credenciales ni configuración interna del alojamiento anterior. Para automatizar despliegues futuros hará falta configurar el proveedor de hosting y un flujo de publicación.

## Dependencias y datos

- JavaScript, HTML y CSS nativos; sin frameworks ni dependencias de npm.
- Tipografía **Plus Jakarta Sans** cargada desde Google Fonts. Si no está disponible, se usa una fuente de sistema.
- No hay analítica, autenticación, base de datos ni servicios de terceros para enviar el formulario.
- La única preferencia guardada por la web es el idioma en `localStorage`.

## Licencia

No se incluye una licencia de reutilización en este repositorio. El código público puede consultarse, pero cualquier permiso adicional debe definirlo el titular del proyecto.
