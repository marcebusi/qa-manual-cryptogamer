# Casos de Prueba — Módulo: Autenticación

**Módulo:** Autenticación  
**Archivo:** TC-AUTH.md  
**Total de casos:** 12  

---

## TC-AUTH-001 — Registro exitoso con datos válidos
**Prioridad:** P1 | **Tipo:** Positivo

| Campo | Detalle |
|---|---|
| **Precondición** | El email no existe en la base de datos |
| **URL** | `/` — modal de registro accesible desde el header |

**Pasos:**
1. Hacer clic en "Registrarse" en el header
2. Completar nombre: `Test User`
3. Completar email: `newuser@test.com`
4. Completar contraseña: `Test1234!`
5. Hacer clic en "Crear cuenta"

**Resultado esperado:**
- El usuario queda autenticado (token en localStorage `cg_token`)
- Se muestra el nombre del usuario en el header
- Redirige a la página principal o cierra el modal

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-AUTH-002 — Login exitoso con credenciales válidas
**Prioridad:** P1 | **Tipo:** Positivo

**Precondición:** Usuario con `test.customer@cg.com` / `Test1234!` existe en DB

**Pasos:**
1. Hacer clic en "Iniciar sesión" en el header
2. Ingresar email: `test.customer@cg.com`
3. Ingresar contraseña: `Test1234!`
4. Hacer clic en "Ingresar"

**Resultado esperado:**
- Token `cg_token` guardado en localStorage
- Header muestra nombre de usuario
- Modal se cierra

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-AUTH-003 — Login con contraseña incorrecta
**Prioridad:** P1 | **Tipo:** Negativo

**Precondición:** Usuario `test.customer@cg.com` existe en DB

**Pasos:**
1. Abrir modal de login
2. Ingresar email: `test.customer@cg.com`
3. Ingresar contraseña: `WrongPass999`
4. Hacer clic en "Ingresar"

**Resultado esperado:**
- Se muestra mensaje de error ("Credenciales inválidas" o similar)
- No se guarda token en localStorage
- Modal permanece abierto

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-AUTH-004 — Login con email inexistente
**Prioridad:** P1 | **Tipo:** Negativo

**Pasos:**
1. Abrir modal de login
2. Ingresar email: `noexiste@fake.com`
3. Ingresar contraseña: `cualquier`
4. Hacer clic en "Ingresar"

**Resultado esperado:**
- Mensaje de error visible
- Sin token en localStorage

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-AUTH-005 — Registro con email ya registrado
**Prioridad:** P1 | **Tipo:** Negativo

**Precondición:** `test.customer@cg.com` ya existe en DB

**Pasos:**
1. Abrir modal de registro
2. Completar con email: `test.customer@cg.com`
3. Completar nombre y contraseña válidos
4. Hacer clic en "Crear cuenta"

**Resultado esperado:**
- Error: "El email ya está registrado" o similar
- No se crea una cuenta duplicada

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-AUTH-006 — Registro con contraseña débil
**Prioridad:** P2 | **Tipo:** Negativo

**Pasos:**
1. Abrir modal de registro
2. Completar nombre y email válido nuevo
3. Ingresar contraseña: `123`
4. Hacer clic en "Crear cuenta"

**Resultado esperado:**
- Error de validación indicando requisitos mínimos de contraseña
- No se crea la cuenta

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-AUTH-007 — Logout limpia la sesión
**Prioridad:** P1 | **Tipo:** Positivo

**Precondición:** Usuario autenticado

**Pasos:**
1. Hacer clic en el avatar/nombre del usuario en el header
2. Seleccionar "Cerrar sesión"

**Resultado esperado:**
- `cg_token` eliminado de localStorage
- Header muestra botones de login/registro
- Rutas protegidas (`/perfil`, `/admin`) redirigen a home

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-AUTH-008 — Acceso a /perfil sin autenticar
**Prioridad:** P1 | **Tipo:** Seguridad

**Precondición:** Usuario NO autenticado (sin token)

**Pasos:**
1. Navegar directamente a `http://localhost:5173/perfil`

**Resultado esperado:**
- Redirigir a home o mostrar modal de login
- No mostrar datos de perfil de ningún usuario

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-AUTH-009 — Recuperación de contraseña — envío de email
**Prioridad:** P2 | **Tipo:** Positivo

**Precondición:** `test.customer@cg.com` existe en DB

**Pasos:**
1. Abrir modal de login
2. Hacer clic en "¿Olvidaste tu contraseña?"
3. Ingresar email: `test.customer@cg.com`
4. Hacer clic en "Enviar"

**Resultado esperado:**
- Confirmación visual de envío ("Se envió un email" o similar)
- Endpoint `POST /api/auth/forgot-password` responde 200

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-AUTH-010 — Reset de contraseña con token válido
**Prioridad:** P2 | **Tipo:** Positivo

**Precondición:** Token de reset válido obtenido (via DB o email)

**Pasos:**
1. Navegar a `/reset-password?token=<TOKEN_VÁLIDO>`
2. Ingresar nueva contraseña: `NewPass1234!`
3. Confirmar contraseña
4. Hacer clic en "Cambiar contraseña"

**Resultado esperado:**
- Confirmación de cambio exitoso
- Login posible con la nueva contraseña

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-AUTH-011 — Reset con token expirado o inválido
**Prioridad:** P2 | **Tipo:** Negativo

**Pasos:**
1. Navegar a `/reset-password?token=token-invalido-12345`
2. Completar formulario con nueva contraseña
3. Hacer clic en "Cambiar contraseña"

**Resultado esperado:**
- Error: token inválido o expirado
- No cambia ninguna contraseña

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado

---

## TC-AUTH-012 — Persistencia de sesión tras refresh
**Prioridad:** P3 | **Tipo:** Positivo

**Precondición:** Usuario autenticado

**Pasos:**
1. Autenticarse con credenciales válidas
2. Recargar la página (F5)

**Resultado esperado:**
- Usuario sigue autenticado (token persiste en localStorage)
- Header muestra nombre de usuario

**Resultado real:** _(completar al ejecutar)_  
**Estado:** ⬜ No ejecutado
