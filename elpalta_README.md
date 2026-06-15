# elpalta.cl — Sistema de Pedidos

Sistema de pedidos con agenda automática, sectores con despacho diferenciado,
franquiciados y cobranza por WhatsApp.

---

## Publicar en Vercel (igual que Aliz)

1. Crear repositorio en GitHub → subir `index.html`
2. Importar en vercel.com → Deploy
3. URL queda como `elpalta.vercel.app` o similar

---

## Configuración inicial (abrir index.html y editar)

```js
const CFG = {
  WSP_NEGOCIO: '56995443268',      // ← tu número WhatsApp
  INSTAGRAM: 'https://www.instagram.com/elpalta.cl',  // ← tu cuenta IG
  REPARTOS: [1, 5],                // 1=lunes, 5=viernes
  HORA_CIERRE: 20,                 // 20:00 hrs
};
```

---

## Actualizar precios y productos

En la sección `PRODUCTOS` del index.html:

```js
{ id: 1, name: 'Palta Hass', desc: 'Malla x6 unidades', price: 4900, cat: 'ancla', icon: '🥑' },
```

- `price`: cambia cuando cambie el precio
- `cat`: `'ancla'` o `'temporada'`
- Para desactivar un producto temporalmente: agrega `activo: false` y filtra en renderProductos()

Productos de temporada actuales (aparecen en el tag banner):
```js
const TEMP_ACTUAL = ['Limones', 'Mandarinas'];
```

---

## Sectores y franquiciados

En el formulario, los sectores están organizados en tres grupos:
- **Despacho gratis**: `value="gratis|Nombre Sector"`
- **Con recargo**: `value="recargo|Nombre Sector"`  
- **Franquiciados**: `value="franquicia|Nombre Sector|569XXXXXXXX"` ← el número es el WhatsApp del franquiciado

Para agregar un franquiciado nuevo, agrega una línea:
```html
<option value="franquicia|Nuevo Sector|56912345678">Nuevo Sector</option>
```

---

## Datos de transferencia

Los datos bancarios están en la pantalla de pago del index.html.
Para cambiarlos, busca la sección `<!-- PANTALLA: pago -->` y edita los valores.

Cuenta actual configurada:
- Banco: BCI / Banco Crédito e Inversiones
- Tipo: Cuenta Corriente
- N°: 777919519570
- RUT: 19.519.570-6
- Nombre: Matías Nicolás Bernales Ávila

---

## Cómo funciona la agenda automática

El sistema calcula solo en base a:
- `REPARTOS: [1, 5]` → lunes y viernes
- `HORA_CIERRE: 20` → cierra el día anterior a las 20:00

Ejemplos de lo que muestra el sistema:
| Cuándo entra el cliente | Qué dice la agenda |
|---|---|
| Lunes a las 10:00 | Entrega el viernes |
| Jueves a las 15:00 | Entrega el viernes |
| Jueves a las 21:00 | Entrega el lunes |
| Domingo a las 10:00 | Entrega el lunes |
| Domingo a las 21:00 | Entrega el viernes |

---

## Flujo de un pedido

1. Cliente ve catálogo → agrega productos → va al carrito
2. Ingresa nombre, WhatsApp, sector y dirección
3. Ve los datos de transferencia y el monto exacto
4. Hace la transferencia y presiona "Ya transferí"
5. Se abre WhatsApp con el mensaje completo prellenado → cliente lo envía
6. Tú recibes el mensaje en WhatsApp con todo el detalle
7. Si el sector es franquiciado, el mensaje va directo al franquiciado

---

## Próximos pasos (Fase 2 con Claude Code)

- [ ] Base de datos de clientes con historial completo
- [ ] Panel de gestión de pedidos del día (ver todos los pedidos, marcar como entregado)
- [ ] Cobranza automática post-entrega con mensajes pregenerados
- [ ] Exportación a Excel de cada ciclo de reparto
- [ ] Liquidación automática de franquiciados
- [ ] Integración Google Sheets como respaldo automático
