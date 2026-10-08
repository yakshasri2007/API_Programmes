9. Simple Spring REST Controller

This is similar to Experiment 7, but we can make it a simpler controller-based implementation.

User.java
package com.example.controller;

public class User {

    private int id;
    private String name;

    public User() {
    }

    public User(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
UserController.java
package com.example.controller;

import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api")
public class UserController {

    @GetMapping("/user")
    public User getUser() {

        return new User(
                1,
                "Satya Sree"
        );
    }

    @GetMapping("/message")
    public String message() {

        return "Welcome to Spring REST Controller";
    }
}
RestControllerApplication.java
package com.example.controller;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class RestControllerApplication {

    public static void main(String[] args) {

        SpringApplication.run(
                RestControllerApplication.class, args);
    }
}

Test:

http://localhost:8080/api/user

Output:

{
    "id": 1,
    "name": "Satya Sree"
}