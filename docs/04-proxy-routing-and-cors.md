# Phase 4: Reverse Proxy & CORS

### Nginx Routing
Nginx was configured to act as the traffic cop, routing all `/` requests to `http://app:8080` while dynamically passing the `$remote_addr` and `$host` headers so the backend application logs the true client origin.

### CORS Security Implementation
To prevent cross-origin vulnerabilities, strict CORS headers were required. 

**Setback & Solution:**
Installing an external `cors` npm package would require altering `package.json` and risking dependency conflicts. Instead, we engineered a native Express middleware solution directly in `server.js` to enforce the origin rule without bloating the app:

```javascript
app.use((req, res, next) => {
  res.header("Access-Control-Allow-Origin", "[http://102.37.21.215](http://102.37.21.215)");
  res.header("Access-Control-Allow-Methods", "GET, PUT, POST, DELETE");
  res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
  next();
});
```
 We verified this configuration using `curl -I http://localhost`, proving the headers were active in the response.
