# Librolink

Aplicación web para consultar un catálogo de libros y gestionar una librería. Integra registro de usuarios, carrito de compras y un panel administrativo con reportes en PDF.

## Funcionalidades

- Registro, inicio y cierre de sesión con perfiles de administrador y usuario.
- Catálogo, búsqueda de libros y vista de detalle.
- Carrito por usuario y confirmación de compras con registro de actividades.
- Administración de libros y categorías, con carga de imágenes.
- Consulta de usuarios y actividades desde el panel administrativo.
- Generación de un reporte general en PDF con JasperReports.

## Tecnologías

Java 17 · Spring Boot 3.5 · Spring MVC · Thymeleaf · Spring Data JPA · MySQL · Maven · Lombok · JasperReports

## Instalación y configuración

Requiere **JDK 17** y **MySQL**. El repositorio incluye Maven Wrapper; la interfaz y la lógica del servidor se ejecutan en una misma aplicación.

```bash
git clone https://github.com/thoonidev/Librolink.git
cd Librolink
```

Ajusta tus credenciales en `src/main/resources/application.properties`:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/librolink_sh?createDatabaseIfNotExist=true
spring.datasource.username=tu_usuario
spring.datasource.password=tu_password
server.port=8081
```

La base puede crearse automáticamente si el usuario de MySQL tiene permisos. Hibernate crea o actualiza las tablas al iniciar. No publiques tus credenciales reales.

## Ejecución

En Windows (PowerShell):

```powershell
.\mvnw.cmd spring-boot:run
```

En Linux o macOS:

```bash
./mvnw spring-boot:run
```

Después del primer inicio, agrega los roles necesarios en MySQL si aún no existen:

```sql
USE librolink_sh;
INSERT INTO roles (nombre)
SELECT 'ROLE_ADMIN'
WHERE NOT EXISTS (SELECT 1 FROM roles WHERE nombre = 'ROLE_ADMIN');
INSERT INTO roles (nombre)
SELECT 'ROLE_USER'
WHERE NOT EXISTS (SELECT 1 FROM roles WHERE nombre = 'ROLE_USER');
```

Abre `http://localhost:8081/registro`. **La primera cuenta registrada recibe el rol de administrador**; las siguientes reciben el rol de usuario. Desde el panel puedes crear categorías y libros para completar el catálogo.

Las imágenes cargadas se guardan en `uploads/img/`, que requiere permisos de escritura. El límite por archivo es de 10 MB.

## Comandos

| Comando (Windows) | Descripción |
| --- | --- |
| `.\mvnw.cmd test` | Ejecuta la prueba de carga del contexto de Spring. |
| `.\mvnw.cmd clean package` | Compila, ejecuta las pruebas y genera el JAR en `target/`. |

En Linux o macOS, sustituye `.\mvnw.cmd` por `./mvnw`. Las pruebas requieren la conexión a MySQL configurada.

## Alcance actual

La confirmación del carrito registra las compras como actividades; no integra una pasarela de pagos. La autenticación utiliza sesiones y actualmente guarda las contraseñas en texto plano, por lo que debe incorporarse almacenamiento seguro de contraseñas antes de utilizar cuentas reales.
