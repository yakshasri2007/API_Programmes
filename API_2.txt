2. Spring Modules

2A. Spring JDBC

pom.xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         https://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.5.6</version>
        <relativePath/>
    </parent>

    <groupId>com.example</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <properties>
        <java.version>17</java.version>
    </properties>

    <dependencies>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>

        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>

    </dependencies>

</project>




JdbcApplication.java

package com.example.jdbc;

import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
public class JdbcApplication {

    public static void main(String[] args) {
        SpringApplication.run(JdbcApplication.class, args);
    }

    @Bean
    CommandLineRunner run(EmployeeRepository repository) {
        return args -> {

            repository.createTable();

            repository.addEmployee(1, "Satya", 50000);
            repository.addEmployee(2, "Rahul", 45000);

            repository.getEmployees()
                    .forEach(System.out::println);
        };
    }
}




EmployeeRepository.java

package com.example.jdbc;

import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;

import java.util.List;

@Repository
public class EmployeeRepository {

    private final JdbcTemplate jdbcTemplate;

    public EmployeeRepository(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    public void createTable() {
        jdbcTemplate.execute("""
                CREATE TABLE IF NOT EXISTS employee(
                    id INT PRIMARY KEY,
                    name VARCHAR(100),
                    salary DOUBLE
                )
                """);
    }

    public void addEmployee(int id, String name, double salary) {

        jdbcTemplate.update(
                "INSERT INTO employee VALUES (?, ?, ?)",
                id, name, salary
        );
    }

    public List<String> getEmployees() {

        return jdbcTemplate.query(
                "SELECT * FROM employee",
                (rs, rowNum) ->
                        rs.getInt("id") + " - "
                        + rs.getString("name") + " - "
                        + rs.getDouble("salary")
        );
    }
}



application.properties

spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driver-class-name=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=





2B. Spring MVC

MvcApplication.java

package com.example.mvc;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MvcApplication {

    public static void main(String[] args) {
        SpringApplication.run(MvcApplication.class, args);
    }
}


HomeController.java

package com.example.mvc;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;


@Controller
public class HomeController {

    @GetMapping("/home")
    public String home(Model model) {

        model.addAttribute("message",
                "Welcome to Spring MVC!");

        return "home";
    }
}

Create:

src/main/resources/templates/home.html
home.html
<!DOCTYPE html>
<html>
<head>
    <title>Spring MVC</title>
</head>
<body>

<h1 th:text="${message}"></h1>

<p>This is a simple Spring MVC application.</p>

</body>
</html>

Add Thymeleaf dependency:

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>