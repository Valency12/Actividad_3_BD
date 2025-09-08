# 🚗 Base de Datos para Concesionaria de Autos

Este proyecto implementa el **modelo lógico** y las consultas de una base de datos para la administración de una asociación de dueños de lotes de autos.  
La práctica incluye creación de tablas, inserción de datos de ejemplo, simulaciones de ventas y consultas SQL.

---

## 📊 Modelo Lógico

### Entidades principales
- **Concesionario**  
- **Servicio Oficial**  
- **Marca**  
- **Modelo**  
- **Equipamiento**  
- **Automóvil (Stock)**  
- **Vendedor**  
- **Venta**  
- **Venta_Extras**  
- **Ventas_Acumuladas**

### Relaciones destacadas
- Un **concesionario** tiene varios **servicios oficiales**.  
- Un **concesionario** tiene varios **vendedores**.  
- Una **marca** tiene varios **modelos**.  
- Un **modelo** tiene muchos **equipamientos** (serie o extras).  
- Un **automóvil** pertenece a un modelo y puede estar en stock en concesionario, servicio oficial o almacén.  
- Una **venta** puede realizarla un vendedor o un servicio oficial.  
- Una **venta** puede incluir varios **extras**.  
- Se registran las ventas en **Ventas_Acumuladas** para simulaciones.  

---<img width="1121" height="851" alt="Carros drawio" src="https://github.com/user-attachments/assets/847b3a6b-523f-4f89-947a-47dc73509e15" />


https://drive.google.com/file/d/152to3lSJI83BuEoRLsmgYkXQXYKMEmxT/view?usp=sharing

## 🛠️ Script SQL

### 1. Creación de tablas
<img width="660" height="387" alt="Captura de pantalla 2025-09-07 175024" src="https://github.com/user-attachments/assets/4c784179-daf5-4aca-a83c-e9cc2b05b59a" />

<img width="761" height="578" alt="Captura de pantalla 2025-09-07 175314" src="https://github.com/user-attachments/assets/ac26cdb3-0656-4a75-81f6-766492996f75" />

```sql
CREATE TABLE Concesionario (
  id_concesionario INT AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  domicilio VARCHAR(200) NOT NULL,
  telefono VARCHAR(15)
);
SELECT * FROM Concesionario;
CREATE TABLE Servicio_Oficial (
  id_servicio INT AUTO_INCREMENT PRIMARY KEY,
  id_concesionario INT,
  nombre VARCHAR(100) NOT NULL,
  domicilio VARCHAR(200) NOT NULL,
  telefono VARCHAR(15),
  FOREIGN KEY (id_concesionario) REFERENCES Concesionario(id_concesionario)
);

CREATE TABLE Marca (
  id_marca INT AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(50) NOT NULL
);

CREATE TABLE Modelo (
  id_modelo INT AUTO_INCREMENT PRIMARY KEY,
  id_marca INT,
  nombre VARCHAR(50) NOT NULL,
  anio YEAR NOT NULL,
  precio_base DECIMAL(12,2) NOT NULL,
  descuento DECIMAL(12,2) DEFAULT 0,
  tipo_motor VARCHAR(50),
  potencia VARCHAR(50),
  cilindros TINYINT,
  FOREIGN KEY (id_marca) REFERENCES Marca(id_marca)
);

CREATE TABLE Equipamiento (
  id_equipo INT AUTO_INCREMENT PRIMARY KEY,
  nombre VARCHAR(100) NOT NULL,
  precio DECIMAL(12,2) DEFAULT 0,
  tipo ENUM('Serie','Extra') NOT NULL
);

CREATE TABLE Modelo_Equipamiento (
  id_modelo INT,
  id_equipo INT,
  PRIMARY KEY (id_modelo, id_equipo),
  FOREIGN KEY (id_modelo) REFERENCES Modelo(id_modelo),
  FOREIGN KEY (id_equipo) REFERENCES Equipamiento(id_equipo)
);

CREATE TABLE Automovil (
  num_serie VARCHAR(30) PRIMARY KEY,
  id_modelo INT,
  ubicacion ENUM('Concesionario','Servicio Oficial','Almacen'),
  id_concesionario INT NULL,
  id_servicio INT NULL,
  disponible BOOLEAN DEFAULT TRUE,
  FOREIGN KEY (id_modelo) REFERENCES Modelo(id_modelo),
  FOREIGN KEY (id_concesionario) REFERENCES Concesionario(id_concesionario),
  FOREIGN KEY (id_servicio) REFERENCES Servicio_Oficial(id_servicio)
);

CREATE TABLE Vendedor (
  id_vendedor INT AUTO_INCREMENT PRIMARY KEY,
  id_concesionario INT,
  nombre VARCHAR(100) NOT NULL,
  domicilio VARCHAR(200),
  telefono VARCHAR(15),
  whatsapp VARCHAR(15),
  FOREIGN KEY (id_concesionario) REFERENCES Concesionario(id_concesionario)
);

CREATE TABLE Venta (
  id_venta INT AUTO_INCREMENT PRIMARY KEY,
  num_serie VARCHAR(30),
  id_vendedor INT NULL,
  id_servicio INT NULL,
  fecha DATE NOT NULL,
  precio_final DECIMAL(12,2) NOT NULL,
  modo_pago ENUM('Contado','Credito'),
  matricula VARCHAR(15),
  stock BOOLEAN,
  encargo_fabrica BOOLEAN,
  FOREIGN KEY (num_serie) REFERENCES Automovil(num_serie),
  FOREIGN KEY (id_vendedor) REFERENCES Vendedor(id_vendedor),
  FOREIGN KEY (id_servicio) REFERENCES Servicio_Oficial(id_servicio)
);

CREATE TABLE Venta_Extras (
  id_venta INT,
  id_equipo INT,
  precio_extra DECIMAL(12,2),
  PRIMARY KEY (id_venta, id_equipo),
  FOREIGN KEY (id_venta) REFERENCES Venta(id_venta),
  FOREIGN KEY (id_equipo) REFERENCES Equipamiento(id_equipo)
);

CREATE TABLE Ventas_Acumuladas (
  id_acumulado INT AUTO_INCREMENT PRIMARY KEY,
  id_venta INT,
  fecha DATE NOT NULL,
  FOREIGN KEY (id_venta) REFERENCES Venta(id_venta)
);


```
<img width="806" height="580" alt="Captura de pantalla 2025-09-07 175537" src="https://github.com/user-attachments/assets/955740d4-97dc-41e8-a62d-7dd1723b9b99" />
<img width="805" height="544" alt="Captura de pantalla 2025-09-07 175707" src="https://github.com/user-attachments/assets/8dbf07e6-6dde-463e-acc9-0340b2a623ad" />
<img width="747" height="572" alt="Captura de pantalla 2025-09-07 175759" src="https://github.com/user-attachments/assets/ceda792c-0d10-498b-b9af-943dc65f8f28" />
<img width="774" height="568" alt="Captura de pantalla 2025-09-07 175856" src="https://github.com/user-attachments/assets/c64e2871-0ff1-4b72-afcb-42ccf59042a9" />
<img width="737" height="521" alt="Captura de pantalla 2025-09-07 175955" src="https://github.com/user-attachments/assets/84183884-af72-4487-9280-0d9706538dd7" />
<img width="811" height="569" alt="Captura de pantalla 2025-09-07 180057" src="https://github.com/user-attachments/assets/8aab88b0-b3f1-4d4b-b143-65ad0070b0d0" />
<img width="824" height="556" alt="Captura de pantalla 2025-09-07 180138" src="https://github.com/user-attachments/assets/fe5cd382-322d-4333-873c-a31e1d8f4f44" />
<img width="725" height="563" alt="Captura de pantalla 2025-09-07 180228" src="https://github.com/user-attachments/assets/9d4a2d5c-e57a-42e2-b81c-1e14d732fe48" />
<img width="752" height="574" alt="Captura de pantalla 2025-09-07 180310" src="https://github.com/user-attachments/assets/8e472b3f-5b77-4fee-a062-8933ea2b3909" />
<img width="679" height="528" alt="Captura de pantalla 2025-09-07 180357" src="https://github.com/user-attachments/assets/4082d50f-2ab2-47a6-8af9-8f1cf5b80eed" />



### 2. Insercion de datos de ejemplo
```
INSERT INTO Concesionario (nombre, domicilio, telefono) VALUES
('Concesionaria López', 'Av. Reforma 123, CDMX', '5551234567'),
('Concesionaria Autos del Sur', 'Av. Juárez 456, Puebla', '2226543210'),
('Concesionaria Norte', 'Calle Hidalgo 789, Monterrey', '818998877'),
('Concesionaria Premium', 'Blvd. Las Torres 555, Guadalajara', '333112233'),
('Concesionaria Centro', 'Av. Central 120, Toluca', '722543210'),
('Concesionaria Rivera', 'Calle Marina 66, Cancún', '998334455'),
('Concesionaria Gómez', 'Av. Insurgentes 444, CDMX', '555222111'),
('Concesionaria Autos Express', 'Calz. Tlalpan 89, CDMX', '555667788'),
('Concesionaria Oriental', 'Av. Universidad 33, Puebla', '222112233'),
('Concesionaria Nacional', 'Carretera Nacional 900, Monterrey', '818556677');
SELECT * FROM Concesionario;

INSERT INTO Servicio_Oficial (id_concesionario, nombre, domicilio, telefono) VALUES
(1, 'Servicio Oficial Reforma', 'Av. Reforma 200, CDMX', '555777999'),
(1, 'Servicio Oficial Roma', 'Calle Colima 123, CDMX', '555888999'),
(2, 'Servicio Oficial Puebla', 'Av. Zaragoza 555, Puebla', '222333444'),
(3, 'Servicio Oficial Norte', 'Av. Universidad 700, Monterrey', '818111222'),
(3, 'Servicio Oficial San Pedro', 'Calle Vasconcelos 90, Monterrey', '818333444'),
(4, 'Servicio Oficial GDL', 'Blvd. Independencia 88, Guadalajara', '333445566'),
(5, 'Servicio Oficial Toluca', 'Av. Morelos 50, Toluca', '722112233'),
(6, 'Servicio Oficial Cancún', 'Zona Hotelera 12, Cancún', '998667788'),
(7, 'Servicio Oficial Insurgentes', 'Av. Insurgentes Sur 123, CDMX', '555223344'),
(8, 'Servicio Oficial Express', 'Calz. Taxqueña 55, CDMX', '555998877');
SELECT * FROM Servicio_Oficial;

INSERT INTO Marca (nombre) VALUES
('Audi'),
('Suzuki'),
('Honda'),
('Volkswagen'),
('Toyota'),
('Ford'),
('Chevrolet'),
('Mazda'),
('BMW'),
('Kia');
SELECT * FROM Marca;

INSERT INTO Modelo (id_marca, nombre, anio, precio_base, descuento, tipo_motor, potencia, cilindros) VALUES
(1, 'Audi A3', 2022, 550000, 20000, 'Gasolina', '190 HP', 4),
(1, 'Audi Q5', 2023, 850000, 0, 'Híbrido', '250 HP', 6),
(2, 'Suzuki Swift', 2021, 300000, 10000, 'Gasolina', '120 HP', 4),
(2, 'Suzuki Vitara', 2022, 420000, 0, 'Gasolina', '140 HP', 4),
(3, 'Honda Civic', 2020, 400000, 15000, 'Gasolina', '160 HP', 4),
(3, 'Honda CR-V', 2023, 600000, 20000, 'Híbrido', '190 HP', 4),
(4, 'VW Jetta', 2019, 350000, 5000, 'Gasolina', '150 HP', 4),
(4, 'VW Tiguan', 2022, 500000, 10000, 'Gasolina', '180 HP', 4),
(5, 'Toyota Corolla', 2023, 450000, 0, 'Gasolina', '170 HP', 4),
(6, 'Ford Ranger', 2021, 700000, 25000, 'Diesel', '210 HP', 6);
SELECT * FROM Modelo;

INSERT INTO Equipamiento (nombre, precio, tipo) VALUES
('Airbag conductor', 0, 'Serie'),
('Cierre automático', 0, 'Serie'),
('Aire acondicionado', 10000, 'Extra'),
('Airbag acompañante', 12000, 'Extra'),
('Pintura metalizada', 8000, 'Extra'),
('Quemacocos', 15000, 'Extra'),
('Accesorios externos', 5000, 'Extra'),
('Pantalla táctil', 0, 'Serie'),
('Cámara trasera', 9000, 'Extra'),
('GPS integrado', 11000, 'Extra');
SELECT * FROM Equipamiento;

INSERT INTO Modelo_Equipamiento (id_modelo, id_equipo) VALUES
(1,1),(1,2),(1,8),
(2,1),(2,2),(2,3),(2,9),
(3,1),(3,2),(3,3),
(4,1),(4,2),(4,4),
(5,1),(5,2),(5,5),
(6,1),(6,2),(6,6),
(7,1),(7,2),(7,7),
(8,1),(8,2),(8,8),
(9,1),(9,2),(9,9),
(10,1),(10,2),(10,10);

INSERT INTO Automovil (num_serie, id_modelo, ubicacion, id_concesionario, id_servicio, disponible) VALUES
('AUDI-A3-001', 1, 'Concesionario', 1, NULL, TRUE),
('AUDI-Q5-001', 2, 'Concesionario', 1, NULL, TRUE),
('SUZU-SWIFT-001', 3, 'Concesionario', 2, NULL, TRUE),
('SUZU-VITA-001', 4, 'Servicio Oficial', NULL, 3, TRUE),
('HON-CIVIC-001', 5, 'Concesionario', 3, NULL, TRUE),
('HON-CRV-001', 6, 'Concesionario', 3, NULL, TRUE),
('VW-JETTA-001', 7, 'Servicio Oficial', NULL, 4, TRUE),
('VW-TIGUAN-001', 8, 'Concesionario', 4, NULL, TRUE),
('TOY-COROL-001', 9, 'Almacen', NULL, NULL, TRUE),
('FORD-RANG-001', 10, 'Concesionario', 5, NULL, TRUE);
SELECT * FROM Automovil;

INSERT INTO Vendedor (id_concesionario, nombre, domicilio, telefono, whatsapp) VALUES
(1, 'Luis Pérez', 'CDMX Centro 12', '555111222', '555333444'),
(1, 'María López', 'Roma Norte 22', '555555666', '555777888'),
(2, 'Carlos García', 'Puebla Centro 33', '222444555', '222666777'),
(3, 'Ana Torres', 'Monterrey Centro 44', '818222333', '818444555'),
(3, 'José Díaz', 'San Pedro 55', '818666777', '818888999'),
(4, 'Laura Sánchez', 'GDL 66', '333111222', '333333444'),
(5, 'Pedro Ramírez', 'Toluca 77', '722555666', '722777888'),
(6, 'Sofía Herrera', 'Cancún 88', '998111222', '998333444'),
(7, 'Raúl Morales', 'CDMX Sur 99', '555888777', '555444333'),
(8, 'Andrea Jiménez', 'CDMX Oriente 11', '555222333', '555666999');
SELECT * FROM Vendedor;

INSERT INTO Venta (num_serie, id_vendedor, id_servicio, fecha, precio_final, modo_pago, matricula, stock, encargo_fabrica) VALUES
('AUDI-A3-001', 1, NULL, '2023-06-15', 540000, 'Contado', 'ABC123', TRUE, FALSE),
('AUDI-Q5-001', 2, NULL, '2023-07-20', 830000, 'Credito', 'XYZ987', TRUE, FALSE),
('SUZU-SWIFT-001', 3, NULL, '2023-08-10', 295000, 'Contado', 'JKL456', TRUE, FALSE),
('SUZU-VITA-001', NULL, 3, '2023-09-05', 420000, 'Credito', 'MNO789', TRUE, FALSE),
('HON-CIVIC-001', 4, NULL, '2023-10-01', 385000, 'Contado', 'PQR321', TRUE, FALSE),
('HON-CRV-001', 5, NULL, '2023-11-11', 580000, 'Credito', 'TUV654', TRUE, FALSE),
('VW-JETTA-001', NULL, 4, '2023-12-02', 340000, 'Contado', 'GHJ852', TRUE, FALSE),
('VW-TIGUAN-001', 6, NULL, '2024-01-14', 490000, 'Credito', 'LMN963', TRUE, FALSE),
('TOY-COROL-001', 7, NULL, '2024-02-10', 450000, 'Contado', 'QWE741', TRUE, FALSE),
('FORD-RANG-001', 8, NULL, '2024-03-18', 680000, 'Credito', 'RTY159', TRUE, FALSE);
SELECT * FROM Venta;

INSERT INTO Venta_Extras (id_venta, id_equipo, precio_extra) VALUES
(1, 3, 10000),
(1, 5, 8000),
(2, 6, 15000),
(3, 7, 5000),
(4, 9, 9000),
(5, 10, 11000),
(6, 4, 12000),
(7, 5, 8000),
(8, 6, 15000),
(9, 7, 5000);
SELECT * FROM Venta_Extras;

```
### 3. Simulacion de 25 Ventas acumuladas
```
INSERT INTO Ventas_Acumuladas (id_venta, fecha) VALUES
(1, '2023-06-15'),
(2, '2023-07-20'),
(3, '2023-08-10'),
(4, '2023-09-05'),
(5, '2023-10-01'),
(6, '2023-11-11'),
(7, '2023-12-02'),
(8, '2024-01-14'),
(9, '2024-02-10'),
(10, '2024-03-18'),
-- Simulaciones adicionales (ligadas a las mismas ventas pero en otras fechas)
(1, '2024-04-02'),
(2, '2024-04-10'),
(3, '2024-04-15'),
(4, '2024-04-20'),
(5, '2024-05-01'),
(6, '2024-05-05'),
(7, '2024-05-12'),
(8, '2024-05-18'),
(9, '2024-06-01'),
(10, '2024-06-10'),
(1, '2024-06-15'),
(2, '2024-06-20'),
(3, '2024-07-01'),
(4, '2024-07-10'),
(5, '2024-07-20');

```
### 4. Consultas SQL Pedidas
```
SELECT v.id_venta, v.fecha, v.precio_final, c.nombre AS concesionaria
FROM Venta v
JOIN Automovil a ON v.num_serie = a.num_serie
JOIN Concesionario c ON a.id_concesionario = c.id_concesionario
WHERE c.id_concesionario = 1
LIMIT 5;

```
### 5. Ventas mayores a $150,000 ordenadas con ORDER BY
```
SELECT v.id_venta, v.fecha, v.precio_final, c.nombre AS concesionaria
FROM Venta v
JOIN Automovil a ON v.num_serie = a.num_serie
JOIN Concesionario c ON a.id_concesionario = c.id_concesionario
WHERE v.precio_final > 150000
ORDER BY v.precio_final DESC;

```
### 6. Modificacion de nombres de concesionarias 
```
UPDATE Concesionario
SET nombre = CONCAT(nombre, '-Nacional');
```
### 7. Eliminar modelos anteriores al año 2020 (DELETE)
```
DELETE FROM Modelo
WHERE CAST(anio AS UNSIGNED) < 2020;
```
### 8. Eliminacion de tabla (solo borrador, no ejecutar)
```
-- DROP TABLE Ventas_Acumuladas;
-- DROP TABLE Venta_Extra;
-- DROP TABLE Venta;
-- DROP TABLE Automovil;
-- DROP TABLE Equipamiento;
-- DROP TABLE Modelo;
-- DROP TABLE Marca;
-- DROP TABLE Vendedor;
-- DROP TABLE Servicio_Oficial;
-- DROP TABLE Concesionario;
```

## Reflexion
El uso de bases de datos relacionales permite organizar de forma clara y eficiente la información de un concesionario:

-Cardinalidades definen correctamente cómo se relacionan los concesionarios, vendedores, modelos y ventas.
-Consultas SQL como LIMIT, ORDER BY y DELETE permiten obtener información específica, ordenada y mantener los datos actualizados.
-Llaves primarias y foráneas aseguran la integridad de la información, evitando duplicados y garantizando la coherencia entre entidades.

Los conceptos de recursividad de datos y divide y vencerás aplicados en SQL permiten manejar consultas complejas de manera eficiente y escalable, facilitando la toma de decisiones en el negocio.

