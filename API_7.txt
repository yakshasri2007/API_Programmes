7. Simple RESTful Web Service
User.java
package com.example.rest;

public class User {

    private Long id;
    private String name;
    private String email;

    public User() {
    }

    public User(Long id, String name, String email) {
        this.id = id;
        this.name = name;
        this.email = email;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}
UserController.java
package com.example.rest;

import org.springframework.web.bind.annotation.*;

import java.util.ArrayList;
import java.util.List;

@RestController
@RequestMapping("/users")
public class UserController {

    private final List<User> users = new ArrayList<>();

    @GetMapping
    public List<User> getUsers() {

        return users;
    }

    @PostMapping
    public User addUser(@RequestBody User user) {

        users.add(user);

        return user;
    }

    @PutMapping("/{id}")
    public User updateUser(
            @PathVariable Long id,
            @RequestBody User updatedUser) {

        for (User user : users) {

            if (user.getId().equals(id)) {

                user.setName(updatedUser.getName());
                user.setEmail(updatedUser.getEmail());

                return user;
            }
        }

        return null;
    }

    @DeleteMapping("/{id}")
    public String deleteUser(@PathVariable Long id) {

        users.removeIf(user ->
                user.getId().equals(id));

        return "User deleted successfully";
    }
}
RestApplication.java
package com.example.rest;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class RestApplication {

    public static void main(String[] args) {

        SpringApplication.run(
                RestApplication.class, args);
    }
}
POST JSON
{
    "id": 1,
    "name": "Satya",
    "email": "satya@gmail.com"
}