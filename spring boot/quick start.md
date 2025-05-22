
# Spring Boot Quick Start Guide

## Basic Project Structure

```
src/
├── main/
│   ├── java/
│   │   └── com.yourpackage/
│   │       ├── config/       # Configuration classes
│   │       ├── controllers/  # REST controllers
│   │       ├── models/       # Data models/entities
│   │       ├── repositories/ # Database repositories
│   │       ├── services/     # Business logic
│   │       └── Application.java # Main class
│   └── resources/
│       ├── application.properties # Config file
│       ├── static/          # Static files (JS/CSS)
│       └── templates/       # Thymeleaf templates (if used)
└── test/                    # Test classes
```

## 1. Main Application Setup

```java
// Application.java
package com.yourpackage;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

## 2. Basic Server Configuration

`application.properties`:
```properties
# Server
server.port=8080
server.servlet.context-path=/api

# CORS (basic setup)
# spring.mvc.cors.allowed-origins=*
# spring.mvc.cors.allowed-methods=GET,POST,PUT,DELETE
```

OR `application.yml`:
```yaml
server:
  port: 8080
  servlet:
    context-path: /api
```

## 3. Basic CORS Configuration

Option 1: Global CORS (in config class)
```java
@Configuration
public class CorsConfig implements WebMvcConfigurer {
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("*")
                .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")
                .allowedHeaders("*");
    }
}
```

Option 2: Per-controller CORS
```java
@RestController
@CrossOrigin(origins = "*")
@RequestMapping("/products")
public class ProductController {
    // ...
}
```

## 4. Basic Controller Setup

```java
package com.yourpackage.controllers;

import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api")
public class BasicController {

    // GET with path variable
    @GetMapping("/hello/{name}")
    public String sayHello(@PathVariable String name) {
        return "Hello, " + name + "!";
    }

    // GET with query parameter
    @GetMapping("/greet")
    public String greetUser(@RequestParam String name) {
        return "Greetings, " + name + "!";
    }

    // POST with request body
    @PostMapping("/user")
    public String createUser(@RequestBody User user) {
        return "Created user: " + user.getName();
    }
}
```

## 5. Handling Different Request Types

### Path Variables
```java
@GetMapping("/users/{id}")
public String getUser(@PathVariable Long id) {
    return "User ID: " + id;
}
```

### Query Parameters
```java
@GetMapping("/search")
public String search(@RequestParam String query, 
                    @RequestParam(required = false, defaultValue = "1") int page) {
    return "Searching for: " + query + " on page " + page;
}
```

### Request Body (JSON)
```java
@PostMapping("/create")
public ResponseEntity<String> create(@RequestBody UserDto user) {
    // Process user
    return ResponseEntity.ok("User created: " + user.getUsername());
}
```

### Form Data
```java
@PostMapping("/login")
public String login(@RequestParam String username, 
                   @RequestParam String password) {
    return "Logged in as: " + username;
}
```

## 6. Basic Response Handling

```java
@RestController
public class ResponseController {

    // Simple response
    @GetMapping("/simple")
    public String simpleResponse() {
        return "Simple response";
    }

    // Response with status code
    @GetMapping("/custom")
    public ResponseEntity<String> customResponse() {
        return ResponseEntity.status(HttpStatus.CREATED).body("Custom response");
    }

    // JSON response
    @GetMapping("/json")
    public Map<String, Object> jsonResponse() {
        return Map.of(
            "status", "success",
            "message", "JSON response",
            "timestamp", LocalDateTime.now()
        );
    }
}
```

## 7. Basic Error Handling

```java
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(Exception.class)
    public ResponseEntity<Map<String, String>> handleAllExceptions(Exception ex) {
        Map<String, String> error = new HashMap<>();
        error.put("error", ex.getMessage());
        error.put("timestamp", LocalDateTime.now().toString());
        return ResponseEntity.internalServerError().body(error);
    }
}
```

## Quick Start Checklist

1. Create main `Application.java` with `@SpringBootApplication`
2. Set up `application.properties` with basic config
3. Create controller package with `@RestController` classes
4. Define routes with `@GetMapping`, `@PostMapping` etc.
5. Handle parameters:
   - `@PathVariable` for path segments
   - `@RequestParam` for query params
   - `@RequestBody` for JSON payloads
6. Configure CORS (either globally or per controller)
7. Add basic error handling with `@ControllerAdvice`

This gives you a working Spring Boot API with:
- Proper project structure
- Basic routing
- Request parameter handling
- CORS configuration
- Simple error handling
- JSON response capability