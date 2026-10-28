# Casos de Prueba — Módulo: Checkout

**Módulo:** Checkout  
**Archivo:** TC-CHECKOUT.md  
**Total de casos:** 10

---

## TC-CHK-001 — Flujo completo de checkout (happy path)
**Prioridad:** P1 | **Tipo:** Positivo / E2E

**Precondición:** Usuario autenticado, 2 productos en carrito, al menos 1 vendedor activo

**Pasos:**
1. Abrir el carrito y hacer clic en "Checkout" o "Comprar"
2. **Paso 1 — Vendedor:** Seleccionar "Elegir al azar"
3. **Paso 2 — Resumen:** Verificar productos y totales, clic en "Continuar"
4. **Paso 3 — Envío:** 
   - Completar Nombre: `Test User`
   - Email: `test@test.com`
   - Teléfono: `1122334455`
   - Seleccionar método de envío disponible
   - Clic en "Continuar al pago"
5. **Paso 4 — Pago:** Seleccionar "Efectivo", clic en "Confirmar pedido"

**Resultado esperado:**
- Paso 5 (success) visible con número de orden
- Orden creada en DB con status `PENDING`
- Carrito limpiado al cerrar el modal
- Link "Seguir pedido" funcional

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-CHK-002 — Checkout con pago por transferencia
**Prioridad:** P1 | **Tipo:** Positivo

**Precondición:** Vendedor con `alias` configurado

**Pasos:**
1. Llegar al Paso 4 — Pago
2. Seleccionar "Transferencia bancaria"
3. Verificar que se muestran los datos del vendedor (alias/CBU)
4. Confirmar pedido

**Resultado esperado:**
- Datos bancarios del vendedor visibles
- Posibilidad de cargar comprobante de transferencia
- Orden creada con `paymentMethod: "transfer"`

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-CHK-003 — Checkout con pago por USDT
**Prioridad:** P1 | **Tipo:** Positivo

**Precondición:** Vendedor con `cryptoAlias` configurado

**Pasos:**
1. Llegar al Paso 4 — Pago
2. Seleccionar "USDT (Crypto)"
3. Confirmar pedido

**Resultado esperado:**
- Dirección de wallet del vendedor visible con botón de copiar
- Indicación de red (TRC-20 / ERC-20)
- Posibilidad de cargar hash de transacción

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-CHK-004 — Validación de formulario de envío — campos vacíos
**Prioridad:** P1 | **Tipo:** Negativo

**Pasos:**
1. Llegar al Paso 3 — Envío
2. Dejar todos los campos vacíos
3. Hacer clic en "Continuar al pago"

**Resultado esperado:**
- Mensajes de error en campos obligatorios: Nombre, Email, Teléfono
- No avanza al Paso 4

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-CHK-005 — [BUG CONOCIDO] Validación de email con formato mínimo
**Prioridad:** P1 | **Tipo:** Negativo

**Pasos:**
1. Llegar al Paso 3 — Envío
2. Ingresar en Email: `@` (solo el símbolo arroba)
3. Completar Nombre y Teléfono válidos
4. Seleccionar un método de envío
5. Hacer clic en "Continuar al pago"

**Resultado esperado:**
- Error de validación: "Email inválido"
- No avanza al paso siguiente

**Resultado real (bug):**
- El campo pasa la validación porque `"@".includes('@')` es `true`
- Se puede proceder con un email inválido

**Estado:** 🔴 Falla — ver BUG-006

---

## TC-CHK-006 — Checkout sin seleccionar método de pago
**Prioridad:** P1 | **Tipo:** Negativo

**Pasos:**
1. Llegar al Paso 4 — Pago
2. NO seleccionar ningún método de pago
3. Intentar hacer clic en "Confirmar pedido"

**Resultado esperado:**
- Botón deshabilitado o mensaje de error "Seleccioná un método de pago"
- No se envía la orden

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-CHK-007 — Cerrar modal en paso intermedio no pierde datos
**Prioridad:** P2 | **Tipo:** Positivo

**Pasos:**
1. Iniciar checkout y llegar al Paso 3 — Envío
2. Completar nombre, email y teléfono
3. Cerrar el modal (botón X o click en backdrop)
4. Reabrir el checkout

**Resultado esperado:**
- El modal empieza desde el Paso 1 (reset del estado)
- Los campos del formulario están vacíos (no hay datos residuales de otro flujo)

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-CHK-008 — Seguimiento de pedido por número de orden
**Prioridad:** P1 | **Tipo:** Positivo

**Precondición:** Orden creada con número de orden conocido

**Pasos:**
1. Obtener número de orden del paso success (ej: `CG-0001`)
2. Navegar a `/seguimiento/CG-0001`

**Resultado esperado:**
- Detalles de la orden visibles: status, productos, método de pago
- Sin errores en la página

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-CHK-009 — Seguimiento con número de orden inexistente
**Prioridad:** P2 | **Tipo:** Negativo

**Pasos:**
1. Navegar a `/seguimiento/ORDEN-FALSA-99999`

**Resultado esperado:**
- Mensaje "No se encontró la orden" o similar
- Sin crash de la aplicación

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-CHK-010 — Cargar comprobante de transferencia
**Prioridad:** P3 | **Tipo:** Positivo

**Precondición:** Orden confirmada con pago por transferencia

**Pasos:**
1. En la pantalla de éxito, en la sección de transferencia
2. Hacer clic en "Cargar comprobante"
3. Seleccionar un archivo de imagen (JPG < 2MB)

**Resultado esperado:**
- Spinner de carga visible durante la subida
- Imagen del comprobante visible tras subir
- Mensaje de confirmación: "Comprobante recibido"
- La URL del comprobante queda ligada a la orden en DB

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado
