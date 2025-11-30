# Contributing Guide - HelpPet-Vet

**Guía Completa para Contribuidores del Proyecto**

---

## 📑 Tabla de Contenidos

1. [Código de Conducta](#código-de-conducta)
2. [Cómo Contribuir](#cómo-contribuir)
3. [Proceso de Setup](#proceso-de-setup)
4. [Estándares de Código](#estándares-de-código)
5. [Proceso de Pull Request](#proceso-de-pull-request)
6. [Testing](#testing)
7. [Documentación](#documentación)
8. [Reportar Issues](#reportar-issues)

---

## 📋 Código de Conducta

### Nuestro Compromiso

Estamos comprometidos con proporcionar un ambiente abierto y acogedor para todos. Como contribuidores y mantenedores, nos comprometemos a hacer la participación en nuestro proyecto y nuestra comunidad una experiencia libre de acoso.

### Expectativas

- **Respetuoso**: Tratamos a todos con respeto
- **Constructivo**: Proporcionamos feedback útil y constructivo
- **Inclusivo**: Bienvenidos todos los orígenes y experiencias
- **Profesional**: Mantenemos un tono profesional

### Aplicación

Las violaciones del código de conducta pueden reportarse contactando al equipo de mantenimiento.

---

## 🚀 Cómo Contribuir

### Tipos de Contribuciones Aceptadas

1. **Reportar Bugs**: Problemas en el código actual
2. **Sugerir Mejoras**: Nuevas funcionalidades o optimizaciones
3. **Escribir Código**: Implementar nuevas features o fixes
4. **Mejorar Documentación**: Claridad y completitud
5. **Escribir Tests**: Mejorar cobertura de pruebas

### Proceso General

```
1. Fork el repositorio
   ↓
2. Crear rama local (feature/bugfix)
   ↓
3. Hacer cambios y commits
   ↓
4. Push a tu fork
   ↓
5. Crear Pull Request
   ↓
6. Revisión y feedback
   ↓
7. Merge a main
```

---

## 🛠️ Proceso de Setup

### Requisitos Previos

#### Sistema
- Git instalado
- Acceso a internet

#### Para Desktop (VB.NET)
```
✓ Visual Studio 2019 o superior
✓ .NET Framework 4.7.2
✓ NuGet Package Manager
✓ Acceso a servidor MySQL
```

#### Para Mobile (Android)
```
✓ Android Studio 4.1 o superior
✓ JDK 1.8+
✓ Gradle 6.7+
✓ Android SDK 30
✓ Emulador Android o dispositivo físico
```

#### Para Backend (PHP)
```
✓ PHP 7.2 o superior
✓ MySQL Client
✓ Un servidor web (Apache/Nginx)
✓ Composer (opcional pero recomendado)
```

### Instalación Local

#### 1. Clonar el Repositorio

```bash
git clone https://github.com/jmtoribio/001-HelpVet.git
cd 001-HelpVet
```

#### 2. Crear Rama de Trabajo

```bash
# Para nuevas features
git checkout -b feature/nombre-descriptivo

# Para bugs
git checkout -b bugfix/nombre-descriptivo

# Para documentación
git checkout -b docs/nombre-descriptivo
```

#### 3. Setup de Desktop

```bash
cd "Aplicacio escriptori/VisualBasic/HelPPet-Vet"

# Abrir en Visual Studio
devenv HelPPet-Vet.sln

# O desde línea de comandos
msbuild HelPPet-Vet.sln /p:Configuration=Debug
```

#### 4. Setup de Mobile

```bash
cd "App mobil/APP-1 - Final"

# Abrir en Android Studio
open -a "Android Studio" .

# O compilar desde CLI
./gradlew build
./gradlew assembleDebug
```

#### 5. Setup de Backend

```bash
cd "App mobil/Volley"

# Copiar archivos a servidor web
cp *.php /var/www/html/volley/

# Verificar conexión a BD en config.php
# Editar variables de conexión si es necesario
```

---

## 📝 Estándares de Código

### Visual Basic .NET

#### Convenciones de Nombres

```vb
' Clases: PascalCase
Public Class LoginForm
End Class

' Métodos públicos: PascalCase
Public Sub ValidateInput()
End Sub

' Métodos privados: camelCase
Private Sub validateEmail()
End Sub

' Variables locales: camelCase
Dim usuarioActual As String = ""

' Constantes: UPPER_SNAKE_CASE
Private Const MAX_INTENTOS_LOGIN = 3

' Propiedades: PascalCase
Public Property UsuarioActual As String
    Get
        Return _usuarioActual
    End Get
    Set(value As String)
        _usuarioActual = value
    End Set
End Property
```

#### Estructura de Clases

```vb
' Encabezado con descripción
''' <summary>
''' Gestiona la autenticación de usuarios en el sistema
''' </summary>
Public Class AutenticacionManager
    ' 1. Constantes
    Private Const MAX_REINTENTOS As Integer = 3
    
    ' 2. Variables miembro
    Private conexion As MySqlConnection
    Private logger As ILogger
    
    ' 3. Constructor
    Public Sub New(conexion As MySqlConnection)
        Me.conexion = conexion
        Me.logger = New Logger()
    End Sub
    
    ' 4. Propiedades públicas
    Public Property UsuarioActual As Usuario
    
    ' 5. Métodos públicos
    Public Function Autenticar(usuario As String, password As String) As Boolean
        ' Implementación
    End Function
    
    ' 6. Métodos privados
    Private Sub LogIntentoFallido(usuario As String)
        ' Implementación
    End Sub
End Class
```

#### Estilo de Código

```vb
' ✓ BUENO: Código legible y comentado
Private Sub RegistrarMascota()
    Try
        If ValidarCamposObligatorios() Then
            Dim mascota As New Mascota()
            mascota.Nombre = txtNombre.Text
            mascota.Especie = cmbEspecie.SelectedItem.ToString()
            
            InsertarEnBD(mascota)
            MostrarMensaje("Mascota registrada exitosamente")
        End If
    Catch ex As Exception
        LogearError("RegistrarMascota", ex)
        MostrarError("Error: " & ex.Message)
    End Try
End Sub

' ✗ MALO: Código confuso y sin comentarios
Private Sub Reg()
    If txt1.Text <> "" And cmbE.SelectedIndex > -1 Then
        m.nm = txt1.Text
        m.esp = cmbE.SelectedItem
        InsDB(m)
        MsgBox("OK")
    End If
End Sub
```

### Java/Android

#### Convenciones de Nombres

```java
// Clases: PascalCase
public class LoginActivity extends AppCompatActivity {
}

// Métodos: camelCase
public void validateInput() {
}

// Variables: camelCase
private String currentUser = "";

// Constantes: UPPER_SNAKE_CASE
private static final String DEFAULT_API_BASE = "http://api.example.com";
private static final int MAX_LOGIN_ATTEMPTS = 3;

// Variables miembro privadas: mPrefix
private String mCurrentUser;
private List<Pet> mPets;

// Recursos XML: snake_case
// activity_login.xml
// fragment_pet_list.xml
// btn_submit (para botón)
```

#### Estructura de Activities

```java
public class PetListActivity extends AppCompatActivity {
    // 1. Constantes
    private static final String TAG = "PetListActivity";
    private static final int REQUEST_CODE_ADD_PET = 100;
    
    // 2. Variables miembro
    private RecyclerView mRecyclerView;
    private PetAdapter mAdapter;
    private List<Pet> mPets;
    private ProgressBar mProgressBar;
    
    // 3. Callbacks
    private PetRepository.OnPetsLoadedListener mPetsListener;
    
    // 4. onCreate
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_pet_list);
        
        initializeViews();
        setupListeners();
        loadPets();
    }
    
    // 5. Métodos lifecycle
    @Override
    protected void onResume() {
        super.onResume();
        refreshPets();
    }
    
    // 6. Métodos privados
    private void initializeViews() {
        mRecyclerView = findViewById(R.id.pet_list);
        mProgressBar = findViewById(R.id.progress_bar);
    }
    
    // 7. Métodos helpers
    private void loadPets() {
        showLoading();
        PetRepository.getPets(mPetsListener);
    }
    
    private void showLoading() {
        mProgressBar.setVisibility(View.VISIBLE);
    }
}
```

### PHP

#### Convenciones

```php
<?php
// Clases: PascalCase
class PetRepository {
    
    // Constantes: UPPER_SNAKE_CASE
    const MAX_DB_ATTEMPTS = 3;
    const DEFAULT_LIMIT = 50;
    
    // Propiedades privadas: $variable (camelCase)
    private $database;
    private $logger;
    
    // Constructor
    public function __construct(Database $db) {
        $this->database = $db;
    }
    
    // Métodos públicos: camelCase
    public function getPetsByOwner($dni) {
        try {
            $query = "SELECT * FROM mascota WHERE Propietari = ?";
            return $this->database->query($query, [$dni]);
        } catch (Exception $e) {
            $this->logger->error("getPetsByOwner failed", $e);
            return null;
        }
    }
    
    // Métodos privados: camelCase
    private function validateDNI($dni) {
        return preg_match('/^[0-9]{8}[A-Z]$/', $dni);
    }
}
?>
```

---

## 📋 Proceso de Pull Request

### Preparar tu PR

#### 1. Actualizar código

```bash
# Asegurarse de estar en la rama correcta
git branch

# Actualizar con cambios más recientes
git fetch origin
git rebase origin/main
```

#### 2. Hacer commits significativos

```bash
# Commits bien descritos
git commit -m "feat: agregar validación de microchip en formulario mascota"
git commit -m "fix: corregir bug en cálculo de peso"
git commit -m "docs: actualizar instrucciones de instalación"
git commit -m "style: aplicar format a código de autenticación"
git commit -m "refactor: extraer lógica de validación a clase separada"
```

#### 3. Push a tu fork

```bash
git push origin feature/nombre-descriptivo
```

### Crear el Pull Request

#### En GitHub

1. Ve a tu fork del repositorio
2. Click en "New Pull Request"
3. Selecciona:
   - **Base**: `main` (repositorio oficial)
   - **Compare**: tu rama de feature

#### Template de PR

```markdown
## Descripción
Breve descripción de los cambios realizados.

## Tipo de Cambio
- [x] Bug fix (cambio que arregla un problema)
- [ ] Nueva feature (cambio que agrega funcionalidad)
- [ ] Breaking change (cambio que causa comportamiento diferente)

## ¿Cómo ha sido probado?
Describe los pasos para probar tus cambios.

## Checklist
- [ ] Mi código sigue el estilo del proyecto
- [ ] He ejecutado tests locales
- [ ] He actualizado la documentación
- [ ] No hay warnings de compilación
- [ ] Mi rama está actualizada con main

## Issues Relacionados
Fixes #123
Closes #456
```

### Revisión y Feedback

- **Tiempo de revisión**: 24-48 horas
- **Mínimo 1 aprobación requerida**
- **Resolver conflictos**: Rebase antes de merge
- **Tests pasando**: Todos los CI/CD checks deben pasar

---

## ✅ Testing

### Desktop (VB.NET)

#### Pruebas Manuales

```vb
' 1. Probar pantalla de login
' ✓ Credenciales válidas → Acceso
' ✓ Credenciales inválidas → Rechazo
' ✓ Campos vacíos → Validación

' 2. Probar registro de mascota
' ✓ Todos los campos llenos → Éxito
' ✓ Campos obligatorios vacíos → Error
' ✓ Microchip inválido → Error

' 3. Probar gestión de casos
' ✓ Crear caso → Registro en BD
' ✓ Actualizar caso → Cambios guardados
' ✓ Ver historial → Datos correctos
```

#### Compilación Sin Errores

```bash
# En Visual Studio
Build → Build Solution (Ctrl+Shift+B)

# O desde CLI
msbuild HelPPet-Vet.sln /p:Configuration=Debug /p:Platform="Any CPU"
```

### Mobile (Android)

#### Tests Unitarios

```bash
# Ejecutar tests
./gradlew test

# Con reporte
./gradlew test --info
```

#### Tests Instrumentados

```bash
# En dispositivo o emulador
./gradlew connectedAndroidTest

# Específico
./gradlew connectedAndroidTest -PtestBuildType=debug
```

#### Pruebas Manuales

```
1. Instalar APK en emulador/dispositivo
   adb install app/debug/app-debug.apk

2. Probar flujo de login
   - Ingreso de credenciales
   - Validación
   - Almacenamiento de sesión

3. Probar navegación
   - Lista de mascotas
   - Detalles de mascota
   - Historial clínico

4. Probar red
   - Conectividad con servidor
   - Manejo de errores
   - Sincronización de datos

5. Probar UI
   - Orientación pantalla
   - Tamaños diferentes
   - Idiomas
```

### Backend (PHP)

#### Pruebas de API

```bash
# Login
curl -X POST http://localhost/volley/login.php \
  -d 'email=usuario@example.com&password=password'

# Obtener mascotas
curl "http://localhost/volley/llenar_spinner.php?DNI=09433726T"

# Con autenticación
curl -H "Authorization: Bearer token123" \
  "http://localhost/volley/llenar_info.php"
```

#### Validaciones

- [ ] Conexión a BD funciona
- [ ] Queries se ejecutan correctamente
- [ ] Respuestas JSON son válidas
- [ ] Manejo de errores funciona
- [ ] Validación de inputs activa

---

## 📚 Documentación

### Qué Documentar

1. **Código Complejo**: Explicar la lógica
2. **APIs Públicas**: Parámetros y retorno
3. **Cambios Importantes**: Actualizar README
4. **Nuevas Features**: Guía de uso

### Formato de Comentarios

#### VB.NET

```vb
' Comentario de una línea

''' <summary>
''' Descripción breve del método
''' </summary>
''' <param name="usuario">DNI del usuario</param>
''' <returns>True si es válido</returns>
Public Function ValidarUsuario(usuario As String) As Boolean
End Function
```

#### Java

```java
// Comentario de una línea

/**
 * Descripción breve del método
 * 
 * @param dni DNI del usuario
 * @return true si el usuario existe
 * @throws SQLException si hay error en BD
 */
public boolean validateUser(String dni) throws SQLException {
}
```

#### PHP

```php
// Comentario de una línea

/**
 * Obtiene todas las mascotas de un propietario
 * 
 * @param string $dni DNI del propietario
 * @return array[] Array de mascotas o null si error
 * @throws PDOException si error en BD
 */
public function getPetsByOwner($dni) {
}
```

---

## 🐛 Reportar Issues

### Template de Bug Report

```markdown
## Descripción del Bug
[Descripción clara y concisa]

## Pasos para Reproducir
1. Paso uno
2. Paso dos
3. Paso tres

## Comportamiento Esperado
[Qué debería ocurrir]

## Comportamiento Actual
[Qué está ocurriendo realmente]

## Capturas de Pantalla
[Si aplica]

## Información del Sistema
- OS: Windows 10 / macOS / Android 11
- Versión App: 1.0.0
- Navegador (si aplica): Chrome/Firefox
- Base Datos: MySQL 8.0.22

## Logs o Errores
[Copiar texto exacto de errores o logs]

## Contexto Adicional
[Cualquier información que ayude a reproducir]
```

### Template de Feature Request

```markdown
## Descripción de la Feature
[Descripción clara de lo que deseas]

## Caso de Uso
[Por qué necesitas esta feature]

## Solución Propuesta
[Tu idea de cómo implementarlo]

## Alternativas Consideradas
[Otras soluciones posibles]

## Contexto Adicional
[Información relevante]
```

---

## 🎓 Recursos de Aprendizaje

### Tutoriales Recomendados

**Visual Basic .NET**
- [Microsoft VB.NET Docs](https://docs.microsoft.com/en-us/dotnet/visual-basic/)
- [MSDN Windows Forms](https://docs.microsoft.com/en-us/dotnet/desktop/winforms/)

**Android/Java**
- [Android Developers](https://developer.android.com)
- [Java Documentation](https://docs.oracle.com/javase/)
- [Android Architecture Components](https://developer.android.com/topic/architecture)

**PHP/MySQL**
- [PHP Official Docs](https://www.php.net/docs.php)
- [MySQL 8.0 Reference](https://dev.mysql.com/doc/refman/8.0/en/)

**Git & GitHub**
- [Git Tutorial](https://git-scm.com/doc)
- [GitHub Flow](https://guides.github.com/introduction/flow/)

---

## 🏆 Tips para Contribuidores

### Do's ✓

- ✓ Comunica antes de cambios grandes
- ✓ Haz commits pequeños y frecuentes
- ✓ Escribe mensajes de commit claros
- ✓ Prueba localmente antes de hacer push
- ✓ Actualiza la documentación
- ✓ Respeta el estilo de código existente
- ✓ Sé respetuoso en comentarios
- ✓ Pregunta si tienes dudas

### Don'ts ✗

- ✗ No hagas commits a main directamente
- ✗ No ignores warnings de compilación
- ✗ No olvides actualizar CHANGELOG
- ✗ No hagas PRs muy grandes
- ✗ No borres código sin comentar
- ✗ No ignores feedback de revisores
- ✗ No hagas commits con mensajes genéricos
- ✗ No subas archivos sensibles (passwords, keys)

---

## 📞 Preguntas o Ayuda

- 📧 Email: contact@example.com
- 💬 Discussions: [GitHub Discussions](https://github.com/jmtoribio/001-HelpVet/discussions)
- 🐛 Issues: [GitHub Issues](https://github.com/jmtoribio/001-HelpVet/issues)

---

**Gracias por contribuir a HelpPet-Vet! 🎉**

*Última actualización: Noviembre 2024*

