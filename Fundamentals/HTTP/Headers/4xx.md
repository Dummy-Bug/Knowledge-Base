
> **Core Philosophy:**
> 
> The **4xx** class of status codes indicates a **Client Error**.
> 
> _The server is functioning correctly, but the request sent is flawed, invalid, or impossible to fulfill in its current state._

---

## 1. Access & Security Errors

### **401 Unauthorized**

- **Plain English:** "I don't know who you are."
    
- **Root Cause Analysis:** **Authentication Failure**. The request is missing credentials (Token/Cookie) or the credentials provided are invalid/expired. It is strictly about **Identity**.
    
- **Scenario:** You send a request without the `Authorization: Bearer <token>` header, or your session cookie has expired on the server side.
    
- **Where to Debug:**
    
    - **Authorization Header:** Check for spelling/format (e.g., `Bearer` space prefix).
        
    - **Token Validity:** Check if the token has been revoked or has reached its `exp` (expiry) time.
        

### **403 Forbidden**

- **Plain English:** "I know who you are, but you aren't allowed here."
    
- **Root Cause Analysis:** **Authorization/Permission Failure**. Your identity is verified, but your **Role** or **Scope** is insufficient for this specific resource.
    
- **Scenario:** You are logged in as a `Staff` user, but you tried to hit an `/admin/delete-database` endpoint.
    
- **Where to Debug:**
    
    - **Permissions/Roles:** Check the ACL (Access Control List) or RBAC (Role-Based Access Control) settings in the backend.
        
    - **IP Whitelisting:** Sometimes 403 is triggered if your IP address is not on a permitted list.
        

---

## 2. Routing & Method Errors

### **404 Not Found**

- **Plain English:** "I can't find what you're looking for."
    
- **Root Cause Analysis:** **Routing or Mapping Failure**. The URL path does not match any code-based controller or physical file.
    
- **Scenario:** Calling `domain.com/api/login` when the actual path is `domain.com/auth/login`.
    
- **Where to Debug:**
    
    - **Base URL & Context Path:** Check if a prefix like `/v1/` or `/api/` is missing.
        
    - **Resource Existence:** If the URL is correct, check if the specific ID (e.g., `/users/99`) actually exists in the database.
        

### **405 Method Not Allowed**

- **Plain English:** "Right address, wrong door."
    
- **Root Cause Analysis:** The URL exists, but the **HTTP Verb** (GET, POST, PUT, DELETE) is not supported by that specific endpoint.
    
- **Scenario:** You try to `POST` data to a search endpoint that only allows `GET` requests.
    
- **Where to Debug:** Compare the request method in Postman with the API documentation/source code.
    

---

## 3. Data & Validation Errors

### **400 Bad Request**

- **Plain English:** "Your request is malformed or invalid."
    
- **Root Cause Analysis:** **Contract Violation**. The client sent data that breaks syntax rules (invalid JSON) or fails basic structural validation.
    
- **Scenario:** You send a JSON body missing a required field, or you send a string where a number is expected.
    
- **Where to Debug:**
    
    - **JSON Syntax:** Look for missing commas or quotes in the body.
        
    - **Data Types:** Ensure numbers aren't sent as strings if the API is strictly typed.
        

### **415 Unsupported Media Type**

- **Plain English:** "I can't read the format you sent."
    
- **Root Cause Analysis:** **Input Format Error**. The `Content-Type` header does not match the parser the server is using.
    
- **Scenario:** Sending a JSON body but the header is set to `Content-Type: text/plain`. The server doesn't know it should use a JSON parser.
    
- **Where to Debug:** Check the **Headers** tab for `Content-Type: application/json`.
    

### **422 Unprocessable Entity**

- **Plain English:** "The format is fine, but the data is logicially wrong."
    
- **Root Cause Analysis:** **Semantic Validation Error**. The JSON is valid, but the values fail business logic rules.
    
- **Scenario:** You submit a form to create a user, but the "Birth Date" provided is in the future.
    
- **Where to Debug:** Check the response body; APIs often return an array of specific field validation errors here.
    

---

## 4. Conflict & Rate Errors

### **409 Conflict**

- **Plain English:** "This request conflicts with the current state of the server."
    
- **Root Cause Analysis:** **State Collision**. Often used for duplicate entries or versioning issues.
    
- **Scenario:** Trying to register a new account with an email address that is already in the database.
    
- **Where to Debug:** Check if the resource you are trying to create already exists.
    

### **429 Too Many Requests**

- **Plain English:** "You are moving too fast."
    
- **Root Cause Analysis:** **Rate Limiting**. The client has exceeded the number of permitted requests in a given timeframe.
    
- **Scenario:** Running a high-speed automation script against an API without throttling.
    
- **Where to Debug:** Check the `Retry-After` header in the response to see when you are allowed to try again.
    