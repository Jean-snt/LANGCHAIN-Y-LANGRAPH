# 🧑‍💻 Guía de Laboratorio: Agente de IA Modular con LangGraph, LangChain y MCP

Este laboratorio tiene como objetivo que elaboren una implementación funcional de un Agente de IA, demostrando el uso de una arquitectura modular: **LangGraph** para la orquestación, **LangChain** para las herramientas, y **MCP** (Model Context Protocol) para estandarizar la conexión a **MongoDB** (simulada).

## 🎯 Objetivo de la Entrega (Git Flow)

Al finalizar, deben demostrar el funcionamiento del agente con los dos casos de prueba.

1.  **Creación de Rama:** Trabajar en una nueva rama, ejemplo: `feature/agente-productos`.
2.  **Archivos:** Implementar la solución en un archivo principal (ej: `agent.py`).
3.  **Demostración:** El script debe incluir las pruebas de invocación para los casos "simple" y "búsqueda".

***

## ⚙️ I. Arquitectura y Componentes Clave

El agente debe seguir un flujo de decisión que utiliza un servicio externo (MongoDB), desacoplado mediante el protocolo MCP.

| Componente | Rol en el Laboratorio | Tarea Principal a Implementar |
| :--- | :--- | :--- |
| **MongoDB (Simulación)** | Fuente de Datos. | Función Python que simula la consulta de productos. |
| **LangChain** | Define la Herramienta. | Crear el objeto `Tool` a partir de la función de MongoDB. |
| **MCP** | El Adaptador Estandarizado. | Simular la carga de la herramienta a través del cliente MCP. |
| **LangGraph** | El Orquestador y Cerebro. | Construir el `StateGraph` con el **Borde Condicional**. |

## 🛠️ II. Pasos de Implementación Lógica

Deberán concentrarse en tres bloques lógicos dentro de su archivo principal (`agent.py`):

### Paso 1: Definición de Estado y Herramienta

Implementen la estructura de datos que compartirá el contexto y la herramienta a utilizar.

1.  **Base de Datos Simulado:** Implementen la función `get_product_details(nombre_producto: str) -> str`.
    * *Sugerencia:* Usen un diccionario simple para simular los datos de MongoDB.
2.  **Definición del Estado:** Creen la clase `AgentState` (`TypedDict`) que debe manejar:
    * `messages`: Historial de la conversación.
    * `classification`: El resultado del decisor (`"simple"` o `"busqueda"`).
    * `tool_result`: El resultado de la consulta a la BD (vía MCP).

### Paso 2: Creación de Nodos (Lógica del Agente)

Creen las funciones de Python que representan la lógica de cada etapa del grafo.

1.  **`classify_message` (Nodo Decisor):**
    * **Función:** Clasifica la intención del usuario.
    * **Lógica:** Debe retornar un `AgentState` con `classification` actualizado.
2.  **`execute_mcp_tool` (Nodo Ejecutor):**
    * **Función:** Invoca la herramienta de MongoDB/LangChain (simulada como cargada vía MCP).
    * **Lógica:** Extrae el producto a buscar del mensaje, llama a la función de la BD y almacena el resultado en el estado (`tool_result`).
3.  **`generate_final_response` (Nodo Finalizador):**
    *
