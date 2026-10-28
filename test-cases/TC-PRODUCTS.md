# Casos de Prueba — Módulo: Catálogo de Productos

**Módulo:** Catálogo de Productos  
**Archivo:** TC-PRODUCTS.md  
**Total de casos:** 8

---

## TC-PROD-001 — Listado de productos en /products
**Prioridad:** P1 | **Tipo:** Positivo

**Precondición:** Al menos 5 productos activos en DB

**Pasos:**
1. Navegar a `http://localhost:5173/products`

**Resultado esperado:**
- Se muestran cards de productos con imagen, nombre y precio
- No hay errores en consola de red (status 200 en `/api/products`)
- Página carga en menos de 3 segundos

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-PROD-002 — Detalle de producto existente
**Prioridad:** P1 | **Tipo:** Positivo

**Precondición:** Conocer el ID de un producto activo

**Pasos:**
1. Navegar a `/products`
2. Hacer clic en cualquier product card

**Resultado esperado:**
- URL cambia a `/products/:id`
- Se muestran: nombre, descripción, precio, imágenes, estado de stock
- Botón "Agregar al carrito" visible y habilitado si `inStock: true`

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-PROD-003 — Detalle de producto sin stock
**Prioridad:** P1 | **Tipo:** Positivo

**Precondición:** Producto con `inStock: false` en DB

**Pasos:**
1. Navegar al detalle del producto sin stock

**Resultado esperado:**
- Badge o indicador "Sin stock" visible
- Botón "Agregar al carrito" deshabilitado o ausente

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-PROD-004 — Producto inexistente (404)
**Prioridad:** P2 | **Tipo:** Negativo

**Pasos:**
1. Navegar a `/products/id-que-no-existe-9999`

**Resultado esperado:**
- Página 404 o mensaje de error amigable
- Sin crash de la aplicación

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-PROD-005 — Filtro por categoría
**Prioridad:** P2 | **Tipo:** Positivo

**Precondición:** Múltiples categorías con productos en DB

**Pasos:**
1. Navegar a `/products`
2. Seleccionar una categoría del filtro lateral o dropdown

**Resultado esperado:**
- Solo se muestran productos de la categoría seleccionada
- El conteo de resultados es correcto

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-PROD-006 — Búsqueda por nombre
**Prioridad:** P2 | **Tipo:** Positivo

**Pasos:**
1. Navegar a `/products`
2. Escribir "PlayStation" (o nombre parcial de un producto existente) en el buscador

**Resultado esperado:**
- Lista filtrada con productos que coinciden
- Sin resultados si el término no existe, con mensaje "No se encontraron productos"

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-PROD-007 — Página de ofertas /ofertas
**Prioridad:** P2 | **Tipo:** Positivo

**Pasos:**
1. Navegar a `http://localhost:5173/ofertas`

**Resultado esperado:**
- Se muestran productos con descuento o precio especial
- Indicador de descuento visible (porcentaje o precio tachado)

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-PROD-008 — Responsividad en mobile (375px)
**Prioridad:** P3 | **Tipo:** UI

**Pasos:**
1. Abrir DevTools → modo responsive → 375×812
2. Navegar a `/products`
3. Abrir detalle de producto

**Resultado esperado:**
- Las cards se muestran en columna única o grid de 2
- Imágenes no desbordan el viewport
- Botones son tapeables (mínimo 44px de alto)

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado
