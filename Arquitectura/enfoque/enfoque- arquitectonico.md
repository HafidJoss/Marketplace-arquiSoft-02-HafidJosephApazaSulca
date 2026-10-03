# Enfoque Arquitectónico del Sistema Marketplace: Clean Architecture

## 1. Información General

| Elemento | Descripción aplicada al Marketplace |
| :--- | :--- |
| **Patrón / Enfoque Arquitectónico** | Clean Architecture (Arquitectura Limpia) |
| **Objetivo Principal** | Separar responsabilidades y controlar que las dependencias internas apunten siempre hacia el dominio. |
| **¿Qué problema resuelve?** | Evita el acoplamiento entre la interfaz visual de Angular, las reglas de negocio del Marketplace y las tecnologías externas (PostgreSQL, APIs REST, Pasarela de Pagos). |
| **Capas Definidas** | Presentación, Aplicación, Dominio e Infraestructura. |
| **Beneficios** | • Facilita el mantenimiento y la realización de pruebas unitarias.<br>• Permite cambiar tecnologías externas sin modificar el núcleo de negocio.<br>• Mejora la organización y separación de responsabilidades en el código. |

---

## 2. Definición de Capas y Responsabilidades

En Clean Architecture, el código se organiza en círculos concéntricos. **La regla de oro de la dependencia establece que las dependencias deben apuntar únicamente hacia adentro**, garantizando que el Dominio no dependa de ningún framework ni tecnología externa.

### 2.1. Capa de Dominio (`src/app/domain`)
Es el núcleo central de la aplicación y contiene la lógica propia del negocio.
* **Entidades (Models):** Objetos de negocio que contienen reglas globales (ej. `Producto`, `Carrito`, `Pedido`, `Usuario`).
* **Contratos / Puertos (Interfaces):** Define lo que el dominio necesita del exterior sin saber cómo se implementa (ej. `RepositorioProducto`, `RepositorioPedido`, `ProcesadorPagos`, `NotificacionCliente`).

### 2.2. Capa de Aplicación (`src/app/application`)
Contiene los casos de uso específicos del Marketplace de mascotas. Coordina el flujo de datos desde y hacia el dominio.
* **Casos de Uso (Use Cases):** Encapsulan las acciones que el usuario puede realizar en la plataforma (ej. `ConsultarCatalogoUseCase`, `AgregarAlCarritoUseCase`, `RegistrarCompraUseCase`).

### 2.3. Capa de Presentación / Adaptadores (`src/app/presentation`)
Contiene los componentes con los que interactúa el usuario en la aplicación Angular.
* **Componentes de UI:** `CatalogoComponent`, `DetalleCarritoComponent`, `CarritoComponent`, `AppComponent`.
* **Controladores de Estado:** Manejan la lógica de renderizado visual y reciben eventos del cliente.

### 2.4. Capa de Infraestructura (`src/app/infrastructure`)
Contiene las implementaciones técnicas concretas y las comunicaciones con el exterior.
* **Repositorios e Implementaciones:** `RepositorioProductoMemoria.ts`, `RepositorioPedidoMemoria.ts`.
* **Adaptadores de Servicios Externos:** `ProcesadorPagosConsola.ts`, `NotificacionConsola.ts`.
* **Conexión HTTP/REST:** Comunicación directa con el servidor backend del Marketplace.

---

## 3. Diagrama de Enfoque Arquitectónico (Mermaid)

# Enfoque Arquitectónico del Sistema Marketplace: Clean Architecture

## Diagrama de Enfoque Arquitectónico (Mermaid)

```mermaid
graph TD
    %% Actor
    Usuario[👤 Usuario / Cliente Web]

    %% Frontend Angular (Clean Architecture)
    subgraph Frontend ["📱 APLICACIÓN FRONTEND (Angular 18)"]
        
        %% CAPA 1: PRESENTACIÓN
        subgraph CapaPres ["1. CAPA DE PRESENTACIÓN (UI)"]
            UI["Componentes Angular<br/>(CatalogoComponent, CarritoComponent)"]
        end

        %% CAPA 2: APLICACIÓN
        subgraph CapaApp ["2. CAPA DE APLICACIÓN (Casos de Uso)"]
            UC["Casos de Uso<br/>(ConsultarCatalogo, AgregarAlCarrito, RegistrarCompra)"]
        end

        %% CAPA 3: DOMINIO
        subgraph CapaDom ["3. CAPA DE DOMINIO (Reglas del Negocio)"]
            Entidades["Entidades / Modelos<br/>(Producto, Carrito, Pedido)"]
            Puertos["Puertos / Contratos (Interfaces)<br/>(RepositorioProducto, ProcesadorPagos)"]
        end

        %% CAPA 4: INFRAESTRUCTURA
        subgraph CapaInfra ["4. CAPA DE INFRAESTRUCTURA (Implementaciones)"]
            Impl["Implementaciones Concretas<br/>(RepositorioMemoria, RepositorioHttp, ServicioPagos)"]
            DataLocal["DATOS.TS (Datos Simulado)"]
        end

        %% Inyección de Dependencias
        AppConfig["app.config.ts (Inyector de Dependencias)"]
    end

    %% Backend Externo
    Backend["🌐 Marketplace API REST (Backend Monolito)"]

    %% FLUJO Y DEPENDENCIAS (Regla de la Dependencia hacia Adentro)
    Usuario --> UI
    UI --> UC
    UC --> Entidades
    UC --> Puertos
    
    %% Inversión de Control
    Impl -.->|Implementa| Puertos
    DataLocal --> Impl
    AppConfig -.->|Inyecta Servicios| Impl

    %% Comunicación Externa
    Impl -->|HTTP / REST| Backend

    %% ESTILOS VISUALES
    style Frontend fill:#fafafa,stroke:#333,stroke-width:2px
    style CapaPres fill:#e3f2fd,stroke:#1565c0,stroke-width:1px
    style CapaApp fill:#e8f5e9,stroke:#2e7d32,stroke-width:1px
    style CapaDom fill:#fff8e1,stroke:#f57f17,stroke-width:2px
    style CapaInfra fill:#f3e5f5,stroke:#7b1fa2,stroke-width:1px
    style Backend fill:#eeeeee,stroke:#616161,stroke-width:1px