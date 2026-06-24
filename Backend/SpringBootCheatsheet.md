# 🍃 Spring Boot Cheatsheet

> Part of [CodeCraftClub-Cheatsheets](../README.md) · Spring Boot (Java) quick reference.

---

## Setup & Run

```bash
# Generate at https://start.spring.io
./mvnw spring-boot:run        # Maven
./gradlew bootRun             # Gradle
./mvnw clean package          # build jar
java -jar target/app.jar
```

## Main Application

```java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

## REST Controller

```java
@RestController
@RequestMapping("/api/posts")
public class PostController {

    @GetMapping
    public List<Post> all() { return service.findAll(); }

    @GetMapping("/{id}")
    public Post one(@PathVariable Long id) {
        return service.findById(id);
    }

    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)
    public Post create(@RequestBody Post post) {
        return service.save(post);
    }

    @PutMapping("/{id}")
    public Post update(@PathVariable Long id, @RequestBody Post post) {
        return service.update(id, post);
    }

    @DeleteMapping("/{id}")
    public void delete(@PathVariable Long id) { service.delete(id); }

    // ?sort=asc
    @GetMapping("/search")
    public List<Post> search(@RequestParam String q) {
        return service.search(q);
    }
}
```

## Dependency Injection

```java
@Service
public class PostService {
    private final PostRepository repo;

    // constructor injection (preferred)
    public PostService(PostRepository repo) {
        this.repo = repo;
    }
}

// Stereotypes: @Component, @Service, @Repository, @Controller, @Configuration
```

## JPA Entity

```java
@Entity
@Table(name = "posts")
public class Post {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String title;

    private String body;

    @ManyToOne
    @JoinColumn(name = "author_id")
    private User author;

    // getters & setters...
}
```

## Spring Data Repository

```java
public interface PostRepository extends JpaRepository<Post, Long> {
    List<Post> findByTitleContaining(String q);   // derived query
    List<Post> findByPublishedTrue();
    Optional<Post> findByTitle(String title);

    @Query("SELECT p FROM Post p WHERE p.author.id = :id")
    List<Post> findByAuthor(@Param("id") Long id);
}

// Built-in: save(), findById(), findAll(), deleteById(), count()
```

## Configuration (application.properties)

```properties
server.port=8080
spring.datasource.url=jdbc:mysql://localhost:3306/app
spring.datasource.username=root
spring.datasource.password=secret
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

## Validation

```java
public class Post {
    @NotBlank private String title;
    @Size(min = 10) private String body;
    @Email private String email;
}

@PostMapping
public Post create(@Valid @RequestBody Post post) { ... }
```

## Exception Handling

```java
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(NotFoundException.class)
    @ResponseStatus(HttpStatus.NOT_FOUND)
    public Map<String, String> handle(NotFoundException e) {
        return Map.of("error", e.getMessage());
    }
}
```

## Common Annotations Reference

```
@SpringBootApplication   bootstrap entry point
@RestController          REST endpoints (returns JSON)
@RequestMapping          base path
@GetMapping / @PostMapping / @PutMapping / @DeleteMapping
@PathVariable            URL path value
@RequestParam            query string value
@RequestBody             JSON body -> object
@Autowired               inject dependency (field)
@Value("${prop}")        inject config property
@Transactional           wrap in DB transaction
```

---

[🔝 Back to README](../README.md)
