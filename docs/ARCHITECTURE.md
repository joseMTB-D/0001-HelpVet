# Documentación Técnica Detallada - HelpPet-Vet

**Arquitectura, Estructura y Diseño del Sistema**

---

## 📑 Tabla de Contenidos

1. [Visión General de la Arquitectura](#visión-general-de-la-arquitectura)
2. [Estructura del Proyecto](#estructura-del-proyecto)
3. [Arquitectura del Software](#arquitectura-del-software)
4. [Componentes Principales](#componentes-principales)
5. [Flujo de Datos](#flujo-de-datos)
6. [Patrones de Diseño](#patrones-de-diseño)
7. [Base de Datos](#base-de-datos)
8. [Seguridad](#seguridad)
9. [Performance](#performance)

---

## 🏗️ Visión General de la Arquitectura

HelpPet-Vet implementa una arquitectura de **tres capas distribuida** con los siguientes componentes:

```
┌────────────────────────────────────────────────────────────────────┐
│                        PRESENTACIÓN (UI)                           │
│  ┌───────────────────────────┬──────────────────────────────────┐  │
│  │   Desktop (VB.NET)        │   Mobile (Android/Java)          │  │
│  │  • Windows Forms          │  • Material Design Activities    │  │
│  │  • .NET Framework 4.7.2   │  • RecyclerView Components       │  │
│  │  • Direct DB Connection   │  • Volley HTTP Client           │  │
│  └───────────────┬───────────┴─────────────┬────────────────────┘  │
└────────────────┼──────────────────────────┼──────────────────────┘
                 │                          │
┌────────────────┼──────────────────────────┼──────────────────────┐
│                │   LÓGICA DE NEGOCIO     │                      │
│  ┌─────────────▼──────────┐  ┌──────────▼─────────────┐          │
│  │   VB.NET Classes       │  │   PHP APIs Backend      │          │
│  │  • Business Logic      │  │  • Request Handlers     │          │
│  │  • Data Validation     │  │  • Business Rules       │          │
│  │  • CRUD Operations     │  │  • Authentication       │          │
│  └──────────┬─────────────┘  └────────┬────────────────┘          │
└─────────────┼──────────────────────────┼──────────────────────────┘
              │                          │
┌─────────────┼──────────────────────────┼──────────────────────────┐
│             │    ACCESO A DATOS        │                          │
│  ┌──────────▼──────────────────────────▼────────────┐             │
│  │         MySQL Database (Clever Cloud)             │             │
│  │  • Schema relacional                              │             │
│  │  • Almacenamiento persistente                     │             │
│  │  • Transactions & Integrity                       │             │
│  └────────────────────────────────────────────────────┘             │
└──────────────────────────────────────────────────────────────────┘
```

### Características Arquitectónicas

| Característica | Descripción |
|---|---|
| **Modelo** | Arquitectura de 3 capas + Servicios |
| **Tipo de BD** | Relacional (MySQL) |
| **Conectividad** | Directa (Desktop) / HTTP+PHP (Mobile) |
| **Sincronización** | Manual por timestamp |
| **Transacciones** | A nivel BD |
| **Caché** | Temporal en memoria (DataTable) |

---

## 📁 Estructura del Proyecto

### Árbol Completo de Directorios

```
001-HelpVet/
│
├── 📋 README.md                          # Documentación principal
├── 🔒 SECURITY.md                        # Políticas de seguridad
│
├── 📁 .github/
│   ├── workflows/                        # GitHub Actions CI/CD
│   ├── ISSUE_TEMPLATE.md
│   └── PULL_REQUEST_TEMPLATE.md
│
├── 📁 docs/                              # Documentación técnica
│   ├── ARCHITECTURE.md                   # Este archivo
│   ├── DATABASE.md                       # Esquema detallado
│   ├── API.md                            # Especificación de endpoints
│   ├── DEPLOYMENT.md                     # Guía de despliegue
│   ├── DEVELOPMENT.md                    # Setup de desarrollo
│   └── TROUBLESHOOTING.md                # Solución de problemas
│
├── 📁 Aplicacio escriptori/              # Aplicación Desktop
│   │
│   ├── VisualBasic/
│   │   └── HelPPet-Vet/
│   │       ├── HelPPet-Vet.sln          # Solución Visual Studio
│   │       │
│   │       └── HelPPet-Vet/             # Proyecto principal
│   │           ├── 📄 App.config         # Configuración (.NET)
│   │           │
│   │           ├── 📄 Login.vb           # Pantalla de autenticación
│   │           │   • FormLogin principal
│   │           │   • Validación de credenciales
│   │           │   • Carga de admin.txt
│   │           │
│   │           ├── 📄 AdminSide.vb       # Panel administrativo
│   │           │   • Gestión de veterinarios
│   │           │   • Alta/Baja de usuarios
│   │           │   • Configuración general
│   │           │
│   │           ├── 📄 Veterinari.vb      # Panel veterinario
│   │           │   • Búsqueda de clientes
│   │           │   • Registro de mascotas
│   │           │   • Gestión de casos clínicos
│   │           │   • Historial de tratamientos
│   │           │
│   │           ├── 📄 DT.vb              # Datos y conexión
│   │           │   • Clase de conexión a MySQL
│   │           │   • String de conexión
│   │           │   • Manejo de excepciones
│   │           │
│   │           ├── My Project/           # Configuración del proyecto
│   │           │   ├── Application.vb
│   │           │   ├── Settings.vb
│   │           │   └── ...
│   │           │
│   │           ├── bin/                  # Archivos compilados
│   │           │   ├── Debug/
│   │           │   └── Release/
│   │           │
│   │           └── obj/                  # Archivos de compilación
│   │
│   ├── Installer Helppet-VET/           # Instalador ClickOnce
│   │   ├── HelPPet-Vet.application
│   │   ├── setup.exe
│   │   ├── autorun.inf
│   │   └── Application Files/
│   │       ├── HelPPet-Vet_1_0_0_12/
│   │       ├── HelPPet-Vet_1_0_0_11/
│   │       └── ...
│   │
│   └── 📄 helppet_database.sql          # Script SQL inicial
│       • CREATE TABLE statements
│       • Sample data
│       • Constraints & Indexes
│
├── 📁 App mobil/                         # Aplicación Móvil
│   │
│   ├── APP-1 - Final/                    # Proyecto Android principal
│   │   ├── 📄 settings.gradle            # Configuración Gradle
│   │   ├── 📄 build.gradle               # Dependencies globales
│   │   ├── 📄 gradle.properties          # Propiedades
│   │   ├── gradle/wrapper/               # Wrapper de Gradle
│   │   │   └── gradle-wrapper.jar
│   │   │
│   │   ├── 📁 app/                       # Módulo app principal
│   │   │   ├── 📄 build.gradle           # Dependencias del app
│   │   │   ├── 📄 proguard-rules.pro     # Reglas de ProGuard
│   │   │   ├── 📄 release/               # Build releases
│   │   │   │
│   │   │   └── 📁 src/
│   │   │       │
│   │   │       ├── main/
│   │   │       │   ├── 📄 AndroidManifest.xml
│   │   │       │   │   • Declaración de activities
│   │   │       │   │   • Permisos necesarios
│   │   │       │   │   • Versiones soportadas
│   │   │       │   │
│   │   │       │   ├── java/com/example/helppet/
│   │   │       │   │   ├── MainActivity.java
│   │   │       │   │   ├── LoginActivity.java
│   │   │       │   │   ├── PetListActivity.java
│   │   │       │   │   ├── CaseDetailsActivity.java
│   │   │       │   │   ├── UserProfileActivity.java
│   │   │       │   │   ├── adapters/
│   │   │       │   │   │   ├── PetAdapter.java
│   │   │       │   │   │   └── CaseAdapter.java
│   │   │       │   │   ├── models/
│   │   │       │   │   │   ├── Pet.java
│   │   │       │   │   │   ├── Case.java
│   │   │       │   │   │   ├── Owner.java
│   │   │       │   │   │   └── Vet.java
│   │   │       │   │   ├── network/
│   │   │       │   │   │   ├── ApiClient.java
│   │   │       │   │   │   └── VolleyRequest.java
│   │   │       │   │   ├── utils/
│   │   │       │   │   │   ├── Constants.java
│   │   │       │   │   │   ├── SharedPrefManager.java
│   │   │       │   │   │   └── DateUtils.java
│   │   │       │   │   └── fragments/
│   │   │       │   │       ├── PetListFragment.java
│   │   │       │   │       ├── HistoryFragment.java
│   │   │       │   │       └── SettingsFragment.java
│   │   │       │   │
│   │   │       │   └── res/
│   │   │       │       ├── layout/
│   │   │       │       │   ├── activity_main.xml
│   │   │       │       │   ├── activity_login.xml
│   │   │       │       │   ├── activity_pet_list.xml
│   │   │       │       │   └── ...
│   │   │       │       ├── values/
│   │   │       │       │   ├── strings.xml
│   │   │       │       │   ├── colors.xml
│   │   │       │       │   └── dimens.xml
│   │   │       │       ├── drawable/
│   │   │       │       ├── mipmap/
│   │   │       │       │   └── ic_launcher.png
│   │   │       │       └── menu/
│   │   │       │
│   │   │       ├── test/                 # Tests unitarios
│   │   │       └── androidTest/          # Tests instrumentados
│   │   │
│   │   └── .idea/                        # Configuración Android Studio
│   │
│   ├── Volley/                           # Backend PHP
│   │   ├── login.php                     # Autenticación de propietarios
│   │   ├── registro.php                  # Registro de nuevos usuarios
│   │   ├── llenar_spinner.php            # Listar mascotas por tipo
│   │   ├── llenar_spinner_casos.php      # Listar casos de veterinario
│   │   ├── llenar_info.php               # Información de mascota
│   │   ├── llenar_trata.php              # Detalles de tratamiento
│   │   └── config.php                    # Configuración BD
│   │
│   ├── 📄 helppet.apk                    # APK compilado
│   └── 📄 QR-APP.png                     # QR code para descargar app
│
└── .git/                                 # Repositorio Git
```

### Responsabilidad de Módulos Principales

#### Aplicación de Escritorio (Desktop)

| Módulo | Responsabilidad | Clase Principal |
|--------|-----------------|-----------------|
| **Autenticación** | Validar credenciales de usuario | `Login.vb` |
| **Administración** | Gestión de veterinarios y configuración | `AdminSide.vb` |
| **Veterinario** | Panel principal para veterinarios | `Veterinari.vb` |
| **Datos** | Conexión y operaciones con BD | `DT.vb` |

#### Aplicación Móvil (Android)

| Módulo | Responsabilidad | Clase Principal |
|--------|-----------------|-----------------|
| **Activities** | Pantallas de la aplicación | `*Activity.java` |
| **Adapters** | Vinculación de datos a UI | `*Adapter.java` |
| **Models** | Entidades de datos | `Pet.java`, `Case.java` |
| **Network** | Comunicación HTTP con servidor | `ApiClient.java` |
| **Utils** | Funciones auxiliares | `Constants.java`, `SharedPrefManager.java` |
| **Fragments** | Componentes reutilizables | `*Fragment.java` |

---

## 🔄 Arquitectura del Software

### Patrón de Arquitectura

HelpPet-Vet implementa una **arquitectura en 3 capas + servicios**:

```
┌─────────────────────────────────────────────────┐
│          CAPA DE PRESENTACIÓN                   │
│  (UI/Views - Forms, Fragments, Activities)      │
└──────────────────┬──────────────────────────────┘
                   │
                   │ Eventos de Usuario
                   │
                   ▼
┌─────────────────────────────────────────────────┐
│       CAPA DE LÓGICA DE NEGOCIO                 │
│  (Controllers, Managers, Services)              │
│  • Validación de datos                          │
│  • Reglas de negocio                            │
│  • Orchestration                                │
└──────────────────┬──────────────────────────────┘
                   │
                   │ Operaciones CRUD
                   │ Queries
                   │
                   ▼
┌─────────────────────────────────────────────────┐
│      CAPA DE ACCESO A DATOS                     │
│  (DAOs, Repositories, DB Adapters)              │
│  • Operaciones con BD                           │
│  • Transformación de datos                      │
│  • Caching (opcional)                           │
└──────────────────┬──────────────────────────────┘
                   │
                   ▼
┌─────────────────────────────────────────────────┐
│    PERSISTENCIA / FUENTES DE DATOS              │
│  • MySQL Database                               │
│  • Local SharedPreferences (Mobile)             │
│  • Files (admin.txt)                            │
└─────────────────────────────────────────────────┘
```

### Patrones Implementados

#### 1. **Data Access Object (DAO)**
Utilizado para abstraer operaciones de BD:
```vb
' En VB.NET
Public Class PetDAO
    Private conn As MySqlConnection
    
    Public Function GetPetById(id As String) As Pet
        ' Implementación
    End Function
    
    Public Sub SavePet(pet As Pet)
        ' Implementación
    End Sub
End Class
```

#### 2. **Active Record (Débil)**
Los formularios acceden directamente a la BD:
```vb
' Login.vb accede directo a MySqlDataAdapter
Dim MySQLDA As New MySqlDataAdapter(queryString, connection)
```

#### 3. **Singleton (Conexión BD)**
```vb
' DT.vb - única instancia de conexión
Public Class DT
    Private Shared instance As DT
    Public Shared Function getInstance() As DT
        If instance Is Nothing Then
            instance = New DT()
        End If
        Return instance
    End Function
End Class
```

#### 4. **Repository Pattern (Mobile)**
Gestión de datos en Android:
```java
public class PetRepository {
    private ApiClient apiClient;
    private SharedPrefManager prefs;
    
    public void getPets(Callback callback) {
        apiClient.fetchPets(callback);
    }
}
```

#### 5. **Adapter Pattern (Mobile)**
Vinculación de datos a vistas:
```java
public class PetAdapter extends RecyclerView.Adapter<PetViewHolder> {
    // Implementación de adaptación datos → views
}
```

---

## ⚙️ Componentes Principales

### Componentes del Desktop

#### **1. Login.vb**
```
Responsabilidad: Autenticación de usuarios

Flujo:
  1. Cargar admin.txt si existe
  2. Mostrar formulario de login
  3. Validar credenciales en BD
  4. Abrir AdminSide o Veterinari según rol

Métodos Principales:
  • Button1_Click() - Procesar login
  • Label1_Click() - Eventos UI
```

#### **2. AdminSide.vb**
```
Responsabilidad: Panel administrativo

Funcionalidades:
  • Gestión de veterinarios (CRUD)
  • Validación de datos completos
  • Almacenamiento de credenciales admin

Métodos Principales:
  • Button1_Click() - Agregar veterinario
  • ValidarCampos() - Validación
```

#### **3. Veterinari.vb**
```
Responsabilidad: Panel del veterinario

Funcionalidades:
  • Búsqueda de propietarios
  • Registro de mascotas
  • Gestión de casos clínicos
  • Seguimiento de tratamientos

Métodos Principales:
  • registrar_mascota_Click() - Registrar mascota
  • Button4_Click() - Buscar cliente
  • Validación de microchip
```

#### **4. DT.vb**
```
Responsabilidad: Gestión de conexión a BD

Método Principal:
  • conexion_bd() - Establecer conexión MySQL

Configuración:
  server=br6yuhxjtf6d9t43hrii-mysql.services.clever-cloud.com
  database=br6yuhxjtf6d9t43hrii
```

### Componentes del Mobile (Android)

#### **Activities Principales**

| Activity | Propósito |
|----------|-----------|
| `MainActivity` | Punto de entrada, navegación principal |
| `LoginActivity` | Autenticación de propietarios |
| `PetListActivity` | Listar mascotas del propietario |
| `CaseDetailsActivity` | Ver detalles de un caso clínico |
| `UserProfileActivity` | Perfil del propietario |

#### **Adapters**

```java
PetAdapter
├─ Vincula lista de mascotas a RecyclerView
├─ Gestiona clicks en elementos
└─ Actualiza UI con cambios

CaseAdapter
├─ Vincula casos clínicos a ListView
└─ Muestra información de tratamientos
```

#### **Network Layer**

```java
ApiClient (Volley)
├─ Configuración base URL
├─ Manejo de requests HTTP
├─ Gestión de respuestas JSON
└─ Manejo de errores
```

---

## 🔀 Flujo de Datos

### Flujo 1: Autenticación de Veterinario (Desktop)

```
┌─────────────────────────────────────────────────────────────┐
│ Usuario en Login.vb ingresa credenciales                    │
└────────────────────────┬────────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────────┐
│ Validar si existe admin.txt (Panel Administrativo)          │
└────────────────────────┬────────────────────────────────────┘
                         │
         ┌───────────────┼───────────────┐
         │               │               │
    EXISTS          NO_EXISTS      USUARIO
   [ADMIN]            │
         │             │
         ▼             ▼
     AdminSide    MySql Query
      Side       ("SELECT * FROM veterinari")
         │             │
         │             ▼
         │        Credenciales Válidas?
         │             │
         │             ├─ SÍ → Abrir Veterinari.vb
         │             │
         │             └─ NO → Mostrar MsgBox error
         │
         └─ Cargar credenciales en TextBox
```

### Flujo 2: Registro de Mascota (Desktop)

```
┌───────────────────────────────────────────┐
│ Veterinario llena formulario de mascota   │
└────────────────┬────────────────────────────┘
                 │
                 ▼
┌───────────────────────────────────────────┐
│ Validar todos los campos llenos           │
│ • Validar longitud microchip = 15 dígitos │
│ • Validar selección de especies           │
└────────────────┬────────────────────────────┘
                 │
         ┌───────┴────────┐
         │                │
        VÁLIDO         INVÁLIDO
         │                │
         ▼                ▼
    INSERT INTO      MsgBox("Error")
      mascota       Return
         │
         ▼
    INSERT INTO
      casos
    (vinculación)
         │
         ▼
    MsgBox("Mascota registrada")
```

### Flujo 3: Consulta de Historial Clínico (Mobile)

```
┌───────────────────────────────┐
│ Usuario abre PetListActivity  │
└────────────┬──────────────────┘
             │
             ▼
┌───────────────────────────────┐
│ Recuperar DNI de SharedPref   │
└────────────┬──────────────────┘
             │
             ▼
┌───────────────────────────────┐
│ Volley Request a               │
│ /llenar_spinner.php?dni=xxx   │
└────────────┬──────────────────┘
             │
             ▼
┌───────────────────────────────┐
│ BD: SELECT * FROM mascota      │
│      WHERE Propietari = xxx   │
└────────────┬──────────────────┘
             │
             ▼
┌───────────────────────────────┐
│ PHP parsea JSON               │
│ Retorna array de mascotas     │
└────────────┬──────────────────┘
             │
             ▼
┌───────────────────────────────┐
│ Deserializar en Pet objects   │
│ Actualizar PetAdapter         │
│ RecyclerView se redibuja      │
└───────────────────────────────┘
```

---

## 🎨 Patrones de Diseño

### 1. **MVC (Modelo-Vista-Controlador)**

**Desktop:**
- **Modelo**: Tablas BD (propietari, mascota, casos)
- **Vista**: Formularios (.Designer.vb)
- **Controlador**: Eventos en .vb (Login_Load, Button_Click)

**Mobile:**
- **Modelo**: Clases en `/models/` (Pet.java, Case.java)
- **Vista**: Layouts XML en `/res/layout/`
- **Controlador**: Activities y Fragments

### 2. **Singleton**
```vb
' DT.vb - Una única conexión compartida
Private Shared instance As DT

Public Shared Function getInstance() As DT
    If instance Is Nothing Then
        instance = New DT()
    End If
    Return instance
End Function
```

### 3. **Factory**
```java
// ApiClient.java
public static ApiClient getInstance() {
    if (instance == null) {
        instance = new ApiClient();
    }
    return instance;
}
```

### 4. **Observer**
```java
// LiveData en AndroidX
private MutableLiveData<List<Pet>> petList = new MutableLiveData<>();

petList.observe(this, pets -> {
    adapter.setPets(pets);
});
```

---

## 💾 Base de Datos

### Esquema General

```
MySQL Database: br6yuhxjtf6d9t43hrii (Clever Cloud)

┌─────────────────┐         ┌──────────────────┐
│    propietari   │◄────┐   │     mascota      │
├─────────────────┤     │   ├──────────────────┤
│ PK: DNI         │     ├───┤ FK: Propietari   │
│ nom             │     │   │ codi (PK)        │
│ email           │     │   │ nom              │
│ pass            │     │   │ Especie          │
│ Adreça          │     │   │ raza             │
│ CP              │     │   │ edat             │
│ t1, t2          │     │   │ peso             │
└─────────────────┘     │   └────────┬─────────┘
                        │            │
                        │            │
                ┌───────┼────────────┼─────────┐
                │       │            │         │
                │   ┌───┴────────────▼──────┐ │
                │   │      casos           │ │
                │   ├──────────────────────┤ │
                │   │ PK: codi_cas         │ │
                │   │ FK: codi_mascota◄────┘ │
                │   │ FK: codi_veterinari◄──┐
                │   │ Data_Registre          │
                │   │ Data_Revisio           │
                │   │ tractament             │
                │   │ medicaments            │
                │   │ enfermetats            │
                │   │ Observacio             │
                │   │ pes                    │
                │   └────────────────────────┘
                │                            │
                └──────────────┬──────────────┘
                               │
                    ┌──────────▼──────────┐
                    │   veterinari        │
                    ├─────────────────────┤
                    │ PK: codi            │
                    │ nom                 │
                    │ clinica             │
                    │ Usuari              │
                    │ password            │
                    └─────────────────────┘
```

### Relaciones

| Relación | Tipo | Descripción |
|----------|------|-------------|
| propietari → mascota | 1:N | Un propietario puede tener varias mascotas |
| mascota → casos | 1:N | Una mascota puede tener varios casos |
| veterinari → casos | 1:N | Un veterinario atiende varios casos |

### Índices Principales
```sql
-- propietari
PRIMARY KEY (DNI)

-- mascota
PRIMARY KEY (codi)
FOREIGN KEY (Propietari) REFERENCES propietari(DNI)

-- casos
PRIMARY KEY (codi_cas)
FOREIGN KEY (codi_mascota) REFERENCES mascota(codi)
FOREIGN KEY (codi_veterinari) REFERENCES veterinari(codi)

-- veterinari
PRIMARY KEY (codi)
```

---

## 🔐 Seguridad

### Estado Actual (⚠️ Educativo)

**Vulnerabilidades Identificadas:**

1. **Contraseñas en texto plano**
   ```vb
   ' INSEGURO - Tal como está ahora
   INSERT INTO veterinari VALUES (codi, nom, clinica, usuario, passwordTextoPlano)
   ```

2. **SQL Injection potencial**
   ```vb
   ' VULNERABLE
   Query = "SELECT * FROM veterinari WHERE usuario = '" & usuario.Text & "'"
   ```

3. **Conexión sin SSL**
   ```vb
   ' Sin encriptación en tránsito
   connection_string = "server=...;user=...;password=..."
   ```

4. **Almacenamiento de credenciales en archivo**
   ```vb
   ' admin.txt - archivo sin protección
   fileReader = My.Computer.FileSystem.ReadAllText(path)
   ```

### Recomendaciones de Mejora

```vb
' 1. USAR PARAMETERIZED QUERIES
Dim cmd As New MySqlCommand("SELECT * FROM veterinari WHERE usuario = @usuario", conn)
cmd.Parameters.AddWithValue("@usuario", usuario.Text)

' 2. HASHEAR CONTRASEÑAS
Using sha256 = System.Security.Cryptography.SHA256.Create()
    Dim hashedPassword = Convert.ToBase64String(sha256.ComputeHash(Encoding.UTF8.GetBytes(password)))
End Using

' 3. USAR HTTPS/SSL
connection_string = "server=...;user=...;password=...;SslMode=Required"

' 4. CIFRAR ARCHIVO admin.txt
' Usar Data Protection API (DPAPI)
```

---

## ⚡ Performance

### Optimizaciones Implementadas

1. **Lazy Loading de Datos**
   - DataGridView carga datos bajo demanda
   - No se cargan todos los registros al abrir

2. **Caché en Memoria**
   - DataTable se mantiene en RAM
   - Consultas repetidas no van a BD

3. **Índices en BD**
   - PRIMARY KEY en tablas principales
   - FOREIGN KEY para integridad referencial

### Mejoras Recomendadas

1. **Paginación**
   ```sql
   SELECT * FROM casos 
   LIMIT 20 OFFSET 40  -- Página 2, 20 elementos por página
   ```

2. **Connection Pooling**
   ```vb
   ' Usar pool de conexiones en lugar de crear nuevas cada vez
   connection_string = "...;Min Pool Size=5;Max Pool Size=20"
   ```

3. **Async/Await en Mobile**
   ```java
   // En lugar de bloquear UI
   Volley.getInstance(context).executeAsync(request);
   ```

4. **Queries Optimizadas**
   ```sql
   -- Usar JOINs en lugar de múltiples queries
   SELECT m.*, c.*, v.nom 
   FROM mascota m
   JOIN casos c ON m.codi = c.codi_mascota
   JOIN veterinari v ON c.codi_veterinari = v.codi
   WHERE m.Propietari = ?
   ```

---

## 📊 Diagrama de Secuencia: Caso de Uso Completo

**Caso: Veterinario registra nueva mascota**

```
Veterinario          Veterinari.vb       DT.vb           MySQL
    │                     │                 │               │
    │ Llena formulario    │                 │               │
    ├────────────────────►│                 │               │
    │                     │                 │               │
    │ Clica "Registrar"   │                 │               │
    ├────────────────────►│                 │               │
    │                     │ Valida datos    │               │
    │                     │ (campos llenos) │               │
    │                     │                 │               │
    │                     │ Genera código   │               │
    │                     │                 │               │
    │                     │ INSERT INTO     │               │
    │                     │   mascota       │               │
    │                     ├────────────────►│               │
    │                     │                 │ INSERT        │
    │                     │                 ├──────────────►│
    │                     │                 │               │
    │                     │                 │ OK            │
    │                     │                 │◄──────────────┤
    │                     │                 │               │
    │                     │ INSERT INTO     │               │
    │                     │   casos         │               │
    │                     ├────────────────►│               │
    │                     │                 │ INSERT        │
    │                     │                 ├──────────────►│
    │                     │                 │               │
    │                     │                 │ OK            │
    │                     │                 │◄──────────────┤
    │                     │◄────────────────┤               │
    │◄────────────────────┤                 │               │
    │ MsgBox OK           │                 │               │
    │                     │                 │               │
```

---

## 📚 Referencias y Recursos

### Documentación Relacionada
- [DATABASE.md](./DATABASE.md) - Esquema SQL completo
- [API.md](./API.md) - Endpoints PHP detallados
- [DEPLOYMENT.md](./DEPLOYMENT.md) - Guía de despliegue
- [DEVELOPMENT.md](./DEVELOPMENT.md) - Setup para desarrolladores

### Tecnologías
- [MySQL Documentation](https://dev.mysql.com/doc/)
- [.NET Framework 4.7.2](https://docs.microsoft.com/en-us/dotnet/framework/)
- [Android Developer Guide](https://developer.android.com/guide)
- [Volley HTTP Library](https://developer.android.com/training/volley)

### Patterns & Best Practices
- [Design Patterns in .NET](https://refactoring.guru/design-patterns/csharp)
- [Android Architecture Components](https://developer.android.com/topic/architecture)
- [Clean Code Principles](https://www.oreilly.com/library/view/clean-code-a/9780136083238/)

---

**Documento actualizado**: Noviembre 2024  
**Versión**: 1.0  
**Autor**: Equipo de Desarrollo HelpPet-Vet

