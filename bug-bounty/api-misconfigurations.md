## Summary
A browser-accessible API endpoint was identified with multiple
low-severity security misconfigurations that allow unauthorized
requests to be processed.

The issues identified include:
- Cross-Site Request Forgery (CSRF)
- Overly permissive CORS configuration
- Missing authentication enforcement

These weaknesses indicate insufficient request validation and
access control on the API endpoint.

>  This write-up is anonymized and shared for learning and
portfolio purposes only.

---

## Affected Component
- Web API endpoint accessible via browsers
- Example (sanitized):

---

## Vulnerability Analysis

###  Cross-Site Request Forgery (CSRF)

**Description**  
The API accepts state-changing POST requests initiated from a
third-party website without any CSRF protection.

**Observed Behavior**
- No CSRF token required
- No Origin or Referer validation
- Requests sent from a malicious webpage are processed successfully

---

###  CORS Misconfiguration

**Description**  
The API responds with an overly permissive CORS policy.

**Observed Response Header**
Access-Control-Allow-Origin: *
Control-Allow-Credentials: true


**Impact**
- Allows cross-origin requests from any domain
- Amplifies the impact of CSRF and unauthenticated requests

---

### Missing Authentication Enforcement

**Description**  
The endpoint processes requests without requiring valid authentication
credentials.

**Observed Behavior**
- Requests succeed without Authorization headers
- Server responds with `204 No Content`

---

## Proof of Concept 

```html
<!DOCTYPE html>
<html>
<body>
<script>
fetch("https://api.example.com/system/trace", {
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify([
    { "level": "debug", "message": "TEST_REQUEST" }
  ])
})
.then(res => console.log("Status:", res.status))
.catch(err => console.error(err));
</script>
</body>
</html>



