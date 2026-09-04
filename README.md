# DeliveryBot - n8n Workflow

Este proyecto es una automatizacion local de n8n diseñada para digitalizar la atencion y los pedidos de una cafeteria a traves de Telegram. El flujo gestiona los menus, las sesiones de los usuarios y el estado de cada pedido utilizando integraciones fluidas con Redis y Google Sheets.

## Archivo Principal
- `DeliveryBot_Final.json`: Archivo de configuracion del flujo de n8n.

## Arquitectura y Tecnologias
- **n8n**: Motor de automatizacion y orquestacion del flujo.
- **Telegram**: Interfaz de comunicacion con el usuario, encargada de capturar comandos de texto y botones interactivos (callback queries).
- **Redis**: Sistema de almacenamiento de clave-valor utilizado para persistir el estado de la sesion del usuario y mantener el carrito de compras temporal.
- **Google Sheets**: Base de datos principal del proyecto. Requiere un documento con al menos dos hojas:
  - `USUARIOS`: Registro de clientes y sus puntos de lealtad.
  - `PRODUCTOS`: Catalogo de inventario, organizacion por categorias (Bebidas, Comida, Postres) y control de stock.

## Funcionamiento General
1. **Captura de interaccion**: El webhook recibe un mensaje de texto o un clic en un boton interactivo.
2. **Validacion de sesion**: Se consulta a Redis para verificar si el usuario tiene una sesion activa mediante su ID de Telegram.
3. **Registro de usuario**: Si no existe una sesion, el flujo verifica la hoja de Google Sheets. Si es un usuario nuevo, lo registra automaticamente.
4. **Enrutamiento de estados**: Dependiendo de la interaccion (Switch Node), el flujo redirige a diferentes logicas:
   - Menu principal.
   - Navegacion por categorias de productos.
   - Validacion de stock y adicion de productos al carrito (`/add ID CANTIDAD`).
   - Visualizacion del carrito de compras y calculo de totales.
5. **Procesamiento de datos**: Diversos nodos de codigo JavaScript (Code Nodes) se encargan de parsear las cadenas JSON de Redis, estructurar el formato HTML de salida y manejar la logica matematica del carrito antes de devolver una respuesta al cliente.

## Instalacion y Despliegue
1. Importar el archivo `DeliveryBot_Final.json` en una instancia local de n8n.
2. Configurar y enlazar las credenciales correspondientes:
   - API de Telegram.
   - Conexion a la instancia local de Redis.
   - API de Google Sheets (OAuth2).
3. Asegurar que el documento de Google Sheets coincida con la estructura de columnas requerida.
4. Activar el flujo para habilitar el Webhook de Telegram.
