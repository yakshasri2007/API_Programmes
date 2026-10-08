8. SOAP Web Service

For SOAP, add:

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web-services</artifactId>
</dependency>
UserService.java
package com.example.soap;

public interface UserService {

    String getUserName(int id);
}
UserServiceImpl.java
package com.example.soap;

import org.springframework.stereotype.Service;

@Service
public class UserServiceImpl implements UserService {

    @Override
    public String getUserName(int id) {

        if (id == 1) {
            return "Satya Sree";
        }

        return "User not found";
    }
}
UserEndpoint.java
package com.example.soap;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/soap")
public class UserEndpoint {

    @Autowired
    private UserService userService;

    @GetMapping("/user/{id}")
    public String getUser(@PathVariable int id) {

        return userService.getUserName(id);
    }
}
SoapApplication.java
package com.example.soap;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SoapApplication {

    public static void main(String[] args) {

        SpringApplication.run(
                SoapApplication.class, args);
    }
}