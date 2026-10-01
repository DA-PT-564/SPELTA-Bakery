# SPELTA Caracas — Panadería Artesanal (Web App Estática)

## Breve descripción
Catálogo web liviano para toma de pedidos directos por WhatsApp con registro previo automático en Google Sheets. Diseñado para un microemprendimiento artesanal sin local físico ni delivery propio.

## Stack Técnico
- **HTML5 + Vanilla JavaScript (ES6)** (Sin frameworks complejos).
- **Tailwind CSS** vía CDN (`https://cdn.tailwindcss.com`).
- **Google Apps Script (Web App API)** como backend serverless para persistencia en Google Sheets.
- **LocalStorage** para persistencia local de configuraciones de administración.

## Arquitectura y Funcionalidades Clave

1. **Consulta de Tasa BCV:**
   - Consume `https://ve.dolarapi.com/v1/dolares/oficial` en tiempo real.
   - Permite sobreescribir la tasa manualmente desde el panel Admin.

2. **Control de Horario Comercial:**
   - Objeto `SCHEDULE_CONFIG` define días (Lunes-Viernes) y horas (8 AM - 6 PM).
   - Bloquea el botón de envío y muestra un banner fuera de horario.

3. **Panel de Administración (Modal Oculto):**
   - Acceso vía botón `⚙️ Admin` con clave predeterminada (`1234`).
   - Permite modificar **precios en USD** de productos individualmente.
   - Permite alternar disponibilidad (**Disponible / Agotado**).
   - Los cambios persisten en el navegador del administrador mediante `localStorage` (`spelta_products`, `spelta_manual_rate`).

4. **Generador de ID de Pedido:**
   - Formato: `SP-YYMMDD-XXX` (Ej: `SP-260930-001`).
   - Mantiene un contador correlativo diario en `localStorage`.

5. **Flujo de Envío de Pedido:**
   - **Paso 1:** Valida campos requeridos (Nombre, Carrito con ítems).
   - **Paso 2:** Genera ID único.
   - **Paso 3:** Envía payload `JSON` vía `fetch` (POST `no-cors`) a `GOOGLE_SHEETS_URL`.
   - **Paso 4:** Redirige al cliente a WhatsApp (`wa.me`) con el resumen formateado.

6. **Logística de Entrega:**
   - Opciones: "Punto de Encuentro Acordado" y "Retiro Previa Cita".

## Estructura de Datos (Products Array)
Cada objeto de producto contiene: `{ id, category, name, price, available, maxQty }`.
