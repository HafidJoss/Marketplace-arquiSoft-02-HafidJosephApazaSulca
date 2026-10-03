# Estilo Arquitectónico del Sistema Marketplace

## 1. Descripción General
En este documento se define el estilo arquitectónico seleccionado para el proyecto Marketplace de productos para mascotas. Se optó por una arquitectura de **Monolito Modular organizado en Capas**[cite: 10, 11].

- **Organización lógica:** Basada en 3 capas principales (Presentación, Lógica de Negocio y Datos) que delimitan las responsabilidades de cada módulo.
- **Unidad de despliegue:** Un único servicio monolítico en Node.js/Express, simplificando la infraestructura y el mantenimiento inicial.

---

## 2. Diagrama de Arquitectura (Mermaid)

```mermaid
graph TD
    %% Usuarios
    Usuarios[Usuarios: Cliente / Seller / Admin] -->|Peticiones HTTP| Pres

    %% Monolito Backend
    subgraph Monolito ["MONOLITO BACKEND"]
        
        subgraph Pres ["1. Capa de Presentacion (Controladores / Rutas)"]
            Controllers["Modulos: Usuarios | Catalogo | Carrito | Pedidos"]
        end

        subgraph Logica ["2. Capa de Logica de Negocio (Servicios)"]
            Services["Reglas de Negocio y Procesamiento"]
        end

        subgraph Datos ["3. Capa de Datos (Repositorios / ORM)"]
            Repositories["Acceso y Consultas de Datos"]
        end

        Controllers --> Logica
        Logica --> Repositories
    end

    %% Base de Datos y Externos
    Repositories --> BD[("Base de Datos<br/>PostgreSQL")]
    Logica --> Pasarela["Pasarela de Pagos"]
    Logica --> Envio["Servicio de Envios"]

    %% Estilos simples
    style Monolito fill:#f8f9fa,stroke:#333,stroke-width:2px
    style Pres fill:#e3f2fd,stroke:#1565c0
    style Logica fill:#e8f5e9,stroke:#2e7d32
    style Datos fill:#fff3e0,stroke:#ef6c00