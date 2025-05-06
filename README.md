### **Notes API Testing with Postman and Newman**

This project demonstrates API testing of the Notes API using Postman. It includes a detailed collection of test cases to validate various user and note-related endpoints.

---

### **Features**

- CRUD operations for Notes and Users (Login, Register, Profile, Logout)
- Tests for GET, POST, PUT, PATCH, DELETE requests
- Environment variables and dynamic random data handling
- Extensive assertion testing with validation messages
- Tests for both positive and negative scenarios

---

## **API Documentation**  
🔗 [API Docs](https://practice.expandtesting.com/notes/api/api-docs/)

---

### **Technology Used**

- Postman  
- Newman  
- JavaScript (Pre-request & Test scripts)

---

### **Prerequisites**

- Node.js  
- Newman  
- newman-reporter-htmlextra (for HTML reports)

---

### **Installation**

1. **Install Postman**  
   [Download Postman](https://www.postman.com/downloads/)

2. **Clone This Repository**
```bash
git clone https://github.com/your-username/Notes-API-Testing-Postman.git
```

3. **Import Files into Postman**
   - `Notes-API.postman_collection.json`
   - `Notes-API.postman_environment.json`

4. **Install Newman and Reporter**
```bash
npm install -g newman
npm install -g newman-reporter-htmlextra
```

---

### **Usage**

1. **Select Environment** in Postman (`Notes API Environment`)
2. **Run Collection** via Collection Runner or Newman
3. **Check Results** in Postman UI or via command line/HTML report

---

### **Run Commands**

- Run in console:
```bash
newman run Notes-API.postman_collection.json -e Notes-API.postman_environment.json
```

- Run with HTML report:
```bash
newman run Notes-API.postman_collection.json -e Notes-API.postman_environment.json -r cli,htmlextra
```

---

## **Test Case Summary**

- ✅ Total Tests: 81  
- 🔁 HTTP Methods Used: 14  
- 🔒 Authenticated & Unauthenticated tests  
- 🎲 Random data used for dynamic testing

---

## **Example Test Case: Login**

### 🔗 Request URL  
```http
POST https://practice.expandtesting.com/users/login
```

### 📦 Request Body
```json
{
    "email": "{{email}}",
    "password": "{{password}}"
}
```

### 🧪 Test Script
```javascript
var statuscode = pm.response.code;

if (statuscode === 200) {
    var jsonData = pm.response.json();
    pm.environment.set("id", jsonData.data.id);
    pm.environment.set("token", jsonData.data.token);

    pm.test("Success Field Validation", function () {
        pm.expect(jsonData.success).to.eql(true);
    });

    pm.test("Status Field Validation", function () {
        pm.expect(jsonData.status).to.eql(200);
    });

    pm.test("Message Field Validation", function () {
        pm.expect(jsonData.message).to.eql("Login successful");
    });

    pm.test("Name Field Validation", function () {
        pm.expect(jsonData.data.name).to.eql(pm.environment.get("name"));
    });

    pm.test("Email Field Validation", function () {
        pm.expect(jsonData.data.email.toLowerCase()).to.eql(pm.environment.get("email").toLowerCase());
    });

    pm.test("Token Field Validation", function () {
        pm.expect(jsonData.data.token).to.not.be.null;
    });

} else if (statuscode == 500) {
    pm.test("Success Field Validation", function () {
        pm.expect(jsonData.success).to.eql(false);
    });
    pm.test("Status Field Validation", function () {
        pm.expect(jsonData.status).to.eql(500);
    });
    pm.test("Message Field Validation", function () {
        pm.expect(jsonData.message).to.eql("Internal Error Server");
    });
} else if (statuscode == 400) {
    pm.test("Success Field Validation", function () {
        pm.expect(jsonData.success).to.eql(false);
    });
    pm.test("Status Field Validation", function () {
        pm.expect(jsonData.status).to.eql(400);
    });
    pm.test("Message Field Validation", function () {
        pm.expect(jsonData.message).to.eql("Bad Request");
    });
} else if (statuscode == 401) {
    pm.test("Success Field Validation", function () {
        pm.expect(jsonData.success).to.eql(false);
    });
    pm.test("Status Field Validation", function () {
        pm.expect(jsonData.status).to.eql(401);
    });
    pm.test("Message Field Validation", function () {
        pm.expect(jsonData.message).to.eql("Unauthorized Request");
    });
}
```

### ✅ Response Body (Success)
```json
{
    "success": true,
    "status": 200,
    "message": "Login successful",
    "data": {
        "id": "68191a5c3e68a902854c616b",
        "name": "Wallace Rosenbaum",
        "email": "joana_mante58@hotmail.com",
        "token": "52e3e6b2e3fe4002bcb6bc20f801a28ed8c5f35aac7f42dabe863c42311af589"
    }
}
```

---

### 📊 Newman Report Sample  
