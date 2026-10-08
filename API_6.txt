6. Pagination and Sorting with JPA
UserRepository.java
package com.example.pagination;

import com.example.pagination.User;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;

public interface UserRepository
        extends JpaRepository<User, Long> {

    Page<User> findAll(Pageable pageable);

    @Query("SELECT u FROM User u ORDER BY u.name ASC")
    Page<User> findUsersSorted(Pageable pageable);
}
User.java
package com.example.pagination;

import jakarta.persistence.Entity;
import jakarta.persistence.Id;

@Entity
public class User {

    @Id
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
package com.example.pagination;

import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Sort;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/users")
public class UserController {

    private final UserRepository repository;

    public UserController(UserRepository repository) {
        this.repository = repository;
    }

    @GetMapping
    public Page<User> getUsers(
            @RequestParam(defaultValue = "0") int page,
            @RequestParam(defaultValue = "5") int size) {

        PageRequest pageable =
                PageRequest.of(
                        page,
                        size,
                        Sort.by("name").ascending()
                );

        return repository.findAll(pageable);
    }

    @GetMapping("/sorted")
    public Page<User> sortedUsers(
            @RequestParam(defaultValue = "0") int page) {

        PageRequest pageable =
                PageRequest.of(page, 5);

        return repository.findUsersSorted(pageable);
    }
}

Example:

http://localhost:8080/users?page=0&size=5