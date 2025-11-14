# Migration Step 5: API/Interface Migration

## Overview and Objectives

The goal of this step is to migrate API endpoints, routing, and middleware from C# to Java. This involves converting API routes, request/response handlers, and adapting middleware to ensure the API behaves consistently in the new Java-based application. By the end of this step, all APIs in your application will function as expected in the target Java environment.

---

## Prerequisites and Dependencies

Before starting, ensure the following are in place:

1. **Java Framework Selection**:
   - If using **Spring Boot**, ensure its dependencies are already set up in your `pom.xml` or `build.gradle`.
   - Example Spring Boot dependencies in `pom.xml`:
     ```xml
     <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-web</artifactId>
     </dependency>
     <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-json</artifactId>
     </dependency>
     ```

2. **C# Project Analysis**:
   - Have a clear understanding of your existing C# routing and middleware logic.
   - Identify the request types: GET, POST, PUT, DELETE, etc.
   - Document input/output payloads for each endpoint.

3. **Java Development Environment**:
   - JDK 17+ installed.
   - An IDE like IntelliJ IDEA or Eclipse configured.
   - A running local server for testing (e.g., Spring Boot application).

4. **Access to the Repository**:
   - Clone the `repository` and identify the files related to API handling in the C# application.

---

## Detailed Implementation Instructions

### Step 1: Port API Routes

1. **Review C# Routing Logic**:
   - In C#, routes are often defined using attributes like `[HttpGet]`, `[HttpPost]`, and `[Route]`.
     Example C# controller:
     ```csharp
     [Route("api/users")]
     public class UsersController : ControllerBase
     {
         [HttpGet("{id}")]
         public IActionResult GetUser(int id)
         {
             // Logic
             return Ok(user);
         }
     }
     ```

2. **Define Routes in Java**:
   - Use `@RestController`, `@RequestMapping`, and specific HTTP method annotations in Spring Boot.
   - Example Java equivalent:
     ```java
     @RestController
     @RequestMapping("/api/users")
     public class UserController {
     
         @GetMapping("/{id}")
         public ResponseEntity<User> getUser(@PathVariable int id) {
             // Logic
             return ResponseEntity.ok(userService.getUserById(id));
         }
     }
     ```

3. **Map Routes**:
   - Ensure all routes in C# have corresponding routes in Java.
   - Example mapping:
     | C# Route                  | Java Route                  |
     |---------------------------|-----------------------------|
     | `[HttpGet("api/users")]`  | `@GetMapping("/api/users")` |
     | `[HttpPost("api/users")]` | `@PostMapping("/api/users")`|

---

### Step 2: Convert Request/Response Handlers

1. **Analyze Request and Response Models**:
   - In C#, models are often defined with `class` and annotations like `[DataContract]`.
     Example C# model:
     ```csharp
     [DataContract]
     public class User {
         [DataMember]
         public int Id { get; set; }
         [DataMember]
         public string Name { get; set; }
     }
     ```

   - Create equivalent Java models using POJOs:
     ```java
     public class User {
         private int id;
         private String name;

         // Getters and setters
         public int getId() { return id; }
         public void setId(int id) { this.id = id; }
         public String getName() { return name; }
         public void setName(String name) { this.name = name; }
     }
     ```

2. **Adapt Request Handling**:
   - Map `HttpRequest` objects in C# to `@RequestBody` or `@PathVariable` in Java.
   - Example:
     ```csharp
     [HttpPost]
     public IActionResult AddUser([FromBody] User user) {
         // Logic
     }
     ```
     Java equivalent:
     ```java
     @PostMapping
     public ResponseEntity<Void> addUser(@RequestBody User user) {
         // Logic
         return ResponseEntity.ok().build();
     }
     ```

3. **Adapt Response Handling**:
   - Replace `IActionResult` in C# with `ResponseEntity` in Java.

---

### Step 3: Adapt Middleware

1. **Identify Middleware in C#**:
   - Middleware in C# is usually implemented via `UseMiddleware` or pipeline configurations like:
     ```csharp
     app.UseAuthentication();
     app.UseAuthorization();
     app.UseMiddleware<LoggingMiddleware>();
     ```

2. **Convert Middleware to Java Filters**:
   - In Java/Spring Boot, middleware is often implemented using filters or interceptors.
   - Example Java filter:
     ```java
     @Component
     public class LoggingFilter extends OncePerRequestFilter {

         @Override
         protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain)
                 throws ServletException, IOException {
             // Logging logic
             filterChain.doFilter(request, response);
         }
     }
     ```

3. **Register Middleware**:
   - Register filters via `@Component` annotation or configure them in `WebSecurityConfigurerAdapter`.

---

## Code Examples and Snippets

### Full Example: User API

#### C# Implementation
```csharp
[Route("api/users")]
public class UsersController : ControllerBase
{
    [HttpGet("{id}")]
    public IActionResult GetUser(int id)
    {
        return Ok(new User { Id = id, Name = "John Doe" });
    }

    [HttpPost]
    public IActionResult CreateUser([FromBody] User user)
    {
        return CreatedAtAction(nameof(GetUser), new { id = user.Id }, user);
    }
}
```

#### Java Implementation (Spring Boot)
```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @GetMapping("/{id}")
    public ResponseEntity<User> getUser(@PathVariable int id) {
        User user = new User(id, "John Doe");
        return ResponseEntity.ok(user);
    }

    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody User user) {
        return ResponseEntity.status(HttpStatus.CREATED).body(user);
    }
}
```

---

## Common Pitfalls and How to Avoid Them

1. **Incorrect Route Mapping**:
   - Ensure all routes in C# have a 1:1 mapping to Java routes.

2. **Case Sensitivity**:
   - Java is case-sensitive; ensure variable and method names match exactly.

3. **Data Serialization Issues**:
   - Verify that JSON serializers in Java (e.g., Jackson) are correctly configured to handle the same data formats.

4. **Middleware Ordering**:
   - Java middleware (filters) execution order matters; configure them properly.

---

## Testing Checklist

1. Verify all endpoints return correct HTTP status codes.
2. Test with valid and invalid payloads for request/response handling.
3. Check middleware functionality (e.g., logging, authentication).
4. Use tools like `Postman` or `cURL` for manual testing.
5. Write integration tests in Java using `MockMvc` or similar libraries.

---

## Validation Criteria

1. All API endpoints are functional and return expected results.
2. Middleware performs as expected.
3. No discrepancies in data formatting between C# and Java APIs.

---

## Troubleshooting Guide

1. **404 Errors**:
   - Verify route mappings and ensure the server is running.
2. **Serialization Errors**:
   - Check Java models for mismatched field names or missing getters/setters.
3. **Middleware Not Triggered**:
   - Ensure filters are registered properly.

---

## Resources and References

- [Spring Boot Reference Guide](https://docs.spring.io/spring-boot/docs/current/reference/html/web.html)
- [Spring Boot Filters](https://www.baeldung.com/spring-boot-add-filter)
- [Microsoft C# API Documentation](https://learn.microsoft.com/en-us/aspnet/core/web-api/)

---

## Next Steps

Proceed to **Step 6: Database Migration**, where you will migrate database schemas, queries, and connections from C# to Java using tools like Hibernate or JPA.