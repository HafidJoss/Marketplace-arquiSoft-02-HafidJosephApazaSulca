# Estilo Arquitectónico del Sistema Marketplace

## 1. Descripción General
En este documento se define el estilo arquitectónico seleccionado para el proyecto Marketplace de productos para mascotas. Se optó por una arquitectura de **Monolito Modular organizado en Capas**[cite: 10, 11].

- **Organización lógica:** Basada en 3 capas principales (Presentación, Lógica de Negocio y Datos) que delimitan las responsabilidades de cada módulo.
- **Unidad de despliegue:** Un único servicio monolítico en Node.js/Express, simplificando la infraestructura y el mantenimiento inicial.

---

## 2. Diagrama de Arquitectura (Mermaid)

```mermaid
graph TB
    %% Definición de Actores / Clientes
    subgraph Clientes ["Clientes del Sistema"]
        C1[Cliente]
        C2[Seller]
        C3[Administrador]
    end

    %% Cliente Web Frontend
    CW["Cliente Web<br/>(Angular / HTML / CSS / TypeScript)"]
    Clientes -->|HTTPS / JSON| CW

    %% Monolito Backend
    subgraph Monolito ["monolito: Marketplace Backend (Node.js 20 LTS - Express)"]
        
        %% Middlewares Transversales
        MW["Middlewares Express (Transversales)<br/>cors | express.json() | auth (JWT) | validación de entrada | manejo de errores | logger"]
        CW -->|HTTPS / JSON / REST| MW

        %% Capa 1: Presentación
        subgraph CapaPres ["1. CAPA DE PRESENTACIÓN"]
            direction LR
            M_Usu_R["usuarios.routes.js<br/>usuarios.controller.js"]
            M_Sel_R["sellers.routes.js<br/>sellers.controller.js"]
            M_Cat_R["catalogo.routes.js<br/>catalogo.controller.js"]
            M_Car_R["carrito.routes.js<br/>carrito.controller.js"]
            M_Ped_R["pedidos.routes.js<br/>pedidos.controller.js"]
        end

        MW --> CapaPres

        %% Capa 2: Lógica de Negocio
        subgraph CapaNeg ["2. CAPA DE LÓGICA DE NEGOCIO"]
            direction LR
            M_Usu_S["usuarios.service.js"]
            M_Sel_S["sellers.service.js"]
            M_Cat_S["catalogo.service.js"]
            M_Car_S["carrito.service.js"]
            M_Ped_S["pedidos.service.js"]
        end

        %% Relaciones entre Presentación y Lógica de Negocio
        M_Usu_R --> M_Usu_S
        M_Sel_R --> M_Sel_S
        M_Cat_R --> M_Cat_S
        M_Car_R --> M_Car_S
        M_Ped_R --> M_Ped_S

        %% Comunicación Inter-módulos (Servicios)
        M_Car_S -.-> M_Cat_S
        M_Ped_S -.-> M_Car_S
        M_Ped_S -.-> M_Usu_S

        %% Capa 3: Datos
        subgraph CapaDatos ["3. CAPA DE DATOS"]
            direction LR
            M_Usu_D["usuarios.repository.js"]
            M_Sel_D["sellers.repository.js"]
            M_Cat_D["catalogo.repository.js"]
            M_Car_D["carrito.repository.js"]
            M_Ped_D["pedidos.repository.js"]
        end

        %% Relaciones entre Lógica de Negocio y Datos
        M_Usu_S --> M_Usu_D
        M_Sel_S --> M_Sel_D
        M_Cat_S --> M_Cat_D
        M_Car_S --> M_Car_D
        M_Ped_S --> M_Ped_D

        %% ORM / Acceso Compartido
        ORM["Acceso a datos compartido: Sequelize (ORM) | modelos | pool de conexiones"]
        CapaDatos --> ORM
    end

    %% Base de Datos
    BD[("PostgreSQL<br/>marketplace_db")]
    ORM -->|SQL - TCP 5432| BD

    %% Servicios Externos
    subgraph Ext ["Sistemas Externos"]
        P_Pago["Pasarela de pagos<br/>(Ej. Culqi / Stripe)"]
        S_Envio["Servicio de envíos<br/>(Ej. Olva / Chazki)"]
    end

    M_Ped_S -->|HTTPS / REST| P_Pago
    M_Ped_S -->|HTTPS / REST| S_Envio

    %% Estilos de Nodos
    style Monolito fill:#f9f9f9,stroke:#333,stroke-width:2px
    style CapaPres fill:#e3f2fd,stroke:#1565c0,stroke-width:1px
    style CapaNeg fill:#e8f5e9,stroke:#2e7d32,stroke-width:1px
    style CapaDatos fill:#fff3e0,stroke:#ef6c00,stroke-width:1px
    style MW fill:#f3e5f5,stroke:#7b1fa2,stroke-width:1px
    style ORM fill:#ffe0b2,stroke:#e65100,stroke-dasharray: 5 5