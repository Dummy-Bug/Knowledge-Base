
> **Core Philosophy:**
> 
> The **3xx** class of status codes indicates **Redirection**.
> 
> _The request was received successfully, but the resource is located elsewhere. The client must take additional action (usually calling a new URL) to complete the task._

---

## 1. Permanent Moves

### **301 Moved Permanently**

- **Plain English:** "This address is dead; we’ve moved house for good."
    
- **Root Cause Analysis:** The requested resource has been assigned a new permanent URI. **Crucial:** Browsers will cache this redirect aggressively. If you visit the old URL again, the browser won't even ask the server; it will just jump to the new one.
    
- **Scenario:** You migrated your site from `http://old-site.com` to `https://new-site.com`.
    
- **Where to Debug:** If you accidentally set a 301, you have to clear the user's browser cache to "undo" it.
    

### **308 Permanent Redirect**

- **Plain English:** "Moved permanently, and don't you dare change my request method."
    
- **Root Cause Analysis:** The modern version of 301. It ensures that if you `POST` data to the old URL, the browser `POSTs` the same data to the new URL.
    
- **Scenario:** An API endpoint moved permanently, but you need the client to keep sending the JSON body via `POST` to the new location.
    
- **Where to Debug:** Use this for APIs instead of 301 to avoid the "POST-turned-into-GET" bug.
    

---

## 2. Temporary Moves

### **302 Found (formerly "Moved Temporarily")**

- **Plain English:** "I'm staying at a hotel for a few days; find me there."
    
- **Root Cause Analysis:** The resource is temporarily at a different URI. Clients should keep using the original URI for future requests.
    
- **Scenario:** You are A/B testing a new landing page or doing short-term maintenance.
    
- **Where to Debug:** Many older browsers incorrectly change a `POST` to a `GET` when they see a 302. If your data disappears during a redirect, this is why.
    

### **307 Temporary Redirect**

- **Plain English:** "I'm temporarily away, and keep the request exactly as it is."
    
- **Root Cause Analysis:** The modern, unambiguous version of 302. It **guarantees** the HTTP method and body will not be changed.
    
- **Scenario:** You are redirecting a login `POST` request to a backup authentication server during a peak traffic spike.
    
- **Where to Debug:** Always prefer 307 over 302 for API development to ensure `POST/PUT/DELETE` payloads aren't dropped.
    

---

## 3. The "State Management" Success

### **303 See Other**

- **Plain English:** "Got it! Now go look at this result page over here."
    
- **Root Cause Analysis:** Specifically designed for the **POST-Redirect-GET** pattern. It tells the browser: "I've processed your data, now use `GET` to see the result."
    
- **Scenario:** After a user submits a "Contact Us" form (POST), you redirect them to a `/thank-you` page (GET).
    
- **Where to Debug:** This prevents the "Confirm Form Resubmission" popup when a user hits the refresh button on a success page.
    

---

## 4. Caching & Performance

### **304 Not Modified**

- **Plain English:** "You already have the latest version; save your bandwidth."
    
- **Root Cause Analysis:** This isn't a "move" redirect. It’s a **Conditional Request** success. The client asks "Has this changed?" and the server says "No."
    
- **Scenario:** A browser has a cached image. It sends an `If-Modified-Since` header. The server sees the file hasn't changed and sends a 304 (no body) instead of a 200 (full image).
    
- **Where to Debug:** If your CSS/JS changes aren't showing up, the server might be incorrectly sending 304s because of faulty ETag or Last-Modified logic.