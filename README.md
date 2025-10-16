# 🤖 Agente de Logística Modular: LangGraph + MCP + MongoDB

Este repositorio contiene la solución para el Laboratorio de Arquitectura de Agentes de IA. El objetivo es construir un agente que use **LangGraph** para tomar decisiones condicionales sobre si consultar la base de datos de **MongoDB** (simulada) para obtener información logística.

La conexión a la base de datos se maneja a través del concepto de **MCP (Model Context Protocol)** para desacoplar la lógica del agente de la implementación de la herramienta.

## 🎯 Objetivo del Ejercicio

Implementar un agente de soporte logístico capaz de bifurcar el flujo de trabajo basándose en la intención del usuario:

1.  **Ruta 1 (Simple):** Si la consulta es un saludo o comentario general.
2.  **Ruta 2 (Consulta de Datos):** Si la consulta es sobre un `item_id`, `SKU`, o `pedido` que requiere la herramienta de la Base de Datos.

## ⚙️ Roles de Componentes

| Tecnología | Rol | Tarea Específica en el Agente |
| :--- | :--- | :--- |
| **LangGraph** | **Orquestador Central** | Define el flujo de nodos y el **Borde Condicional**. |
| **LangChain** | **Herramientas (Tool)** | Define la función `check_inventory` (consulta MongoDB simulada). |
| **MongoDB (Simulada)**| **Datos Persistentes** | Provee información como el estado de un pedido (`ORD-101`). |
| **MCP** | **Estandarización** | Desacopla la lógica del agente de la implementación de la herramienta de LangChain/MongoDB. |

## 🛠️ Estructura del Flujo de LangGraph

Tu implementación debe replicar esta lógica de grafo, donde el nodo `Decisor` clasifica y dirige el flujo:

```mermaid
graph LR
    A[START: Mensaje del Usuario] --> B(Nodo Decisor: Clasificación);
    B -->|'chat_simple'| C(Nodo Finalizador: Responder Directo);
    B -->|'consulta_logistica'| D(Nodo Ejecutor: Llama a Herramienta (VÍA MCP));
    D --> C;
    C --> E[END: Respuesta Final];
