# Sesión Exploratoria 02 — Autenticación y Autorización

| Campo | Detalle |
|---|---|
| **Sesión** | SESSION-02 |
| **Área** | Autenticación, Autorización, Rutas protegidas |
| **Tester** | Marcelo Busignani |
| **Fecha** | 2026-10-27 |
| **Duración** | 45 minutos |
| **Objetivo** | Explorar los límites del sistema de autenticación: tokens, roles, acceso a rutas protegidas y comportamientos del borde |

---

## Charter

> **Explorar** el sistema de autenticación y control de acceso por roles  
> **Con el objetivo de** verificar que las rutas protegidas no sean accesibles sin las credenciales correctas  
> **Para encontrar** brechas de autorización, manejo inseguro de tokens o comportamientos inesperados en el flujo de auth

---

## Setup

- [ ] Backend corriendo
- [ ] Frontend corriendo
- [ ] DevTools → Network y Application (localStorage)
- [ ] Dos cuentas disponibles: una CUSTOMER, una ADMIN

---

## Notas de Exploración

### [00:00–15:00] Acceso a rutas protegidas

**Acción:** Sin autenticar, navegar a `http://localhost:5173/perfil`  
**Observación:** _(a registrar al ejecutar)_  
**Resultado esperado:** Redirect o modal de login

---

**Acción:** Sin autenticar, navegar a `http://localhost:5173/admin/dashboard`  
**Observación:** _(a registrar al ejecutar)_  
**Resultado esperado:** Redirect a home, no exponer el panel admin

---

**Acción:** Autenticarse como CUSTOMER y navegar a `/admin/dashboard`  
**Observación:** _(a registrar al ejecutar)_  
**Resultado esperado:** Acceso denegado o redirect — el rol CUSTOMER no tiene permisos de admin

---

**Acción:** Tomar el token JWT de un usuario CUSTOMER desde localStorage. Decodificar el payload con base64 (sin verificación). Observar el rol.  
```javascript
const token = localStorage.getItem('cg_token')
const payload = JSON.parse(atob(token.split('.')[1]))
console.log(payload)
```
**Observación:** _(registrar el payload decodificado — no contiene la firma, solo los datos)_  
**Notas de seguridad:** Solo observación/análisis, no modificación del token.

---

### [15:00–30:00] Sesión y expiración

**Acción:** Autenticarse, esperar (o simular expiración del token modificando manualmente la fecha `exp` en DevTools — solo observable, no modificable sin la firma)  
**Observación:** Verificar qué pasa cuando el token expira mientras el usuario está en la página. ¿La app muestra un error? ¿Hace redirect automático?

---

**Acción:** Autenticarse, copiar el token, cerrar sesión, pegar el token de vuelta en localStorage manualmente  
```javascript
localStorage.setItem('cg_token', '<TOKEN_COPIADO>')
location.reload()
```
**Observación:** Verificar si la app acepta el token de vuelta (token reuse post-logout)  
**Resultado esperado:** La sesión debería no ser válida si el backend invalida tokens al logout (blacklist). Si no, el token vuelve a funcionar.

---

**Acción:** Registrar un usuario nuevo → Login → Verificar que el token en `cg_token` es un JWT válido (3 partes separadas por `.`)  
```javascript
const parts = localStorage.getItem('cg_token').split('.')
console.log('Parts:', parts.length) // debe ser 3
```
**Observación:** _(registrar resultado)_

---

### [30:00–45:00] Comportamiento en múltiples pestañas

**Acción:** Abrir dos pestañas del mismo origen. Loguearse en pestaña 1. Verificar si pestaña 2 detecta el cambio de sesión  
**Observación:** El Storage Event puede o no propagarse. Si no se usa, la pestaña 2 mostrará "sin sesión" hasta que se recargue.

---

**Acción:** Cerrar sesión en pestaña 1. Ver qué pasa en pestaña 2 sin recargar  
**Observación:** La pestaña 2 probablemente muestre el usuario logueado hasta que se recargue, ya que AuthContext está en memoria.  
**Severidad estimada:** P3 — Comportamiento esperado en SPAs sin sincronización cross-tab

---

**Acción:** Intentar acceder a `/perfil` con un token inválido manualmente puesto en localStorage  
```javascript
localStorage.setItem('cg_token', 'token.invalido.manipulado')
location.reload()
```
**Observación:** _(registrar el comportamiento — ¿crash, redirect, mensaje de error?)_  
**Resultado esperado:** La app debería manejar el token inválido gracefully, limpiarlo y mostrar la vista de no autenticado

---

## Hallazgos

| # | Descripción | Severidad | Estado |
|---|---|---|---|
| H-01 | Token JWT no se invalida en el backend al hacer logout (blacklist no implementada) | P2 | A confirmar |
| H-02 | Sin sincronización de sesión entre pestañas del mismo origen | P3 | Comportamiento esperado en SPA |
| H-03 | Token inválido en localStorage — verificar manejo graceful | P2 | A ejecutar |

---

## Deuda / Próximos pasos

- [ ] Confirmar si el backend implementa token blacklist o expiración corta para mitigar token reuse
- [ ] Verificar que rutas de admin sean inaccesibles para CUSTOMER (middleware de rol en el frontend y backend)
- [ ] Documentar la política de expiración de tokens JWT (`exp` claim)
