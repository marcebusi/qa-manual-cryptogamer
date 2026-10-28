# Casos de Prueba — Módulo: Carrito de Compras

**Módulo:** Carrito  
**Archivo:** TC-CART.md  
**Total de casos:** 7

---

## TC-CART-001 — Agregar producto al carrito
**Prioridad:** P1 | **Tipo:** Positivo

**Precondición:** Producto con `inStock: true` disponible

**Pasos:**
1. Navegar al detalle de un producto en stock
2. Hacer clic en "Agregar al carrito"

**Resultado esperado:**
- El producto aparece en el carrito (icono con badge +1)
- `cart_items` en localStorage contiene el producto
- Contador del carrito en el header se actualiza

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-CART-002 — Agregar mismo producto incrementa cantidad
**Prioridad:** P1 | **Tipo:** Positivo

**Precondición:** Producto ya agregado al carrito (qty: 1)

**Pasos:**
1. Hacer clic en "Agregar al carrito" nuevamente en el mismo producto
   _O_ abrir carrito y usar botón "+"

**Resultado esperado:**
- Quantity del producto se incrementa a 2
- Subtotal se actualiza correctamente

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-CART-003 — Eliminar producto del carrito
**Prioridad:** P1 | **Tipo:** Positivo

**Precondición:** Al menos un producto en el carrito

**Pasos:**
1. Abrir el carrito lateral o panel de carrito
2. Hacer clic en el botón de eliminar (×) del producto

**Resultado esperado:**
- Producto eliminado del carrito
- `cart_items` en localStorage ya no contiene el producto
- Si el carrito queda vacío, se muestra mensaje "Tu carrito está vacío"

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-CART-004 — Carrito persiste tras refresh de página
**Prioridad:** P1 | **Tipo:** Positivo

**Precondición:** Producto en el carrito

**Pasos:**
1. Agregar un producto al carrito
2. Recargar la página (F5)

**Resultado esperado:**
- El producto sigue en el carrito (persiste en localStorage)
- Cantidad y precio son los mismos

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-CART-005 — Intentar agregar producto sin stock
**Prioridad:** P1 | **Tipo:** Negativo

**Precondición:** Producto con `inStock: false` en DB

**Pasos:**
1. Navegar al detalle del producto sin stock
2. Intentar agregar al carrito (si el botón está presente)

**Resultado esperado:**
- El botón está deshabilitado o ausente
- Si por algún medio se intenta, no se agrega al carrito

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-CART-006 — [BUG CONOCIDO] Carrito no se limpia al cerrar sesión
**Prioridad:** P2 | **Tipo:** Negativo / Seguridad

**Precondición:** Usuario autenticado con productos en el carrito

**Pasos:**
1. Autenticarse como `test.customer@cg.com`
2. Agregar 2 productos al carrito
3. Cerrar sesión (logout)
4. Verificar localStorage en DevTools → Application → Local Storage

**Resultado esperado:**
- `cart_items` debería limpiarse al hacer logout (buena práctica de privacidad)

**Resultado real (bug):**
- `cart_items` permanece en localStorage después del logout
- Un usuario diferente en el mismo dispositivo puede ver el carrito anterior

**Estado:** 🔴 Falla — ver BUG-001

---

## TC-CART-007 — Modificar cantidad con controles +/-
**Prioridad:** P3 | **Tipo:** Positivo

**Precondición:** Producto en carrito con qty: 1

**Pasos:**
1. Abrir el carrito
2. Hacer clic en "+" tres veces (qty: 1 → 4)
3. Hacer clic en "-" dos veces (qty: 4 → 2)

**Resultado esperado:**
- Quantity refleja el valor correcto en cada paso
- Subtotal se recalcula correctamente
- Al llegar a qty: 0, el producto se elimina del carrito

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado
