# Test Plan — cryptogamer-ecommerce
**Proyecto 7 — QA Manual Portfolio**

| Campo | Detalle |
|---|---|
| **Proyecto** | cryptogamer-ecommerce |
| **Versión** | 1.0.0 |
| **Autor** | Marcelo Busignani |
| **Fecha** | 2026-10-27 |
| **Estado** | Activo |

---

## 1. Introducción

Este documento define la estrategia de testing manual para **cryptogamer-ecommerce**, una tienda online de videojuegos y consolas con frontend en React + TypeScript (puerto 5173) y backend en Node.js + Prisma + SQLite (puerto 3001).

El objetivo es garantizar que los flujos críticos de negocio funcionen correctamente desde la perspectiva del usuario final, y documentar defectos que podrían impactar la experiencia de compra.

---

## 2. Alcance

### 2.1 En scope

| Módulo | Funcionalidades |
|---|---|
| **Autenticación** | Registro, login, logout, recuperación de contraseña, cambio de contraseña |
| **Catálogo de productos** | Listado, búsqueda, filtros, detalle de producto |
| **Carrito** | Agregar, quitar, modificar cantidades, persistencia |
| **Checkout** | Flujo completo: vendedor → resumen → envío → pago → confirmación |
| **Seguimiento** | Consulta de pedido por número de orden |
| **Perfil** | Ver y editar datos del usuario |
| **Ofertas** | Listado de productos en descuento |
| **Contacto** | Formulario de contacto |

### 2.2 Out of scope

- Panel de administración (`/admin/*`)
- Panel de superadmin (`/superadmin`)
- Integración real de pagos (no hay pasarela activa)
- Performance y carga (cubiertos en Proyecto 4 con k6)
- Accesibilidad (WCAG) — pendiente para sprint futuro

---

## 3. Estrategia de Testing

### 3.1 Niveles de testing

```
Exploración libre  →  Casos de prueba formales  →  Regresión
     (ET)                    (TC)                    (subset TC)
```

### 3.2 Tipos de tests

| Tipo | Objetivo | Herramienta |
|---|---|---|
| Funcional | Verificar que cada funcionalidad cumple los requisitos | Manual + checklist |
| Negativo | Validar que el sistema maneja correctamente inputs inválidos | Manual |
| Regresión | Asegurar que cambios no rompan funcionalidades existentes | Subset de TC críticos |
| Exploratorio | Descubrir defectos no contemplados en los casos formales | Session charters |

### 3.3 Priorización (MoSCoW)

| Prioridad | Módulo | Justificación |
|---|---|---|
| **Must** | Checkout completo | Flujo core de negocio |
| **Must** | Autenticación | Todos los flujos dependen de esto |
| **Must** | Carrito | Paso previo al checkout |
| **Should** | Catálogo y búsqueda | Entrada principal de usuarios |
| **Could** | Perfil | Funcionalidad secundaria |
| **Won't** | Admin panels | Out of scope en esta iteración |

---

## 4. Criterios de Entrada y Salida

### 4.1 Criterios de entrada

- [ ] Backend corriendo en `http://localhost:3001`
- [ ] Frontend corriendo en `http://localhost:5173`
- [ ] Base de datos inicializada con `prisma db seed`
- [ ] Al menos 1 usuario CUSTOMER y 1 vendedor activo en la DB
- [ ] Al menos 5 productos con `inStock: true` disponibles

### 4.2 Criterios de salida

- [ ] 100% de casos de prueba críticos (P1) ejecutados
- [ ] 0 defectos bloqueantes (P1) sin resolver
- [ ] ≤ 2 defectos mayores (P2) abiertos con workaround documentado
- [ ] Todos los defectos reportados tienen pasos de reproducción claros
- [ ] Al menos 2 sesiones de testing exploratorio completadas

---

## 5. Ambiente de Testing

| Componente | Detalle |
|---|---|
| **OS** | Windows 11 / macOS 14+ |
| **Browsers** | Chrome 120+ (principal), Firefox 120+ (secundario) |
| **Resoluciones** | Desktop: 1920×1080, Tablet: 768×1024, Mobile: 375×812 |
| **Backend** | Node.js 20 + Prisma + SQLite |
| **Frontend** | Vite + React 18 + TypeScript |
| **Variables de entorno** | `VITE_API_URL=http://localhost:3001` |

### 5.1 Datos de prueba

| Rol | Email | Contraseña |
|---|---|---|
| Customer | test.customer@cg.com | Test1234! |
| Admin | test.admin@cg.com | Admin1234! |

> Crear con `POST /api/auth/register` o mediante seed personalizado.

---

## 6. Roles y Responsabilidades

| Rol | Responsable | Tarea |
|---|---|---|
| QA Engineer | Marcelo Busignani | Diseño y ejecución de casos, reporte de bugs |
| Dev (referencia) | — | Clarificación de comportamiento esperado, fix de bugs |
| Product Owner (referencia) | — | Validación de criterios de aceptación |

---

## 7. Riesgos y Mitigación

| Riesgo | Impacto | Probabilidad | Mitigación |
|---|---|---|---|
| Ambiente local inestable (DB corrupta) | Alto | Medio | Script de reset: `prisma db push --force-reset && prisma db seed` |
| Datos de prueba insuficientes | Medio | Bajo | Seed extendido con más productos y órdenes |
| Sin pasarela de pago real | Alto | Alto | Verificar flujo hasta confirmación, sin transacción real |
| Cambios en la API durante testing | Medio | Bajo | Fijar versión del repo antes de ejecutar |

---

## 8. Herramientas

| Herramienta | Uso |
|---|---|
| Chrome DevTools | Inspección de red, consola, localStorage |
| Postman / Thunder Client | Verificación directa de endpoints API |
| SQLite Browser | Inspección de la base de datos |
| Markdown | Documentación de casos y bugs |
| GitHub Issues | Tracking de bugs reportados |

---

## 9. Resumen de Casos de Prueba

| Módulo | Archivo | Total TC | P1 | P2 | P3 |
|---|---|---|---|---|---|
| Autenticación | TC-AUTH.md | 12 | 7 | 4 | 1 |
| Productos | TC-PRODUCTS.md | 8 | 3 | 4 | 1 |
| Carrito | TC-CART.md | 7 | 4 | 2 | 1 |
| Checkout | TC-CHECKOUT.md | 10 | 7 | 2 | 1 |
| **Total** | | **37** | **21** | **12** | **4** |

---

## 10. Cronograma estimado

| Actividad | Estimado |
|---|---|
| Setup de ambiente | 30 min |
| Ejecución TC — Autenticación | 1.5 hs |
| Ejecución TC — Productos | 45 min |
| Ejecución TC — Carrito | 45 min |
| Ejecución TC — Checkout | 2 hs |
| Sesiones exploratorias (×3) | 2.5 hs |
| Reporte de bugs | 1 hs |
| **Total** | **~9 hs** |

---

## 11. Definición de Severidad de Bugs

| Severidad | Criterio | Ejemplo |
|---|---|---|
| **P1 — Bloqueante** | Impide completar un flujo crítico de negocio | No se puede confirmar un pedido |
| **P2 — Mayor** | Degrada significativamente la experiencia, tiene workaround | Precio del carrito cambia silenciosamente |
| **P3 — Menor** | Problema de UX o cosmético que no bloquea flujos | Texto cortado en mobile |
| **P4 — Trivial** | Sugerencia de mejora | Typo en un label |
