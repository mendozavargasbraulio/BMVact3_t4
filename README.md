# Actividad 3: CRUD con Spring Boot, JPA y MySQL

## 1. Descripción del proyecto

En esta actividad se desarrolló un sistema de gestión de empleados y departamentos utilizando Spring Boot, Spring Data JPA, MySQL y Thymeleaf.

El objetivo es implementar las operaciones CRUD (crear, consultar, actualizar y eliminar) sobre una base de datos real, estableciendo una relación entre las entidades Empleado y Departamento.


## 2. Tecnologías utilizadas

- Java 21: lenguaje de programación utilizado.
- Spring Boot 4.1.2: framework principal de la aplicación.
- Spring Web MVC: manejo de controladores y peticiones HTTP.
- Spring Data JPA: acceso a la base de datos mediante repositorios.
- Hibernate: mapeo de entidades Java a tablas de MySQL.
- MySQL 8: almacenamiento de empleados y departamentos.
- Thymeleaf: creación de páginas HTML dinámicas.
- Maven: administración de dependencias y compilación.
- Visual Studio Code: entorno de desarrollo.

## 3. Estructura y funcionamiento del proyecto

El sistema está organizado en diferentes capas para separar las responsabilidades.

### Entidades

Se implementaron dos entidades:

- Departamento: contiene el identificador, nombre y descripción.
- Empleado: contiene el identificador, nombre, apellido, puesto y departamento asignado.

La relación es de uno a muchos: un departamento puede tener varios empleados, pero cada empleado pertenece a un solo departamento.

Esta relación se implementó mediante las anotaciones `@OneToMany`, `@ManyToOne` y `@JoinColumn`.

La columna `departamento_id` funciona como llave foránea en la tabla empleados.

### Repository

Se crearon los repositorios `EmpleadoRepository` y `DepartamentoRepository`, ambos extendiendo `JpaRepository`.

Estos permiten realizar operaciones de consulta y persistencia de datos sin escribir manualmente las consultas SQL básicas.

### Service

Las clases `EmpleadoService` y `DepartamentoService` contienen la lógica necesaria para trabajar con las entidades.

En esta capa se realizan las operaciones de consulta, registro, actualización y eliminación de empleados, además de comprobar que el departamento asignado exista.

### Controller

Se implementaron los controladores `EmpleadoController` y `DepartamentoController`.

Estos reciben las peticiones HTTP, utilizan los servicios correspondientes y envían la información a las vistas Thymeleaf.

## 4. Operaciones CRUD

El sistema permite realizar las siguientes operaciones:

| Operación | Función |
|---|---|
| Crear | Registrar empleados y asignarles un departamento |
| Leer | Consultar los empleados registrados |
| Actualizar | Modificar la información de un empleado |
| Eliminar | Eliminar un empleado de la base de datos |

### Rutas implementadas

| Método | Endpoint | Función |
|---|---|---|
| GET | `/departamentos` | Mostrar departamentos |
| POST | `/departamentos/crear` | Registrar departamento |
| GET | `/empleados` | Consultar empleados |
| GET | `/empleados/nuevo` | Mostrar formulario de registro |
| POST | `/empleados/crear` | Guardar empleado |
| GET | `/empleados/editar/{id}` | Mostrar formulario de edición |
| POST | `/empleados/actualizar/{id}` | Actualizar empleado |
| POST | `/empleados/eliminar/{id}` | Eliminar empleado |

Las vistas se desarrollaron con Thymeleaf dentro del mismo proyecto Spring Boot.

Se utilizaron tres archivos HTML: `departamentos.html`, `empleados.html` y `empleado-form.html`.

El formulario de empleados se reutiliza tanto para registrar como para actualizar información.

## 5. Configuración de MySQL

Se creó la base de datos `bd_empleados_act3`, que contiene las tablas `departamentos` y `empleados`.

La conexión se configuró en `application.properties` mediante variables de entorno para evitar almacenar las credenciales directamente en el repositorio.

```properties
spring.application.name=demoCrud
server.port=8086

spring.datasource.url=jdbc:mysql://127.0.0.1:${DB_PORT:3306}/bd_empleados_act3
spring.datasource.username=${DB_USER}
spring.datasource.password=${DB_PASSWORD}

spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.thymeleaf.cache=false
```

La propiedad `spring.jpa.hibernate.ddl-auto=update` permite actualizar el esquema de la base de datos de acuerdo con las entidades JPA.

Durante el desarrollo local se utilizó un túnel SSH para conectar la aplicación de Windows con MySQL del VPS.

Se verificó la relación entre las tablas y el almacenamiento de los datos en MySQL.

## 6. Compilación y despliegue

La aplicación se compiló mediante Maven para generar el archivo ejecutable JAR.

```powershell
.\mvnw.cmd package -DskipTests
```

El archivo generado fue:

`demoCrud-0.0.1-SNAPSHOT.jar`

Posteriormente se transfirió al VPS mediante SCP, en una carpeta independiente de las actividades anteriores:

`/opt/springboot-8086`

La aplicación se ejecutó utilizando Java 21, las variables de entorno para MySQL y el puerto 8086.

Se configuró el firewall para permitir conexiones TCP en dicho puerto.

Las actividades anteriores utilizan los puertos 8082 y 8084, por lo que se mantienen independientes.

## 7. Enlaces del proyecto

### Repositorio de GitHub

https://github.com/mendozavargasbraulio/BMVact3_t4

### Aplicación en el VPS

Gestión de departamentos:

http://13.140.209.186:8086/departamentos

Listado de empleados:

http://13.140.209.186:8086/empleados

Registro de empleados:

http://13.140.209.186:8086/empleados/nuevo

## 8. Capturas de evidencia

### Actualización de empleados

![Actualización de empleados](img/EditEmple.png)

### Eliminación de empleados

![Eliminación de empleados](img/ElimEmple.png)

### Relación entre entidades en MySQL

![Relación de empleados y departamentos](img/RelacionBD.png)

### Registro de departamentos

![Registro de departamentos](img/RegDepa.png)

### Registro de empleados


![Registro de empleados](img/RegEmple.png)

### Gestión de empleados


![Gestión de empleados](img/GesEmple.png)

