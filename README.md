CREATE TABLE Salones (
    SalonID INT PRIMARY KEY,
    Nombre VARCHAR(50) NOT NULL,
    Edificio VARCHAR(50) NOT NULL
);

INSERT INTO Salones (SalonID, Nombre, Edificio) VALUES
(1, 'Salón A', 'Edificio Principal'),
(2, 'Salón B', 'Edificio Principal'),
(3, 'Salón C', 'Edificio Secundario'),
(4, 'Salón D', 'Edificio Secundario');

CREATE TABLE Extintores (
    ExtintorID INT PRIMARY KEY,
    SalonID INT,
    Tipo VARCHAR(10),
    FOREIGN KEY (SalonID) REFERENCES Salones(SalonID)
);

INSERT INTO Extintores (ExtintorID, SalonID, Tipo) VALUES
(1, 1, 'Clase A'),
(2, 1, 'Clase B'),
(3, 1, 'Clase C'),
(4, 2, 'Clase A'),
(5, 2, 'Clase B'),
(6, 2, 'Clase C'),
(7, 3, 'Clase A'),
(8, 3, 'Clase B'),
(9, 3, 'Clase C'),
(10, 4, 'Clase A'),
(11, 4, 'Clase B'),
(12, 4, 'Clase C');

CREATE VIEW Vista_Salones AS
SELECT * FROM Salones;

CREATE VIEW Vista_Extintores AS
SELECT e.ExtintorID, s.Nombre AS Salon, e.Tipo
FROM Extintores e
INNER JOIN Salones s ON e.SalonID = s.SalonID;

SELECT s.Nombre, COUNT(e.ExtintorID) AS 'Cantidad de Extintores'
FROM Salones s
LEFT JOIN Extintores e ON s.SalonID = e.SalonID
GROUP BY s.SalonID, s.Nombre;

SELECT COUNT(*) AS 'Total de Extintores'
FROM Extintores;

SELECT s.Nombre, COUNT(e.ExtintorID) AS 'Cantidad de Extintores'
FROM Salones s
INNER JOIN Extintores e ON s.SalonID = e.SalonID
GROUP BY s.Nombre
HAVING COUNT(e.ExtintorID) = (
    SELECT COUNT(e2.ExtintorID)
    FROM Extintores e2
    INNER JOIN Salones s2 ON e2.SalonID = s2.SalonID
    GROUP BY s2.Nombre
    ORDER BY COUNT(e2.ExtintorID) DESC
    LIMIT 1
);
