# Quick Start Guide - HelpPet-Vet

**Guía de Inicio Rápido para Desarrolladores**

---

## ⚡ Inicio Rápido (5 minutos)

### Opción 1: Desktop (Windows) - Sin compilar

```bash
# 1. Descargar el APK precompilado
cd "Aplicacio escriptori/Installer Helppet-VET"

# 2. Ejecutar setup.exe o HelPPet-Vet.exe directamente
./setup.exe

# 3. Seguir el instalador de ClickOnce
```

### Opción 2: Mobile - Sin compilar

```bash
# Descargar APK precompilado
cd "App mobil"
# Instalar en dispositivo/emulador
adb install helppet.apk
```

### Opción 3: Desarrollo Local

**Requisitos**: Visual Studio 2019+ y Android Studio 4.1+

```bash
# Desktop
cd "Aplicacio escriptori/VisualBasic/HelPPet-Vet"
devenv HelPPet-Vet.sln
# Presionar F5 para ejecutar

# Mobile
cd "App mobil/APP-1 - Final"
open -a "Android Studio" .
# Click "Run" en Android Studio
```

---

## 🔑 Credenciales de Prueba

### Propietarios (Mobile App)

| Email | Contraseña | DNI |
|-------|-----------|-----|
| eusebiovera@gmail.com | 2222 | 09433726T |
| eliasborrego@gmail.com | 1111 | 00685667Z |

### Veterinarios (Desktop)

| Usuario | Contraseña | Nombre |
|---------|-----------|--------|
| jep | 2222 | JOAQUIN ESTEBAN POZO |
| lth | 1234 | Lucia teresa de la huerta |

### Administrador

- Usuario: (en archivo `admin.txt`)
- Contraseña: (en archivo `admin.txt`)

---

## 🏗️ Arquitectura en 30 segundos

```
Aplicación Desktop (VB.NET)
        ↓
    MySQL BD (Clever Cloud)
        ↑
Aplicación Móvil (Android)
        ↑
    API PHP Backend
        ↓
    MySQL BD (Clever Cloud)
```

---

## 📱 Primeros Pasos con Mobile

### 1. Login

```
Pantalla: LoginActivity
Entrada: 
  - Email: eusebiovera@gmail.com
  - Password: 2222
Resultado: Acceso a panel de propietario
```

### 2. Ver Mascotas

```
Pantalla: PetListActivity
Acción: Clic en "Mis Mascotas"
Resultado: Lista de mascotas del propietario
```

### 3. Ver Historial

```
Pantalla: PetDetailActivity
Acción: Clic en una mascota
Resultado: Casos clínicos y tratamientos
```

---

## 🖥️ Primeros Pasos con Desktop

### 1. Login

```
Pantalla: Login.vb
Seleccionar: "Veterinario"
Ingreso:
  Usuario: jep
  Password: 2222
Resultado: Panel Veterinari.vb
```

### 2. Buscar Cliente

```
Campo: Nombre del propietario
Botón: "Buscar"
Resultado: Lista de propietarios encontrados
```

### 3. Registrar Mascota

```
Pasos:
1. Seleccionar propietario
2. Llenar datos de mascota
3. Ingreso de microchip (15 dígitos)
4. Click "Registrar"
Resultado: Mascota guardada en BD
```

### 4. Registrar Caso Clínico

```
Pasos:
1. Seleccionar mascota
2. Llenar datos del caso
3. Ingreso de tratamiento
4. Click "Guardar Caso"
Resultado: Caso vinculado a mascota
```

---

## 🔧 Configuración Rápida de BD

### Conectar a MySQL Remoto

```bash
# Usar credentials existentes
mysql -h br6yuhxjtf6d9t43hrii-mysql.services.clever-cloud.com \
       -u upc64zf66fxq8gq9 \
       -phqD9cgVKNkL0zGpLCSoJ \
       br6yuhxjtf6d9t43hrii

# O importar schema
mysql -u user -p database < "Aplicacio escriptori/helppet_database.sql"
```

### Conectar Localmente (opcional)

```bash
# 1. Instalar MySQL Server
brew install mysql
# o descargar de https://dev.mysql.com/downloads/mysql/

# 2. Iniciar servidor
mysql.server start

# 3. Crear BD
mysql -u root -p
> CREATE DATABASE helppet;
> USE helppet;
> SOURCE path/to/helppet_database.sql;

# 4. Actualizar conexión en DT.vb
' Cambiar en DT.vb:
conexion = New MySqlConnection("server=localhost; user=root; password=tu_password; database=helppet")
```

---

## 📂 Estructura de Carpetas Clave

```
001-HelpVet/
├── Aplicacio escriptori/          ← Código Desktop
│   └── VisualBasic/HelPPet-Vet/
│       └── HelPPet-Vet.sln        ← Abrir en Visual Studio
├── App mobil/                     ← Código Mobile
│   ├── APP-1 - Final/
│   │   └── build.gradle           ← Abrir con Android Studio
│   └── Volley/                    ← APIs PHP
├── docs/                          ← Documentación
├── README.md                      ← Comienza aquí
└── SECURITY.md                    ← Notas de seguridad
```

---

## 🚨 Problemas Comunes

### "No se conecta a la BD"

```
✓ Verificar credenciales en DT.vb
✓ Verificar conectividad: ping br6yuhxjtf6d9t43hrii-mysql.services.clever-cloud.com
✓ Verificar puerto: 3306
✓ Verificar firewall
```

### "Login incorrecto"

```
✓ Verificar credenciales exactas (caso sensible)
✓ Verificar BD tiene datos (SELECT * FROM veterinari)
✓ Verificar archivo admin.txt si es admin
```

### "Mascota no aparece en móvil"

```
✓ Verificar mascota registrada en Desktop
✓ Verificar Propietari coincide con DNI
✓ Verificar API backend accesible
✓ Verificar logs: adb logcat en Android
```

### "APK no instala"

```
✓ Verificar arquitectura (arm64-v8a, armeabi-v7a)
✓ Verificar versión Android del dispositivo
✓ Desinstalar versión anterior primero:
  adb uninstall com.example.helppet
✓ Hacer clean y rebuild:
  ./gradlew clean
  ./gradlew assembleDebug
```

---

## 🔍 Comandos Útiles

### Git

```bash
# Actualizar repositorio local
git pull origin main

# Ver cambios sin confirmar
git status

# Deshacer cambios
git checkout -- archivo.vb

# Ver commits recientes
git log --oneline -5
```

### Android/Gradle

```bash
# Compilar sin instalar
./gradlew assemble

# Instalar en emulador
./gradlew installDebug

# Tests
./gradlew test

# Limpiar build cache
./gradlew clean

# Ver dependencias
./gradlew dependencies
```

### MySQL

```bash
# Conectar
mysql -h HOST -u USER -p DATABASE

# Ver tablas
SHOW TABLES;

# Ver estructura tabla
DESCRIBE mascota;

# Contar registros
SELECT COUNT(*) FROM mascota;

# Buscar mascota
SELECT * FROM mascota WHERE nom LIKE '%monet%';
```

### Adb (Android Debug Bridge)

```bash
# Listar dispositivos
adb devices

# Instalar APK
adb install app-release.apk

# Ver logs
adb logcat | grep -i helpet

# Ejecutar comando en dispositivo
adb shell

# Copiar archivo desde dispositivo
adb pull /data/local/tmp/archivo.txt

# Borrar cache app
adb shell pm clear com.example.helppet
```

---

## 📊 Flujo Típico de Desarrollo

### 1. Empezar

```bash
# Crear rama
git checkout -b feature/mi-feature

# Abrir IDE
devenv HelPPet-Vet.sln           # Desktop
open -a "Android Studio" .        # Mobile
```

### 2. Hacer cambios

```
Editar archivos → Compilar → Probar localmente
```

### 3. Confirmar cambios

```bash
# Ver cambios
git status
git diff

# Preparar cambios
git add .

# Confirmar
git commit -m "feat: descripción del cambio"

# Enviar
git push origin feature/mi-feature
```

### 4. Crear PR

```
Ir a GitHub → Create Pull Request
Describir cambios → Esperar revisión
```

---

## 🎯 Checklist antes de Hacer Push

- [ ] Código compila sin errores
- [ ] No hay warnings críticos
- [ ] Cambios están en rama correcta
- [ ] Tests pasan (si aplica)
- [ ] Commits tienen mensajes descriptivos
- [ ] Cambios están listos para revision
- [ ] No hay archivos sensibles (.env, passwords)

---

## 📚 Próximos Pasos

1. **Entender Arquitectura**: Leer [ARCHITECTURE.md](./docs/ARCHITECTURE.md)
2. **Aprender API**: Leer [API.md](./docs/API.md)
3. **BD**: Leer [DATABASE.md](./docs/DATABASE.md)
4. **Contribuir**: Leer [CONTRIBUTING.md](./docs/CONTRIBUTING.md)
5. **Seguridad**: Leer [SECURITY.md](./SECURITY.md)

---

## ✅ Verificación de Setup

Ejecuta esto para verificar que todo está configurado:

**Desktop**
```bash
cd "Aplicacio escriptori/VisualBasic/HelPPet-Vet"
msbuild HelPPet-Vet.sln /p:Configuration=Debug
# Si no hay errores → ✓ Listo
```

**Mobile**
```bash
cd "App mobil/APP-1 - Final"
./gradlew build
# Si termina con "BUILD SUCCESSFUL" → ✓ Listo
```

**BD**
```bash
mysql -h br6yuhxjtf6d9t43hrii-mysql.services.clever-cloud.com \
       -u upc64zf66fxq8gq9 -phqD9cgVKNkL0zGpLCSoJ \
       br6yuhxjtf6d9t43hrii -e "SELECT COUNT(*) FROM mascota;"
# Si muestra número → ✓ Listo
```

---

## 🆘 Necesitas Ayuda?

- 📖 Ver documentación: `/docs/`
- 🐛 Reportar bug: GitHub Issues
- 💬 Preguntar: GitHub Discussions
- 📧 Email: contact@example.com

---

**¡Felicidades! Ya estás listo para desarrollar en HelpPet-Vet 🚀**

*Última actualización: Noviembre 2024*

