4. Implementing AOP Advices

Add AOP dependency:

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
Student.java
package com.example.aop;

import org.springframework.stereotype.Component;

@Component
public class Student {

    public String getName() {
        return "Satya Sree";
    }
}
StudentService.java
package com.example.aop;

import org.springframework.stereotype.Service;

@Service
public class StudentService {

    public void displayStudent() {

        System.out.println("Student service method executed.");
    }

    public void calculateMarks() {

        System.out.println("Calculating student marks.");
    }
}
LoggingAspect.java
package com.example.aop;

import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.springframework.stereotype.Component;

@Aspect
@Component
public class LoggingAspect {

    @Before("execution(* com.example.aop.StudentService.*(..))")
    public void beforeMethod() {

        System.out.println(
                "AOP Advice: StudentService method is about to execute."
        );
    }
}
AopApplication.java
package com.example.aop;

import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
public class AopApplication {

    public static void main(String[] args) {

        SpringApplication.run(AopApplication.class, args);
    }

    @Bean
    CommandLineRunner run(StudentService service) {

        return args -> {

            service.displayStudent();

            service.calculateMarks();
        };
    }
}