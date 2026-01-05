# Horus - Binance Market Sync 📈

**Horus** es una aplicación de consola de alto rendimiento desarrollada en **.NET 8** para la monitorización y persistencia de datos del mercado de criptomonedas en tiempo real. El sistema automatiza la extracción de datos desde la API de Binance, filtra los activos con mayor rendimiento y sincroniza la información con una base de datos **SQL Server**.

---

## 🛠️ Stack Tecnológico y Dependencias

* **Lenguaje:** C# 12.
* **Framework:** .NET 8.0.
* **Base de Datos:** SQL Server (Compatible con Azure SQL y LocalDB).
* **Bibliotecas (NuGet):**
    * `Newtonsoft.Json` (v13.0.4): Para el procesamiento y deserialización de datos JSON.
    * `Microsoft.Data.SqlClient` (v6.1.3): Proveedor de datos para la comunicación con SQL Server.

---

## 🧠 Características Técnicas Destacadas

Este proyecto implementa patrones de diseño y técnicas de desarrollo modernas:

### 1. Programación Asíncrona (Async/Await)
Toda la lógica de Entrada/Salida (I/O) es no bloqueante. Se utilizan métodos como `GetStringAsync`, `OpenAsync`, `ExecuteNonQueryAsync` y `ReadAsync` para maximizar la escalabilidad del sistema.

### 2. Procesamiento de Datos con LINQ
Se utiliza **Language Integrated Query** para transformar la respuesta masiva de la API en información accionable:
* **Filtrado:** Selección exclusiva de pares comerciales `USDT`.
* [cite_start]**Ordenamiento:** Clasificación dinámica basada en el porcentaje de cambio de precio (`priceChangePercent`).
* **Ranking:** Selección automatizada del Top 10 del mercado.

### 3. Gestión de Base de Datos y Seguridad
* **Protección contra SQL Injection:** Uso de consultas parametrizadas para la inserción de datos.
* **DDL Automático:** El sistema incluye lógica para la creación automática de la tabla `Sales.CriptoMoneda` si no existe en el esquema.
* **Consistencia de Datos:** Implementación de un flujo de "Truncate and Load" para mantener el ranking siempre actualizado.

---

## 📋 Arquitectura del Proyecto



La solución se divide en componentes con responsabilidades claras (Separation of Concerns):

| Clase | Responsabilidad |
| :--- | :--- |
| `BinanceTicker` | [cite_start]Clase POCO para mapear los atributos `symbol`, `lastPrice` y `priceChangePercent`. |
| `DatabaseService` | Encapsula la cadena de conexión, la inicialización del esquema y las operaciones de persistencia. |
| `Program` | Orquestador principal que gestiona el ciclo de vida de la aplicación y el flujo de trabajo asíncrono. |

---

## 🚀 Guía de Instalación y Uso

### Requisitos Previos
* Visual Studio 2022 o VS Code con el SDK de .NET 8.
* Instancia de SQL Server activa.

### Pasos para Ejecutar
1. **Configurar la conexión:**
   Abre `DatabaseService.cs` y actualiza la cadena de conexión con tus credenciales:
   ```csharp
   _connectionString = @"Server=TU_SERVIDOR;Database=AdventureWorks2016_EXT;User Id=TU_USUARIO;Password=TU_CLAVE;Encrypt=True;TrustServerCertificate=True;";
