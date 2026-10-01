# 🥖 SPELTA Caracas - Web App de Pedidos

Catálogo web interactivo y sistema de recepción de pedidos para la panadería artesanal **SPELTA Caracas**. Permite a los clientes armar su pedido, calcular totales en USD y Bolívares (tasa BCV) y registrar la orden simultáneamente en Google Sheets y WhatsApp.

---

## 🚀 Funcionalidades Principales

### 🛒 Para los Clientes
- **Catálogo Dinámico:** Selección de panes y galletas con límites por producto.
- **Cálculo Multimoneda:** Conversión en tiempo real de USD a Bolívares usando la tasa oficial del Banco Central de Venezuela (BCV).
- **Control de Horarios:** Deshabilitación automática del botón de pedido fuera de la jornada laboral.
- **Confirmación Integrada:** Cierre y limpieza automática del carrito/formulario tras abrir el enlace de WhatsApp.

### ⚙️ Panel de Administración (`⚙️ Admin`)
- **Gestión de Precios:** Edición en tiempo real de precios en USD.
- **Control de Stock:** Marcar productos como **Disponible** o **Agotado**.
- **Ocultar / Mostrar Productos:** Control de visibilidad (👁️/🙈) para lanzar o pausar ítems sin eliminar código.
- **Tasa BCV Manual:** Posibilidad de sobrescribir manualmente la tasa oficial del día.
- **Persistencia Local:** Los cambios del panel de control se guardan en el navegador vía `localStorage`.

---

## 📊 Integración con Google Sheets

Los pedidos se registran en Google Sheets mediante un Web App de Google Apps Script antes de redirigir al cliente a WhatsApp.

### Estructura de la Hoja de Cálculo
El script escribe automáticamente en las siguientes columnas:

| Columna | Nombre | Descripción | Ejemplo |
| :--- | :--- | :--- | :--- |
| **A** | ID Pedido | Código correlativo diario `#SP-YYMMDD-XXX` | `#SP-261001-001` |
| **B** | Fecha / Hora | Estampa de tiempo local | `1/10/2026, 3:54:49 p. m.` |
| **C** | Cliente | Nombre completo del cliente | `Joseph Joestar` |
| **D** | Modalidad Entrega | Acordar punto o retiro | `Punto de Encuentro Acordado` |
| **E** | Método de Pago | Pago Móvil, Zelle o Efectivo USD | `Efectivo USD` |
| **F** | Notas | Observaciones o detalles adicionales | `N/A` |
| **G** | Productos | Resumen concatenado del pedido | `3x Mini Galletas, 2x Pan Trenzado` |
| **H** | Total USD | Monto total en dólares | `40.50` |
| **I** | Tasa BCV | Tasa de cambio aplicada (Bs/$) | `860.18` |
| **J** | Total Bs | Monto equivalente en Bolívares | `34837.10` |

---

## 🛠️ Claves de `localStorage` Utilizadas

- `spelta_products`: Guarda las modificaciones de precio, disponibilidad y visibilidad de los productos.
- `spelta_manual_rate`: Almacena el valor de la tasa oficial forzada manualmente desde el panel de control.

---

## 📲 Flujo de Procesamiento del Pedido

1. **Validación:** Se verifica que la tienda esté dentro del horario configurado y que los campos requeridos estén llenos.
2. **Registro en Sheets:** Se realiza una petición `POST` al endpoint de Google Apps Script para almacenar la fila del pedido y generar el ID correlativo.
3. **Generación de Enlace WhatsApp:** Se construye el mensaje preformateado incluyendo el ID de la orden.
4. **Reseteo de Interfaz:** Al hacer clic en *"Abrir WhatsApp"* o cerrar el modal de confirmación, la función `finishOrder()` limpia los campos y vacía el carrito para permitir un nuevo pedido.
