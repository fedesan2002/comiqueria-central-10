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

## Persistencia

El estado se guarda bajo claves versionadas de `localStorage`. Cada acción que
modifica productos, stock o carrito actualiza el array, persiste los cambios y
vuelve a renderizar la interfaz.

El código utiliza operadores modernos (`??`, `?.` y ternarios) y destructuring
para recuperar los datos de cada producto sin perder los métodos de la clase
`Producto` al recargar la página.

## Fuente de datos y flujo de la petición

`data.json` simula la base de datos de la comiquería. Contiene nueve productos
con `id`, `nombre`, `precio`, `categoria`, `stock`, `imagen`, `editorial` y
`enlaceEditorial`. Los precios son datos de ejemplo del simulador.

1. `DOMContentLoaded` ejecuta `iniciarAplicacion()` en `js/main.js`.
2. `cargarCatalogoDesdeJSON()` muestra “Cargando productos…” en el DOM y en
   Toastify. Deshabilita los controles que dependen del catálogo mientras espera.
3. En el `try`, `await fetch("./data.json")` realiza la petición. Se comprueba
   `respuesta.ok`, se espera `respuesta.json()` y se valida la estructura.
   Respuestas HTTP fallidas, JSON inválido, productos incompletos e IDs repetidos
   generan un error que llega al `catch`.
4. Los datos se convierten en instancias de `Producto` con `map()`. Se recuperan
   el catálogo y el carrito de `localStorage` cuando existen, para conservar
   los cambios de visitas anteriores. La petición a JSON ocurre en cada inicio,
   incluso cuando hay datos guardados. Para ver la fuente original, borrá las
   dos claves del proyecto indicadas en la sección de pruebas.
5. `actualizarVistaCatalogo()` y `renderizarCarrito()` dibujan la interfaz con
   los datos y `mostrarMensaje()` comunica el éxito usando también Toastify.
6. El `catch` comunica el error con Toastify y un mensaje en la página, y
   habilita “Reintentar carga”. No borra los datos guardados del navegador.
7. El `finally` termina el estado de carga, retira su notificación persistente,
   libera el botón de reintento y habilita el simulador cuando hay catálogo.

Además del JSON inicial, la búsqueda automática de portadas consulta Google
Books y Open Library con `fetch()` y `async/await`, cuando no encuentra una
portada local. Si esas fuentes fallan, permite agregar el título sin portada.

## Librería de notificaciones

Se integra [Toastify 1.12.0](https://github.com/apvarun/toastify-js) por CDN
(jsDelivr), con CSS en el `<head>` y JavaScript antes de los scripts del proyecto.
`notificarUsuario()` centraliza el color, la duración y la posición de los avisos.
Hay notificaciones de carga, éxito, errores y acciones como agregar al carrito.

La librería necesita conexión a Internet para su descarga inicial. Si no se
puede cargar, los mensajes del DOM siguen funcionando. No se usan `alert()`,
`prompt()` ni `confirm()`.

## Funciones conservadas de la Pre-Entrega 9

`programarAvisoDeDescuento()` presenta los descuentos de la tienda a los 3
segundos mediante `setTimeout()`. `agregarProductoDesdeFormulario()` conserva
su `try-catch-finally`, revierte el alta cuando falla el guardado y siempre
restaura el botón. El formulario mantiene sus campos ante un error para poder
reintentar.

## Cómo ejecutar y comprobar

1. Abrí la carpeta con Visual Studio Code y usá **Open with Live Server** sobre
   `index.html`. También podés servirla con `python -m http.server 8000` y abrir
   `http://localhost:8000`. Usá un servidor local: abrir con `file://` puede
   impedir la petición al JSON.
2. Al ingresar aparece “Cargando productos…” y, al resolver la petición,
   aparecen los productos y la notificación de éxito.
3. En DevTools → Network podés elegir “Slow 3G” y recargar para observar la
   carga. Se puede navegar la página mientras la petición está pendiente.
4. Para probar un HTTP 404, renombrá temporalmente `data.json` y recargá.
   Aparecerá el aviso de error y “Reintentar carga”. Restaurá el nombre del
   archivo y presioná ese botón: se cargará el catálogo sin recargar la página.
5. También podés introducir un error de sintaxis en JSON, probar, restaurarlo y
   reintentar. Un ID duplicado o un stock negativo también se rechaza.
6. Agregá un producto y recargá: se mantienen tus cambios. Si querés volver al
   catálogo original de `data.json`, ejecutá en la consola y recargá:

   ```js
   localStorage.removeItem("comiqueriaCentral.catalogo.v2");
   localStorage.removeItem("comiqueriaCentral.carrito.v2");
   location.reload();
   ```

7. Para probar el error de guardado del formulario, ejecutá:

   ```js
   window.setItemOriginal = Storage.prototype.setItem;
   Storage.prototype.setItem = function () {
       throw new Error("Error de guardado simulado");
   };
   ```

   Intentá agregar un título nuevo. Se conservan los campos, se muestra el error
   y el botón vuelve a habilitarse. Restaurá el método para reintentar:

   ```js
   Storage.prototype.setItem = window.setItemOriginal;
   delete window.setItemOriginal;
   ```

