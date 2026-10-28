# Sesión Exploratoria 03 — Responsividad y UX Mobile

| Campo | Detalle |
|---|---|
| **Sesión** | SESSION-03 |
| **Área** | UI/UX, Responsividad, Accesibilidad básica |
| **Tester** | Marcelo Busignani |
| **Fecha** | 2026-10-27 |
| **Duración** | 45 minutos |
| **Objetivo** | Explorar la experiencia en dispositivos móviles y tabletas, identificar problemas de responsividad, elementos de difícil uso con dedos, y texto/imágenes que se corten o desordenen |

---

## Charter

> **Explorar** la interfaz de cryptogamer-ecommerce en resoluciones mobile y tablet  
> **Con el objetivo de** identificar problemas de UX que impidan o dificulten completar una compra desde un dispositivo móvil  
> **Para encontrar** elementos no responsivos, overlaps, texto ilegible o CTAs muy pequeños

---

## Setup

- [ ] Chrome DevTools → Toggle Device Toolbar
- [ ] Resoluciones a probar: 375×812 (iPhone SE), 390×844 (iPhone 14), 768×1024 (iPad)
- [ ] No requiere backend activo para la mayor parte (verificación visual)

---

## Notas de Exploración

### [00:00–15:00] Home y catálogo

**Resolución: 375×812**

**Acción:** Navegar a `/`  
**Observación:** _(registrar si el hero, el navbar y las cards del home se adaptan)_

---

**Acción:** Navegar a `/products`  
**Observación:** _(registrar el layout del grid de productos — ¿1 columna, 2 columnas?)_  
**Criterio:** El texto de los nombres de producto no debería cortarse bruscamente. Las imágenes deberían ser cuadradas o proporcionales.

---

**Acción:** Abrir el detalle de un producto  
**Observación:** _(verificar que el botón "Agregar al carrito" tiene al menos 44px de alto — estándar de accesibilidad táctil)_

---

**Acción:** Intentar usar el buscador o filtros en mobile  
**Observación:** _(verificar que el input de búsqueda sea tapeable sin hacer zoom — font-size mínimo 16px para evitar zoom automático en iOS)_

---

### [15:00–30:00] Carrito y Checkout en mobile

**Acción:** Agregar un producto al carrito y abrir el panel/modal del carrito  
**Observación:** _(verificar que el carrito no tape el 100% del viewport sin posibilidad de scroll)_

---

**Acción:** Iniciar el flujo de checkout en 375×812  
**Observación:** El modal de checkout usa `items-end sm:items-center` — en mobile se ancla al fondo como un bottom sheet.

Verificar:
- [ ] El modal ocupa el ancho completo
- [ ] El scroll interno funciona sin que el fondo scrollee también
- [ ] Los botones de "Continuar" y "Confirmar" son fácilmente tapeables
- [ ] El teclado virtual no tapa los inputs del formulario de envío

---

**Acción:** Completar el formulario de envío en mobile — anotar si el teclado virtual tapa algún campo crítico  
**Observación:** _(registrar específicamente si el campo "Código postal" queda oculto)_

---

**Acción:** Paso 4 — Pago: seleccionar una opción de pago en mobile  
**Observación:** _(los botones de método de pago tienen texto largo — verificar que no se corten o desborden en 375px)_

---

### [30:00–45:00] Navegación y elementos globales

**Acción:** Verificar el navbar en mobile  
**Observación:** _(existe hamburger menu? Los links de nav son accesibles? El logo lleva al home?)_

---

**Acción:** Verificar el footer en mobile  
**Observación:** _(el footer es legible en 375px? Los links tienen suficiente espacio entre sí?)_

---

**Acción:** Navegar a `/seguimiento/ORDEN` en mobile  
**Observación:** _(verificar que la tabla de detalles de la orden hace scroll horizontal si es muy ancha, no se desborda del viewport)_

---

**Resolución: 768×1024 (iPad)**

**Acción:** Abrir `/products` en iPad  
**Observación:** _(el grid debería mostrar 2-3 columnas, no 1 ni 4)_

---

## Hallazgos

| # | Descripción | Resolución | Severidad |
|---|---|---|---|
| H-01 | _(completar al ejecutar)_ | 375×812 | — |
| H-02 | _(completar al ejecutar)_ | 390×844 | — |
| H-03 | _(completar al ejecutar)_ | 768×1024 | — |

---

## Checklist de responsividad

| Elemento | 375px | 768px | 1280px |
|---|---|---|---|
| Navbar / header | ⬜ | ⬜ | ⬜ |
| Grid de productos | ⬜ | ⬜ | ⬜ |
| Detalle de producto | ⬜ | ⬜ | ⬜ |
| Panel del carrito | ⬜ | ⬜ | ⬜ |
| Modal de checkout | ⬜ | ⬜ | ⬜ |
| Formulario de envío | ⬜ | ⬜ | ⬜ |
| Pantalla de éxito | ⬜ | ⬜ | ⬜ |
| Footer | ⬜ | ⬜ | ⬜ |

> Leyenda: ✅ OK | ❌ Problema | ⚠️ Aceptable con observación
