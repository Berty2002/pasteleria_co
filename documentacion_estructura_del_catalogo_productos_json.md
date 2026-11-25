# Estructura de `productos.json`

> Nota: El estándar JSON no permite comentarios (`//` o `/* */`) dentro del archivo `productos.json`. Mantener siempre una sintaxis JSON válida (comillas dobles, sin coma final en el último elemento).

## Campos de cada producto

| Campo | Tipo | Descripción | Ejemplo |
| :--- | :--- | :--- | :--- |
| `id` | `number` | Identificador único del producto en todo el catálogo. | `21` |
| `cat` | `string` | Categoría: `"boxes"`, `"tortas"`, `"cookies"` o `"budines"`. | `"boxes"` |
| `activo` | `boolean` | `true` muestra el producto; `false` lo oculta sin borrarlo. | `true` |
| `nombre` | `string` | Nombre visible en la tarjeta y en el mensaje de WhatsApp. | `"BOX Navidad y Fin de Año"` |
| `desc` | `string` | Descripción breve de ingredientes o presentación. | `"Seis tentaciones..."` |
| `precio` | `number` | Precio unitario en pesos (sin puntos ni símbolos). | `60000` |
| `img` | `string` | Nombre o ruta del archivo de imagen. | `"el1.jpg"` |