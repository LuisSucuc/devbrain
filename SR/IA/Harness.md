# 1. Concepto de "Arnés"
Es una metáfora: así como se usan riendas para controlar un caballo, el Harness es el entorno o envoltorio que rodea al modelo de IA para controlarlo, darle contexto y permitirle realizar tareas complejas de forma eficiente.

# 2. Los pilares del Harness
El Harness se basa en cuatro pilares fundamentales:

- **Contexto:** La información necesaria para que la IA sepa qué hacer.
- **Herramientas:** Funciones específicas a las que la IA tiene acceso.
- **Memoria:** Un sistema para recordar qué ha hecho y qué falta.
- **Validación:** Mecanismos para asegurar que el código generado funciona.

# 3. La paradoja de la complejidad
Vercel descubrió que menos es más. Al eliminar herramientas hiperespecializadas y usar herramientas simples de Unix (como `grep`, `cat`, `ls`), el rendimiento aumentó (3 veces más rápido) y el consumo de tokens bajó un 37%.

# 4. Gestión del contexto
La IA se degrada cuando su "ventana de contexto" está muy llena. Se recomienda extraer información fuera del contexto (memoria externa/archivos) y usar subagentes para tareas pequeñas, evitando que un solo agente herede toda la información innecesaria.

# 5. Verificación y calidad
La IA puede generar código convincente pero erróneo. Es vital obligar a la IA a verificar su trabajo mediante tests automatizados o agentes revisores.

# 6. Arquitectura multiagente
Se propone un modelo de tres pilares:

- Usar el repositorio como sistema.
- Orquestación multiagente (un líder que delega a otros).
- La verificación constante.

# 7. Implementación práctica
El autor muestra un ejemplo real (disponible en su GitHub) utilizando:

- **agents.md:** El protocolo de entrada.
- **init.sh:** Un script que verifica que el entorno esté listo antes de empezar.
- **feature_list.json:** Lista de tareas pendientes.
- **progress/:** Carpeta donde se guarda el histórico (memoria externa).
- Un agente **"Reviewer"** para asegurar calidad.