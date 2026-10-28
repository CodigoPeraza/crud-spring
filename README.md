<div align="center">

# CRUD de usuarios con Spring Boot

**Aplicación web Full Stack para crear, listar, editar y eliminar usuarios, con Spring Boot, Thymeleaf y MariaDB en Docker.**

![Java](https://img.shields.io/badge/Java_17-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot_3.2.4-6DB33F?style=for-the-badge&logo=springboot&logoColor=white)
![Thymeleaf](https://img.shields.io/badge/Thymeleaf-005F0F?style=for-the-badge&logo=thymeleaf&logoColor=white)
![MariaDB](https://img.shields.io/badge/MariaDB-003545?style=for-the-badge&logo=mariadb&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Maven](https://img.shields.io/badge/Maven-C71A36?style=for-the-badge&logo=apachemaven&logoColor=white)

</div>

## Descripción

CRUD de usuarios construido con Spring Boot 3 y Java 17. La aplicación expone una interfaz web renderizada con Thymeleaf, persiste la información en MariaDB mediante Spring Data JPA y se ejecuta junto a la base de datos con Docker Compose.

El proyecto demuestra el flujo completo de una aplicación MVC: controlador, modelo, repositorio, plantillas y contenedores.

## Funcionalidades

- Listado de usuarios registrados.
- Creación de usuarios desde un formulario web.
- Edición de usuarios existentes.
- Eliminación de usuarios con confirmación en el navegador.
- Interfaz responsive con Bootstrap 5 servido mediante WebJars.
- Creación automática del esquema con Hibernate (`ddl-auto=update`).
- Endpoint de salud y base para métricas con Spring Boot Actuator.
- Script SQL de ejemplo para insertar un usuario de prueba.

## Stack técnico

| Capa | Tecnología |
| --- | --- |
| Lenguaje | Java 17 |
| Framework | Spring Boot 3.2.4 |
| Web | Spring MVC + Thymeleaf |
| Persistencia | Spring Data JPA + Hibernate |
| Base de datos | MariaDB |
| UI | Bootstrap 5 (WebJars) |
| Build | Maven Wrapper |
| Infraestructura | Docker + Docker Compose |
| Observabilidad | Spring Boot Actuator |

## Arquitectura

```text
src/main/java/com/pm18077/crud/crud/
├── CrudApplication.java
└── users/
    ├── Controllers/PageUserController.java   # Rutas y vistas
    ├── entities/User.java                    # Entidad JPA
    └── repository/UserRepository.java        # Acceso a datos

src/main/resources/
├── application.properties                    # Configuración de MariaDB
└── templates/
    ├── form-user.html                        # Formulario de alta/edición
    └── list-users.html                       # Tabla de usuarios
```

La aplicación sigue una separación por capas:

- **Controlador:** recibe las peticiones HTTP y prepara el modelo.
- **Repositorio:** extiende `JpaRepository` y abstrae las consultas.
- **Entidad:** mapea la tabla `users` con JPA.
- **Vistas:** plantillas Thymeleaf con Bootstrap.

## Endpoints

| Método | Ruta | Acción |
| --- | --- | --- |
| `GET` | `/users` | Lista todos los usuarios |
| `GET` | `/nuevo` | Muestra el formulario de creación |
| `POST` | `/nuevo` | Guarda un usuario |
| `GET` | `/editar/{id}` | Muestra el formulario de edición |
| `GET` | `/eliminar/{id}` | Elimina un usuario y redirige al listado |

La aplicación queda disponible en `http://localhost:8080`.

## Ejecución con Docker

El `Dockerfile` copia el JAR ya compilado, por lo que primero se debe generar el artefacto:

```bash
./mvnw clean package -DskipTests
docker compose up --build
```

Servicios levantados:

- **Aplicación:** `http://localhost:8080`
- **MariaDB:** `localhost:3305` (mapeado al puerto `3306` del contenedor)
- **Base de datos:** `mydatabase`
- **Usuario:** `usuario`
- **Contraseña:** `password`

Para detener los contenedores:

```bash
docker compose down
```

## Ejecución local

La configuración apunta al host `java_db`, nombre que Docker Compose resuelve dentro de la red de contenedores. Para ejecutar la aplicación fuera de Docker hay que cambiar `spring.datasource.url` en `src/main/resources/application.properties` por un host accesible, por ejemplo:

```properties
spring.datasource.url=jdbc:mariadb://localhost:3306/mydatabase
```

Después:

```bash
./mvnw spring-boot:run
```

## Datos de prueba

El archivo `pgdatabase.session.sql` contiene un `INSERT` de ejemplo:

```sql
INSERT INTO users (id, name)
VALUES (2, 'Pedro Sanchez');
```

La tabla `users` se crea automáticamente al iniciar la aplicación.

## Pruebas

```bash
./mvnw test
```

El proyecto incluye la prueba de contexto generada por Spring Boot, que verifica que la aplicación puede arrancar con su configuración.

## Decisiones técnicas destacadas

- Uso de **Spring Data JPA** para eliminar la necesidad de escribir consultas CRUD manuales.
- **Thymeleaf** para renderizar HTML en el servidor y mantener una integración natural con Spring MVC.
- **WebJars** para versionar Bootstrap desde Maven y evitar dependencias de CDN.
- **Docker Compose** para reproducir el entorno de aplicación y base de datos con un solo comando.
- **Maven Wrapper** para que el build no dependa de una instalación local de Maven.
- Separación clara entre controlador, repositorio, entidad y plantilla.

## Limitaciones y siguientes pasos

- Las credenciales de la base de datos están embebidas con fines educativos; en producción deberían moverse a variables de entorno o un gestor de secretos.
- El CRUD no incluye validación de formularios ni manejo centralizado de errores.
- La entidad `User` solo contiene `id` y `name`.
- Se puede ampliar con paginación, búsqueda, pruebas de integración y una API REST.

## Autor

**Francisco Javier Peraza** · [GitHub](https://github.com/CodigoPeraza) · [contacto.peraza@gmail.com](mailto:contacto.peraza@gmail.com)
