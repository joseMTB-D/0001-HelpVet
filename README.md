# HelpPet-Vet 🐾

**Sistema integral de gestión veterinaria multiplataforma**

[![Build Status](https://img.shields.io/badge/build-passing-brightgreen)](./docs/BUILD.md)
[![License](https://img.shields.io/badge/license-MIT-blue)](./LICENSE)
[![Version](https://img.shields.io/badge/version-1.0.0-orange)](https://github.com)
[![Maintainer](https://img.shields.io/badge/maintainer-Jose%20Manuel%20Toribio-blueviolet)](mailto:contact@example.com)

## 📋 Descripción del Proyecto

**HelpPet-Vet** es una plataforma multiplataforma de código educativo diseñada para gestionar de manera eficiente las operaciones de una clínica veterinaria. La aplicación integra un sistema administrativo de escritorio (Windows) y una aplicación móvil (Android) para proporcionar acceso flexible a registros de pacientes, mascotas, tratamientos y citas veterinarias.

### Problema que Resuelve

- ❌ **Antes**: Gestión manual y desorganizada de registros veterinarios
- ✅ **Ahora**: Plataforma centralizada con acceso desde múltiples dispositivos

## ⚡ Características Principales

- **Gestión de Clientes**: Registro, búsqueda y administración de propietarios de mascotas
- **Gestión de Mascotas**: Base de datos completa de animales con detalles de especie, raza, características físicas
- **Historial Clínico**: Registro de casos, tratamientos, medicamentos y enfermedades
- **Autenticación Dual**: Sistema de login para veterinarios y administradores
- **Interfaz Multiplataforma**: Aplicación de escritorio (Windows Forms) y móvil (Android)
- **Búsqueda Avanzada**: Filtrado de pacientes, mascotas y casos por múltiples criterios
- **Reportes**: Generación de historialles clínicos detallados

## 🛠️ Tech Stack

### Backend & Base de Datos
| Tecnología | Versión | Propósito |
|-----------|---------|----------|
| **MySQL** | 8.0.22 | Base de datos relacional en la nube (Clever Cloud) |
| **PHP** | 7.2+ | APIs backend para integración móvil |

### Aplicación de Escritorio
| Tecnología | Versión | Propósito |
|-----------|---------|----------|
| **.NET Framework** | 4.7.2 | Framework de la aplicación |
| **Visual Basic .NET** | - | Lenguaje de programación |
| **Windows Forms** | - | UI framework desktop |
| **MySql.Data** | - | Conector MySQL para .NET |
| **Renci.SshNet** | - | Soporte para conexiones SSH |
| **BouncyCastle** | - | Librería de criptografía |
| **Google.Protobuf** | - | Serialización de datos |

### Aplicación Móvil
| Tecnología | Versión | Propósito |
|-----------|---------|----------|
| **Android** | 5.0+ (API 16) | Sistema operativo móvil |
| **Android Gradle** | 4.1.1 | Build system |
| **Java** | 1.8 | Lenguaje de programación |
| **Volley** | 1.1.1 | Librería HTTP para solicitudes de red |
| **Material Design** | 1.2.1 | Componentes UI modernos |
| **AndroidX** | - | Bibliotecas de soporte compatibles |

## 📦 Instalación y Configuración

### Requisitos Previos

#### Para la Aplicación de Escritorio
- Windows 7 o superior
- .NET Framework 4.7.2 instalado
- Acceso a internet (conexión a BD en la nube)

#### Para la Aplicación Móvil
- Android 5.0 (API 16) o superior
- Mínimo 50 MB de espacio libre

#### Para Desarrollo
- Visual Studio 2019+ (con soporte Visual Basic)
- Android Studio 4.1+
- Git
- MySQL Client (opcional)

### Instalación de la Aplicación de Escritorio

1. **Clonar el repositorio**
   ```bash
   git clone https://github.com/jmtoribio/001-HelpVet.git
   cd 001-HelpVet
   ```

2. **Abrir el proyecto en Visual Studio**
   ```bash
   # Navegar a la carpeta del proyecto de escritorio
   cd "Aplicacio escriptori/VisualBasic/HelPPet-Vet"
   
   # Abrir la solución
   open HelPPet-Vet.sln
   ```

3. **Configurar la conexión a base de datos**
   
   Editar el archivo `DT.vb` con tus credenciales:
   ```vb
   conexion = New MySqlConnection("server=YOUR_SERVER; user=YOUR_USER; password=YOUR_PASSWORD; database=YOUR_DATABASE")
   ```

4. **Restaurar paquetes NuGet**
   ```
   En Visual Studio: Tools → NuGet Package Manager → Package Manager Console
   Ejecutar: Update-Package
   ```

5. **Compilar la solución**
   ```
   Build → Build Solution (Ctrl+Shift+B)
   ```

6. **Ejecutar la aplicación**
   ```
   Debug → Start Debugging (F5)
   ```

### Instalación de la Aplicación Móvil

1. **Clonar el repositorio**
   ```bash
   git clone https://github.com/jmtoribio/001-HelpVet.git
   cd 001-HelpVet
   ```

2. **Abrir en Android Studio**
   ```bash
   cd "App mobil/APP-1 - Final"
   # Abrir la carpeta en Android Studio
   ```

3. **Sincronizar Gradle**
   ```
   File → Sync Now
   ```

4. **Configurar conexión al servidor PHP**
   
   Editar las URLs base en los archivos de actividad:
   ```properties
   # Buscar referencias a "Volley" y actualizar las URLs base
   ```

5. **Compilar el APK**
   ```
   Build → Build Bundle(s) / APK(s) → Build APK(s)
   ```

6. **Instalar en dispositivo o emulador**
   ```bash
   adb install app/release/app-release.apk
   # O usar Android Studio: Run → Run 'app'
   ```

### Configuración de Base de Datos

1. **Importar esquema SQL**
   ```bash
   mysql -u YOUR_USER -p YOUR_DATABASE < "Aplicacio escriptori/helppet_database.sql"
   ```

2. **Usuarios de demostración** (después de importar)
   
   **Veterinarios:**
   - Usuario: `jep` | Contraseña: `2222`
   - Usuario: `lth` | Contraseña: `1234`

   **Propietarios (ejemplo):**
   - DNI: `09433726T` | Contraseña: `2222`
   - DNI: `00685667Z` | Contraseña: `1111`

## 🚀 Uso

### Aplicación de Escritorio

```vb
' Iniciar sesión como Veterinario
1. Ejecutar HelPPet-Vet.exe
2. Ingresar credenciales de veterinario
3. Navegar a "Gestión de Mascotas" para ver el historial
4. Usar "Nuevo Caso" para registrar una revisión

' Acceso Administrador
1. El archivo admin.txt en el directorio debe contener credenciales de admin
2. Acceso a gestión de veterinarios y configuración
```

### Aplicación Móvil

```java
// Flujo típico de usuario
1. Abrir la aplicación
2. Iniciar sesión con credenciales de propietario
3. Ver lista de mascotas registradas
4. Consultar casos y tratamientos
5. Solicitar citas (si se implementa)
```

## 📁 Estructura del Proyecto

```
001-HelpVet/
├── Aplicacio escriptori/                 # Aplicación de escritorio (Windows)
│   ├── VisualBasic/
│   │   └── HelPPet-Vet/
│   │       ├── HelPPet-Vet.sln          # Solución Visual Studio
│   │       ├── HelPPet-Vet/
│   │       │   ├── Login.vb             # Pantalla de autenticación
│   │       │   ├── AdminSide.vb         # Panel administrativo
│   │       │   ├── Veterinari.vb        # Panel veterinario
│   │       │   ├── DT.vb                # Conexión a BD
│   │       │   ├── App.config           # Configuración
│   │       │   └── ...
│   │       └── bin/                     # Compilados
│   ├── Installer Helppet-VET/           # Instalador Windows
│   │   └── Application Files/           # Versiones compiladas
│   └── helppet_database.sql             # Esquema de BD
│
├── App mobil/                            # Aplicación móvil (Android)
│   ├── APP-1 - Final/
│   │   ├── app/
│   │   │   ├── src/main/
│   │   │   │   ├── java/com/example/helppet/  # Código Java
│   │   │   │   ├── res/                       # Recursos (layouts, strings)
│   │   │   │   └── AndroidManifest.xml        # Configuración Android
│   │   │   ├── build.gradle              # Dependencias
│   │   │   └── proguard-rules.pro         # Reglas de ofuscación
│   │   ├── gradle/                       # Wrapper de Gradle
│   │   └── build.gradle                  # Build configuration
│   ├── Volley/                           # APIs PHP backend
│   │   ├── login.php
│   │   ├── registro.php
│   │   ├── llenar_spinner.php
│   │   └── ...
│   └── helppet.apk                       # Aplicación compilada
│
├── .github/                              # GitHub Actions
├── README.md                             # Este archivo
├── SECURITY.md                           # Políticas de seguridad
└── docs/                                 # Documentación técnica
```

## 🏗️ Arquitectura del Sistema

### Diagrama de Componentes

```
┌─────────────────────────────────────────────────────────────┐
│                    HELPVET-VET PLATFORM                     │
├──────────────────────────┬──────────────────────────────────┤
│   APLICACIÓN DESKTOP     │    APLICACIÓN MÓVIL (ANDROID)    │
│  (Windows Forms VB.NET)  │        (Java/Kotlin)             │
│                          │                                  │
│ • Login Form             │ • Login Activity                 │
│ • Admin Panel            │ • Pet List Activity              │
│ • Vet Panel              │ • Case Details Activity          │
│ • Veterinarian Mgmt      │ • Appointment Booking            │
│ • Case Management        │ • Owner Profile                  │
└──────┬───────────────────┴────────────────┬──────────────────┘
       │                                     │
       └──────────────────┬──────────────────┘
                          │
                    ┌─────▼──────┐
                    │   API PHP   │  (Volley HTTP Requests)
                    │  Backend    │
                    └─────┬──────┘
                          │
                    ┌─────▼──────────────────┐
                    │  MySQL Database        │
                    │  (Clever Cloud)        │
                    │                        │
                    │ • propietari (Owners)  │
                    │ • mascota (Pets)       │
                    │ • veterinari (Vets)    │
                    │ • casos (Cases)        │
                    └──────────────────────┘
```

### Flujo de Datos

1. **Autenticación**: Usuario → Aplicación → MySQL (validación de credenciales)
2. **Gestión de Mascotas**: CRUD operations → API/Direct DB → Sincronización entre apps
3. **Registro de Casos**: Veterinario → Caso → Medicamentos/Enfermedades → BD
4. **Consultas**: Usuario consulta historial → API queries → Presentación en UI

## 💾 Modelos de Datos

### Tabla `propietari` (Propietarios)
```sql
CREATE TABLE propietari (
  nom VARCHAR(50),              -- Nombre del propietario
  DNI VARCHAR(9) PRIMARY KEY,   -- Cédula/NIF
  t1 INT,                       -- Teléfono 1
  t2 INT,                       -- Teléfono 2
  Adreça VARCHAR(40),           -- Dirección
  CP INT,                       -- Código postal
  gmail VARCHAR(24),            -- Email
  pass VARCHAR(12)              -- Contraseña
);
```

### Tabla `mascota` (Mascotas)
```sql
CREATE TABLE mascota (
  codi BIGINT PRIMARY KEY,      -- ID único de mascota
  Propietari VARCHAR(9),        -- DNI del propietario
  nom VARCHAR(20),              -- Nombre de la mascota
  edat DATE,                    -- Fecha de nacimiento
  enfermetat VARCHAR(60),       -- Enfermedad actual
  tractat INT,                  -- ¿Tratado?
  Especie VARCHAR(20),          -- Perro, Gato, Ave, etc.
  raza VARCHAR(30),             -- Raza específica
  sexo VARCHAR(30),             -- Macho/Hembra
  color VARCHAR(30),            -- Color
  Tamaño VARCHAR(20),           -- Pequeño/Mediano/Grande
  pelo VARCHAR(35),             -- Tipo de pelaje
  castrado VARCHAR(20),         -- Estado de castración
  peso DOUBLE                   -- Peso en kg
);
```

### Tabla `veterinari` (Veterinarios)
```sql
CREATE TABLE veterinari (
  codi BIGINT PRIMARY KEY,      -- Identificador único
  nom VARCHAR(50),              -- Nombre completo
  clinica VARCHAR(50),          -- Clínica/Hospital asociado
  Usuari VARCHAR(3),            -- Nombre de usuario
  password INT                  -- Contraseña (sin cifrar - MEJORAR)
);
```

### Tabla `casos` (Casos Clínicos)
```sql
CREATE TABLE casos (
  codi_cas BIGINT PRIMARY KEY,  -- ID del caso
  codi_veterinari BIGINT,       -- Veterinario responsable
  codi_mascota BIGINT,          -- Mascota tratada
  Data_Registre TIMESTAMP,      -- Fecha de registro
  Data_Revisio DATE,            -- Fecha de próxima revisión
  Observacio VARCHAR(800),      -- Notas del veterinario
  pes DOUBLE,                   -- Peso actual
  tractament VARCHAR(400),      -- Tratamiento prescrito
  medicaments VARCHAR(400),     -- Medicamentos
  enfermetats VARCHAR(400)      -- Enfermedades diagnosticadas
);
```

## 🔌 API Endpoints (Backend PHP)

### Autenticación
```
POST /login.php
  body: { usuario, password }
  response: { success, datos_usuario }

POST /registro.php
  body: { nom, DNI, email, password, ... }
  response: { success, message }
```

### Gestión de Mascotas
```
GET /llenar_spinner.php
  params: { tipo_mascota }
  response: [ { codi, nom, ... } ]

POST /llenar_info.php
  params: { codi_mascota }
  response: { propietario, mascota, casos }
```

### Gestión de Casos
```
POST /llenar_spinner_casos.php
  params: { codi_veterinari }
  response: [ casos ]

POST /llenar_trata.php
  params: { codi_caso }
  response: { tratamiento, medicamentos, observaciones }
```

## 📚 Documentación Técnica Adicional

Para documentación más detallada, ver:
- [ARCHITECTURE.md](./docs/ARCHITECTURE.md) - Detalles técnicos profundos
- [DATABASE.md](./docs/DATABASE.md) - Esquema completo de BD
- [API.md](./docs/API.md) - Especificación de endpoints
- [DEPLOYMENT.md](./docs/DEPLOYMENT.md) - Guía de despliegue

## 🤝 Guía de Contribución

### Proceso de Contribución

1. **Fork el repositorio**
   ```bash
   git clone https://github.com/YOUR_USERNAME/001-HelpVet.git
   cd 001-HelpVet
   ```

2. **Crear rama de feature**
   ```bash
   git checkout -b feature/nueva-funcionalidad
   # o para bugs:
   git checkout -b bugfix/nombre-del-bug
   ```

3. **Realizar cambios**
   - Seguir las convenciones de código del proyecto
   - Comentar código complejo
   - Realizar commits pequeños y descriptivos

4. **Validar cambios**
   - Compilar sin errores
   - Probar en dispositivo/emulador
   - Verificar compatibilidad con versiones anteriores

5. **Hacer commit y push**
   ```bash
   git add .
   git commit -m "feat: descripción clara del cambio"
   git push origin feature/nueva-funcionalidad
   ```

6. **Crear Pull Request**
   - Describir cambios realizados
   - Referenciar issues relacionados
   - Adjuntar capturas de pantalla si aplica

### Estándares de Código

#### Visual Basic .NET
```vb
' Nombres en PascalCase para clases y métodos
Public Class NuevoFormulario
    ' Variables privadas con prefijo descriptivo
    Private conexion As MySqlConnection
    Private datos As DataTable
    
    ' Métodos con documentación
    ''' <summary>
    ''' Descripción del método
    ''' </summary>
    Public Sub MiMetodo()
    End Sub
End Class
```

#### Java/Android
```java
// Clases en PascalCase
public class LoginActivity extends AppCompatActivity {
    // Constantes en UPPER_CASE
    private static final String BASE_URL = "http://api.example.com/";
    
    // Variables en camelCase
    private EditText emailInput;
    private Button submitButton;
    
    // Métodos descriptivos
    private void validateInput() {
    }
}
```

### Proceso de Review

- Mínimo 1 aprobación requerida
- Todos los tests deben pasar
- Sin conflictos de merge
- Cobertura de código > 70%

### Reporte de Bugs

Usar template en [ISSUE_TEMPLATE.md](.github/ISSUE_TEMPLATE.md):
```markdown
## Descripción del Bug
[Descripción clara y concisa]

## Pasos para Reproducir
1. Paso uno
2. Paso dos

## Comportamiento Esperado
[Qué debería pasar]

## Comportamiento Actual
[Qué está pasando]

## Capturas de Pantalla
[Si aplica]

## Información del Sistema
- OS: Windows 10
- Versión App: 1.0.0
```

## 🔐 Seguridad

⚠️ **NOTA IMPORTANTE**: Este proyecto es educativo. Para producción, implementar:

- Hashing seguro de contraseñas (bcrypt, PBKDF2)
- Validación y sanitización de inputs
- Encriptación SSL/TLS
- Autenticación OAuth2
- HTTPS en todas las conexiones
- Rate limiting en APIs
- Auditoría de acceso
- GDPR compliance para datos de mascotas

Ver [SECURITY.md](./SECURITY.md) para más detalles.

## 📄 Licencia

Este proyecto es propiedad de Jose Manuel Toribio. 
Módulo de síntesis del Ciclo Formativo de Grado Superior de Desarrollo de Aplicaciones Multiplataforma y Videojuegos.

```
MIT License (Educativo)

Copyright (c) 2021 Jose Manuel Toribio
```

## 👥 Autores y Contribuidores

- **Jose Manuel Toribio** - Creador principal

## 📞 Soporte

Para reportar issues o sugerencias:
- 📧 Email: [contact@example.com](mailto:contact@example.com)
- 🐛 Issues: [GitHub Issues](https://github.com/jmtoribio/001-HelpVet/issues)
- 💬 Discussions: [GitHub Discussions](https://github.com/jmtoribio/001-HelpVet/discussions)

## 🎯 Roadmap

- [ ] Implementar cifrado de contraseñas
- [ ] Agregar notificaciones push en móvil
- [ ] Crear interfaz web adicional
- [ ] Soporte para múltiples idiomas
- [ ] Sistema de backup automático
- [ ] Generación de reportes PDF
- [ ] QR para mascotas (ver QR-APP.png)
- [ ] API REST completa

---

**Última actualización**: Noviembre 2024
**Estado del Proyecto**: Educativo / En Desarrollo 

