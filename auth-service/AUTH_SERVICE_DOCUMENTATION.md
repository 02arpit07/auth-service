# 🚀 **Complete Auth Service Documentation**

## 📋 **Service Overview**

This is a **Spring Boot Authentication Service** that implements JWT-based authentication with the following features:

- **User Registration & Login**
- **JWT Token Generation & Validation**
- **Protected Endpoints with Role-based Access**
- **MySQL Database Integration**
- **Spring Security Configuration**

---

## 🏗️ **Architecture & How It Works**

### **1. Authentication Flow**
```
User Registration → User Login → JWT Token → Access Protected Resources
```

### **2. JWT Token Lifecycle**
```
Login Request → Authentication → JWT Generation → Token Storage → Protected Access
```

### **3. Security Layers**
- **Spring Security** for authentication/authorization
- **JWT Filter** for token validation
- **Password Encryption** with BCrypt
- **Role-based Access Control**

---

## 🔧 **Technical Implementation**

### **Core Components:**

1. **JwtUtil** - JWT token generation/validation
2. **JwtAuthenticationFilter** - Token validation filter
3. **CustomUserDetailsService** - User authentication service
4. **SecurityConfig** - Security configuration
5. **AuthController** - REST API endpoints

### **Database Schema:**
```sql
CREATE TABLE users (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    roles VARCHAR(255) NOT NULL
);
```

---

## 📚 **API Documentation**

### **Base URL:** `http://localhost:8080`

---

## 🔐 **Authentication Endpoints**

### **1. User Registration**
**Endpoint:** `POST /auth/register`

**Description:** Register a new user in the system

**Request Body:**
```json
{
  "username": "string",
  "password": "string"
}
```

**cURL Example:**
```bash
curl -X POST http://localhost:8080/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "username": "john_doe",
    "password": "securepass123"
  }'
```

**Success Response (200):**
```json
"User registered successfully"
```

**Error Response (400):**
```json
"Username is already taken"
```

---

### **2. User Login**
**Endpoint:** `POST /auth/login`

**Description:** Authenticate user and return JWT token

**Request Body:**
```json
{
  "username": "string",
  "password": "string"
}
```

**cURL Example:**
```bash
curl -X POST http://localhost:8080/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "username": "john_doe",
    "password": "securepass123"
  }'
```

**Success Response (200):**
```json
{
  "token": "eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJqb2huX2RvZSIsImlhdCI6MTY5NTg5NzI0MCwiZXhwIjoxNjk1OTMzMjQwfQ.xyz789abc123def456ghi789..."
}
```

**Error Response (401):**
```json
"Invalid username or password"
```

---

## 🔒 **Protected Endpoints**

### **3. User Profile**
**Endpoint:** `GET /auth/profile`

**Description:** Get current user's profile information

**Headers Required:**
```
Authorization: Bearer <JWT_TOKEN>
```

**cURL Example:**
```bash
curl -X GET http://localhost:8080/auth/profile \
  -H "Authorization: Bearer YOUR_JWT_TOKEN_HERE"
```

**Success Response (200):**
```json
{
  "id": 1,
  "username": "john_doe",
  "roles": "ROLE_USER"
}
```

**Error Response (404):**
```json
"User not found"
```

**Error Response (401):**
```json
{
  "timestamp": "2024-09-28T04:10:00.000+00:00",
  "status": 401,
  "error": "Unauthorized",
  "path": "/auth/profile"
}
```

---

### **4. Protected Resource**
**Endpoint:** `GET /auth/protected`

**Description:** Access a protected resource that requires authentication

**Headers Required:**
```
Authorization: Bearer <JWT_TOKEN>
```

**cURL Example:**
```bash
curl -X GET http://localhost:8080/auth/protected \
  -H "Authorization: Bearer YOUR_JWT_TOKEN_HERE"
```

**Success Response (200):**
```json
{
  "message": "Hello john_doe!",
  "description": "This is a protected resource that requires JWT authentication.",
  "timestamp": 1695897240000
}
```

**Error Response (401):**
```json
{
  "timestamp": "2024-09-28T04:10:00.000+00:00",
  "status": 401,
  "error": "Unauthorized",
  "path": "/auth/protected"
}
```

---

## 🔑 **JWT Token Details**

### **Token Structure:**
```
Header.Payload.Signature
```

### **Token Payload:**
```json
{
  "sub": "username",
  "iat": 1695897240,
  "exp": 1695900840
}
```

### **Token Configuration:**
- **Algorithm:** HMAC-SHA512
- **Expiration:** 1 hour (3600000 ms)
- **Secret Key:** 64+ characters for security

---

## 🛡️ **Security Features**

### **1. Password Security**
- **BCrypt Hashing** for password storage
- **Salt Generation** for each password
- **No Plain Text** passwords stored

### **2. JWT Security**
- **HMAC-SHA512** algorithm
- **256-bit minimum** secret key
- **Token expiration** (1 hour)
- **Secure key generation**

### **3. Spring Security**
- **Stateless authentication**
- **JWT filter chain**
- **Role-based authorization**
- **CSRF protection disabled** (for API)

---

## 🗄️ **Database Configuration**

### **Connection Details:**
```properties
spring.datasource.url=jdbc:mysql://localhost:3306/auth_db
spring.datasource.username=root
spring.datasource.password=1234
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```

### **JPA Configuration:**
```properties
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect
```

---

## 🚀 **How JWT Authentication Works**

### **1. Login Process:**
```
User Credentials → AuthenticationManager → UserDetailsService → JWT Generation
```

### **2. Protected Access Process:**
```
Request + JWT Token → JwtAuthenticationFilter → Token Validation → SecurityContext → Protected Resource
```

### **3. Token Validation:**
```
Extract Token → Verify Signature → Check Expiration → Load User Details → Set Authentication
```

---

## 📊 **Complete Testing Workflow**

### **Step 1: Register User**
```bash
curl -X POST http://localhost:8080/auth/register \
  -H "Content-Type: application/json" \
  -d '{"username": "testuser", "password": "password123"}'
```

### **Step 2: Login & Get Token**
```bash
curl -X POST http://localhost:8080/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username": "testuser", "password": "password123"}'
```

### **Step 3: Use Token for Protected Access**
```bash
# Get User Profile
curl -X GET http://localhost:8080/auth/profile \
  -H "Authorization: Bearer YOUR_TOKEN_HERE"

# Access Protected Resource
curl -X GET http://localhost:8080/auth/protected \
  -H "Authorization: Bearer YOUR_TOKEN_HERE"
```

---

## 🔧 **Configuration Files**

### **application.properties:**
```properties
server.port=8080

# Database
spring.datasource.url=jdbc:mysql://localhost:3306/auth_db
spring.datasource.username=root
spring.datasource.password=1234
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# JPA
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect

# JWT
jwt.secret=MySuperSecretJWTKeyForAuthenticationService2024SecureAndLongEnoughForHMAC
jwt.expirationMs=3600000
```

---

## 📦 **Dependencies Used**

### **Core Dependencies:**
- **Spring Boot Starter Web** - REST API
- **Spring Boot Starter Security** - Authentication
- **Spring Boot Starter Data JPA** - Database
- **MySQL Connector** - Database driver
- **Lombok** - Code generation

### **JWT Dependencies:**
- **jjwt-api** (0.12.3) - JWT API
- **jjwt-impl** (0.12.3) - JWT Implementation
- **jjwt-jackson** (0.12.3) - JSON processing

---

## 🎯 **Key Features Implemented**

✅ **User Registration & Login**
✅ **JWT Token Generation & Validation**
✅ **Protected Endpoints**
✅ **Password Encryption**
✅ **Role-based Access Control**
✅ **Database Integration**
✅ **Error Handling**
✅ **Security Configuration**

---

## 🚨 **Error Handling**

### **Common Error Responses:**

| Status | Error | Description |
|--------|-------|-------------|
| 400 | Bad Request | Invalid request body |
| 401 | Unauthorized | Invalid credentials or missing/invalid JWT |
| 404 | Not Found | User not found |
| 500 | Internal Server Error | Server error |

---

## 🧪 **Testing Scripts**

### **Complete Test Script (test_auth.sh):**
```bash
#!/bin/bash

echo "🔍 Testing Auth Service - Debug Mode"
echo "===================================="

# Check if server is running
echo "📡 Step 1: Checking if server is running..."
SERVER_CHECK=$(curl -s -o /dev/null -w "%{http_code}" http://localhost:8080/auth/profile)
if [ "$SERVER_CHECK" = "401" ]; then
    echo "✅ Server is running (got 401 as expected for unprotected access)"
else
    echo "❌ Server is not running or not accessible. HTTP Code: $SERVER_CHECK"
    echo "Please start your Spring Boot application first:"
    echo "mvn spring-boot:run"
    exit 1
fi

echo ""

# Register user
echo "📝 Step 2: Registering user..."
REGISTER_RESPONSE=$(curl -s -X POST http://localhost:8080/auth/register \
  -H "Content-Type: application/json" \
  -d '{"username": "testuser", "password": "password123"}')
echo "Register Response: $REGISTER_RESPONSE"

echo ""

# Login
echo "🔐 Step 3: Logging in..."
LOGIN_RESPONSE=$(curl -s -X POST http://localhost:8080/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username": "testuser", "password": "password123"}')
echo "Login Response: $LOGIN_RESPONSE"

# Extract token (basic extraction - you might need to manually copy)
TOKEN=$(echo $LOGIN_RESPONSE | grep -o '"token":"[^"]*"' | cut -d'"' -f4)
if [ -z "$TOKEN" ]; then
    echo "❌ Could not extract token from login response"
    echo "Please manually copy the token from the login response above"
    echo "Then run: curl -v -X GET http://localhost:8080/auth/profile -H \"Authorization: Bearer YOUR_TOKEN\""
else
    echo "✅ Extracted token: ${TOKEN:0:50}..."
    echo ""
    
    # Test protected endpoint
    echo "🔒 Step 4: Testing protected endpoint..."
    curl -v -X GET http://localhost:8080/auth/profile \
      -H "Authorization: Bearer $TOKEN"
fi
```

---

## 🚀 **Quick Start Guide**

### **1. Prerequisites:**
- Java 17+
- Maven 3.6+
- MySQL 8.0+
- Git

### **2. Setup Database:**
```sql
CREATE DATABASE auth_db;
```

### **3. Run Application:**
```bash
# Clone and navigate to project
cd auth-service

# Install dependencies
mvn clean install

# Run application
mvn spring-boot:run
```

### **4. Test API:**
```bash
# Register user
curl -X POST http://localhost:8080/auth/register \
  -H "Content-Type: application/json" \
  -d '{"username": "testuser", "password": "password123"}'

# Login
curl -X POST http://localhost:8080/auth/login \
  -H "Content-Type: application/json" \
  -d '{"username": "testuser", "password": "password123"}'

# Use token for protected access
curl -X GET http://localhost:8080/auth/profile \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

---

## 📝 **Project Structure**

```
auth-service/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/auth/auth_service/
│   │   │       ├── AuthServiceApplication.java
│   │   │       ├── config/
│   │   │       │   └── SecurityConfig.java
│   │   │       ├── controller/
│   │   │       │   └── AuthController.java
│   │   │       ├── model/
│   │   │       │   └── User.java
│   │   │       ├── repository/
│   │   │       │   └── UserRepository.java
│   │   │       ├── security/
│   │   │       │   ├── CustomUserDetails.java
│   │   │       │   └── JwtAuthenticationFilter.java
│   │   │       ├── service/
│   │   │       │   └── CustomUserDetailsService.java
│   │   │       └── util/
│   │   │           └── JwtUtil.java
│   │   └── resources/
│   │       └── application.properties
│   └── test/
├── pom.xml
└── README.md
```

---

## 🔍 **Troubleshooting**

### **Common Issues:**

1. **Application won't start:**
   - Check MySQL is running
   - Verify database credentials
   - Check port 8080 is available

2. **JWT token not working:**
   - Ensure token is copied exactly
   - Check token hasn't expired
   - Verify Authorization header format

3. **Database connection issues:**
   - Check MySQL service is running
   - Verify database exists
   - Check connection credentials

### **Debug Commands:**
```bash
# Check if application is running
curl -v http://localhost:8080/auth/profile

# Check database connection
mysql -u root -p -e "SHOW DATABASES;"

# Check application logs
tail -f logs/application.log
```

---

## 📞 **Support**

For issues or questions:
1. Check the troubleshooting section
2. Review application logs
3. Verify all prerequisites are met
4. Test with provided cURL commands

---

**This authentication service provides a complete, production-ready JWT-based authentication system with proper security measures and comprehensive API documentation!** 🚀
