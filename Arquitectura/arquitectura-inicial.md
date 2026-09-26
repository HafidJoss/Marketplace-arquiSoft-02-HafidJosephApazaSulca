# Ejercicio 09: Arquitectura en Capas Inicial

A continuación se presenta la propuesta de arquitectura organizada en tres capas según los módulos e interacciones identificados en el análisis del sistema:

---

## 1. Organización de Capas y Responsabilidades

| Capa | Pregunta que responde | Responsabilidades y Módulos |
| :--- | :--- | :--- |
| **Presentación** | ¿Cómo interactúa el usuario? | • Interfaz web de usuario (Frontend).<br>• Interfaz de API REST para la comunicación con el cliente y clientes externos. |
| **Lógica de Negocio** | ¿Qué hace el sistema? | Módulos encargados de procesar las reglas de negocio:<br>• **Usuarios:** Gestión de cuentas y autenticación.<br>• **Sellers:** Gestión de vendedores.<br>• **Catálogo:** Gestión y consulta de productos.<br>• **Carrito:** Gestión del carrito de compras.<br>• **Pedidos:** Creación, consulta y estado de las órdenes. |
| **Datos** | ¿Dónde se almacena la información? | • Base de datos persistente para almacenar usuarios, productos, carrito, pedidos y transacciones. |

---

## 2. Diagrama Estructural de la Arquitectura en Capas
![alt text](<Diagrama sin título.drawio.png>)
## 3. Descripción de Componentes e Integraciones

* **Capa de Presentación:** Muestra la interfaz gráfica a los actores (*Cliente*, *Seller* y *Administrador*) y se comunica con la lógica de negocio consumiendo servicios mediante una **API REST**[cite: 1].
* **Capa de Lógica de Negocio:** Encapsula las reglas operativas y valida los datos antes de persistirlos o comunicarse con servicios externos[cite: 1].
* **Capa de Datos:** Se encarga de gestionar las transacciones e información en la base de datos[cite: 1].
* **Integraciones Externas:** La capa de negocio o datos se conecta con la **Pasarela de Pago**, **ERP** y **Servicio de Envío** para completar los flujos comerciales[cite: 1].

> **Archivo destino:** Puedes guardar este contenido en `arquitectura/arquitectura-inicial.md` dentro del repositorio del proyecto[cite: 1].