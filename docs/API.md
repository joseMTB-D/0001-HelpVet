# API Reference - HelpPet-Vet

**Especificación Completa de Endpoints PHP y Comunicación Backend**

---

## 📑 Tabla de Contenidos

1. [Información General](#información-general)
2. [Autenticación](#autenticación)
3. [Gestión de Propietarios](#gestión-de-propietarios)
4. [Gestión de Mascotas](#gestión-de-mascotas)
5. [Gestión de Casos Clínicos](#gestión-de-casos-clínicos)
6. [Gestión de Veterinarios](#gestión-de-veterinarios)
7. [Formatos de Respuesta](#formatos-de-respuesta)
8. [Códigos de Error](#códigos-de-error)
9. [Ejemplos de Uso](#ejemplos-de-uso)

---

## ℹ️ Información General

### Descripción

La API de HelpPet-Vet permite la comunicación entre la aplicación móvil Android y el servidor backend. Se implementa mediante PHP y HTTP/JSON.

### Detalles Técnicos

| Propiedad | Valor |
|-----------|-------|
| **Protocolo** | HTTP/HTTPS |
| **Formato de Datos** | JSON |
| **Método de Autenticación** | Session-based (cookies) |
| **Ubicación** | `/App mobil/Volley/` |
| **Base de Datos** | MySQL (Clever Cloud) |

### URL Base

```
http://[SERVIDOR]/volley/              (Desarrollo)
https://[SERVIDOR]/volley/             (Producción)
```

### Headers Requeridos

```http
Content-Type: application/json
Accept: application/json
User-Agent: HelpPet-Android/1.0
```

---

## 🔐 Autenticación

### 1. Login de Propietario

Autentica un propietario (dueño de mascota) en el sistema.

**Endpoint**
```
POST /login.php
```

**Parámetros (JSON Body)**

| Parámetro | Tipo | Requerido | Descripción |
|-----------|------|----------|------------|
| `email` | string | SÍ | Email del propietario |
| `password` | string | SÍ | Contraseña en texto plano |
| `device_id` | string | NO | ID único del dispositivo |

**Ejemplo de Request**

```bash
curl -X POST http://api.example.com/volley/login.php \
  -H "Content-Type: application/json" \
  -d '{
    "email": "eusebiovera@gmail.com",
    "password": "2222",
    "device_id": "device_abc123"
  }'
```

**Respuesta Exitosa (200)**

```json
{
  "success": true,
  "message": "Autenticación exitosa",
  "user": {
    "DNI": "09433726T",
    "nom": "EUSEBIO VERA VIVES",
    "email": "eusebiovera@gmail.com",
    "telefono1": "685322005",
    "telefono2": "685322004",
    "direccion": "PLACETA DE ESPAÑA, 8",
    "CP": "3863"
  },
  "session_token": "sess_5f8c9a2e3b1d4f6a",
  "expires_in": 86400
}
```

**Respuesta Fallida (401)**

```json
{
  "success": false,
  "error": "INVALID_CREDENTIALS",
  "message": "Email o contraseña incorrectos"
}
```

---

### 2. Login de Veterinario

Autentica un veterinario en el sistema.

**Endpoint**
```
POST /login_vet.php
```

**Parámetros**

| Parámetro | Tipo | Requerido |
|-----------|------|----------|
| `usuario` | string | SÍ |
| `password` | string | SÍ |

**Ejemplo**

```bash
curl -X POST http://api.example.com/volley/login_vet.php \
  -d 'usuario=jep&password=2222'
```

**Respuesta Exitosa**

```json
{
  "success": true,
  "veterinario": {
    "codi": "23426546545",
    "nom": "JOAQUIN ESTEBAN POZO",
    "clinica": "Hospital Veterinari de Mollerussa",
    "usuario": "jep"
  },
  "token": "vet_token_xyz789"
}
```

---

### 3. Logout

Cierra la sesión del usuario.

**Endpoint**
```
POST /logout.php
```

**Parámetros**

| Parámetro | Tipo | Requerido |
|-----------|------|----------|
| `session_token` | string | SÍ |

**Respuesta**

```json
{
  "success": true,
  "message": "Sesión cerrada correctamente"
}
```

---

### 4. Registro de Nuevo Propietario

Registra un nuevo usuario propietario en el sistema.

**Endpoint**
```
POST /registro.php
```

**Parámetros**

| Parámetro | Tipo | Requerido | Validación |
|-----------|------|----------|-----------|
| `nom` | string | SÍ | Max 50 caracteres |
| `DNI` | string | SÍ | Formato: 8 dígitos + 1 letra |
| `email` | string | SÍ | Email válido, único |
| `password` | string | SÍ | Min 4 caracteres |
| `telefono1` | int | NO | - |
| `telefono2` | int | NO | - |
| `direccion` | string | NO | Max 40 caracteres |
| `CP` | int | NO | Entero positivo |

**Ejemplo de Request**

```bash
curl -X POST http://api.example.com/volley/registro.php \
  -d 'nom=Juan%20Garcia&DNI=12345678A&email=juan@example.com&password=pass123&telefono1=555123456'
```

**Respuesta Exitosa**

```json
{
  "success": true,
  "message": "Usuario registrado correctamente",
  "DNI": "12345678A"
}
```

**Respuesta Fallida**

```json
{
  "success": false,
  "error": "EMAIL_EXISTS",
  "message": "El email ya está registrado"
}
```

---

## 👥 Gestión de Propietarios

### 1. Obtener Información de Propietario

Recupera detalles del propietario autenticado.

**Endpoint**
```
GET /get_propietario.php?DNI=09433726T
```

**Respuesta**

```json
{
  "success": true,
  "propietario": {
    "nom": "EUSEBIO VERA VIVES",
    "DNI": "09433726T",
    "email": "eusebiovera@gmail.com",
    "telefono1": "685322005",
    "telefono2": "685322004",
    "direccion": "PLACETA DE ESPAÑA, 8",
    "CP": "3863"
  }
}
```

---

### 2. Actualizar Información de Propietario

Actualiza datos personales del propietario.

**Endpoint**
```
POST /update_propietario.php
```

**Parámetros**

```json
{
  "DNI": "09433726T",
  "nom": "EUSEBIO VERA VIVES",
  "email": "nuevo_email@example.com",
  "telefono1": "685322999",
  "telefono2": "685322888",
  "direccion": "Nueva Dirección 123",
  "CP": "08970"
}
```

**Respuesta**

```json
{
  "success": true,
  "message": "Propietario actualizado correctamente"
}
```

---

## 🐾 Gestión de Mascotas

### 1. Obtener Mascotas por Propietario

Lista todas las mascotas de un propietario.

**Endpoint**
```
GET /llenar_spinner.php?DNI=09433726T
```

**Parámetros**

| Parámetro | Tipo | Descripción |
|-----------|------|------------|
| `DNI` | string | DNI del propietario |

**Respuesta Exitosa**

```json
{
  "success": true,
  "mascotas": [
    {
      "codi": "642654264443655",
      "nom": "Monet",
      "Especie": "Perro",
      "raza": "bodegero",
      "sexo": "Macho",
      "color": "blanco",
      "Tamaño": "Pequeño",
      "peso": "5",
      "edat": "2019-11-21",
      "castrado": "Castrado"
    },
    {
      "codi": "123123124532613",
      "nom": "roce",
      "Especie": "Perro",
      "raza": "bodegero",
      "sexo": "Hembra",
      "color": "blanco",
      "Tamaño": "Pequeño",
      "peso": "5",
      "edat": "2020-11-10",
      "castrado": "Sin Castrar"
    }
  ],
  "count": 2
}
```

---

### 2. Obtener Información de Mascota

Recupera detalles completos de una mascota específica.

**Endpoint**
```
POST /llenar_info.php
```

**Parámetros**

```json
{
  "codi_mascota": "642654264443655"
}
```

**Respuesta**

```json
{
  "success": true,
  "mascota": {
    "codi": "642654264443655",
    "nom": "Monet",
    "Especie": "Perro",
    "raza": "bodegero",
    "sexo": "Macho",
    "color": "blanco",
    "Tamaño": "Pequeño",
    "pelo": "Semi Largo",
    "peso": "5",
    "edat": "2019-11-21",
    "castrado": "Castrado",
    "enfermetat": "Ninguna"
  },
  "propietario": {
    "DNI": "09433726T",
    "nom": "EUSEBIO VERA VIVES",
    "telefono1": "685322005"
  },
  "casos_totales": 2
}
```

---

### 3. Registrar Nueva Mascota

Registra una mascota nueva en el sistema.

**Endpoint**
```
POST /registrar_mascota.php
```

**Parámetros**

```json
{
  "Propietari": "09433726T",
  "nom": "Rex",
  "edat": "2021-03-15",
  "Especie": "Perro",
  "raza": "Labrador",
  "sexo": "Macho",
  "color": "negro",
  "Tamaño": "Grande",
  "pelo": "Corto",
  "castrado": "No",
  "peso": "30",
  "microchip": "123456789012345"
}
```

**Respuesta**

```json
{
  "success": true,
  "message": "Mascota registrada correctamente",
  "codi_mascota": "123456789012345"
}
```

---

### 4. Actualizar Información de Mascota

Actualiza datos de una mascota existente.

**Endpoint**
```
POST /update_mascota.php
```

**Parámetros**

```json
{
  "codi": "642654264443655",
  "nom": "Monet",
  "peso": "5.5",
  "castrado": "Castrado",
  "enfermetat": "Alergia alimentaria"
}
```

**Respuesta**

```json
{
  "success": true,
  "message": "Mascota actualizada correctamente"
}
```

---

## 🏥 Gestión de Casos Clínicos

### 1. Obtener Casos de Mascota

Lista todos los casos clínicos de una mascota.

**Endpoint**
```
POST /llenar_spinner_casos.php
```

**Parámetros**

```json
{
  "codi_mascota": "642654264443655"
}
```

**Respuesta**

```json
{
  "success": true,
  "casos": [
    {
      "codi_cas": "1",
      "Data_Registre": "2021-04-28 10:30:45",
      "Data_Revisio": "2021-04-28",
      "Observacio": "Revisión general",
      "pes": "5",
      "tractament": "Reposo",
      "medicaments": "Paracetamol",
      "enfermetats": "Gripe",
      "veterinario": "JOAQUIN ESTEBAN POZO"
    },
    {
      "codi_cas": "2",
      "Data_Registre": "2021-05-10 14:15:20",
      "Data_Revisio": "2029-12-28",
      "Observacio": "Revisión de seguimiento",
      "pes": "5.2",
      "tractament": "Reposo",
      "medicaments": "Ibuprofeno",
      "enfermetats": "Inflamación",
      "veterinario": "JOAQUIN ESTEBAN POZO"
    }
  ],
  "count": 2
}
```

---

### 2. Obtener Detalles de Caso

Recupera detalles completos de un caso específico.

**Endpoint**
```
POST /llenar_trata.php
```

**Parámetros**

```json
{
  "codi_caso": "1"
}
```

**Respuesta**

```json
{
  "success": true,
  "caso": {
    "codi_cas": "1",
    "codi_veterinari": "23426546545",
    "codi_mascota": "642654264443655",
    "Data_Registre": "2021-04-28 10:30:45",
    "Data_Revisio": "2021-04-28",
    "Observacio": "Revisión general. Mascota en buen estado. Se recomienda dieta balanceada y ejercicio regular.",
    "pes": "5",
    "tractament": "Reposo 2 semanas. Aplicar pomada en la zona afectada. Ejercicio moderado.",
    "medicaments": "Paracetamol 500mg cada 8 horas. Pomada antiinflamatoria 2 veces al día.",
    "enfermetats": "Gripe leve. Inflamación articular"
  },
  "veterinario": {
    "nom": "JOAQUIN ESTEBAN POZO",
    "clinica": "Hospital Veterinari de Mollerussa",
    "telefono": "973123456"
  }
}
```

---

### 3. Registrar Nuevo Caso

Crea un nuevo caso clínico para una mascota.

**Endpoint**
```
POST /registrar_caso.php
```

**Parámetros**

```json
{
  "codi_veterinari": "23426546545",
  "codi_mascota": "642654264443655",
  "Observacio": "Herida en la pata trasera izquierda",
  "pes": "5.1",
  "tractament": "Limpiar y desinfectar la herida. Reposo 3 días",
  "medicaments": "Clorexidina, Ibuprofeno 200mg cada 6 horas",
  "enfermetats": "Herida contaminada, riesgo de infección",
  "Data_Revisio": "2021-05-25"
}
```

**Respuesta**

```json
{
  "success": true,
  "message": "Caso registrado correctamente",
  "codi_caso": "5"
}
```

---

### 4. Actualizar Caso

Modifica la información de un caso existente.

**Endpoint**
```
POST /update_caso.php
```

**Parámetros**

```json
{
  "codi_caso": "1",
  "Observacio": "Actualización: Mascota mejorando",
  "pes": "5.2",
  "tractament": "Continuar con reposo",
  "medicaments": "Aumentar dosis de ibuprofeno",
  "Data_Revisio": "2021-05-30"
}
```

**Respuesta**

```json
{
  "success": true,
  "message": "Caso actualizado correctamente"
}
```

---

## 👨‍⚕️ Gestión de Veterinarios

### 1. Obtener Lista de Veterinarios

Lista todos los veterinarios disponibles.

**Endpoint**
```
GET /lista_veterinarios.php
```

**Respuesta**

```json
{
  "success": true,
  "veterinarios": [
    {
      "codi": "23426546545",
      "nom": "JOAQUIN ESTEBAN POZO",
      "clinica": "Hospital Veterinari de Mollerussa",
      "usuario": "jep"
    },
    {
      "codi": "54265453462",
      "nom": "Lucia teresa de la huerta de plata",
      "clinica": "el campos",
      "usuario": "lth"
    }
  ],
  "count": 2
}
```

---

### 2. Obtener Información de Veterinario

Recupera detalles de un veterinario específico.

**Endpoint**
```
GET /get_veterinario.php?codi=23426546545
```

**Respuesta**

```json
{
  "success": true,
  "veterinario": {
    "codi": "23426546545",
    "nom": "JOAQUIN ESTEBAN POZO",
    "clinica": "Hospital Veterinari de Mollerussa",
    "usuario": "jep",
    "especialidades": "Cirugía, Medicina General",
    "casos_atendidos": 15
  }
}
```

---

### 3. Obtener Citas Disponibles

Lista las citas disponibles para un veterinario en una fecha.

**Endpoint**
```
GET /citas_disponibles.php?codi_veterinario=23426546545&fecha=2021-05-25
```

**Respuesta**

```json
{
  "success": true,
  "citas": [
    {
      "hora": "09:00",
      "disponible": true
    },
    {
      "hora": "09:30",
      "disponible": true
    },
    {
      "hora": "10:00",
      "disponible": false
    }
  ]
}
```

---

## 📋 Formatos de Respuesta

### Estructura Estándar de Respuesta Exitosa

```json
{
  "success": true,
  "message": "Descripción opcional",
  "data": {
    // Datos específicos de la respuesta
  },
  "timestamp": "2021-05-20T14:30:00Z"
}
```

### Estructura Estándar de Respuesta de Error

```json
{
  "success": false,
  "error": "ERROR_CODE",
  "message": "Descripción del error",
  "details": {
    // Detalles adicionales (opcional)
  },
  "timestamp": "2021-05-20T14:30:00Z"
}
```

---

## ⚠️ Códigos de Error

| Código | HTTP | Descripción |
|--------|------|------------|
| `INVALID_CREDENTIALS` | 401 | Email/contraseña incorrectos |
| `USER_NOT_FOUND` | 404 | Usuario no existe |
| `EMAIL_EXISTS` | 409 | Email ya registrado |
| `INVALID_INPUT` | 400 | Parámetros inválidos |
| `MISSING_REQUIRED_FIELD` | 400 | Falta un parámetro requerido |
| `DATABASE_ERROR` | 500 | Error en la base de datos |
| `SESSION_EXPIRED` | 401 | Sesión expirada |
| `UNAUTHORIZED` | 403 | Sin permisos |
| `RESOURCE_NOT_FOUND` | 404 | Recurso no existe |
| `SERVER_ERROR` | 500 | Error del servidor |

---

## 💡 Ejemplos de Uso

### Flujo Completo: Consultar Casos de Mascota

#### Paso 1: Autenticar Usuario

```bash
curl -X POST http://api.example.com/volley/login.php \
  -H "Content-Type: application/json" \
  -d '{
    "email": "eusebiovera@gmail.com",
    "password": "2222"
  }'
```

Respuesta:
```json
{
  "success": true,
  "user": {
    "DNI": "09433726T",
    "nom": "EUSEBIO VERA VIVES"
  },
  "session_token": "sess_5f8c9a2e3b1d4f6a"
}
```

#### Paso 2: Obtener Mascotas del Propietario

```bash
curl -X GET "http://api.example.com/volley/llenar_spinner.php?DNI=09433726T" \
  -H "Authorization: Bearer sess_5f8c9a2e3b1d4f6a"
```

Respuesta:
```json
{
  "success": true,
  "mascotas": [
    {
      "codi": "642654264443655",
      "nom": "Monet"
    }
  ]
}
```

#### Paso 3: Obtener Casos de la Mascota

```bash
curl -X POST http://api.example.com/volley/llenar_spinner_casos.php \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer sess_5f8c9a2e3b1d4f6a" \
  -d '{"codi_mascota": "642654264443655"}'
```

Respuesta:
```json
{
  "success": true,
  "casos": [
    {
      "codi_cas": "1",
      "Data_Registre": "2021-04-28 10:30:45",
      "tractament": "Reposo"
    }
  ]
}
```

### Implementación en Android (Volley)

```java
// 1. Crear request de login
String loginUrl = "http://api.example.com/volley/login.php";
JSONObject loginParams = new JSONObject();
loginParams.put("email", "usuario@example.com");
loginParams.put("password", "password");

JsonObjectRequest loginRequest = new JsonObjectRequest(
    Request.Method.POST,
    loginUrl,
    loginParams,
    response -> {
        // Guardar token
        String sessionToken = response.getString("session_token");
        SharedPreferences.getInstance().saveToken(sessionToken);
        
        // Continuar con siguiente request
        fetchMascotas(sessionToken);
    },
    error -> handleError(error)
);

// 2. Hacer request de mascotas
private void fetchMascotas(String sessionToken) {
    String mascotasUrl = "http://api.example.com/volley/llenar_spinner.php?DNI=09433726T";
    
    JsonObjectRequest mascotasRequest = new JsonObjectRequest(
        Request.Method.GET,
        mascotasUrl,
        null,
        response -> updateUI(response.getJSONArray("mascotas")),
        error -> handleError(error)
    ) {
        @Override
        public Map<String, String> getHeaders() {
            Map<String, String> headers = new HashMap<>();
            headers.put("Authorization", "Bearer " + sessionToken);
            return headers;
        }
    };
    
    RequestQueue queue = Volley.newRequestQueue(context);
    queue.add(mascotasRequest);
}
```

---

## 🔄 Rate Limiting

Para proteger el servidor, se implementan límites de velocidad:

| Endpoint | Límite | Ventana |
|----------|--------|---------|
| `/login.php` | 5 intentos | 15 minutos |
| `/registro.php` | 3 intentos | 1 hora |
| Otros endpoints | 100 requests | 1 minuto |

---

## 📝 Versionamiento de API

La API usa versionamiento por header:

```http
X-API-Version: 1.0
```

Versiones soportadas:
- **v1.0** - Versión inicial (actual)
- **v1.1** - En desarrollo

---

**Documento actualizado**: Noviembre 2024  
**Versión**: 1.0  
**Autor**: Equipo de Desarrollo HelpPet-Vet

