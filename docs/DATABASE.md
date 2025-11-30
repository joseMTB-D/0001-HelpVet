# Database Schema - HelpPet-Vet

**Especificación Completa de la Base de Datos**

---

## 📑 Tabla de Contenidos

1. [Información General](#información-general)
2. [Tablas Principales](#tablas-principales)
3. [Relaciones](#relaciones)
4. [Índices](#índices)
5. [Constraints](#constraints)
6. [Vistas SQL](#vistas-sql)
7. [Procedimientos Almacenados](#procedimientos-almacenados)
8. [Datos de Ejemplo](#datos-de-ejemplo)
9. [Migraciones](#migraciones)

---

## ℹ️ Información General

| Propiedad | Valor |
|-----------|-------|
| **Sistema de BD** | MySQL |
| **Versión** | 8.0.22-13 |
| **Nombre BD** | `br6yuhxjtf6d9t43hrii` |
| **Servidor** | `br6yuhxjtf6d9t43hrii-mysql.services.clever-cloud.com:3306` |
| **Charset** | utf8mb4 |
| **Collation** | utf8mb4_0900_ai_ci |
| **Engine** | InnoDB (Transacciones soportadas) |

### Configuración de Conexión

```
Host: br6yuhxjtf6d9t43hrii-mysql.services.clever-cloud.com
Port: 3306
Database: br6yuhxjtf6d9t43hrii
User: upc64zf66fxq8gq9
Password: hqD9cgVKNkL0zGpLCSoJ
SSL: Recomendado pero no obligatorio

JDBC URL:
jdbc:mysql://br6yuhxjtf6d9t43hrii-mysql.services.clever-cloud.com:3306/br6yuhxjtf6d9t43hrii

Connection String (.NET):
server=br6yuhxjtf6d9t43hrii-mysql.services.clever-cloud.com;user=upc64zf66fxq8gq9;password=hqD9cgVKNkL0zGpLCSoJ;database=br6yuhxjtf6d9t43hrii
```

---

## 📊 Tablas Principales

### 1. Tabla `propietari` (Propietarios)

**Propósito**: Almacenar información de propietarios/dueños de mascotas

```sql
CREATE TABLE `propietari` (
  `nom` VARCHAR(50) DEFAULT NULL,
  `DNI` VARCHAR(9) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL,
  `t1` INT DEFAULT NULL,
  `t2` INT DEFAULT NULL,
  `Adreça` VARCHAR(40) DEFAULT NULL,
  `CP` INT DEFAULT NULL,
  `gmail` VARCHAR(24) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci DEFAULT NULL,
  `pass` VARCHAR(12) DEFAULT NULL,
  PRIMARY KEY (`DNI`),
  UNIQUE KEY `gmail` (`gmail`),
  UNIQUE KEY `t1` (`t1`),
  UNIQUE KEY `t2` (`t2`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
```

#### Columnas

| Columna | Tipo | Null | Clave | Descripción |
|---------|------|------|-------|------------|
| `nom` | VARCHAR(50) | SÍ | - | Nombre completo del propietario |
| `DNI` | VARCHAR(9) | NO | PK | Documento Nacional de Identidad (único) |
| `t1` | INT | SÍ | UNIQUE | Teléfono principal |
| `t2` | INT | SÍ | UNIQUE | Teléfono secundario |
| `Adreça` | VARCHAR(40) | SÍ | - | Dirección del domicilio |
| `CP` | INT | SÍ | - | Código Postal |
| `gmail` | VARCHAR(24) | SÍ | UNIQUE | Correo electrónico |
| `pass` | VARCHAR(12) | SÍ | - | Contraseña (⚠️ SIN ENCRIPTAR - MEJORAR) |

#### Datos de Ejemplo

```sql
INSERT INTO `propietari` VALUES
('ELIAS BORREGO CARRERA', '00685667Z', 625815553, 625815552, 'PLACETA HORNO, 64', 46012, 'eliasborrego@gmail.com', '1111'),
('EUSEBIO VERA VIVES', '09433726T', 685322005, 685322004, 'PLACETA DE ESPAÑA, 8', 3863, 'eusebiovera@gmail.com', '2222'),
('CESAR GRANDE DOMINGO', '33903434P', 764550751, 764550751, 'PASAJE NUEVA, 81', 17971, NULL, NULL),
('LUCIANO SOLE GRANDE', '40961821V', 632127648, 632127647, 'ESTRADA DE ESPAÑA, 93', 47119, NULL, NULL);
```

---

### 2. Tabla `mascota` (Mascotas/Animales)

**Propósito**: Catálogo de mascotas y sus características

```sql
CREATE TABLE `mascota` (
  `codi` BIGINT NOT NULL,
  `Propietari` VARCHAR(9) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci DEFAULT NULL,
  `nom` VARCHAR(20) DEFAULT NULL,
  `edat` DATE DEFAULT NULL,
  `enfermetat` VARCHAR(60) DEFAULT NULL,
  `tractat` INT DEFAULT NULL,
  `Especie` VARCHAR(20) DEFAULT NULL,
  `raza` VARCHAR(30) DEFAULT NULL,
  `sexo` VARCHAR(30) DEFAULT NULL,
  `color` VARCHAR(30) DEFAULT NULL,
  `Tamaño` VARCHAR(20) DEFAULT NULL,
  `pelo` VARCHAR(35) DEFAULT NULL,
  `castrado` VARCHAR(20) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci DEFAULT NULL,
  `peso` DOUBLE DEFAULT NULL,
  PRIMARY KEY (`codi`),
  KEY `fk_mascota_propietari` (`Propietari`),
  CONSTRAINT `fk_mascota_propietari` FOREIGN KEY (`Propietari`) REFERENCES `propietari` (`DNI`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
```

#### Columnas

| Columna | Tipo | Null | Clave | Descripción |
|---------|------|------|-------|------------|
| `codi` | BIGINT | NO | PK | ID único de la mascota (microchip o generado) |
| `Propietari` | VARCHAR(9) | SÍ | FK | DNI del propietario |
| `nom` | VARCHAR(20) | SÍ | - | Nombre de la mascota |
| `edat` | DATE | SÍ | - | Fecha de nacimiento |
| `enfermetat` | VARCHAR(60) | SÍ | - | Enfermedad actual/diagnosticada |
| `tractat` | INT | SÍ | - | ¿Está siendo tratado? (0/1) |
| `Especie` | VARCHAR(20) | SÍ | - | Tipo (Perro, Gato, Ave, Roedor, Reptil) |
| `raza` | VARCHAR(30) | SÍ | - | Raza específica (ej: Collie, Persa) |
| `sexo` | VARCHAR(30) | SÍ | - | Macho/Hembra |
| `color` | VARCHAR(30) | SÍ | - | Color del pelaje |
| `Tamaño` | VARCHAR(20) | SÍ | - | Pequeño/Mediano/Grande |
| `pelo` | VARCHAR(35) | SÍ | - | Tipo de pelaje (Corto, Semi Largo, Largo) |
| `castrado` | VARCHAR(20) | SÍ | - | Estado de castración/esterilización |
| `peso` | DOUBLE | SÍ | - | Peso en kilogramos |

#### Datos de Ejemplo

```sql
INSERT INTO `mascota` VALUES
(123123124532613, '09433726T', 'roce', '2020-11-10', NULL, NULL, 'Perro', 'bodegero', 'Hembra', 'blanco', 'Pequeño', 'Semi Largo', 'Sin Castrar', 5),
(123136425654656, '00685667Z', 'Janis', '2019-11-21', NULL, NULL, 'Ave', 'Carolina', 'Hembra', 'blanco', 'Pequeño', 'Semi Largo', 'Sin Castrar', 1),
(231236541324165, '40961821V', 'Klaus', '2020-10-22', NULL, NULL, 'Perro', 'Pug', 'Macho', 'blanco', 'Pequeño', 'Corto', 'Sin Castrar', 5),
(642654264443655, '09433726T', 'Monet', '2019-11-21', NULL, NULL, 'Perro', 'bodegero', 'Macho', 'blanco', 'Pequeño', 'Semi Largo', 'Castrado', 5);
```

---

### 3. Tabla `veterinari` (Veterinarios)

**Propósito**: Registro de profesionales veterinarios con acceso al sistema

```sql
CREATE TABLE `veterinari` (
  `codi` BIGINT NOT NULL,
  `nom` VARCHAR(50) DEFAULT NULL,
  `clinica` VARCHAR(50) DEFAULT NULL,
  `Usuari` VARCHAR(3) DEFAULT NULL,
  `password` INT DEFAULT NULL,
  PRIMARY KEY (`codi`),
  UNIQUE KEY `Usuari` (`Usuari`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
```

#### Columnas

| Columna | Tipo | Null | Clave | Descripción |
|---------|------|------|-------|------------|
| `codi` | BIGINT | NO | PK | ID único del veterinario |
| `nom` | VARCHAR(50) | SÍ | - | Nombre completo |
| `clinica` | VARCHAR(50) | SÍ | - | Clínica/Hospital asociado |
| `Usuari` | VARCHAR(3) | SÍ | UNIQUE | Nombre de usuario para login |
| `password` | INT | SÍ | - | Contraseña (⚠️ ALMACENADA COMO INT - CRÍTICO) |

#### Datos de Ejemplo

```sql
INSERT INTO `veterinari` VALUES
(23426546545, 'JOAQUIN ESTEBAN POZO', 'Hospital Veterinari de Mollerussa', 'jep', 2222),
(54265453462, 'Lucia teresa de la huerta de plata', 'el campos', 'lth', 1234);
```

---

### 4. Tabla `casos` (Casos Clínicos)

**Propósito**: Registro de atenciones veterinarias y tratamientos

```sql
CREATE TABLE `casos` (
  `codi_cas` BIGINT NOT NULL,
  `codi_veterinari` BIGINT DEFAULT NULL,
  `codi_mascota` BIGINT DEFAULT NULL,
  `Data_Registre` TIMESTAMP NULL DEFAULT CURRENT_TIMESTAMP,
  `Data_Revisio` DATE DEFAULT NULL,
  `Observacio` VARCHAR(800) DEFAULT NULL,
  `pes` DOUBLE DEFAULT NULL,
  `tractament` VARCHAR(400) DEFAULT NULL,
  `medicaments` VARCHAR(400) DEFAULT NULL,
  `enfermetats` VARCHAR(400) DEFAULT NULL,
  PRIMARY KEY (`codi_cas`),
  KEY `fk_casos_veterinari` (`codi_veterinari`),
  KEY `fk_casos_mascota` (`codi_mascota`),
  CONSTRAINT `fk_casos_mascota` FOREIGN KEY (`codi_mascota`) REFERENCES `mascota` (`codi`),
  CONSTRAINT `fk_casos_veterinari` FOREIGN KEY (`codi_veterinari`) REFERENCES `veterinari` (`codi`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
```

#### Columnas

| Columna | Tipo | Null | Clave | Descripción |
|---------|------|------|-------|------------|
| `codi_cas` | BIGINT | NO | PK | ID único del caso |
| `codi_veterinari` | BIGINT | SÍ | FK | ID del veterinario responsable |
| `codi_mascota` | BIGINT | SÍ | FK | ID de la mascota tratada |
| `Data_Registre` | TIMESTAMP | SÍ | - | Fecha/hora de registro (auto: NOW()) |
| `Data_Revisio` | DATE | SÍ | - | Fecha de próxima revisión |
| `Observacio` | VARCHAR(800) | SÍ | - | Notas clínicas del veterinario |
| `pes` | DOUBLE | SÍ | - | Peso de la mascota en esa visita |
| `tractament` | VARCHAR(400) | SÍ | - | Tratamiento prescrito |
| `medicaments` | VARCHAR(400) | SÍ | - | Medicamentos recomendados |
| `enfermetats` | VARCHAR(400) | SÍ | - | Enfermedades diagnosticadas |

#### Datos de Ejemplo

```sql
INSERT INTO `casos` VALUES
(1, 64264416524, 234341241234253, '2021-04-28 10:30:45', '2021-04-28', 'N/A', 6, 'Reposo', 'N/A', 'N/A'),
(2, 23426546545, 642654264443655, '2021-05-10 14:15:20', '2029-12-28', 'Castrar', 8, 'reposo', 'N/A', 'N/A'),
(3, 23426546545, 642654264443655, '2021-05-11 09:45:00', '2029-12-11', 'vomitos', 8, 'dieta', 'N/A', 'mareos'),
(4, 23426546545, 123136425654656, '2021-05-15 11:30:15', NULL, 'Herida en pata trasera izquierda. Perdida de pelo.', 20, 'Curar herida de la pata. Dar pastillas 3 veces al dia.', 'Hibuprofeno, clorexidina', 'Herida contaminada'),
(5, 23426546545, 123123124532613, '2021-05-20 16:20:40', '2029-12-25', 'wetgsegqqwre', 97, 'fwergfwegf', 'wrgwergweg', 'rwfwqfe');
```

---

## 🔗 Relaciones

### Diagrama ER

```
┌──────────────┐
│  propietari  │
├──────────────┤
│ PK: DNI      │
│ nom          │
│ t1, t2       │
│ Adreça       │
│ CP           │
│ gmail        │
│ pass         │
└──────┬───────┘
       │
       │ 1
       │
       │ N
       ▼
┌──────────────────────┐
│      mascota         │
├──────────────────────┤
│ PK: codi             │
│ FK: Propietari (DNI) │◄─────┐
│ nom                  │      │
│ Especie              │      │
│ raza                 │      │
│ edat                 │      │
│ peso                 │      │
│ color                │      │
│ Tamaño               │      │
│ pelo                 │      │
│ castrado             │      │
│ sexo                 │      │
│ enfermetat           │      │
│ tractat              │      │
└──────┬───────────────┘      │
       │                      │
       │ 1                    │
       │                      │
       │ N                    │
       ▼                      │
┌──────────────────────┐     │
│       casos          │     │
├──────────────────────┤     │
│ PK: codi_cas         │     │
│ FK: codi_mascota─────┼─────┘
│ FK: codi_veterinari  │
│ Data_Registre        │
│ Data_Revisio         │
│ Observacio           │
│ pes                  │
│ tractament           │
│ medicaments          │
│ enfermetats          │
└──────┬───────────────┘
       │
       │ N
       │
       │ 1
       ▼
┌──────────────┐
│ veterinari   │
├──────────────┤
│ PK: codi     │
│ nom          │
│ clinica      │
│ Usuari       │
│ password     │
└──────────────┘
```

### Tabla de Relaciones

| De | A | Relación | Tipo | Descripción |
|---|---|----------|------|------------|
| propietari | mascota | DNI → Propietari | 1:N | Un propietario tiene N mascotas |
| mascota | casos | codi → codi_mascota | 1:N | Una mascota tiene N casos |
| veterinari | casos | codi → codi_veterinari | 1:N | Un veterinario atiende N casos |

---

## 🔑 Índices

### Índices Primarios

```sql
-- Tabla propietari
ALTER TABLE propietari ADD PRIMARY KEY (DNI);

-- Tabla mascota
ALTER TABLE mascota ADD PRIMARY KEY (codi);

-- Tabla veterinari
ALTER TABLE veterinari ADD PRIMARY KEY (codi);

-- Tabla casos
ALTER TABLE casos ADD PRIMARY KEY (codi_cas);
```

### Índices Foráneos (Foreign Keys)

```sql
-- mascota.Propietari → propietari.DNI
ALTER TABLE mascota 
ADD CONSTRAINT fk_mascota_propietari 
FOREIGN KEY (Propietari) REFERENCES propietari(DNI);

-- casos.codi_mascota → mascota.codi
ALTER TABLE casos 
ADD CONSTRAINT fk_casos_mascota 
FOREIGN KEY (codi_mascota) REFERENCES mascota(codi);

-- casos.codi_veterinari → veterinari.codi
ALTER TABLE casos 
ADD CONSTRAINT fk_casos_veterinari 
FOREIGN KEY (codi_veterinari) REFERENCES veterinari(codi);
```

### Índices Secundarios (Performance)

```sql
-- Para búsquedas frecuentes
CREATE INDEX idx_mascota_propietari ON mascota(Propietari);
CREATE INDEX idx_mascota_especie ON mascota(Especie);
CREATE INDEX idx_casos_veterinari ON casos(codi_veterinari);
CREATE INDEX idx_casos_mascota ON casos(codi_mascota);
CREATE INDEX idx_casos_fecha ON casos(Data_Registre);
CREATE INDEX idx_propietari_email ON propietari(gmail);
CREATE INDEX idx_veterinari_usuario ON veterinari(Usuari);
```

---

## 🛡️ Constraints

### NOT NULL

```sql
-- propietari
ALTER TABLE propietari MODIFY nom VARCHAR(50) NOT NULL;
ALTER TABLE propietari MODIFY DNI VARCHAR(9) NOT NULL;

-- mascota
ALTER TABLE mascota MODIFY codi BIGINT NOT NULL;

-- veterinari
ALTER TABLE veterinari MODIFY codi BIGINT NOT NULL;

-- casos
ALTER TABLE casos MODIFY codi_cas BIGINT NOT NULL;
```

### UNIQUE

```sql
-- propietari
ALTER TABLE propietari ADD CONSTRAINT uk_propietari_email UNIQUE(gmail);
ALTER TABLE propietari ADD CONSTRAINT uk_propietari_t1 UNIQUE(t1);
ALTER TABLE propietari ADD CONSTRAINT uk_propietari_t2 UNIQUE(t2);

-- mascota
ALTER TABLE mascota ADD CONSTRAINT uk_mascota_microchip UNIQUE(codi);

-- veterinari
ALTER TABLE veterinari ADD CONSTRAINT uk_veterinari_usuario UNIQUE(Usuari);
```

### CHECK (para integridad de datos)

```sql
-- Validar que el peso sea positivo
ALTER TABLE mascota 
ADD CONSTRAINT ck_mascota_peso CHECK (peso > 0);

-- Validar que tractat sea 0 o 1
ALTER TABLE mascota 
ADD CONSTRAINT ck_mascota_tractat CHECK (tractat IN (0, 1));

-- Validar que CP sea positivo
ALTER TABLE propietari 
ADD CONSTRAINT ck_propietari_cp CHECK (CP > 0);

-- Validar que Data_Revisio sea posterior a Data_Registre
ALTER TABLE casos 
ADD CONSTRAINT ck_casos_fechas CHECK (Data_Revisio > Date(Data_Registre));
```

---

## 👁️ Vistas SQL

### Vista: Historial de Mascotas por Propietario

```sql
CREATE VIEW vw_historial_mascota_propietario AS
SELECT 
    p.DNI,
    p.nom AS propietario,
    m.codi,
    m.nom AS mascota,
    m.Especie,
    m.raza,
    m.edat,
    m.peso,
    c.codi_cas,
    c.Data_Registre,
    c.Data_Revisio,
    c.tractament,
    c.medicaments,
    c.enfermetats,
    v.nom AS veterinario
FROM propietari p
LEFT JOIN mascota m ON p.DNI = m.Propietari
LEFT JOIN casos c ON m.codi = c.codi_mascota
LEFT JOIN veterinari v ON c.codi_veterinari = v.codi
ORDER BY p.nom, m.nom, c.Data_Registre DESC;
```

### Vista: Carga de Trabajo del Veterinario

```sql
CREATE VIEW vw_carga_veterinario AS
SELECT 
    v.codi,
    v.nom,
    v.clinica,
    COUNT(c.codi_cas) AS total_casos,
    COUNT(DISTINCT c.codi_mascota) AS total_mascotas,
    COUNT(DISTINCT c.codi_veterinari) AS animales_en_tratamiento,
    MAX(c.Data_Registre) AS ultima_atencion
FROM veterinari v
LEFT JOIN casos c ON v.codi = c.codi_veterinari
GROUP BY v.codi, v.nom, v.clinica;
```

### Vista: Mascotas Pendientes de Revisión

```sql
CREATE VIEW vw_mascotas_pendiente_revison AS
SELECT 
    m.codi,
    m.nom,
    m.Especie,
    p.nom AS propietario,
    p.t1,
    c.Data_Revisio,
    DATEDIFF(c.Data_Revisio, CURDATE()) AS dias_pendientes
FROM mascota m
JOIN propietari p ON m.Propietari = p.DNI
JOIN casos c ON m.codi = c.codi_mascota
WHERE c.Data_Revisio > CURDATE()
    AND c.Data_Revisio IS NOT NULL
ORDER BY c.Data_Revisio ASC;
```

---

## 🔧 Procedimientos Almacenados

### SP: Registrar Nuevo Caso

```sql
DELIMITER $$

CREATE PROCEDURE sp_registrar_caso(
    IN p_codi_cas BIGINT,
    IN p_codi_veterinari BIGINT,
    IN p_codi_mascota BIGINT,
    IN p_observacio VARCHAR(800),
    IN p_peso DOUBLE,
    IN p_tractament VARCHAR(400),
    IN p_medicaments VARCHAR(400),
    IN p_enfermetats VARCHAR(400),
    IN p_fecha_revisio DATE
)
BEGIN
    DECLARE EXIT HANDLER FOR SQLEXCEPTION
    BEGIN
        ROLLBACK;
        SIGNAL SQLSTATE '45000' 
        SET MESSAGE_TEXT = 'Error al registrar el caso';
    END;
    
    START TRANSACTION;
    
    -- Validar que la mascota existe
    IF NOT EXISTS (SELECT 1 FROM mascota WHERE codi = p_codi_mascota) THEN
        SIGNAL SQLSTATE '45000' 
        SET MESSAGE_TEXT = 'La mascota especificada no existe';
    END IF;
    
    -- Validar que el veterinario existe
    IF NOT EXISTS (SELECT 1 FROM veterinari WHERE codi = p_codi_veterinari) THEN
        SIGNAL SQLSTATE '45000' 
        SET MESSAGE_TEXT = 'El veterinario especificado no existe';
    END IF;
    
    -- Insertar el caso
    INSERT INTO casos (
        codi_cas, codi_veterinari, codi_mascota, 
        Data_Registre, Data_Revisio, Observacio, 
        pes, tractament, medicaments, enfermetats
    ) VALUES (
        p_codi_cas, p_codi_veterinari, p_codi_mascota,
        NOW(), p_fecha_revisio, p_observacio,
        p_peso, p_tractament, p_medicaments, p_enfermetats
    );
    
    -- Actualizar mascota como tratada
    UPDATE mascota 
    SET tractat = 1 
    WHERE codi = p_codi_mascota;
    
    COMMIT;
END$$

DELIMITER ;
```

### SP: Obtener Historial de Mascota

```sql
DELIMITER $$

CREATE PROCEDURE sp_obtener_historial_mascota(
    IN p_codi_mascota BIGINT
)
BEGIN
    SELECT 
        c.codi_cas,
        c.Data_Registre,
        c.Data_Revisio,
        c.Observacio,
        c.pes,
        c.tractament,
        c.medicaments,
        c.enfermetats,
        v.nom AS veterinario,
        v.clinica
    FROM casos c
    JOIN veterinari v ON c.codi_veterinari = v.codi
    WHERE c.codi_mascota = p_codi_mascota
    ORDER BY c.Data_Registre DESC;
END$$

DELIMITER ;
```

---

## 📦 Datos de Ejemplo

### Propietarios

```sql
INSERT INTO propietari (nom, DNI, t1, t2, Adreça, CP, gmail, pass) VALUES
('ELIAS BORREGO CARRERA', '00685667Z', 625815553, 625815552, 'PLACETA HORNO, 64', 46012, 'eliasborrego@gmail.com', '1111'),
('EUSEBIO VERA VIVES', '09433726T', 685322005, 685322004, 'PLACETA DE ESPAÑA, 8', 3863, 'eusebiovera@gmail.com', '2222'),
('CESAR GRANDE DOMINGO', '33903434P', 764550751, 764550751, 'PASAJE NUEVA, 81', 17971, NULL, NULL),
('LUCIANO SOLE GRANDE', '40961821V', 632127648, 632127647, 'ESTRADA DE ESPAÑA, 93', 47119, NULL, NULL),
('CELESTINO SOTO AVILES', '43482452D', 766904867, 766904867, 'ESTRADA CATALUNYA, 34', 16243, NULL, NULL),
('carles', '65465466J', 625255483, 973656484, 'Arriba España', 56597, NULL, NULL);
```

### Mascotas

```sql
INSERT INTO mascota (codi, Propietari, nom, edat, Especie, raza, sexo, color, Tamaño, pelo, castrado, peso) VALUES
(123123124532613, '09433726T', 'roce', '2020-11-10', 'Perro', 'bodegero', 'Hembra', 'blanco', 'Pequeño', 'Semi Largo', 'Sin Castrar', 5),
(123136425654656, '00685667Z', 'Janis', '2019-11-21', 'Ave', 'Carolina', 'Hembra', 'blanco', 'Pequeño', 'Semi Largo', 'Sin Castrar', 1),
(231236541324165, '40961821V', 'Klaus', '2020-10-22', 'Perro', 'Pug', 'Macho', 'blanco', 'Pequeño', 'Corto', 'Sin Castrar', 5),
(642654264443655, '09433726T', 'Monet', '2019-11-21', 'Perro', 'bodegero', 'Macho', 'blanco', 'Pequeño', 'Semi Largo', 'Castrado', 5);
```

### Veterinarios

```sql
INSERT INTO veterinari (codi, nom, clinica, Usuari, password) VALUES
(23426546545, 'JOAQUIN ESTEBAN POZO', 'Hospital Veterinari de Mollerussa', 'jep', 2222),
(54265453462, 'Lucia teresa de la huerta de plata', 'el campos', 'lth', 1234);
```

---

## 📝 Migraciones

### Migración v1.0 → v1.1: Mejora de Seguridad

```sql
-- Cambiar tipo de contraseña en veterinari
ALTER TABLE veterinari MODIFY COLUMN password VARCHAR(255) DEFAULT NULL;

-- Añadir columna de hash de contraseña
ALTER TABLE veterinari 
ADD COLUMN password_hash VARCHAR(255) DEFAULT NULL,
ADD COLUMN password_salt VARCHAR(255) DEFAULT NULL;

-- Añadir auditoría
CREATE TABLE audit_log (
    id INT AUTO_INCREMENT PRIMARY KEY,
    table_name VARCHAR(100),
    operation VARCHAR(10), -- INSERT, UPDATE, DELETE
    old_values JSON,
    new_values JSON,
    user VARCHAR(100),
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Migración v1.1 → v1.2: Soporte Citas

```sql
-- Nueva tabla para citas
CREATE TABLE citas (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    codi_mascota BIGINT NOT NULL,
    codi_veterinari BIGINT NOT NULL,
    fecha_hora DATETIME NOT NULL,
    tipo_cita VARCHAR(50), -- Consulta, Vacuna, Cirugía
    estado VARCHAR(20), -- Pendiente, Confirmado, Completado, Cancelado
    notas VARCHAR(500),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (codi_mascota) REFERENCES mascota(codi),
    FOREIGN KEY (codi_veterinari) REFERENCES veterinari(codi),
    INDEX idx_fecha_hora (fecha_hora),
    INDEX idx_estado (estado)
);
```

---

## 🔍 Queries Útiles

### Obtener todas las mascotas de un propietario

```sql
SELECT m.* 
FROM mascota m
WHERE m.Propietari = '09433726T'
ORDER BY m.nom;
```

### Casos en los últimos 30 días

```sql
SELECT c.*, m.nom as mascota, v.nom as veterinario
FROM casos c
JOIN mascota m ON c.codi_mascota = m.codi
JOIN veterinari v ON c.codi_veterinari = v.codi
WHERE c.Data_Registre >= DATE_SUB(NOW(), INTERVAL 30 DAY)
ORDER BY c.Data_Registre DESC;
```

### Mascotas que necesitan revisión

```sql
SELECT m.*, p.nom as propietario, p.t1
FROM mascota m
JOIN propietari p ON m.Propietari = p.DNI
JOIN casos c ON m.codi = c.codi_mascota
WHERE c.Data_Revisio BETWEEN CURDATE() AND DATE_ADD(CURDATE(), INTERVAL 7 DAY)
ORDER BY c.Data_Revisio ASC;
```

### Estadísticas por veterinario

```sql
SELECT 
    v.nom,
    COUNT(DISTINCT c.codi_mascota) as mascotas_atendidas,
    COUNT(c.codi_cas) as total_casos,
    AVG(m.peso) as peso_promedio_mascota,
    MAX(c.Data_Registre) as ultima_atencion
FROM veterinari v
LEFT JOIN casos c ON v.codi = c.codi_veterinari
LEFT JOIN mascota m ON c.codi_mascota = m.codi
GROUP BY v.codi, v.nom;
```

---

**Documento actualizado**: Noviembre 2024  
**Versión**: 1.0  
**Autor**: Equipo de Desarrollo HelpPet-Vet

