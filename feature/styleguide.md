# Go Coding Style Guide

## Function Naming

In Go, proper function naming is crucial for code readability and maintainability:

- Use CamelCase for exported functions (visible outside the package): `func ExportedFunction()`
- Use camelCase for unexported functions (package-private): `func unexportedFunction()`
- Names should be descriptive and convey the function's purpose

## JSON Tags for Structs

JSON tags in Go structs allow you to control how struct fields are serialized to and deserialized from JSON:

```go
type Person struct {
    Name    string `json:"name"`
    Age     int    `json:"age"`
    Address string `json:"address,omitempty"`
}
```

- Use `json:"fieldname"` to specify JSON field names
- Use `omitempty` to exclude empty fields from JSON output

## Validation Tags

While not explicitly mentioned in the original document, validation tags are often used with libraries like `go-playground/validator` to enforce data constraints:

```go
type User struct {
    Email string `validate:"required,email"`
    Age   int    `validate:"gte=0,lte=130"`
}
```

## Anonymous Inline Structs

Anonymous inline structs are useful for quick, temporary data storage where the struct doesn't need to be reused elsewhere in the code:

```go
person := struct {
    Name string
    Age  int
}{
    Name: "Alice",
    Age:  30,
}
```

## Test-Driven Development (TDD)

TDD is a software development approach where tests are written before the actual code:

```go
func TestIsEven(t *testing.T) {
    if !isEven(2) {
        t.Error("Expected 2 to be even")
    }
    if isEven(3) {
        t.Error("Expected 3 to be odd")
    }
}

func isEven(n int) bool {
    return n%2 == 0
}
```

This approach encourages developers to think critically about the desired behavior of their code and ensures thorough testing.

## Closures in Go

Closures in Go are functions that capture and access variables from their surrounding scope:

```go
func counter() func() int {
    count := 0
    return func() int {
        count++
        return count
    }
}
```

Closures are particularly useful for creating functions that need to encapsulate and maintain state, providing a clean and elegant way to manage stateful logic within functions.

## Database Connection Pooling

For MongoDB connection pooling, the document recommends using the official MongoDB driver's built-in support:

```go
clientOptions := options.Client().
    ApplyURI("mongodb://localhost:27017").
    SetMaxPoolSize(50).
    SetMinPoolSize(10).
    SetMaxConnIdleTime(10 * time.Minute)
```

This approach optimizes database performance and ensures efficient resource usage.

## Dependency Injection

The document discourages heavy use of dependency injection frameworks. Instead, it recommends explicit constructor functions:

```go
func NewNotificationService(notifier Notifier) *NotificationService {
    return &NotificationService{notifier: notifier}
}
```

This approach keeps the code straightforward and predictable.

## Designing to Interfaces

Interface-based design in Go promotes flexibility and decoupling:

```go
type Notifier interface {
    SendNotification(message string) error
}

type EmailNotifier struct {}
func (e *EmailNotifier) SendNotification(message string) error {
    // Implementation
}
```

This approach allows for easy swapping of implementations and better testability.

## SOLID Principles in Go

The document emphasizes the importance of SOLID principles, particularly the Single Responsibility Principle (SRP). Here's a brief overview of SOLID in the context of Go:

1. **Single Responsibility Principle (SRP)**: 
   - Each function, method, or struct should have only one reason to change.
   - Example: Separate data models from business logic.

2. **Open/Closed Principle**:
   - Software entities should be open for extension but closed for modification.
   - Use interfaces and composition to allow extending functionality without modifying existing code.

3. **Liskov Substitution Principle**:
   - Objects of a superclass should be replaceable with objects of its subclasses without affecting program correctness.
   - Ensure that interfaces are properly implemented across types.

4. **Interface Segregation Principle**:
   - Many specific interfaces are better than one general-purpose interface.
   - Keep interfaces focused and minimal.

5. **Dependency Inversion Principle**:
   - Depend on abstractions, not concretions.
   - Use interfaces to decouple high-level and low-level modules.

The document particularly stresses SRP, encouraging developers to create modular, focused components that are easier to maintain and test.

## Interfaces and Mocking in Test Scenarios

Interfaces in Go are powerful tools for writing testable code, especially when it comes to mocking dependencies. Here's how interfaces facilitate easier testing:

1. **Defining Interfaces for Dependencies**:
   Instead of relying on concrete implementations, define interfaces for your dependencies:

   ```go
   type DataStore interface {
       Save(data []byte) error
       Load() ([]byte, error)
   }
   ```

2. **Using Interfaces in Your Code**:
   Design your functions and structs to depend on interfaces rather than concrete types:

   ```go
   type UserService struct {
       store DataStore
   }

   func (u *UserService) SaveUser(user User) error {
       data, err := json.Marshal(user)
       if err != nil {
           return err
       }
       return u.store.Save(data)
   }
   ```

3. **Creating Mock Implementations**:
   For testing, create mock implementations of your interfaces:

   ```go
   type MockDataStore struct {
       mock.Mock
   }

   func (m *MockDataStore) Save(data []byte) error {
       args := m.Called(data)
       return args.Error(0)
   }

   func (m *MockDataStore) Load() ([]byte, error) {
       args := m.Called()
       return args.Get(0).([]byte), args.Error(1)
   }
   ```

4. **Writing Tests with Mocks**:
   Use the mock implementations in your tests to control behavior and verify interactions:

   ```go
   func TestUserService_SaveUser(t *testing.T) {
       mockStore := new(MockDataStore)
       service := &UserService{store: mockStore}

       user := User{ID: 1, Name: "Alice"}
       expectedData, _ := json.Marshal(user)

       mockStore.On("Save", expectedData).Return(nil)

       err := service.SaveUser(user)

       assert.NoError(t, err)
       mockStore.AssertExpectations(t)
   }
   ```

By using interfaces, you can easily swap out real implementations with mocks in your tests. This allows you to:

- Test your code in isolation from external dependencies
- Control the behavior of dependencies to test different scenarios
- Verify that your code interacts correctly with its dependencies

## Variable-based Constructor

Here's an example of a variable-based constructor, which assigns a constructor function to a variable:

```go
type Config struct {
    Host string
    Port int
}

var NewConfig = func(host string, port int) *Config {
    return &Config{
        Host: host,
        Port: port,
    }
}

// Usage
config := NewConfig("localhost", 8080)
```

This approach provides flexibility in how objects are created and initialized.

## CORS and Preflight for React UI

To allow CORS for a React app running on localhost:9000, you can use the following middleware with Gin:

```go
func CORS() gin.HandlerFunc {
    return func(c *gin.Context) {
        c.Writer.Header().Set("Access-Control-Allow-Origin", "http://localhost:9000")
        c.Writer.Header().Set("Access-Control-Allow-Credentials", "true")
        c.Writer.Header().Set("Access-Control-Allow-Headers", "Content-Type, Content-Length, Accept-Encoding, X-CSRF-Token, Authorization, accept, origin, Cache-Control, X-Requested-With")
        c.Writer.Header().Set("Access-Control-Allow-Methods", "POST, OPTIONS, GET, PUT, DELETE")

        if c.Request.Method == "OPTIONS" {
            c.AbortWithStatus(204)
            return
        }

        c.Next()
    }
}

// In your main.go or router setup
r := gin.Default()
r.Use(CORS())
```

This middleware handles CORS headers and preflight requests, allowing your React frontend to communicate with your Go backend.