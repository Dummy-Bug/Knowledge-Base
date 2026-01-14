
> **Core Philosophy:**
> 
> The **5xx** class of status codes indicates a **Server Error**.
> 
> _The client sent a valid request, but the server failed to process it. The "fault" lies entirely within the server's infrastructure, code, or upstream dependencies._

---

## 1. The Application Failures

### **500 Internal Server Error**

- **Plain English:** "Something broke inside, and I don't have a specific name for it."
    
- **Root Cause Analysis:** A generic catch-all for **Unhandled Exceptions**. This usually means the code crashed (e.g., Null Pointer, Database Connection failure, or Syntax error in a script).
    
- **Scenario:** A user submits a form, but the backend code tries to save it to a database that is currently down. The code crashes, and the server returns a 500.
    
- **Where to Debug:**
    
    - **Server Logs:** This is the _only_ place to find the truth. Look for "Stack Traces."
        
    - **Permissions:** Check if the web server has permission to execute the script or read the `.env` file.
        

### **501 Not Implemented**

- **Plain English:** "I don't know how to do that yet."
    
- **Root Cause Analysis:** The server does not support the HTTP method used in the request. This is rare on modern servers but common in early-stage API development.
    
- **Scenario:** You send a `PATCH` request to a legacy server that only understands `GET` and `POST`.
    
- **Where to Debug:** Check your HTTP Verb. If you are building the API, you need to add a handler for that specific method.
    

---

## 2. The Infrastructure & Gateway Failures

### **502 Bad Gateway**

- **Plain English:** "The guy I'm talking to sent me garbage."
    
- **Root Cause Analysis:** **Communication Failure between servers**. The server acting as a gateway (Nginx, Apache, Cloudflare) received an invalid response from the backend application (Node.js, Java, PHP).
    
- **Scenario:** Your Nginx proxy is running fine, but the PHP-FPM process it's trying to talk to has crashed or returned a malformed header.
    
- **Where to Debug:**
    
    - Check if the **Backend Service** is actually running.
        
    - Check the **Proxy Configuration** (IP/Port) to ensure the gateway is pointing to the right place.
        

### **504 Gateway Timeout**

- **Plain English:** "I've been waiting for the backend to answer, but I've given up."
    
- **Root Cause Analysis:** The gateway waited for a response from the upstream server, but the **Timeout Limit** was reached. The backend is likely still working, but it's too slow.
    
- **Scenario:** A complex SQL query takes 60 seconds to run, but your Nginx proxy is configured to "timeout" after 30 seconds.
    
- **Where to Debug:**
    
    - **Slow Queries:** Check for unoptimized database calls.
        
    - **Timeout Settings:** Increase `proxy_read_timeout` (Nginx) or equivalent.
        

---

## 3. Capacity & Availability

### **503 Service Unavailable**

- **Plain English:** "I'm too busy or closed for cleaning."
    
- **Root Cause Analysis:** The server is physically up but cannot handle the request. This is usually due to **Overload** (CPU/RAM spikes) or **Scheduled Maintenance**.
    
- **Scenario:** A viral tweet sends 1 million people to a small server at once. The server refuses new connections to keep from crashing.
    
- **Where to Debug:**
    
    - **Resources:** Check CPU and Memory usage.
        
    - **Retry-After Header:** Good APIs send this header to tell you when to try again.
        

### **507 Insufficient Storage** (WebDAV)

- **Plain English:** "My hard drive is full."
    
- **Root Cause Analysis:** The server cannot create or modify the resource because it has run out of disk space or reached a quota.
    
- **Scenario:** An automated logging script filled up the server's primary partition, leaving 0 bytes for new data.
    
- **Where to Debug:** Run `df -h` on Linux to check disk space.
    

---

## 4. Troubleshooting Decision Tree (Mental Model)

When you see a 5xx error, determine the **depth** of the failure:

1. **Is it a 502/504?**
    
    - _Focus:_ The "Bridge." The problem is in the connection between your Load Balancer/Proxy and your App Code.
        
2. **Is it a 500?**
    
    - _Focus:_ The "Code." The app started to process the request but hit a bug. Check the application logs/Sentry.
        
3. **Is it a 503?**
    
    - _Focus:_ The "Health." The server is choking. Scale up your instances or optimize your resource usage.
        

---

## 5. Resources for Mastery

- **[Sentry.io Blog on 500 Errors](https://www.google.com/search?q=https://sentry.io/answers/http-500-internal-server-error/):** Excellent guide on tracking down code crashes.
    
- **[Nginx Error Log Documentation](https://nginx.org/en/docs/debugging_log.html):** Essential for fixing 502 and 504 errors.
    
- **[The Twelve-Factor App - Logs](https://12factor.net/logs):** Why treating logs as event streams is the 1% way to manage 5xx errors.
    