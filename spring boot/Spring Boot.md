
# Spring Boot Cheat Sheet

## Table of Contents
1. [Basics](#basics)
2. [Controllers & Routes](#controllers--routes)
3. [JSON Responses](#json-responses)
4. [HTTP Status Codes](#http-status-codes)
5. [Error Handling](#error-handling)
6. [Middleware](#middleware)
7. [Client Communication](#client-communication)
8. [Database (JPA)](#database-jpa)
9. [Configuration](#configuration)
10. [Testing](#testing)
11. [Advanced Topics](#advanced-topics)

---

## Basics

```java
@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

### Project Structure
```
src/
├── main/
│   ├── java/
│   │   └── com/example/
│   │       ├── controllers/
│   │       ├── models/
│   │       ├── repositories/
│   │       └── services/
│   └── resources/
│       ├── static/
│       ├── templates/
│       └── application.properties
└── test/
```

---

## Controllers & Routes

```java
@RestController
@RequestMapping("/api")
public class MyController {
    
    @GetMapping("/hello")
    public String hello() {
        return "Hello World";
    }

    @GetMapping("/users/{id}")
    public User getUser(@PathVariable Long id) {
        // ...
    }

    @PostMapping("/users")
    public ResponseEntity<User> createUser(@RequestBody User user) {
        // ...
        return ResponseEntity.status(HttpStatus.CREATED).body(savedUser);
    }
}
```

---

## JSON Responses

```java
@Getter @Setter // Lombok annotations
public class User {
    private Long id;
    private String name;
    private String email;
}

@RestController
public class UserController {
    @GetMapping("/user")
    public User getUser() {
        return new User(1L, "John", "john@example.com");
    }
}
```

---

## HTTP Status Codes

```java
@RestController
public class ResponseController {
    
    @GetMapping("/success")
    public ResponseEntity<String> success() {
        return ResponseEntity.ok("Success");
    }

    @GetMapping("/not-found")
    public ResponseEntity<String> notFound() {
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body("Not found");
    }

    @GetMapping("/error")
    public ResponseEntity<String> error() {
        return ResponseEntity.internalServerError().body("Error");
    }
}
```

---

## Error Handling

### Global Exception Handler
```java
@ControllerAdvice
public class GlobalExceptionHandler {
    
    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<String> handleNotFound(ResourceNotFoundException ex) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(ex.getMessage());
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<String> handleAll(Exception ex) {
        return ResponseEntity.internalServerError().body("Error: " + ex.getMessage());
    }
}
```

### Custom Exception
```java
public class ResourceNotFoundException extends RuntimeException {
    public ResourceNotFoundException(String message) {
        super(message);
    }
}
```

---

## Middleware

### Filter Example
```java
@Component
public class LoggingFilter implements Filter {
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) {
        System.out.println("Before request");
        chain.doFilter(request, response);
        System.out.println("After request");
    }
}
```

### Interceptor Example
```java
@Component
public class LoggingInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        System.out.println("Pre-handle: " + request.getRequestURI());
        return true;
    }
}

@Configuration
public class WebConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoggingInterceptor());
    }
}
```

---

## Client Communication

### Request Handling
```java
@RestController
public class UserController {
    
    // Query params: /search?q=term
    @GetMapping("/search")
    public String search(@RequestParam String q) {
        return "Search: " + q;
    }

    // Path variable: /users/123
    @GetMapping("/users/{id}")
    public User getUser(@PathVariable Long id) {
        // ...
    }

    // Request body (JSON)
    @PostMapping("/users")
    public User createUser(@RequestBody User user) {
        // ...
    }
}
```

---

## Database (JPA)

### Entity
```java
@Entity
@Getter @Setter
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String email;
}
```

### Repository
```java
public interface UserRepository extends JpaRepository<User, Long> {
    List<User> findByName(String name);
}
```

### Service
```java
@Service
@RequiredArgsConstructor
public class UserService {
    private final UserRepository userRepository;

    public User getUserById(Long id) {
        return userRepository.findById(id)
                .orElseThrow(() -> new ResourceNotFoundException("User not found"));
    }
}
```

---

## Configuration

### application.properties
```properties
server.port=8080
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.hibernate.ddl-auto=update
```

---

## Testing

### Unit Test
```java
@ExtendWith(MockitoExtension.class)
class UserServiceTest {
    @Mock
    private UserRepository userRepository;
    
    @InjectMocks
    private UserService userService;

    @Test
    void getUserById_NotFound() {
        when(userRepository.findById(1L)).thenReturn(Optional.empty());
        assertThrows(ResourceNotFoundException.class, () -> userService.getUserById(1L));
    }
}
```

### Integration Test
```java
@SpringBootTest
@AutoConfigureMockMvc
class UserControllerTest {
    @Autowired
    private MockMvc mockMvc;

    @Test
    void getUser_NotFound() throws Exception {
        mockMvc.perform(get("/users/999"))
               .andExpect(status().isNotFound());
    }
}
```

---

## Advanced Topics

### Validation
```java
public class User {
    @NotBlank
    private String name;
    
    @Email
    private String email;
}

@PostMapping("/users")
public ResponseEntity<User> createUser(@Valid @RequestBody User user) {
    // ...
}
```

### Caching
```java
@Cacheable("users")
public User getUserById(Long id) {
    // ...
}

@CacheEvict(value = "users", key = "#user.id")
public User updateUser(User user) {
    // ...
}
```

### Security
```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {
    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
                .antMatchers("/public/**").permitAll()
                .anyRequest().authenticated()
            .and()
            .httpBasic();
    }
}
```