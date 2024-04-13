# Diseño de la Base de Datos para un Sistema de Información de Películas

Este documento describe el diseño de una base de datos para un sistema de información de películas. El sistema está diseñado para manejar información sobre películas, estudios de cine, y personas (actores y directores). Se incluyen detalles técnicos sobre la creación de tablas y la inserción de datos de muestra.

## Creación de Tablas

### Tabla de Géneros
```sql
CREATE TABLE Genre (
    ID INT IDENTITY(1,1) PRIMARY KEY,
    Name VARCHAR(255) NOT NULL
);
```
Esta tabla almacena los géneros de películas. Cada género tiene un identificador único y un nombre.

### Tabla de Personas
```
CREATE TABLE Person (
    ID INT IDENTITY(1,1) PRIMARY KEY,
    FirstName VARCHAR(255),
    LastName VARCHAR(255),
    Birthdate DATE
);
```

Esta tabla guarda información de las personas, ya sean actores o directores, con nombres, apellidos y fechas de nacimiento.

### Tabla de Estudios
```
CREATE TABLE Studio (
    ID INT IDENTITY(1,1) PRIMARY KEY,
    Name VARCHAR(255),
    Location VARCHAR(255),
    Country VARCHAR(255)
);
```

Esta tabla representa los estudios de cine, incluyendo su nombre, ubicación y país.

### Tabla de Películas
```
CREATE TABLE Film (
    ID INT IDENTITY(1,1) PRIMARY KEY,
    Title VARCHAR(255),
    Year INT,
    GenreID INT,
    DirectorID INT,
    StudioID INT,
    Budget DECIMAL(15,2),
    FOREIGN KEY (GenreID) REFERENCES Genre(ID),
    FOREIGN KEY (DirectorID) REFERENCES Person(ID),
    FOREIGN KEY (StudioID) REFERENCES Studio(ID)
);
```

Esta tabla incluye detalles sobre las películas, como el título, año, género, director, estudio y presupuesto. Incluye claves foráneas que enlazan con las tablas de géneros, personas y estudios.

### Tabla de Roles
```
CREATE TABLE Role (
    ID INT IDENTITY(1,1) PRIMARY KEY,
    FilmID INT,
    PersonID INT,
    CharacterName VARCHAR(255),
    FOREIGN KEY (FilmID) REFERENCES Film(ID),
    FOREIGN KEY (PersonID) REFERENCES Person(ID)
);
```

Esta tabla relaciona a personas con películas específicas, indicando qué personaje interpretaron en cada film.

## Inserción de Datos de Ejemplo
### Insertar Géneros
```
INSERT INTO Genre (Name) VALUES ('Drama'), ('Comedia'), ('Ciencia Ficción'), ('Documental');
```

### Insertar Estudios
```
INSERT INTO Studio (Name, Location, Country) VALUES 
('Universal Pictures', '100 Universal City Plaza', 'USA'),
('Warner Bros', '4000 Warner Blvd', 'USA');
```

### Insertar Personas
```
INSERT INTO Person (FirstName, LastName, Birthdate) VALUES 
('Steven', 'Spielberg', '1946-12-18'),
('Tom', 'Hanks', '1956-07-09');
```

### Insertar Películas
```
INSERT INTO Film (Title, Year, GenreID, DirectorID, StudioID, Budget) VALUES 
('Bridge of Spies', 2015, 1, 1, 1, 40000000);
```

### Insertar Roles
```
INSERT INTO Role (FilmID, PersonID, CharacterName) VALUES 
(1, 2, 'James B. Donovan');
```

### Consulta de Información de Películas
```
-- Consulta para obtener detalles de una película y su director y actores
SELECT 
    f.Title, 
    f.Year, 
    g.Name AS Genre,
    p.FirstName + ' ' + p.LastName AS Director,
    s.Name AS Studio,
    r.CharacterName,
    act.FirstName + ' ' + act.LastName AS Actor
FROM Film f
JOIN Genre g ON f.GenreID = g.ID
JOIN Person p ON f.DirectorID = p.ID
JOIN Studio s ON f.StudioID = s.ID
JOIN Role r ON r.FilmID = f.ID
JOIN Person act ON r.PersonID = act.ID
WHERE f.Title = 'Bridge of Spies';
```

Este script SQL recupera información detallada sobre la película "Bridge of Spies", incluyendo el género, director, estudio, y los actores y sus personajes.
