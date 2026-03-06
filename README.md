<p align="center">
  <img src="https://replanta.net/wp-content/uploads/2025/12/icono.png" alt="Replanta" width="100" style="border-radius: 50%; background: #1a1a2e; padding: 10px;">
</p>

<h1 align="center">SAP Woo Suite</h1>

<p align="center">
  <strong>Plataforma multicanal para SAP Business One</strong><br>
  Sincroniza WooCommerce y canales de venta con SAP B1 via Service Layer<br>
  Desarrollado por <a href="https://replanta.net">Replanta</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/versi%C3%B3n-2.11.0-blue" alt="Versión">
  <img src="https://img.shields.io/badge/WooCommerce-6.0%2B-purple" alt="WooCommerce">
  <img src="https://img.shields.io/badge/SAP%20B1-9.3%20%7C%2010.0-orange" alt="SAP B1">
  <img src="https://img.shields.io/badge/PHP-7.4%2B-777BB4" alt="PHP">
  <img src="https://img.shields.io/badge/HPOS-compatible-brightgreen" alt="HPOS">
  <img src="https://img.shields.io/badge/licencia-GPLv2-green" alt="Licencia">
</p>

<p align="center">
  <a href="https://replantadev.github.io/sap-woo-suite-info">🌐 Landing Page</a> · 
  <a href="https://replanta.net">Web</a> · 
  <a href="mailto:info@replanta.net">Contacto</a>
</p>

---

## ¿Qué es SAP Woo Suite?

**SAP Woo Suite** es un plugin de WordPress que conecta tu tienda WooCommerce con SAP Business One a través de Service Layer. Sincroniza pedidos, stock, precios, clientes y catálogo de productos de forma bidireccional.

Desde la versión 2.0 incorpora una **arquitectura multicanal** con Channel Manager extensible. Desde la versión 2.1, incluye **detección automática de canal** por metadata: instala el plugin oficial de TikTok o Amazon y SAP Woo Suite etiqueta cada pedido automáticamente sin configuración extra. Miravia estará disponible como addon propio.

> **Un solo plugin. Todos tus canales. Un solo ERP.**

---

## Modos de operación

| Modo | Descripción | Uso típico |
|------|-------------|------------|
| **Ecommerce (B2C)** | Clientes genéricos por región (Península, Canarias, Portugal) | Tiendas online con cliente anónimo |
| **B2B** | Cada usuario WooCommerce = un CardCode en SAP | Portales mayoristas con precios personalizados |

---

## Funcionalidades

### Pedidos (WooCommerce → SAP)
- Envío automático o manual de pedidos como documentos de venta en SAP
- Asignación inteligente de cliente por región geográfica
- Mapeo completo de direcciones de facturación y envío
- Soporte para gastos de envío (DocumentAdditionalExpenses)
- Notas de pedido enriquecidas con datos del cliente
- Panel de pedidos fallidos con opción de reintento

### Stock y Precios (SAP → WooCommerce)
- Sincronización de stock por almacén configurable
- Tarifas de precios por región (Península, Canarias, Portugal)
- Mapeo almacén-tarifa personalizable
- Sincronización automática programable con toggles independientes

### Catálogo (SAP → WooCommerce)
- Importación masiva de productos (Items) por lotes
- Importación de categorías (ItemGroups)
- Importación selectiva con preview de campos antes de importar
- Barra de progreso y logs en tiempo real

### Clientes B2B
- Sincronización automática de clientes marcados como "web" en SAP
- Email de bienvenida personalizado con enlace para establecer contraseña
- Mapeo flexible de CardCode
- Cron diario de sincronización

### Arquitectura Multicanal (v2.1)
- **Channel Manager**: Registro centralizado de canales/marketplace
- **Channel Detector**: Detecta automáticamente el canal por metadata del pedido
- **TikTok Shop**: detección automática vía plugin oficial `tiktok-for-woocommerce`
- **Amazon**: detección automática vía `amazon-for-woocommerce` (CedCommerce, meta `_umb_marketplace`)
- **Miravia**: próximamente como addon propio (mismo patrón que TikTok addon)
- **Dashboard Multicanal**: Vista unificada del estado de SAP y todos los canales conectados
- **API para Addons**: Interface y clase base para desarrollar integraciones propias
- **Hooks de extensión**: Los addons reaccionan automáticamente a eventos del core (pedidos enviados, productos importados, stock actualizado...)
- **Compatibilidad HPOS**: Preparado para WooCommerce 9.x con High-Performance Order Storage

### Logs y Diagnóstico
- Logs detallados de cada operación
- Panel de pedidos fallidos con reintento
- Vista directa de pedidos SAP desde WordPress

---

## Flujo de sincronización

```
┌─────────────────┐                          ┌─────────────────┐
│   WooCommerce   │                          │  SAP Business   │
│                 │                          │      One        │
├─────────────────┤                          ├─────────────────┤
│                 │                          │                 │
│  Nuevo Pedido   │─────────────────────────▶│  Order creado   │
│                 │                          │  (DocEntry)     │
│                 │                          │                 │
│  Stock/Precios  │◀─────────────────────────│  Items/Prices   │
│  actualizados   │                          │                 │
│                 │                          │                 │
│  Productos      │◀─────────────────────────│  Items          │
│  importados     │                          │                 │
│                 │                          │                 │
│  Categorías     │◀─────────────────────────│  ItemGroups     │
│  importadas     │                          │                 │
│                 │                          │                 │
│  Clientes B2B   │◀─────────────────────────│  BusinessPart.  │
│  sincronizados  │                          │                 │
└─────────────────┘                          └─────────────────┘
         ▲                                            │
         │        ┌─────────────────────┐             │
         │        │  Canales (Addons)   │             │
         │        ├─────────────────────┤             │
         └────────│  TikTok Shop        │─────────────┘
                  │  Amazon             │
                  │  Miravia            │
                  │  ...                │
                  └─────────────────────┘
```

---

## Requisitos técnicos

### WordPress / WooCommerce
- WordPress 5.8+
- WooCommerce 6.0+ (compatible con WooCommerce 9.x y HPOS)
- PHP 7.4+ (recomendado 8.0+)

### SAP Business One
- SAP Business One 9.3 PL14+ o 10.0+
- **Service Layer activo** (`/b1s/v1` accesible vía HTTPS)
- Usuario de Service Layer con permisos en: Orders, BusinessPartners, Items, ItemGroups, PriceLists, Warehouses

---

## Extensibilidad: Desarrollo de Addons

SAP Woo Suite permite conectar cualquier canal de venta mediante addons que se registran como "canales":

```php
add_action('sapwc_loaded', function () {
    class My_Channel extends SAPWC_Channel_Base {
        public function get_slug()         { return 'mi-canal'; }
        public function get_name()         { return 'Mi Canal'; }
        public function get_version()      { return '1.0.0'; }
        public function get_capabilities() { return ['orders', 'products']; }
        public function is_configured()    { return true; }
        public function test_connection()  { return true; }
        public function admin_page()       { echo '<h2>Config de Mi Canal</h2>'; }
    }
    SAPWC_Channel_Manager::get_instance()->register(new My_Channel());
});
```

### Hooks disponibles

| Hook | Tipo | Descripción |
|------|------|-------------|
| `sapwc_loaded` | action | Plugin cargado — momento de registrar canales |
| `sapwc_admin_menu` | action | Añadir submenús al menú de SAP Woo |
| `sapwc_order_payload` | filter | Modificar payload del pedido antes de enviar a SAP |
| `sapwc_before_send_order` | action | Antes de enviar un pedido a SAP |
| `sapwc_after_send_order` | action | Después de enviar un pedido (éxito o error) |
| `sapwc_before_save_product` | filter | Modificar producto antes de guardarlo en WC |
| `sapwc_after_import_product` | action | Después de importar un producto desde SAP |
| `sapwc_after_import_customer` | action | Después de importar un cliente desde SAP |
| `sapwc_documents_owner` | filter | Personalizar DocumentsOwner por pedido |
| `sapwc_api_request_args` | filter | Modificar argumentos de petición a Service Layer |
| `sapwc_channel_registered` | action | Un canal se ha registrado en el Channel Manager |
| `sapwc_channel_detectors` | filter | Registrar detectores de canal personalizados |
| `sapwc_channel_payload` | filter | Modificar payload tras inyectar información de canal |
| `sapwc_builtin_channels_registered` | action | Canales built-in registrados — punto de extensión |

---

## Versiones

| Versión | Destacado |
|---------|-----------|
| **2.2.0** | Fix detección Amazon (`_umb_marketplace`), documentación y landing actualizados |
| **2.1.0** | Channel Detector automático, soporte TikTok Shop oficial, 3 nuevos hooks |
| **2.0.0** | Arquitectura multicanal, Channel Manager, HPOS, API Client singleton, 11 hooks de extensión |
| 1.4.2-beta | Importación selectiva con preview de campos |
| 1.4.0-beta | Importación unificada con guía de operario |
| 1.3.0-beta | Sincronización automática de clientes B2B |

---

## Contacto y Soporte

Desarrollado y mantenido por **Replanta**.

| | |
|-|-|
| 🌐 Web | [replanta.net](https://replanta.net) |
| 📧 Email | [info@replanta.net](mailto:info@replanta.net) |
| 📍 Ubicación | España |

**¿Interesado en SAP Woo Suite para tu negocio?** Escríbenos a [info@replanta.net](mailto:info@replanta.net) para una demo o presupuesto personalizado.

---

## Licencia

GPLv2 o posterior.

```
SAP Woo Suite - Plataforma multicanal para SAP Business One
Copyright (C) 2024-2026 Replanta (https://replanta.net)
```

---

<p align="center">
  <sub>Desarrollado con cuidado por <a href="https://replanta.net">Replanta</a></sub>
</p>
