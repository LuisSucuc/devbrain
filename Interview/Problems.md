
# Limitar request por minuto por Usuarios

¿Como diseñarias un sistema para limitar el numero de requests (100 requests) por minuto por Usuarios?

1. Preguntar que tipo de sistema se esta limitando, si tiene microservicios y si tiene gateway.
2. Preguntar si se quiere cubrir un endpoint en especifico o toda la API.
3. Proponer que se pueda implementar el rate limit en el gateway en caso se pueda implementar esa caracteristica ahí.
4. Proponer que se podría utilizar un log de request por usuario en redis con tiempo de expiracion de 1 minuto. En donde la clave seria el user_id y el valor seria el numero de requests realizadas en ese minuto. Esto se validaría en el primer middleware del microservicio, asumiendo que la validación del token de usuario es encargada por el gateway. Obviamente el key value del usuario debería de iniciar en 1 si no existe. Además si es necesario también en este mismo microservicio se envia a un log para registrar los intentos de los usuarios de hacer request.

# Microservicio con event driven

Un usuario realizará una compra, queremos responderle en 40ms pero necesitoamos realizar varias acciones como:
- Consultar al proveedor el precio y stock
- Realizar el pago
- Actualizar el inventario
- Notificar al usuario via correo electrónico
- Actualizar el inventario

¿Cómo lo diseñarías?

1. Responderia primero exitosamente el request al usuario, confirmandole de que su orden ha sido recibida.
2. Luego se enviaria un mensaje al bus de eventos, el cual sería recibido por los microservicios correspondientes para realizar las acciones necesarias.
3. Utilizaria SQS y SNS para el envio de eventos, SNS enviaria el mensaje a cada lambda que realiza la Consulta al proveedor, Pago, Notificaciones, etc.
Aquí recordar  que SQS es un FIFO y SNS es un FIFO.