# B2---Trabajo-Consulta-2-Conexi-n-base-de-datos-relacional

##¿Qué es JDBC y cuáles son sus componentes?

### ¿Qué es JDBC?  

**JDBC (Java Database Connectivity)** es una API de Java que permite a las aplicaciones interactuar con bases de datos relacionales. JDBC proporciona un conjunto de clases e interfaces que hacen posible ejecutar consultas SQL, recuperar resultados y gestionar la conexión con las bases de datos de manera eficiente.  

Es una herramienta esencial en el desarrollo de aplicaciones que requieren acceso a bases de datos, ofreciendo independencia entre la aplicación y el sistema de gestión de bases de datos (DBMS).  

---

### **Componentes principales de JDBC**  

1. **Driver Manager**  
   - Es el componente que gestiona los controladores (drivers) de base de datos.  
   - Carga el controlador adecuado en tiempo de ejecución para establecer la conexión entre la aplicación y la base de datos.  
   - Proporciona el método `getConnection()` para conectarse a la base de datos.  

2. **Driver**  
   - Es una implementación específica de un controlador que permite la comunicación con un tipo de base de datos (por ejemplo, MySQL, Oracle, PostgreSQL).  
   - Traduce las llamadas de la API JDBC en instrucciones que entiende la base de datos.  

3. **Connection**  
   - Representa una conexión activa con la base de datos.  
   - Proporciona métodos para gestionar transacciones y crear objetos para ejecutar consultas, como `Statement` y `PreparedStatement`.  

4. **Statement**  
   - Permite enviar consultas SQL a la base de datos.  
   - Tipos de declaraciones:  
     - `Statement`: Para consultas simples.  
     - `PreparedStatement`: Para consultas precompiladas y seguras contra inyecciones SQL.  
     - `CallableStatement`: Para ejecutar procedimientos almacenados.  

5. **ResultSet**  
   - Representa el conjunto de resultados obtenidos tras ejecutar una consulta SQL.  
   - Permite recorrer los datos fila por fila mediante métodos como `next()`.  

6. **SQLException**  
   - Maneja los errores que ocurren durante la interacción con la base de datos.  
   - Proporciona información detallada sobre el error (código, estado y mensaje).  

---

### **Flujo básico de JDBC**  

1. Cargar el driver de la base de datos.  
2. Establecer una conexión usando `DriverManager`.  
3. Crear un objeto `Statement` o `PreparedStatement` para enviar consultas.  
4. Ejecutar la consulta y procesar los resultados con `ResultSet`.  
5. Cerrar la conexión, el `Statement` y el `ResultSet` para liberar recursos.  

---

## Documente 2 librerías de Scala que permitan conectarse a una base de datos relacional. En una tabla resuma sus diferencias.

### Librerías de Scala para Conexión a Bases de Datos Relacionales  

Scala cuenta con varias librerías para interactuar con bases de datos relacionales de manera eficiente. Dos de las más utilizadas son **Slick** y **Doobie**.  

---

#### **1. Slick**  
**Slick** (Scala Language-Integrated Connection Kit) es una librería que combina consultas SQL con el paradigma funcional y un enfoque orientado a objetos. Es ampliamente utilizada por su capacidad de generar consultas SQL desde un modelo Scala.  

Características principales:  
- Ofrece un DSL (Domain-Specific Language) para escribir consultas de manera segura y legible.  
- Soporta tanto consultas asincrónicas como sincrónicas.  
- Permite integración directa con frameworks como Play y Akka.  

---

#### **2. Doobie**  
**Doobie** es una librería funcional pura que utiliza la potencia de **cats** y **cats-effect** para la interacción con bases de datos. Su diseño promueve la programación funcional, haciendo que las conexiones, consultas y transacciones sean seguras y componibles.  

Características principales:  
- Totalmente funcional, basada en `cats` y `cats-effect`.  
- Ofrece un control explícito de transacciones y recursos.  
- Soporta pruebas unitarias simplificadas mediante el uso de datos controlados.  

---

### Comparación entre Slick y Doobie  

| **Característica**           | **Slick**                                                                 | **Doobie**                                                       |
|------------------------------|---------------------------------------------------------------------------|------------------------------------------------------------------|
| **Paradigma**                | Mixto: Orientado a objetos y funcional.                                   | Completamente funcional.                                         |
| **Curva de aprendizaje**     | Moderada: Familiaridad con el DSL de consultas es necesaria.              | Más pronunciada: Requiere conocimientos en `cats` y programación funcional. |
| **Consultas SQL**            | Generación automática a partir de modelos Scala o DSL.                   | Consultas SQL explícitas (strings escritos por el desarrollador). |
| **Manejo de recursos**       | Simplificado, menos control explícito por parte del desarrollador.        | Manejo explícito de recursos mediante `cats-effect`.             |
| **Integración con frameworks**| Excelente integración con Play Framework y Akka.                         | Compatible con frameworks funcionales como ZIO o Http4s.         |
| **Asincronía**               | Soporte total usando `Future` o `DBIO`.                                   | Basada en `cats-effect` para asincronía pura y segura.           |
| **Tipo de transacciones**    | Soporte de transacciones implícitas.                                      | Transacciones explícitas y seguras.                              |

---

# CAPTURAS DE PANTALLA
### Creación de la base de datos y la tabla de prueba 
![Captura de pantalla 2025-01-22 190858](https://github.com/user-attachments/assets/3cb1a1c3-e853-49f4-973a-1a0a6c1605a3)

### Conección de la base de datos con scala en Intellij
![Captura de pantalla 2025-01-22 191005](https://github.com/user-attachments/assets/bd22a64c-fadb-465d-b5f2-fd5d926468b0)

### Código de la connecion de la base de datos con scala 

``` Scala
import java.sql.{Connection, DriverManager, ResultSet}

object DatabaseExample {
  def main(args: Array[String]): Unit = {
    // Establecer la conexión con la base de datos
    val url = "jdbc:mysql://localhost:3306/prueba"
    val username = "root"
    val password = "Jean1105727992"

    var connection: Connection = null

    try {
      // Cargar el controlador de MySQL
      Class.forName("com.mysql.cj.jdbc.Driver")

      // Conectar a la base de datos
      connection = DriverManager.getConnection(url, username, password)

      // Crear la consulta SQL
      val query = "SELECT * FROM usuarios"

      // Ejecutar la consulta
      val statement = connection.createStatement()
      val resultSet: ResultSet = statement.executeQuery(query)

      // Iterar sobre los resultados y mostrarlos
      while (resultSet.next()) {
        val id = resultSet.getInt("id")
        val nombre = resultSet.getString("nombre")
        val email = resultSet.getString("email")
        val edad = resultSet.getInt("edad")
        val fechaRegistro = resultSet.getDate("fecha_registro")

        println(s"ID: $id, Nombre: $nombre, Email: $email, Edad: $edad, Fecha Registro: $fechaRegistro")
      }

    } catch {
      case e: Exception => e.printStackTrace()
    } finally {
      if (connection != null) {
        connection.close()
      }
    }
  }
}

```

### Consulta desde scala a la base de datos
![Captura de pantalla 2025-01-22 191517](https://github.com/user-attachments/assets/f63212c4-4f7a-4003-8bce-b8fc598d278d)

### Resultado 
![Captura de pantalla 2025-01-22 191547](https://github.com/user-attachments/assets/3bd414ce-84e7-4ad8-bac7-4b5fb3f66423)

## BIBLIOGRAFIA 
## Bibliografía

1. **Slick Documentation**  
   "Slick: Functional Relational Mapping for Scala"  
   [https://slick.lightbend.com/](https://slick.lightbend.com/)

2. **Doobie Documentation**  
   "Doobie: A Functional Database Access Library for Scala"  
   [https://tpolecat.github.io/doobie/](https://tpolecat.github.io/doobie/)

3. **Markdown Guide**  
   "Mastering Markdown"  
   [https://guides.github.com/features/mastering-markdown/](https://guides.github.com/features/mastering-markdown/)

4. **MySQL Documentation**  
   "MySQL: Reference Manual"  
   [https://dev.mysql.com/doc/](https://dev.mysql.com/doc/)







