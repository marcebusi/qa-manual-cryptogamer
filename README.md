# qa-manual-cryptogamer

![Manual QA](https://img.shields.io/badge/QA-Manual_Testing-6366f1)
![Status](https://img.shields.io/badge/Estado-Activo-22c55e)

Proyecto 7 del portfolio de QA — **Documentación de testing manual** sobre [cryptogamer-ecommerce](https://github.com/marcebusi/cryptogamer-ecommerce): test plan, casos de prueba, reportes de bugs y sesiones de testing exploratorio.

---

## Estructura del repositorio

```
qa-manual-cryptogamer/
├── test-plan/
│   └── TEST-PLAN.md              ← Estrategia, alcance, criterios de entrada/salida
├── test-cases/
│   ├── TC-AUTH.md                ← 12 casos — Autenticación
│   ├── TC-PRODUCTS.md            ← 8 casos — Catálogo de productos
│   ├── TC-CART.md                ← 7 casos — Carrito de compras
│   └── TC-CHECKOUT.md            ← 10 casos — Checkout completo
├── bug-reports/
│   ├── BUG-001.md                ← Carrito persiste post-logout
│   ├── BUG-002.md                ← Precio no congelado al agregar al carrito
│   ├── BUG-003.md                ← Sin límite de cantidad por producto
│   ├── BUG-004.md                ← Incremento de cantidad no verifica stock
│   ├── BUG-005.md                ← Estado del checkout no se resetea al cerrar
│   ├── BUG-006.md                ← Validación de email acepta "@" como válido
│   ├── BUG-007.md                ← Round-robin de vendedor es por dispositivo
│   └── BUG-008.md                ← Página 404 sin CTA de retorno (a verificar)
└── exploratory-testing/
    ├── SESSION-01.md             ← Carrito y checkout — casos de borde
    ├── SESSION-02.md             ← Autenticación y autorización
    └── SESSION-03.md             ← Responsividad y UX mobile
```

---

## Resumen ejecutivo

| Módulo | TC totales | P1 | Bugs encontrados |
|---|---|---|---|
| Autenticación | 12 | 7 | — |
| Productos | 8 | 3 | — |
| Carrito | 7 | 4 | BUG-001, BUG-002, BUG-003, BUG-004 |
| Checkout | 10 | 7 | BUG-005, BUG-006 |
| **Total** | **37** | **21** | **8 bugs (todos encontrados por código review)** |

---

## Bugs por severidad

| ID | Descripción | Severidad | Módulo |
|---|---|---|---|
| BUG-001 | Carrito no se limpia al hacer logout | P2 | Carrito |
| BUG-002 | Precio no congelado al agregar al carrito | P2 | Carrito |
| BUG-003 | Sin límite máximo de cantidad por producto | P2 | Carrito |
| BUG-004 | `addToCart` no verifica stock al incrementar | P2 | Carrito |
| BUG-005 | Estado del checkout no se resetea al cerrar | P2 | Checkout |
| BUG-006 | Email `@` pasa la validación del formulario | P2 | Checkout |
| BUG-007 | Round-robin de vendedor persiste por dispositivo | P3 | Checkout |
| BUG-008 | Página 404 sin CTA de retorno | P3 | Navegación |

---

## Metodología

### Testing basado en especificación
Los casos de prueba formales cubren:
- **Flujos positivos (happy path):** Registro exitoso, login, checkout completo
- **Flujos negativos:** Campos vacíos, datos inválidos, emails malformados
- **Casos de borde:** Tokens expirados, productos sin stock, cantidades cero
- **Seguridad básica:** Acceso sin autenticación, manipulación de localStorage

### Testing exploratorio
Tres sesiones estructuradas con charters:
1. **SESSION-01:** Carrito y checkout — casos de borde y manipulación de datos
2. **SESSION-02:** Autenticación y autorización — tokens, roles, rutas protegidas
3. **SESSION-03:** Responsividad y UX mobile — resoluciones 375px, 768px, 1280px

### Análisis de código (grey-box testing)
Los bugs BUG-001 al BUG-007 fueron identificados mediante revisión del código fuente antes de ejecutar los casos formalmente. Este enfoque grey-box permite detectar bugs antes de invertir tiempo en ejecución.

---

## Ambiente de testing

| Componente | Versión |
|---|---|
| Node.js | 20.x |
| React | 18.x |
| TypeScript | 5.x |
| Prisma | 5.x |
| SQLite | 3.x |
| Browser | Chrome 120+ |

### Setup rápido
```bash
# Backend
cd cryptogamer-ecommerce/backend
npm ci
npx prisma db push && npx prisma db seed
npm run dev  # Puerto 3001

# Frontend
cd cryptogamer-ecommerce/frontend
npm ci
npm run dev  # Puerto 5173
```

---

## Proyectos relacionados

| # | Proyecto | Repo |
|---|---|---|
| 1 | E2E con Playwright + TypeScript | [qa-playwright-cryptogamer](https://github.com/marcebusi/qa-playwright-cryptogamer) |
| 2 | API Testing con Playwright | [qa-api-testing-cryptogamer](https://github.com/marcebusi/qa-api-testing-cryptogamer) |
| 3 | E2E con Robot Framework | [qa-robot-framework-cryptogamer](https://github.com/marcebusi/qa-robot-framework-cryptogamer) |
| 4 | Performance Testing con k6 | [qa-performance-cryptogamer](https://github.com/marcebusi/qa-performance-cryptogamer) |
| 5 | Data Quality con Python | [qa-data-quality-cryptogamer](https://github.com/marcebusi/qa-data-quality-cryptogamer) |
| 6 | Pipeline CI/CD integrado | [qa-pipeline-cryptogamer](https://github.com/marcebusi/qa-pipeline-cryptogamer) |
| 7 | Manual QA (este repo) | ← aquí |

---

## Autor

**Marcelo Busignani** — QA Engineer  
[LinkedIn](https://www.linkedin.com/in/marcelo-busignani/) · [GitHub](https://github.com/marcebusi)
