# Comiquería Central

Entrega de JavaScript: Pre-Entrega 10 — APIs, Peticiones y Librerías.

## Funcionalidades

- Carga inicial del catálogo desde `data.json` mediante `fetch()` y `async/await`.
- Estados de carga, éxito y error; botón para reintentar la petición.
- Notificaciones Toastify por CDN para la carga y las acciones del simulador.
- Búsqueda en vivo y filtro por categoría.
- Catálogo renderizado dinámicamente con DOM, template strings y eventos.
- Carrito con suma, resta, eliminación por producto, total y descuento.
- Formulario para agregar títulos propios al catálogo.
- Detección de portadas por nombre: primero usa portadas locales y, si no
  encuentra el título, consulta catálogos públicos para asignar una portada
  automáticamente antes de guardar el producto.
- Persistencia de catálogo, carrito y stock mediante `localStorage`.
- Recuperación de datos con `JSON.parse()` y guardado con `JSON.stringify()`.
- Aviso de descuentos a los 3 segundos de ingresar, mediante `setTimeout()`.
- Alta de productos con `try-catch-finally`, mensaje de error y botón disponible
  para reintentar después de finalizar la operación.

## Catálogo visual

Las portadas están organizadas dentro de `assets/portadas/` y cada tarjeta
incluye su editorial y un enlace a la ficha original.

- Los mangas toman como referencia el catálogo oficial de
  [Editorial Ivrea Argentina](https://www.ivrea.com.ar/) y, para Naruto, la
  ficha oficial de [VIZ Media](https://www.viz.com/).
- Los cómics toman como referencia el catálogo oficial de
  [OVNI Press](https://www.ovnipress.net/).

Las imágenes se utilizan como referencia visual dentro de este proyecto
académico y conservan su atribución mediante los enlaces de cada ficha.

## Estructura

```text
index.html
data.json
css/styles.css
js/catalogo.js
js/main.js
assets/portadas/
```


   Storage.prototype.setItem = window.setItemOriginal;
   delete window.setItemOriginal;
   ```

