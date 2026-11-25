# CO Pastelería — Documentación del Proyecto

Sitio web estático de catálogo y toma de pedidos por WhatsApp para **CO Pastelería**, compuesto por `index.html`, `productos.json` e imágenes locales en la misma carpeta (`.jpg`).

---

## 1. Arquitectura y dependencias

- **HTML5 + Tailwind CSS v4 (CDN):** Maquetado y estilos visuales.
- **Google Fonts:** `Playfair Display` (títulos/detalles editoriales) y `Plus Jakarta Sans` (textos e interfaz).
- **Lucide Icons (CDN):** Íconos vectoriales renderizados vía `lucide.createIcons()`.
- **Google reCAPTCHA v2:** Dos widgets (`g-recaptcha`) en la página:
  - Índice `0` (`grecaptcha.getResponse(0)`): modal de **Tu Pedido** (carrito).
  - Índice `1` (`grecaptcha.getResponse(1)`): modal de **Regalar Gift Card**.
- **Catálogo externo (`productos.json`):** Se lee dinámicamente mediante `fetch("productos.json?v=...", { cache: "no-store" })`.

---

## 2. Reglas de negocio implementadas

1. **Buscador en tiempo real arriba del catálogo:**
   - Ubicado al inicio de `<main>`, filtra en vivo sobre los productos activos (`activo !== false`) comparando contra `nombre` y `desc`.
   - Normaliza el texto (`normalizeSearchText()`) para ignorar mayúsculas, minúsculas y tildes.
   - Oculta las secciones sin coincidencias en la búsqueda actual, muestra el contador de resultados y presenta un estado vacío con botón para limpiar el filtro si no hay coincidencias.
2. **Orden de secciones en el catálogo:**
   - `Ediciones Especiales` (`#boxes`) aparece primero.
   - Luego `Nuestras Creaciones` (`#tortas`).
   - `Pequeños Bocados` (`#cookies-alfajores`) y `Budines Artesanales` (`#budines`) están ocultos si no tienen productos activos (`activo !== false`).
3. **Cronómetro de Edición Especial:**
   - Cuenta regresiva hasta el **15 de diciembre de 2026 a las 23:59:59** (`iniciarContadorEdicionEspecial()`).
   - Al vencer la fecha, cambia el mensaje a **"Edición Finalizada"**.
4. **Reglas de entrega para Box Especiales (`cat === "boxes"`):**
   - Si el carrito contiene al menos un producto con `cat === "boxes"`, aparece el selector **Navidad (24/12)** / **Fin de Año (31/12)**.
   - **Navidad:** fija la fecha/hora de entrega en el rango `2026-12-24T12:00` a `2026-12-24T16:00` (por defecto `12:00`).
   - **Fin de Año:** fija la fecha/hora de entrega en el rango `2026-12-31T12:00` a `2026-12-31T16:00` (por defecto `12:00`).
   - Si no hay boxes en el carrito, vuelve a la regla general de **mínimo 72 horas** desde el momento actual.
5. **Persistencia en `localStorage` con invalidación por cambios en `productos.json`:**
   - Guarda el carrito en `co_pasteleria_cart_v1` y una firma del catálogo en `co_pasteleria_catalog_sig_v1`.
   - Si se modifica algún precio, nombre, descripción, imagen, categoría, estado `activo` o cantidad de productos en `productos.json`, la firma deja de coincidir y **se borra automáticamente el carrito guardado** para evitar precios o datos desactualizados.

---

## 3. Tareas frecuentes de mantenimiento

- **Cambiar precios o descripciones:** Editar el campo `precio` o `desc` en `productos.json` y guardar. Al recargar la web, el cambio se refleja y se limpia cualquier carrito viejo.
- **Activar o pausar una sección o producto:** Cambiar `"activo": false` a `"activo": true` (o viceversa) en el objeto correspondiente dentro de `productos.json`. Si una categoría queda sin ítems activos, su `<section>` se oculta sola.
- **Cambiar el número de WhatsApp:** Actualizar la variable `telefono` (`'5491154892779'`) dentro de `submitGiftCardWhatsApp()` y `checkoutWhatsApp()` en `index.html`, así como el texto del `<footer>`.
- **Cambiar la fecha de cierre del cronómetro:** Modificar `'2026-12-15T23:59:59'` dentro de `iniciarContadorEdicionEspecial()` en `index.html`.