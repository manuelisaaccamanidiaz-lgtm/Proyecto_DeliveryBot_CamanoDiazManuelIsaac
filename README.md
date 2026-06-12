# 🤖 DeliveryBot – Gestión de Pedidos Internos de Cafetería

<div align="center">

![n8n](https://img.shields.io/badge/n8n-Automation-orange?style=for-the-badge&logo=n8n)
![Telegram](https://img.shields.io/badge/Telegram-Bot-2CA5E0?style=for-the-badge&logo=telegram)
![Google Sheets](https://img.shields.io/badge/Google%20Sheets-Database-34A853?style=for-the-badge&logo=googlesheets)
![JavaScript](https://img.shields.io/badge/JavaScript-Expressions-F7DF1E?style=for-the-badge&logo=javascript)

**Sistema automatizado de pedidos internos para cafetería, operado 100% desde Telegram**

*Proyecto Académico – Automatización con n8n*

**Autor:** Manuel Isaac Camaño Diaz — Campusland
**Fecha:** Junio 2026

</div>

---

## 📋 Tabla de Contenidos

1. [Descripción General](#-descripción-general)
2. [Objetivos](#-objetivos)
3. [Arquitectura del Sistema](#-arquitectura-del-sistema)
4. [Tecnologías Utilizadas](#-tecnologías-utilizadas)
5. [Estructura de Base de Datos](#-estructura-de-base-de-datos)
6. [Funcionalidades Implementadas](#-funcionalidades-implementadas)
7. [Flujo del Usuario](#-flujo-del-usuario)
8. [Flujo Administrativo](#-flujo-administrativo)
9. [Evidencias del Proyecto](#-evidencias-del-proyecto)
10. [Pruebas Realizadas](#-pruebas-realizadas)
11. [Expresiones y Lógica Utilizada](#-expresiones-y-lógica-utilizada)
12. [Configuración de Google Sheets](#-configuración-de-google-sheets)
13. [Resultados Obtenidos](#-resultados-obtenidos)
14. [Posibles Mejoras Futuras](#-posibles-mejoras-futuras)
15. [Conclusiones](#-conclusiones)

---

## 📌 Descripción General

### Problema Identificado

En entornos institucionales como universidades, oficinas o centros de trabajo, la cafetería es un punto crítico que suele colapsar en horas pico. El proceso tradicional implica largas filas, pedidos mal tomados, confusiones entre el cliente y el personal de cocina, y nula visibilidad sobre el estado del pedido una vez realizado.

> 🧠 *Imagina esto: son las 12:00 del mediodía, 40 personas hacen fila, el cocinero intenta recordar 15 pedidos de memoria y el cliente número 23 no sabe si su empanada ya está lista o si todavía está cruda. Caos total.*

### Solución Propuesta

**DeliveryBot** es una solución de automatización construida sobre **n8n** que convierte a **Telegram** en una terminal de pedidos inteligente. Los empleados o estudiantes pueden consultar el menú, construir su carrito de compras y recibir notificaciones automáticas sobre el estado de su pedido, todo sin salir del chat.

El sistema elimina el papel, la fila y los errores humanos. La cocina recibe alertas inmediatas y el administrador obtiene reportes financieros en tiempo real sin hacer clic en nada.

### Contexto de Uso

El sistema está diseñado para instituciones educativas o empresas que cuenten con una cafetería interna y necesiten digitalizar el proceso de pedidos sin invertir en infraestructura costosa. Solo se requiere Telegram (que ya todos tienen instalado) y una hoja de Google Sheets como base de datos.

---

## 🎯 Objetivos

### Objetivo General

Desarrollar un sistema automatizado de gestión de pedidos internos de cafetería mediante una interfaz conversacional en Telegram, integrando n8n como motor de automatización y Google Sheets como base de datos centralizada, con el fin de eliminar los procesos manuales, reducir errores y mejorar la experiencia del usuario final.

### Objetivos Específicos

- Implementar una interfaz conversacional guiada por menús en Telegram, diferenciando los roles de cliente y administrador.
- Automatizar el cálculo de totales, la generación de identificadores únicos de pedido y el registro en la base de datos.
- Gestionar el ciclo de vida completo del pedido a través de estados dinámicos: **Recibido → Preparación → En camino → Entregado**.
- Validar la disponibilidad de stock en tiempo real antes de confirmar cualquier pedido.
- Notificar automáticamente al cliente cuando su pedido cambia de estado.
- Implementar un sistema básico de puntos de lealtad que recompense a los clientes frecuentes.
- Generar reportes financieros automáticos accesibles desde el panel administrativo de Telegram.
- Centralizar el inventario y el menú en Google Sheets para facilitar su administración sin conocimientos técnicos.

---

## 🏗 Arquitectura del Sistema

El sistema sigue una arquitectura de tres capas conectadas por automatizaciones en n8n:

```
┌─────────────────────────────────────────────┐
│              USUARIO / ADMIN                │
│           (Telegram Chat)                   │
└──────────────────┬──────────────────────────┘
                   │ Mensajes de texto
                   ▼
┌─────────────────────────────────────────────┐
│           MOTOR DE AUTOMATIZACIÓN           │
│                  (n8n)                      │
│                                             │
│  ┌──────────────┐   ┌──────────────────┐    │
│  │ Gestión de   │   │  Procesamiento   │    │
│  │  Sesiones   │   │  de Pedidos      │    │
│  └──────────────┘   └──────────────────┘    │
│  ┌──────────────┐   ┌──────────────────┐    │
│  │ Validación   │   │  Notificaciones  │    │
│  │  de Stock    │   │  y Reportes      │    │
│  └──────────────┘   └──────────────────┘    │
└──────────────────┬──────────────────────────┘
                   │ Lectura / Escritura
                   ▼
┌─────────────────────────────────────────────┐
│           BASE DE DATOS                     │
│           (Google Sheets)                   │
│                                             │
│   MENU  │  PEDIDOS  │  USUARIOS  │ SESSIONS │
└─────────────────────────────────────────────┘
```

**Telegram** actúa como la interfaz de usuario: recibe comandos de texto simples y devuelve respuestas formateadas. Es el frontend del sistema.

**n8n** es el cerebro de la operación. Procesa cada mensaje entrante, determina en qué pantalla está el usuario (gracias a las sesiones), ejecuta la lógica de negocio correspondiente y actualiza la base de datos.

**Google Sheets** funciona como base de datos relacional simplificada. Cuatro hojas interconectadas almacenan el menú, los pedidos, los usuarios y las sesiones activas.

---

## 🛠 Tecnologías Utilizadas

| Tecnología | Versión / Plan | Rol en el Proyecto |
|---|---|---|
| **Telegram Bot API** | Bot API v6+ | Interfaz conversacional para clientes y administradores |
| **n8n** | Self-hosted / Cloud | Motor de automatización y orquestador de flujos |
| **Google Sheets** | API v4 | Base de datos principal: menú, pedidos, usuarios y sesiones |
| **JavaScript Expressions** | ES6+ | Lógica de negocio: cálculos, formateo, validaciones |
| **Google Drive** | API v3 | Almacenamiento y acceso al archivo de base de datos |

**¿Por qué esta combinación?** Telegram tiene más de 900 millones de usuarios activos y no requiere instalación adicional. n8n permite construir flujos complejos de forma visual sin necesidad de un servidor backend dedicado. Google Sheets elimina la necesidad de configurar una base de datos SQL, siendo editable directamente por el administrador sin conocimientos técnicos.

---

## 🗃 Estructura de Base de Datos

La base de datos está alojada en Google Sheets bajo el nombre **DeliveryBot_DB**, compuesta por cuatro hojas que trabajan en conjunto.

### Hoja: MENU

Es el catálogo de productos disponibles en la cafetería. El administrador puede agregar, editar o eliminar productos directamente desde la hoja sin tocar el workflow.

| Campo | Tipo | Descripción |
|---|---|---|
| `id_producto` | Número | Identificador único del producto |
| `nombre` | Texto | Nombre visible al cliente (ej: "Café") |
| `descripcion` | Texto | Descripción corta del producto |
| `precio` | Número | Precio unitario en pesos colombianos |
| `categoria` | Texto | Categoría de agrupación (Bebidas, Snacks) |
| `stock` | Número | Unidades disponibles. Se descuenta automáticamente con cada pedido confirmado |

> 📦 Ejemplo real: `1 | Café | Café negro | 3000 | Bebidas | 48`

---

### Hoja: PEDIDOS

Registro histórico de todas las órdenes generadas en el sistema. Cada fila es un pedido.

| Campo | Tipo | Descripción |
|---|---|---|
| `id_pedido` | Texto | Identificador único con formato `ORD-XXXX` (generado automáticamente) |
| `id_usuario` | Número | Telegram ID del cliente que realizó el pedido |
| `detalles_pedido` | Texto | Lista de productos con sus precios individuales |
| `total_pago` | Número | Suma total del pedido en pesos |
| `estado` | Texto | Estado actual: Recibido / Preparación / En camino / Entregado |
| `fecha` | Fecha | Fecha de creación del pedido |
| `hora` | Hora | Hora exacta de creación del pedido |

> 🧾 Ejemplo real: `ORD-6208 | 6975833647 | Café - $3000 / Empanada - $4000 | 7000 | Entregado | 7/6/2026 | 9:13:03`

---

### Hoja: USUARIOS

Registro de todos los usuarios que han interactuado con el bot. Permite diferenciar clientes de administradores y llevar el seguimiento de puntos de lealtad.

| Campo | Tipo | Descripción |
|---|---|---|
| `telegram_id` | Número | ID único de Telegram del usuario |
| `nombre_completo` | Texto | Nombre registrado (puede quedar vacío) |
| `departamento/oficina` | Texto | Área de trabajo del usuario (ej: "Campusland") |
| `puntos_lealtad` | Número | Puntos acumulados (1 punto por cada $1.000 gastados) |
| `rol` | Texto | Define el menú que ve el usuario: `cliente` o `administrador` |

> 👤 El campo `rol` es clave: un mismo usuario puede actuar como administrador y acceder a opciones como cambiar estados de pedidos o ver el reporte financiero.

---

### Hoja: SESSIONS

Esta es la hoja más dinámica del sistema. Almacena el estado conversacional de cada usuario en tiempo real, permitiendo que el bot "recuerde" en qué punto de la conversación está cada persona.

| Campo | Tipo | Descripción |
|---|---|---|
| `telegram_id` | Número | Identificador del usuario |
| `pantalla_actual` | Texto | Estado actual en el flujo (ej: `menu_principal`, `esperando_producto`) |
| `carrito_temporal` | Texto | Productos agregados al carrito durante la sesión activa |
| `precio_temporal` | Número | Suma acumulada del carrito en curso |
| `nombre_temporal` | Texto | Campo auxiliar para almacenamiento intermedio |
| `pedido_temporal` | Texto | ID del último pedido procesado en la sesión |
| `rol` | Texto | Rol del usuario en la sesión actual |
| `ultimo_cambio` | Timestamp | Fecha y hora de la última interacción |
| `agotados` | JSON | Lista de productos sin stock detectados en la sesión |

> 🧠 *Piensa en esta hoja como la "memoria de corto plazo" del bot. Si un usuario va por la mitad de un pedido y manda otro mensaje, el sistema sabe exactamente dónde lo dejó.*

---

## ⚙️ Funcionalidades Implementadas

### 🍽 Consulta de Menú

El usuario escribe `1` en el menú principal y recibe al instante el catálogo completo de productos disponibles con nombre, precio y opción de proceder al pedido. El bot lee la hoja MENU en tiempo real, por lo que cualquier cambio de precio o producto nuevo se refleja de inmediato sin modificar el workflow.

### 🛒 Gestión de Carrito (Sesión Temporal)

El sistema permite al usuario construir un pedido con múltiples productos. Cada producto seleccionado se concatena en el campo `carrito_temporal` de la hoja SESSIONS y su precio se acumula en `precio_temporal`. El carrito persiste entre mensajes gracias al sistema de sesiones, permitiendo que el usuario seleccione varios ítems antes de confirmar.

> 🛍 *Ejemplo: El usuario pide "Empanada, Café". El bot acumula ambos en el carrito, calcula $7.000 en total y presenta el resumen antes de confirmar.*

### 📝 Registro Automático de Pedidos

Una vez confirmado el pedido, el sistema genera automáticamente un registro completo en la hoja PEDIDOS sin intervención humana. Se almacenan: identificador único, usuario, detalle de productos, total, estado inicial "Recibido", fecha y hora exacta.

### ✅ Validación de Stock

Antes de aceptar productos en el carrito, el sistema consulta el campo `stock` de la hoja MENU. Si un producto está agotado (stock = 0), el bot notifica al usuario en el momento y no permite agregarlo al pedido. Los productos agotados se registran en el campo `agotados` de la sesión para evitar consultas repetidas.

### 💰 Cálculo Automático de Totales

El total del pedido se calcula dinámicamente sumando los precios de todos los productos del carrito. El resultado se muestra al usuario en el resumen de confirmación y se registra en la hoja PEDIDOS. No hay cálculo manual ni posibilidad de error aritmético.

### 🔑 Generación de Identificadores Únicos

Cada pedido recibe un ID único con el formato `ORD-XXXX` donde XXXX es un número aleatorio de 4 dígitos. Esto garantiza que cada orden sea rastreable tanto por el cliente como por el administrador. El ID se incluye en el recibo digital y se puede usar para consultar el estado del pedido.

### 🔄 Gestión de Estados del Pedido

Los pedidos pasan por un ciclo de vida gestionado por el administrador: **Recibido → Preparación → En camino → Entregado**. El administrador selecciona el pedido por su ID y elige el nuevo estado desde el panel de Telegram. El cambio se registra inmediatamente en Google Sheets.

### 🔔 Notificaciones Automáticas al Cliente

Cada vez que el administrador actualiza el estado de un pedido, el sistema obtiene automáticamente el `telegram_id` del cliente asociado a ese pedido y le envía una notificación directa al chat. El cliente no necesita preguntar ni consultar nada: la información llega sola.

### 🏆 Sistema de Puntos de Lealtad

Al confirmar un pedido, el sistema calcula automáticamente los puntos ganados (1 punto por cada $1.000 gastados) y los acumula en el campo `puntos_lealtad` de la hoja USUARIOS. El cliente recibe una notificación informando los puntos obtenidos y su total acumulado.

### 🍳 Notificación a Cocina

Cuando se confirma un nuevo pedido, el sistema busca en la hoja USUARIOS todos los registros con rol `administrador` y les envía automáticamente una notificación con el detalle completo del pedido entrante: ID, productos, total y datos del cliente.

### 📊 Reporte Financiero

Desde el panel administrativo, el administrador puede solicitar un reporte financiero que incluye: total vendido del día, número de pedidos, producto estrella (el más vendido con su precio) y hora pico de mayor actividad. El reporte se genera calculando sobre la hoja PEDIDOS en tiempo real.

---

## 👤 Flujo del Usuario

El usuario estándar (rol `cliente`) interactúa con DeliveryBot siguiendo este flujo de negocio:

```
1. Usuario envía cualquier mensaje al bot
         │
         ▼
2. Sistema detecta su Telegram ID y verifica su sesión en SESSIONS
   → Si no existe sesión: crea el registro y muestra el menú principal
   → Si existe sesión: retoma desde la pantalla donde quedó
         │
         ▼
3. Menú Principal (opciones 1, 2, 3):
   [1] Ver menú     [2] Realizar pedido     [3] Consultar estado
         │
         ▼ (Opción 1: Ver menú)
4. Sistema lee la hoja MENU y formatea la lista de productos disponibles
   → Se muestra: nombre, precio por producto
   → Opción de proceder al pedido
         │
         ▼ (Opción 2: Realizar pedido)
5. Sistema solicita los productos deseados (texto libre)
   → El usuario escribe: "Empanada, Café"
   → Sistema valida stock de cada producto en MENU
   → Si hay stock: agrega al carrito temporal en SESSIONS
   → Si no hay stock: notifica los productos agotados
         │
         ▼
6. Sistema presenta el resumen del carrito:
   "🛒 Café - $3.000 / Empanada - $4.000
    💰 Total: $7.000
    ¿Deseas confirmar? SI / NO"
         │
         ▼ (Usuario responde SI)
7. Sistema ejecuta la confirmación:
   → Genera ID único: ORD-XXXX
   → Registra pedido completo en hoja PEDIDOS
   → Descuenta stock en hoja MENU
   → Calcula y suma puntos de lealtad en USUARIOS
   → Envía recibo digital al cliente con ID del pedido
   → Notifica a administradores sobre el nuevo pedido
   → Reinicia sesión al menú principal
         │
         ▼ (Usuario responde NO)
7b. Sistema cancela el pedido y notifica al usuario
         │
         ▼ (Opción 3: Consultar estado)
8. Sistema solicita el ID del pedido (ORD-XXXX)
   → Busca en PEDIDOS y devuelve el estado actual
```

---

## 🔧 Flujo Administrativo

Los usuarios con rol `administrador` acceden a un panel extendido de Telegram con 6 opciones:

```
🧑‍💼 Panel de Administración:
1. Ver menú
2. Realizar pedido
3. Consultar estado
4. Cambiar estado de pedido
5. Ver pedidos pendientes
6. Reporte financiero
```

**Gestión de Inventario:** El administrador actualiza precios, stock y productos directamente en Google Sheets. El bot lee siempre en tiempo real, por lo que el cambio es inmediato.

**Actualización de Estados (opción 4):** El administrador ingresa el ID del pedido (ej: `ORD-2532`), el sistema lo busca en la hoja PEDIDOS, confirma que existe y presenta las opciones de estado: Preparación, En camino, Entregado. Al seleccionar, el estado se actualiza en Sheets y se dispara la notificación automática al cliente.

**Ver Pedidos Pendientes (opción 5):** Filtra y muestra los pedidos con estado "Recibido" o "Preparación" para que el administrador tenga visibilidad de la carga de trabajo actual.

**Reporte Financiero (opción 6):** Genera al instante un resumen ejecutivo del día con métricas clave de ventas, sin necesidad de abrir Google Sheets manualmente.

---

## 📸 Evidencias del Proyecto

### Captura 1 – Workflow Completo

<a href="https://ibb.co/D6Hmb7Y"><img src="https://i.ibb.co/5zgC95k/workflow-completo.png" alt="workflow-completo" border="0"></a>

**Descripción:** Vista general del flujo de automatización en n8n. Se pueden observar los módulos principales diferenciados por colores: inicio y control de sesión (morado), catálogo y menú (rojo oscuro), gestión de pedidos (naranja), inventario y lealtad (verde oscuro), notificaciones (rojo), reportes y administración (verde), consulta de estado (azul) y administración - cambio de estado (naranja/café). El workflow completo cuenta con más de 80 nodos interconectados.

---

### Captura 2 – Menú Cliente en Telegram

<a href="https://ibb.co/ZQXFFKJ"><img src="https://i.ibb.co/XnLKKp4/telegram-menu-cliente.png" alt="telegram-menu-cliente" border="0"></a>

**Descripción:** Vista del menú principal que recibe un usuario con rol `cliente` al iniciar la conversación. Presenta tres opciones claras: ver menú, realizar pedido y consultar estado del pedido. La interfaz es limpia y guiada, no requiere ningún conocimiento previo.

---

### Captura 3 – Panel de Administración en Telegram

<a href="https://ibb.co/3YSsKfL6"><img src="https://i.ibb.co/pr4PqW9V/telegram-menu-administrador.png" alt="telegram-menu-administrador" border="0"></a>

**Descripción:** Panel exclusivo para usuarios con rol `administrador`. Incluye todas las opciones del cliente más tres adicionales: cambiar estado de pedido, ver pedidos pendientes y reporte financiero. El sistema detecta automáticamente el rol del usuario y muestra el menú correspondiente.

---

### Captura 4 – Menú Disponible (Catálogo de Productos)

<a href="https://ibb.co/393Zf5sx"><img src="https://i.ibb.co/G46SsfJZ/menu.png" alt="menu" border="0"></a>

**Descripción:** Resultado de seleccionar la opción "Ver menú". El bot formatea y presenta todos los productos activos de la hoja MENU con nombre y precio. La información se lee en tiempo real, por lo que refleja siempre el estado actual del inventario.

---

### Captura 5 – Flujo de Menú en n8n

<a href="https://ibb.co/4ZcQGf4n"><img src="https://i.ibb.co/8DJhpBPL/flujo-menu.png" alt="flujo-menu" border="0"></a>

**Descripción:** Módulo de catálogo en n8n. Muestra los tres nodos del proceso de consulta de menú: lectura de productos desde Google Sheets, formateo del mensaje y envío al cliente vía Telegram.

---

### Captura 6 – Selección de Productos

<a href="https://ibb.co/q8vSv0z"><img src="https://i.ibb.co/g8x2xvG/seleccion-producto.png" alt="seleccion-producto" border="0"></a>

**Descripción:** El usuario responde con los nombres de los productos deseados. El bot procesa el texto, valida contra el inventario y presenta el resumen del carrito con el total calculado automáticamente.

---

### Captura 7 – Flujo de Selección de Producto en n8n

<a href="https://ibb.co/QFwSgSQ1"><img src="https://i.ibb.co/4Zrvqvt5/flujo-seleccion-producto.png" alt="flujo-seleccion-producto" border="0"></a>

**Descripción:** Módulo de procesamiento de pedido. Incluye los nodos de configuración de sesión, guardado de estado, solicitud de productos y preparación del tipo de consulta para la siguiente interacción.

---

### Captura 8 – Confirmación del Pedido

<a href="https://ibb.co/svL6tK5R"><img src="https://i.ibb.co/VWXQ3gH9/confirmacion-pedido.png" alt="confirmacion-pedido" border="0"></a>

**Descripción:** Recibo digital enviado automáticamente al cliente tras confirmar el pedido. Incluye el ID único del pedido (ORD-6208), detalle de productos, total y estado inicial. El mensaje invita al cliente a guardar el ID para seguimiento posterior.

---

### Captura 9 – Flujo de Confirmación en n8n

<a href="https://ibb.co/5W3JMXT8"><img src="https://i.ibb.co/DDFjVP4C/flujo-confirmacion.png" alt="flujo-confirmacion" border="0"></a>

**Descripción:** Módulo de confirmación y procesamiento final. Gestiona el registro del pedido, actualización de stock, cálculo de puntos de lealtad, envío de recibo al cliente y notificación a cocina/administradores.

---

### Captura 10 – Registro en Google Sheets

<a href="https://ibb.co/tPLYWcvY"><img src="https://i.ibb.co/1JzRwKPR/google-sheet-pedidos.png" alt="google-sheet-pedidos" border="0"></a>

**Descripción:** Vista de la hoja PEDIDOS con los registros generados durante las pruebas. Se observan 4 pedidos con IDs únicos (ORD-9702, ORD-6208, ORD-8847, ORD-2532), sus detalles, totales, estados y marcas de tiempo. El pedido ORD-6208 ya figura como "Entregado", confirmando el ciclo de vida completo.

---

### Captura 11 – Base de Datos Completa en Google Sheets

<a href="https://ibb.co/qLSfs0sV"><img src="https://i.ibb.co/twGtsZsF/DB-google-sheet.png" alt="DB-google-sheet" border="0"></a>

**Descripción:** Vista de las cuatro hojas de la base de datos: MENU (con 3 productos y su stock actual), USUARIOS (con el registro del administrador y sus puntos de lealtad), SESSIONS (con el estado de sesión activo) y PEDIDOS (con el historial de órdenes).

---

### Captura 12 – Notificación de Cambio de Estado

<a href="https://imgbb.com/"><img src="https://i.ibb.co/B5fqQYkB/notificacion-cambio-estado.png" alt="notificacion cambio estado" border="0"></a>

**Descripción:** Mensaje recibido automáticamente por el cliente cuando el administrador cambia el estado de su pedido. El bot notifica el nuevo estado (Preparación) sin que el usuario tenga que preguntar nada.

---

### Captura 13 – Flujo de Cambio de Estado en n8n

<a href="https://ibb.co/Vcfmm01n"><img src="https://i.ibb.co/bMcJJj0w/flujo-cambio-estado.png" alt="flujo-cambio-estado" border="0"></a>

**Descripción:** Módulo administrativo de actualización de estados. Incluye la lógica de búsqueda del pedido, validación de existencia, actualización en Sheets, obtención de datos del cliente y envío de notificación automática.

---

### Captura 14 – Reporte Financiero

<a href="https://ibb.co/Q74dBCtr"><img src="https://i.ibb.co/Y4xZ9ywp/reporte-de-ventas.png" alt="reporte-de-ventas" border="0"></a>

**Descripción:** Reporte financiero generado automáticamente desde el panel administrativo de Telegram. Muestra total vendido ($23.000), número de pedidos (4), producto(s) estrella (Café - $3.000, Empanada - $4.000) y hora pico de actividad (9:11 a.m.).

---

## 🧪 Pruebas Realizadas

### Prueba 1 – Consulta de Menú

| Campo | Detalle |
|---|---|
| **Objetivo** | Verificar que el bot presenta el catálogo actualizado desde Google Sheets |
| **Entrada** | Usuario envía `1` en el menú principal |
| **Resultado esperado** | Lista de productos con nombre y precio leída en tiempo real |
| **Resultado obtenido** | ✅ Bot devuelve el menú correctamente con Café ($3.000), Jugo ($5.000) y Empanada ($4.000) |

---

### Prueba 2 – Construcción de Carrito y Confirmación

| Campo | Detalle |
|---|---|
| **Objetivo** | Verificar la acumulación de productos y cálculo correcto del total |
| **Entrada** | Usuario selecciona "Empanada, Café" |
| **Resultado esperado** | Carrito con 2 productos y total de $7.000 presentado para confirmación |
| **Resultado obtenido** | ✅ Carrito muestra "Café - $3.000 / Empanada - $4.000. Total: $7.000. ¿Confirmar?" |

---

### Prueba 3 – Validación de Stock

| Campo | Detalle |
|---|---|
| **Objetivo** | Confirmar que el sistema detecta y reporta productos sin stock |
| **Entrada** | Solicitud de producto con stock = 0 |
| **Resultado esperado** | Notificación de producto agotado sin registrar el pedido |
| **Resultado obtenido** | ✅ Bot notifica correctamente los productos no disponibles y registra el estado en `agotados` de la sesión |

---

### Prueba 4 – Generación y Registro de Pedido

| Campo | Detalle |
|---|---|
| **Objetivo** | Confirmar el ciclo completo: confirmación → registro → recibo |
| **Entrada** | Usuario responde `SI` al resumen del carrito |
| **Resultado esperado** | Pedido registrado en PEDIDOS con ID único, stock descontado, recibo enviado, administrador notificado |
| **Resultado obtenido** | ✅ Pedido ORD-6208 registrado correctamente. Stock actualizado en MENU. Recibo digital enviado al cliente. Notificación enviada al administrador |

---

### Prueba 5 – Actualización de Estado por Administrador

| Campo | Detalle |
|---|---|
| **Objetivo** | Verificar el flujo de cambio de estado y notificación automática |
| **Entrada** | Administrador selecciona opción 4, ingresa `ORD-2532`, selecciona estado "Preparación" |
| **Resultado esperado** | Estado actualizado en PEDIDOS y notificación enviada al cliente |
| **Resultado obtenido** | ✅ Estado cambiado a "Preparación" en Google Sheets. Cliente recibe: "Actualización de pedido - ORD-2532 - Estado actual: Preparación" |

---

### Prueba 6 – Reporte Financiero

| Campo | Detalle |
|---|---|
| **Objetivo** | Validar la generación del reporte con métricas correctas |
| **Entrada** | Administrador selecciona opción 6 en el panel |
| **Resultado esperado** | Reporte con total vendido, número de pedidos, producto estrella y hora pico |
| **Resultado obtenido** | ✅ Reporte generado: Total $23.000, 4 pedidos, Producto estrella: Café y Empanada, Hora pico: 9:11:33 a.m. |

---

### Prueba 7 – Sistema de Puntos de Lealtad

| Campo | Detalle |
|---|---|
| **Objetivo** | Confirmar el cálculo y acumulación de puntos por pedido |
| **Entrada** | Pedido confirmado por $7.000 |
| **Resultado esperado** | 7 puntos acumulados en USUARIOS |
| **Resultado obtenido** | ✅ Puntos acumulados correctamente. Registro en USUARIOS muestra 9 puntos acumulados tras múltiples pedidos de prueba |

---

## 💡 Expresiones y Lógica Utilizada

### Generación de IDs Únicos de Pedido

Cada pedido recibe un identificador con formato `ORD-XXXX` generado mediante una expresión JavaScript en n8n:

```javascript
// Generación de ID único para cada pedido
const randomNum = Math.floor(Math.random() * 9000) + 1000;
const orderId = `ORD-${randomNum}`;
return orderId;
// Ejemplo de salida: ORD-6208
```

### Cálculo del Total del Carrito

El total se calcula a partir del campo `precio_temporal` de la sesión activa, que acumula los precios de cada producto agregado:

```javascript
// Suma acumulativa de precios en la sesión
const precioActual = parseInt($('Obtener Sesión').item.json.precio_temporal) || 0;
const precioProducto = parseInt($json.precio) || 0;
const nuevoTotal = precioActual + precioProducto;
return nuevoTotal;
```

### Manejo y Persistencia de Sesiones

El sistema de sesiones utiliza el `telegram_id` como clave primaria para identificar al usuario y almacenar su estado en la hoja SESSIONS:

```javascript
// Configuración de sesión con pantalla actual
{
  telegram_id: $json.message.from.id,
  pantalla_actual: "esperando_producto",
  carrito_temporal: carritoActual,
  precio_temporal: totalActual,
  ultimo_cambio: new Date().toLocaleString("es-CO")
}
```

### Actualización de Stock al Confirmar Pedido

Tras confirmar el pedido, el sistema obtiene los productos del carrito, busca su stock actual en MENU y calcula el nuevo valor:

```javascript
// Cálculo de nuevo stock
const stockActual = parseInt($('Obtener Productos Para Stock').item.json.stock) || 0;
const nuevoStock = stockActual - 1;
return nuevoStock >= 0 ? nuevoStock : 0; // Nunca menor que 0
```

### Cálculo de Puntos de Lealtad

Los puntos se calculan con base en el total del pedido (1 punto por cada $1.000):

```javascript
// 1 punto por cada $1000 gastados
const totalPedido = parseInt($json.total_pago) || 0;
const puntosGanados = Math.floor(totalPedido / 1000);
const puntosActuales = parseInt($('Buscar Usuario Para Puntos').item.json.puntos_lealtad) || 0;
const nuevosPuntos = puntosActuales + puntosGanados;
return nuevosPuntos;
```

### Formateo del Reporte Financiero

El reporte financiero se construye procesando todos los registros de la hoja PEDIDOS:

```javascript
// Estructura del reporte
const mensaje = `
📊 REPORTE FINANCIERO

💰 Total vendido: $${totalVendido}
📦 Pedidos: ${totalPedidos}
🏆 Producto estrella: ${productoEstrella}
🕐 Hora pico: ${horaPico}

This message was sent automatically with n8n
`;
return mensaje;
```

---

## ⚙️ Configuración de Google Sheets

### URL del Archivo

```
https://docs.google.com/spreadsheets/d/1zS-Su2flzTAmO6rsAp2rG8iIrRnZfUNar_MX6MXGsjk/edit?usp=sharing
```

### Estructura de Hojas

El archivo **DeliveryBot_DB** contiene exactamente cuatro hojas en el siguiente orden:

| # | Nombre | Función |
|---|---|---|
| 1 | **MENU** | Catálogo de productos con stock |
| 2 | **PEDIDOS** | Historial de órdenes registradas |
| 3 | **USUARIOS** | Registro de usuarios y puntos de lealtad |
| 4 | **SESSIONS** | Estado conversacional activo por usuario |

### Datos de Prueba Utilizados

**MENU (3 productos):**

| id | nombre | precio | categoría | stock |
|---|---|---|---|---|
| 1 | Café | $3.000 | Bebidas | 48 |
| 2 | Jugo | $5.000 | Bebidas | 28 |
| 3 | Empanada | $4.000 | Snacks | 38 |

**PEDIDOS (4 órdenes de prueba):**

| ID Pedido | Total | Estado | Fecha |
|---|---|---|---|
| ORD-9702 | $7.000 | Recibido | 7/6/2026 |
| ORD-6208 | $7.000 | Entregado | 7/6/2026 |
| ORD-8847 | $4.000 | Recibido | 7/6/2026 |
| ORD-2532 | $5.000 | Preparación | 7/6/2026 |

> 💡 Para configurar el proyecto en un nuevo entorno: crear el archivo de Google Sheets con las mismas cuatro hojas y los encabezados exactos indicados, actualizar las credenciales de Google Sheets API en n8n y el token del bot de Telegram.

---
# ** Update ** : Examen [1]
se añadio la funcionalidad para que el sistema solo acepte pedidos dentro del rango del horario,para que solo cuando la hora actual este entre 8:00 am y 5:00 pm sea pasible hacer pedido,se hizo implementando un nodo code seguido de un nodo if donde valida que el resultado del nodo code sea verdadero,en caso de que lo sea sigue el flujo normal,en caso de que no enviara un mensaje recordando el horario y cerrando el flujo
imagen de la funcionalidad:
<a href="https://ibb.co/wh17WhP5"><img src="https://i.ibb.co/0y3KMy6H/imagen-2026-06-12-074445352.png" alt="imagen-2026-06-12-074445352" border="0"></a>

imagen de mensaje prueba:
<a href="https://ibb.co/LhXNpQxv"><img src="https://i.ibb.co/Wp4VKDks/imagen-2026-06-12-074621000.png" alt="imagen-2026-06-12-074621000" border="0"></a>

codgo del nodo code:
```javascript
try {
const close1 = "17:00";
  const close = new Date(close1)
const open1 = "08:00"
  const  open = new Date (open1)
      const currentTimeDate = new Date();

      if (open > currentTimeDate ||close < currentTimeDate) {
        return{hora_es_valida :true};
      }else {
        return{hora_es_valida :false};
      }
;}
catch (error) {
  return [{ json: { error: true, mensaje_error: '❌ Error en el nodo de codigo hora', telegram_id: (() => { try { return $('Telegram Trigger').first().json.message.chat.id; } catch(e) { return 0; } })() } }];
}
```
---

## 📈 Resultados Obtenidos

### Automatizaciones Implementadas

Durante el desarrollo e implementación de DeliveryBot se lograron las siguientes automatizaciones funcionales y verificadas:

- **Registro automático de pedidos** sin intervención humana, con marca de tiempo y ID único.
- **Validación de stock en tiempo real** que previene la confirmación de pedidos con productos agotados.
- **Notificaciones push** al cliente cada vez que su pedido cambia de estado.
- **Alertas automáticas a cocina/administración** con cada nuevo pedido confirmado.
- **Cálculo y acumulación de puntos de lealtad** post-confirmación.
- **Generación de reportes financieros bajo demanda** desde Telegram.
- **Gestión de sesiones persistentes** que mantienen el contexto conversacional entre mensajes.
- **Control de roles** que diferencia la experiencia entre clientes y administradores.

### Beneficios Logrados

| Área | Antes | Después |
|---|---|---|
| **Toma de pedidos** | Manual, propensa a errores | 100% automatizada y registrada |
| **Visibilidad del pedido** | El cliente no sabía el estado | Notificaciones en tiempo real |
| **Carga de cocina** | Sin visibilidad de pedidos entrantes | Alertas inmediatas por Telegram |
| **Inventario** | Actualización manual o inexistente | Descuento automático por pedido |
| **Reportes** | Cálculo manual al final del día | Disponibles en segundos desde Telegram |
| **Fidelización** | Sin mecanismo de recompensa | Sistema de puntos automatizado |

### Impacto Esperado

La implementación de DeliveryBot en un entorno real permitiría reducir significativamente los tiempos de espera en la cafetería al eliminar el proceso manual de toma de pedidos, minimizar los errores por comunicación deficiente entre clientes y cocina, y proporcionar a la administración datos accionables sobre patrones de consumo para optimizar el inventario y la planificación de producción.

---

## 🚀 Posibles Mejoras Futuras

El sistema actual sienta una base sólida. Estas son las mejoras que representarían el siguiente nivel de evolución:

**💳 Integración de Pagos Digitales**
Conectar con pasarelas de pago como Nequi, Wompi o PSE para permitir el pago directamente desde el chat, eliminando la necesidad de pago en efectivo al retirar el pedido.

**📊 Dashboard Web de Administración**
Desarrollar un panel web con gráficas históricas de ventas, gestión visual del inventario y administración de usuarios, complementando las funcionalidades ya disponibles en Telegram.

**🤖 Recomendaciones con Inteligencia Artificial**
Integrar un modelo de lenguaje para analizar el historial de pedidos de cada usuario y hacer recomendaciones personalizadas ("Hola, hoy tenemos tu Café favorito con 10% de descuento").

**🎁 Sistema de Fidelización Avanzado**
Expandir el sistema de puntos actual para incluir recompensas canjeables, niveles de membresía (Bronce, Plata, Oro) y descuentos automáticos según el historial de compras.

**📦 Gestión Avanzada de Inventario**
Implementar alertas automáticas cuando el stock de un producto baje de un umbral configurable, y reportes de reabastecimiento para el administrador de la cafetería.

**⏰ Pedidos Programados**
Permitir al usuario programar su pedido para una hora específica, ideal para quien quiere tener su café listo al llegar a la oficina a las 8:00 a.m.

---

## 🏁 Conclusiones

### Conclusión Técnica

DeliveryBot demuestra que es posible construir un sistema de gestión operacional completo utilizando únicamente herramientas de bajo costo y sin infraestructura de servidor dedicada. La combinación de n8n como orquestador, Telegram como interfaz y Google Sheets como base de datos configura un stack tecnológico accesible, mantenible y escalable para pequeñas organizaciones.

La arquitectura basada en sesiones persistentes en Google Sheets resuelve de forma elegante el mayor desafío de los bots conversacionales: mantener el contexto entre mensajes. Esto permite flujos de múltiples pasos (como construir un carrito) sin necesidad de una base de datos relacional compleja.

### Conclusión Académica

El proyecto integra conceptos fundamentales de ingeniería de software: modelado de datos, diseño de flujos de negocio, manejo de estados, integración de APIs externas y experiencia de usuario, aplicados a un caso de uso real y tangible. La implementación exitosa de los 8 módulos funcionales del workflow valida la capacidad de las herramientas de automatización low-code para resolver problemas empresariales concretos.

### Conclusión de Negocio

La digitalización del proceso de pedidos en cafeterías institucionales no es un lujo, es una necesidad. DeliveryBot demuestra que el costo de implementación es significativamente menor que los costos ocultos de los errores manuales, los pedidos perdidos y la insatisfacción del cliente. Un sistema de esta naturaleza puede estar operativo en días, no meses.

> 🎯 *En resumen: DeliveryBot convierte una fila ruidosa y caótica en una conversación silenciosa y organizada. Y eso, en el mundo real, tiene un valor enorme.*

---

<div align="center">

**DeliveryBot** – Desarrollado con ❤️ usando n8n, Telegram y Google Sheets

*Proyecto académico – Campusland 2026*

</div>
