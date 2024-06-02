# Tarea_11
## Ejercicios Propuestos
#### 1. Consulta de Proyectos

SELECT IDProyecto, NombreProyecto
FROM Proyecto;

#### 2. Consulta de Proyectos por Ubicación

SELECT IDProyecto, NombreProyecto
FROM Proyecto
WHERE Ubicacion = 'CHICAGO';

-- 3. Consulta de Proyectos por Departamento
SELECT IDProyecto, NombreProyecto
FROM Proyecto
WHERE IDDepartamento = 2;

-- 4. Consulta de Proyectos y Departamentos
SELECT Proyecto.NombreProyecto, Proyecto.Ubicacion, Departamento.NombreDepartamento
FROM Proyecto
JOIN Departamento ON Proyecto.IDDepartamento = Departamento.IDDepartamento;

-- 5. Consulta de Empleados por Proyecto
SELECT Empleado.IDEmpleado, Empleado.NombreEmpleado
FROM EmpleadoProyecto
JOIN Empleado ON EmpleadoProyecto.IDEmpleado = Empleado.IDEmpleado
WHERE EmpleadoProyecto.IDProyecto = 4;

-- 6. Consulta de Proyectos por Empleado
SELECT Proyecto.IDProyecto, Proyecto.NombreProyecto
FROM EmpleadoProyecto
JOIN Proyecto ON EmpleadoProyecto.IDProyecto = Proyecto.IDProyecto
WHERE EmpleadoProyecto.IDEmpleado = 4;

-- 7. Consulta de Horas Trabajadas por Proyecto
SELECT SUM(HorasTrabajadas) AS TotalHoras
FROM EmpleadoProyecto
WHERE IDProyecto = 2;

-- 8. Consulta de Empleados con Horas Trabajadas
SELECT Empleado.IDEmpleado, Empleado.NombreEmpleado
FROM EmpleadoProyecto
JOIN Empleado ON EmpleadoProyecto.IDEmpleado = Empleado.IDEmpleado
WHERE EmpleadoProyecto.IDProyecto = 2
  AND EmpleadoProyecto.HorasTrabajadas > 10;

-- 9. Consulta de Total de Horas por Empleado
SELECT Empleado.IDEmpleado, Empleado.NombreEmpleado, SUM(EmpleadoProyecto.HorasTrabajadas) AS TotalHoras
FROM Empleado
JOIN EmpleadoProyecto ON Empleado.IDEmpleado = EmpleadoProyecto.IDEmpleado
GROUP BY Empleado.IDEmpleado, Empleado.NombreEmpleado;

-- 10. Consulta de Empleados con Múltiples Proyectos
SELECT Empleado.IDEmpleado, Empleado.NombreEmpleado, COUNT(EmpleadoProyecto.IDProyecto) AS NumeroProyectos
FROM Empleado
JOIN EmpleadoProyecto ON Empleado.IDEmpleado = EmpleadoProyecto.IDEmpleado
GROUP BY Empleado.IDEmpleado, Empleado.NombreEmpleado
HAVING NumeroProyectos > 1;

-- 11. Consulta de Empleados y Horas Totales
SELECT Empleado.IDEmpleado, Empleado.NombreEmpleado, SUM(EmpleadoProyecto.HorasTrabajadas) AS TotalHoras
FROM Empleado
JOIN EmpleadoProyecto ON Empleado.IDEmpleado = EmpleadoProyecto.IDEmpleado
GROUP BY Empleado.IDEmpleado, Empleado.NombreEmpleado
HAVING TotalHoras > 30;

-- 12. Consulta de Proyectos y Horas Promedio
SELECT Proyecto.IDProyecto, Proyecto.NombreProyecto, AVG(EmpleadoProyecto.HorasTrabajadas) AS HorasPromedio
FROM Proyecto
JOIN EmpleadoProyecto ON Proyecto.IDProyecto = EmpleadoProyecto.IDProyecto
GROUP BY Proyecto.IDProyecto, Proyecto.NombreProyecto;

## Consultas Avanzadas
-- 1: Empleados en Proyectos Específicos y con Salario Alto
SELECT DISTINCT Empleado.IDEmpleado, Empleado.NombreEmpleado
FROM Empleado
JOIN EmpleadoProyecto ON Empleado.IDEmpleado = EmpleadoProyecto.IDEmpleado
JOIN Proyecto ON EmpleadoProyecto.IDProyecto = Proyecto.IDProyecto
WHERE Proyecto.Ubicacion = 'CHICAGO'
  AND (Empleado.Salario + COALESCE(Empleado.Comision, 0)) > 2000;

-- 2: Empleados con Jefe y en Proyectos Múltiples
SELECT Empleado.IDEmpleado, Empleado.NombreEmpleado, SUM(EmpleadoProyecto.HorasTrabajadas) AS TotalHoras, COUNT(EmpleadoProyecto.IDProyecto) AS NumeroProyectos
FROM Empleado
JOIN EmpleadoProyecto ON Empleado.IDEmpleado = EmpleadoProyecto.IDEmpleado
WHERE Empleado.IDJefe IS NOT NULL
GROUP BY Empleado.IDEmpleado, Empleado.NombreEmpleado
HAVING NumeroProyectos > 1 AND TotalHoras > 15;

-- 3: Empleados sin Comisión en Departamentos Específicos
SELECT Empleado.IDEmpleado, Empleado.NombreEmpleado
FROM Empleado
JOIN Departamento ON Empleado.IDDepartamento = Departamento.IDDepartamento
WHERE Empleado.Comision IS NULL
  AND (Departamento.Ubicacion = 'DALLAS' OR Departamento.Ubicacion = 'NEW YORK');
