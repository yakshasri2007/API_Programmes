3. Spring Boot Auto Wiring and Logging
pom.xml
<project xmlns="http://maven.apache.org/POM/4.0.0">

    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.5.6</version>
    </parent>

    <groupId>com.example</groupId>
    <artifactId>autowiring</artifactId>
    <version>1.0</version>

    <properties>
        <java.version>17</java.version>
    </properties>

    <dependencies>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

    </dependencies>

</project>
Employee.java
package com.example.autowiring;

import org.springframework.stereotype.Component;

@Component
public class Employee {

    public String getDetails() {
        return "Employee Name: Satya Sree";
    }
}
EmployeeService.java
package com.example.autowiring;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class EmployeeService {

    private static final Logger logger =
            LoggerFactory.getLogger(EmployeeService.class);

    @Autowired
    private Employee employee;

    public void displayEmployee() {

        logger.info("Employee service started");

        System.out.println(employee.getDetails());

        logger.info("Employee details displayed successfully");
    }
}
AutowiringApplication.java
package com.example.autowiring;

import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
public class AutowiringApplication {

    public static void main(String[] args) {
        SpringApplication.run(
                AutowiringApplication.class, args);
    }

    @Bean
    CommandLineRunner run(EmployeeService service) {
        return args -> service.displayEmployee();
    }
}
Test
package com.example.autowiring;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class EmployeeServiceTest {

    @Autowired
    private EmployeeService service;

    @Test
    void testAutoWiring() {

        service.displayEmployee();
    }
}