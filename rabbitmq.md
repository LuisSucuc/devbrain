# RabbitMQ — Guía completa para principiantes

## ¿Qué es RabbitMQ?

RabbitMQ es un **message broker** (intermediario de mensajes). Su trabajo es recibir mensajes de un lado (productor) y entregarlos al otro lado (consumidor), sin que ambos lados necesiten conocerse ni estar disponibles al mismo tiempo.

Piénsalo como el sistema de correo postal:
- **Tú** escribes una carta (productor)
- **El correo** la guarda y enruta (RabbitMQ)
- **El destinatario** la recoge cuando puede (consumidor)

---

## Conceptos fundamentales

### 1. Message (Mensaje)

La unidad básica de información que viaja por el sistema. Contiene:
- **Body**: el contenido (JSON, texto, bytes, etc.)
- **Headers**: metadatos opcionales
- **Properties**: configuración como prioridad, TTL, tipo de contenido

```json
{
  "body": "{\"userId\": 42, \"action\": \"send_email\"}",
  "properties": {
    "content_type": "application/json",
    "delivery_mode": 2
  }
}
```

---

### 2. Producer (Productor)

Aplicación que **envía** mensajes a RabbitMQ. El productor nunca envía directamente a una cola; envía a un **exchange**.

---

### 3. Consumer (Consumidor)

Aplicación que **recibe y procesa** mensajes desde una cola. Puede haber múltiples consumidores en la misma cola.

---

### 4. Queue (Cola)

Buffer donde se almacenan los mensajes hasta que un consumidor los procesa. Las colas son **FIFO** (primero en entrar, primero en salir) por defecto.

Propiedades importantes:
| Propiedad | Descripción |
|-----------|-------------|
| `durable` | La cola sobrevive a reinicios del broker |
| `exclusive` | Solo la puede usar la conexión que la creó |
| `auto-delete` | Se elimina cuando no hay consumidores |
| `arguments` | Configuración extra (TTL, límite de mensajes, etc.) |

---

### 5. Exchange (Intercambiador)

El productor nunca habla directamente con una cola. Envía al exchange, que **enruta** el mensaje a la(s) cola(s) correcta(s) según reglas.

---

### 6. Binding (Enlace)

Es la regla que conecta un exchange con una cola. Le dice al exchange: "los mensajes con estas características van a esta cola".

---

### 7. Routing Key (Clave de enrutamiento)

Etiqueta que el productor pone en el mensaje. El exchange la usa para decidir a qué cola enviarlo.

---

## Flujo general

```mermaid
flowchart LR
    P["🖥️ Producer\n(tu app)"]
    E["📬 Exchange"]
    B1["🔗 Binding\nrouting_key=A"]
    B2["🔗 Binding\nrouting_key=B"]
    Q1["📦 Queue A"]
    Q2["📦 Queue B"]
    C1["🖥️ Consumer 1"]
    C2["🖥️ Consumer 2"]

    P -->|"Mensaje +\nrouting_key"| E
    E --> B1 --> Q1 --> C1
    E --> B2 --> Q2 --> C2
```

---

## Tipos de Exchange

Los exchanges son el corazón del enrutamiento. Hay 4 tipos:

### Direct Exchange

Enruta el mensaje a la cola cuyo binding key coincide **exactamente** con la routing key del mensaje.

```mermaid
flowchart LR
    P["Producer"]
    E["Direct Exchange"]
    Q1["Queue: email"]
    Q2["Queue: sms"]
    C1["Consumer Email"]
    C2["Consumer SMS"]

    P -->|"routing_key = email"| E
    E -->|"binding_key = email"| Q1
    E -->|"binding_key = sms"| Q2
    Q1 --> C1
    Q2 --> C2
```

**Cuándo usarlo**: cuando quieres enviar un mensaje a un destino específico.

---

### Fanout Exchange

Ignora la routing key y envía el mensaje a **todas** las colas enlazadas. Broadcast total.

```mermaid
flowchart LR
    P["Producer"]
    E["Fanout Exchange"]
    Q1["Queue: logs_db"]
    Q2["Queue: logs_file"]
    Q3["Queue: logs_monitor"]

    P -->|"cualquier mensaje"| E
    E --> Q1
    E --> Q2
    E --> Q3
```

**Cuándo usarlo**: notificaciones generales, eventos que necesitan múltiples procesadores (guardar log + enviar alerta + actualizar dashboard).

---

### Topic Exchange

Enruta usando **patrones** en la routing key. Los patrones usan:
- `*` → exactamente una palabra
- `#` → cero o más palabras

Las palabras se separan con puntos: `orden.creada.mexico`

```mermaid
flowchart LR
    P["Producer"]
    E["Topic Exchange"]
    Q1["Queue: ordenes_mx"]
    Q2["Queue: ordenes_all"]
    Q3["Queue: errores"]

    P -->|"orden.creada.mexico"| E
    E -->|"orden.*.mexico"| Q1
    E -->|"orden.#"| Q2
    E -->|"#.error"| Q3
```

**Cuándo usarlo**: sistemas con eventos categorizados (logs por nivel y servicio, eventos por región, etc.).

---

### Headers Exchange

Enruta basándose en los **headers** del mensaje en lugar de la routing key. Más flexible pero menos común.

**Cuándo usarlo**: cuando necesitas enrutar por múltiples atributos complejos.

---

## Tabla comparativa de Exchanges

| Tipo | Routing Key | Uso típico |
|------|-------------|------------|
| `direct` | Match exacto | Tareas específicas por tipo |
| `fanout` | Se ignora | Broadcast / notificaciones |
| `topic` | Patrones con `*` y `#` | Eventos categorizados |
| `headers` | Headers del mensaje | Enrutamiento complejo |

---

## Acknowledgements (ACK/NACK)

Cuando un consumidor recibe un mensaje, RabbitMQ espera confirmación:

- **ACK (acknowledge)**: "procesé el mensaje correctamente, puedes borrarlo"
- **NACK (negative acknowledge)**: "algo salió mal, reencola el mensaje"
- **Reject**: "rechazo este mensaje" (con opción de descartarlo o reencolarlo)

```mermaid
sequenceDiagram
    participant R as RabbitMQ
    participant C as Consumer

    R->>C: Entrega mensaje
    C->>C: Procesa mensaje
    alt Procesamiento exitoso
        C->>R: ACK ✅
        R->>R: Elimina mensaje
    else Error en procesamiento
        C->>R: NACK ❌
        R->>R: Reencola mensaje
    end
```

> **Importante**: si el consumidor muere sin enviar ACK, RabbitMQ reencola el mensaje automáticamente.

---

## Durabilidad y persistencia

Para no perder mensajes si RabbitMQ se reinicia, necesitas **dos cosas**:

1. **Cola durable**: la cola se recrea al reiniciar
2. **Mensaje persistente**: `delivery_mode = 2`

```mermaid
flowchart TD
    A["¿Se reinicia RabbitMQ?"]
    B["Cola durable = true\n+ Mensaje persistente"]
    C["Cola durable = false\no Mensaje no persistente"]
    D["✅ Mensaje sobrevive"]
    E["❌ Mensaje se pierde"]

    A --> B --> D
    A --> C --> E
```

---

## Dead Letter Queue (DLQ)

Una **cola de mensajes muertos** recibe los mensajes que no pudieron ser procesados (NACK + no reencolar, TTL expirado, cola llena).

```mermaid
flowchart LR
    P["Producer"]
    Q["Queue principal"]
    C["Consumer"]
    DLQ["Dead Letter Queue"]
    D["Equipo de soporte\n/ análisis"]

    P --> Q
    Q --> C
    C -->|"NACK / TTL expirado"| DLQ
    DLQ --> D
```

**Cuándo usarlo**: siempre que quieras inspeccionar mensajes fallidos en lugar de perderlos.

---

## Prefetch y distribución de trabajo

Cuando hay múltiples consumidores, RabbitMQ puede distribuir mensajes en round-robin. Con `prefetch_count` controlas cuántos mensajes no confirmados puede tener un consumidor a la vez.

```mermaid
flowchart LR
    Q["Queue\n(100 mensajes)"]
    C1["Consumer 1\nprefetch=1"]
    C2["Consumer 2\nprefetch=1"]
    C3["Consumer 3\nprefetch=1"]

    Q -->|"msg 1"| C1
    Q -->|"msg 2"| C2
    Q -->|"msg 3"| C3
    Q -->|"msg 4 (cuando alguno libere)"| C1
```

**Recomendación**: usa `prefetch_count = 1` para distribuir trabajo de forma justa cuando las tareas tienen duración variable.

---

## Virtual Hosts (vhost)

Son espacios de nombres dentro de RabbitMQ. Permiten tener múltiples entornos (dev, staging, prod) en un mismo servidor con total aislamiento.

```
RabbitMQ Server
├── /vhost-dev
│   ├── exchange: orders
│   └── queue: orders_queue
├── /vhost-staging
│   ├── exchange: orders
│   └── queue: orders_queue
└── /vhost-prod
    ├── exchange: orders
    └── queue: orders_queue
```

---

## Conexiones y Canales

- **Connection**: conexión TCP al servidor RabbitMQ. Es costosa de crear.
- **Channel**: canal virtual dentro de una conexión. Son baratos. Úsalos para operaciones individuales.

```mermaid
flowchart TD
    A["Tu aplicación"]
    CONN["TCP Connection\n(1 por app)"]
    CH1["Channel 1\n(producer)"]
    CH2["Channel 2\n(consumer)"]
    CH3["Channel 3\n(admin)"]
    RMQ["RabbitMQ"]

    A --> CONN
    CONN --> CH1 --> RMQ
    CONN --> CH2 --> RMQ
    CONN --> CH3 --> RMQ
```

> **Buena práctica**: una conexión por aplicación, un canal por hilo/operación.

---

## Ejemplo práctico: Sistema de notificaciones

Supón que tienes una app que necesita enviar emails, SMS y guardar logs cuando un usuario se registra.

```mermaid
flowchart LR
    APP["API\n(Producer)"]
    EX["Fanout Exchange\nuser.registered"]
    QE["Queue\nemail_notifications"]
    QS["Queue\nsms_notifications"]
    QL["Queue\naudit_log"]
    CE["Consumer\nEmail Service"]
    CS["Consumer\nSMS Service"]
    CL["Consumer\nLog Service"]

    APP -->|"user.registered event"| EX
    EX --> QE --> CE
    EX --> QS --> CS
    EX --> QL --> CL
```

**Flujo**:
1. Usuario se registra → API publica evento en el exchange `user.registered`
2. El fanout exchange distribuye a las 3 colas
3. Cada servicio procesa de forma independiente y asíncrona
4. Si el servicio de SMS falla, los otros no se ven afectados

---

## Ejemplo práctico: Procesamiento de órdenes

```mermaid
flowchart LR
    API["API"]
    EX["Topic Exchange\norders"]
    QMX["Queue: orders_mx"]
    QUS["Queue: orders_us"]
    QVIP["Queue: orders_vip"]
    CMX["Consumer MX"]
    CUS["Consumer US"]
    CVIP["Consumer VIP\n(prioridad)"]

    API -->|"order.new.mx"| EX
    API -->|"order.new.us"| EX
    API -->|"order.vip.mx"| EX

    EX -->|"order.*.mx"| QMX
    EX -->|"order.*.us"| QUS
    EX -->|"order.vip.#"| QVIP

    QMX --> CMX
    QUS --> CUS
    QVIP --> CVIP
```

---

## Resumen de conceptos clave

```mermaid
mindmap
  root((RabbitMQ))
    Componentes
      Producer
      Consumer
      Queue
      Exchange
      Binding
    Tipos de Exchange
      Direct
      Fanout
      Topic
      Headers
    Confiabilidad
      ACK / NACK
      Durabilidad
      Persistencia
      Dead Letter Queue
    Performance
      Prefetch
      Channels
      Connections
    Organización
      Virtual Hosts
      Routing Keys
```

---

## Checklist para tu sistema

Antes de implementar, define:

- [ ] ¿Qué **eventos** va a publicar tu sistema?
- [ ] ¿Cuántos **consumidores** procesarán cada evento?
- [ ] ¿Qué tipo de **exchange** necesitas para cada caso?
- [ ] ¿Los mensajes deben ser **durables** (sobrevivir reinicios)?
- [ ] ¿Cómo manejarás los mensajes **fallidos** (DLQ)?
- [ ] ¿Necesitas **múltiples entornos** (vhosts)?
- [ ] ¿Cuál es el `prefetch_count` adecuado para tus consumers?

---

## Recursos para continuar

- [Documentación oficial](https://www.rabbitmq.com/documentation.html)
- [Tutoriales oficiales (6 partes)](https://www.rabbitmq.com/tutorials) — hay versiones en Python, Java, Go, JS, etc.
- Management UI: `http://localhost:15672` (usuario/contraseña: `guest/guest` por defecto)
