10. @RequestBody and ResponseEntity
User.java
package com.example.response;

public class User {

    private int id;
    private String name;
    private String email;

    public User() {
    }

    public User(int id, String name, String email) {
        this.id = id;
        this.name = name;
        this.email = email;
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

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}
UserController.java
package com.example.response;

import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/users")
public class UserController {

    @PostMapping
    public ResponseEntity<User> createUser(
            @RequestBody User user) {

        return new ResponseEntity<>(
                user,
                HttpStatus.CREATED
        );
    }

    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(
            @PathVariable int id) {

        if (id == 1) {

            User user = new User(
                    1,
                    "Satya Sree",
                    "satya@gmail.com"
            );

            return ResponseEntity.ok(user);
        }

        return ResponseEntity.notFound().build();
    }
}
ResponseApplication.java
package com.example.response;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class ResponseApplication {

    public static void main(String[] args) {

        SpringApplication.run(
                ResponseApplication.class, args);
    }
}
POST request in Postman

URL:

http://localhost:8080/users

Method:

POST

Body → raw → JSON

{
    "id": 1,
    "name": "Satya Sree",
    "email": "satya@gmail.com"
}

Response:

{
    "id": 1,
    "name": "Satya Sree",
    "email": "satya@gmail.com"
}

HTTP status:

201 CREATED