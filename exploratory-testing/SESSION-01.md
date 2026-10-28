# Sesión Exploratoria 01 — Flujo de Carrito y Checkout

| Campo | Detalle |
|---|---|
| **Sesión** | SESSION-01 |
| **Área** | Carrito de compras + Checkout |
| **Tester** | Marcelo Busignani |
| **Fecha** | 2026-10-27 |
| **Duración** | 60 minutos |
| **Objetivo** | Explorar el comportamiento del carrito y el flujo de checkout buscando casos límite, inconsistencias de datos y oportunidades de manipulación |

---

## Charter

> **Explorar** el módulo de carrito y el flujo de checkout completo  
> **Con el objetivo de** descubrir comportamientos inesperados en los límites de los datos, el manejo de estado y la validación de formularios  
> **Para encontrar** defectos que no estarían cubiertos por los casos de prueba positivos

---

## Setup

- [ ] Backend corriendo: `http://localhost:3001`
- [ ] Frontend corriendo: `http://localhost:5173`
- [ ] DevTools abierto en pestaña Application (localStorage) y Network
- [ ] Usuario autenticado: `test.customer@cg.com`
- [ ] Al menos 3 productos en stock disponibles

---

## Notas de Exploración

### [00:00–10:00] Carrito básico

**Acción:** Agregar múltiples productos rápidamente (click spam en "Agregar")  
**Observación:** Cada click agrega +1 sin debounce. Con clicks muy rápidos, la cantidad puede saltar de forma inconsistente dependiendo de la velocidad del setState.  
**Severidad estimada:** P3

---

**Acción:** Abrir carrito, luego abrir pestaña adicional con el mismo usuario. Modificar el carrito en una pestaña.  
**Observación:** La segunda pestaña no refleja los cambios del carrito en tiempo real (no hay sincronización entre pestañas via localStorage events).  
**Severidad estimada:** P3 — No bloquea pero puede causar confusión.

---

**Acción:** Verificar `cart_items` en localStorage antes y después de logout  
**Observación:** `cart_items` permanece después del logout → **BUG-001 confirmado**

---

### [10:00–25:00] Formulario de envío

**Acción:** Ingresar email `@` en el campo email  
**Observación:** El sistema acepta el valor y avanza al paso de pago → **BUG-006 confirmado**

---

**Acción:** Ingresar datos con caracteres especiales en el campo Nombre: `<script>alert(1)</script>`  
**Observación:** El campo acepta el valor (es esperado ya que es texto libre). Verificar que el backend lo sanitice antes de guardarlo.  
**Acción de seguimiento:** Verificar el dato en la DB y cómo se muestra en el panel admin.

---

**Acción:** Copiar y pegar un número de teléfono con espacios: `011 4444-5555`  
**Observación:** El campo acepta espacios y guiones. El backend puede o no normalizar el número. A verificar.

---

**Acción:** Intentar enviar el formulario sin seleccionar método de envío (aunque los campos estén completos)  
**Observación:** El botón "Continuar al pago" muestra estado deshabilitado ("Elegí un envío para continuar"). Funciona correctamente.  
**Resultado:** PASS

---

### [25:00–45:00] Proceso de pago y confirmación

**Acción:** Seleccionar "Transferencia bancaria" → Confirmar pedido → Intentar subir un archivo de 15MB como comprobante  
**Observación:** El backend debería tener un límite de tamaño. A verificar si hay validación del lado cliente primero.  
**Acción de seguimiento:** Revisar el endpoint `/api/upload/comprobante` para ver si valida tamaño/tipo MIME.

---

**Acción:** Confirmar pedido → En la pantalla de éxito, hacer clic en "Seguir pedido" en una nueva pestaña y luego cerrar el modal original  
**Observación:** El modal se cierra y el carrito se limpia (correcto). El número de orden en la URL es correcto.  
**Resultado:** PASS

---

**Acción:** Intentar cerrar el modal en distintos pasos con el botón X y con ESC  
**Observación:** ESC cierra el modal en todos los pasos. Pero el estado no se resetea (pasos 1-4) → **BUG-005 confirmado**

---

**Acción:** Seleccionar un vendedor manualmente → Confirmar la orden → Volver a abrir checkout → ¿El mismo vendedor sigue seleccionado?  
**Observación:** No — el vendedor no persiste entre aperturas del modal (solo dentro de la misma apertura). Correcto.

---

### [45:00–60:00] Casos de borde y manipulación

**Acción:** Modificar `cart_items` en localStorage para poner `quantity: 0`  
```javascript
let items = JSON.parse(localStorage.getItem('cart_items'))
items[0].quantity = 0
localStorage.setItem('cart_items', JSON.stringify(items))
location.reload()
```
**Observación:** El carrito muestra qty: 0 pero el producto sigue en la lista. El subtotal queda en $0 para ese producto. Posible comportamiento inesperado.

---

**Acción:** Modificar `cart_items` en localStorage para poner `price: -100` (precio negativo)  
```javascript
let items = JSON.parse(localStorage.getItem('cart_items'))
items[0].product.price = -100
localStorage.setItem('cart_items', JSON.stringify(items))
location.reload()
```
**Observación:** El total del carrito disminuye por el precio negativo. El checkout acepta el total reducido. La orden se crea con el total manipulado.  
**Severidad estimada:** P2 — Requiere validación de precio en el backend antes de guardar la orden.

---

## Hallazgos

| # | Descripción | Severidad | Relacionado con |
|---|---|---|---|
| H-01 | BUG-001 confirmado: `cart_items` persiste post-logout | P2 | BUG-001 |
| H-02 | BUG-005 confirmado: estado del modal no se resetea al cerrar | P2 | BUG-005 |
| H-03 | BUG-006 confirmado: email `@` pasa validación | P2 | BUG-006 |
| H-04 | `quantity: 0` en localStorage no elimina el item del carrito | P3 | Nuevo |
| H-05 | Precio manipulado en localStorage afecta el total del pedido | P2 | Nuevo |
| H-06 | Sin validación de tamaño de archivo en el cliente para comprobante | P3 | A investigar |

---

## Deuda / Próximos pasos

- [ ] Verificar que el backend valide el precio del pedido contra la DB (no confiar en el precio del cliente)
- [ ] Revisar endpoint `POST /api/upload/comprobante` para límites de tamaño y tipos MIME permitidos
- [ ] Verificar comportamiento de `quantity: 0` en el cart render
