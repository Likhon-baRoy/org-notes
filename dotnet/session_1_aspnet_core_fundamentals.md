# Session 1 — ASP.NET Core Fundamentals

1.1 — Request Lifecycle
What is Request Lifecycle?

A request lifecycle means: The complete journey of an HTTP request from frontend/browser to backend and back as a response.

Example request: GET /employees

This request does not magically go to controller. Many things happen internally before controller executes.

Understanding this is important because:
When APIs fail,
you can debug properly.

You understand:
Where request breaks
Who checks authentication
How routing works
Why middleware matters

Many beginners only learn: Controller → DB → Response

But professional backend developers understand the full request pipeline.

Real Request Flow

Imagine:

GET /employees

Full journey:

1. Frontend/browser/mobile
   creates HTTP request

2. DNS resolves domain
   into server IP

3. Request travels
   through internet

4. Reverse proxy
   (Nginx/IIS)
   receives request

5. Reverse proxy performs:
 - SSL/HTTPS handling
 - Security filtering
 - Rate limiting
 - Load balancing
 - Static file handling

6. Reverse proxy forwards
   request to Kestrel

7. ASP.NET Core server
   (Kestrel)
   receives request

8. Middleware pipeline starts

9. Routing middleware
   identifies endpoint

10. Authentication middleware
    checks identity
    Example:
    Who is this user?
    Is JWT token valid?

11. Authorization middleware
    checks permission
    Example:
    Can user access this endpoint?

12. Controller method executes

13. Service layer executes
14. Repository / EF Core executes
15. Database query runs
16. Response returns

Mental model:
Request goes through checkpoints before reaching controller.

Think: Airport security checkpoints.
You do not directly enter airplane.

You pass:
Security
Passport
Verification
Gate
Boarding

Same concept in backend requests.

Step 1 — Frontend Creates HTTP Request

Frontend/browser/app creates request.

Example: GET /employees

Frontend can be:
Angular
Mobile app
Browser
Postman
Another backend service

Example request: GET /employees
Authorization: Bearer token123
Contains:
URL
HTTP method
Headers
Authentication token
Body (if exists)

Step 2 — DNS Resolves Server

Example:

Frontend calls: api.myerp.com
Computer cannot understand domain.

Needs: IP Address

DNS converts: api.myerp.com -> 192.168.x.x

Think: DNS = internet phonebook

Step 3 — Reverse Proxy Receives Request

Usually: Request does NOT directly hit ASP.NET Core.

Usually:

Internet
↓
Nginx / IIS
↓
Kestrel
↓
ASP.NET Core App

Why?

Because reverse proxy helps:
Security
SSL/HTTPS
Load balancing
Performance
Rate limiting

Example:

Nginx receives: GET /employees

Then forwards to: Kestrel

Step 4 — Kestrel Receives Request

Kestrel is: ASP.NET Core built-in web server.

Think: Main server engine that runs your API.
(.NET is engine to run C#)

Here: Kestrel runs the web application.

Step 5 — Middleware Pipeline

Before controller: Middleware runs first.

Many beginners think:

Request
↓
Controller

Wrong.

Correct:

Request
↓
Middleware
↓
Controller

# What is Reverse Proxy?

A reverse proxy is:

```txt
A middleman server
that sits in front
of your backend server.
```

Without reverse proxy:

```txt
Internet
↓
Kestrel directly exposed
```

Not ideal for production.

Production setup:

```txt
Internet
↓
Nginx / IIS
↓
Kestrel
↓
ASP.NET Core App
```

Think:

```txt
Hotel receptionist
```

Customer says:

```txt
Need room 301
```

Receptionist:

```txt
Checks rules
Checks security
Forwards request
```

Receptionist:

```txt
Reverse proxy
```

Room:

```txt
Kestrel/API
```

## Why Use Reverse Proxy?

### 1. HTTPS / SSL Handling

Browser:

```txt
https://api.company.com
```

Nginx handles:

```txt
SSL certificate
HTTPS encryption
```

Then forwards internally:

```txt
HTTP → Kestrel
```

Kestrel stays simple.

---

### 2. Security

Can block:

```txt
Malicious requests
Bad bots
Rate limiting abuse
DDoS
```

Think:

```txt
Security guard
before office entrance
```

---

### 3. Load Balancing

Can distribute traffic.

Example:

```txt
Request 1 → Server A
Request 2 → Server B
Request 3 → Server C
```

Useful in enterprise systems.

---

### 4. Static File Serving

Nginx serves:

```txt
Images
CSS
JavaScript
```

faster than ASP.NET Core.

---

## Why Called "Reverse" Proxy?

Normal proxy:

```txt
Represents client
```

Example:

```txt
You
↓
Proxy/VPN
↓
Internet
```

Reverse proxy:

```txt
Represents server
```

Example:

```txt
Internet
↓
Nginx
↓
Backend
```

Client thinks:

```txt
Talking directly to server
```

Actually:

```txt
Reverse proxy forwards request
to backend server
```

## Why This is Safer

Better:

```txt
Internet
↓
Nginx
↓
Kestrel
```

Because reverse proxy handles:

```txt
SSL certificate
HTTPS encryption
Security filtering
Rate limiting
Load balancing
Static files
```

Kestrel remains protected and focused on application logic.

What is Middleware?
Think of middleware as: Request checkpoints/pipeline

Every request passes through middleware before controller.
Think airport security.

Request comes: GET /employees

Typical middleware flow:

```txt
Request
↓
Exception middleware
(error handling)

↓
Logging middleware
(request tracking)

↓
Routing middleware
(find matching endpoint)

↓
Authentication middleware
(check identity)

↓
Authorization middleware
(check permission)

↓
Controller
↓
Service
↓
Database
↓
Response
```
### Important Note

Middleware order matters.

Wrong order can break application behavior.

Example:

Bad:

```txt
Authorization
↓
Routing
```

Problem:

```txt
Authorization does not know
which endpoint user wants.
```

Correct:

```txt
Routing
↓
Authorization
```

Because routing identifies:

```txt
Which controller?
Which method?
What permissions required?
```

Then authorization checks access.

### Key Idea

Middleware can:

```txt
1. Continue request
2. Modify request
3. Stop request
4. Return response early
```

Example:

Invalid JWT token:

```txt
GET /employees
```

Authentication middleware:

```txt
Token invalid
```

Response:

```txt
401 Unauthorized
```

Controller:

```txt
Never executes
```

This saves:

```txt
CPU
Database execution
Resources
Security risk
```

* Common Middlewares

Logging Middleware

Logs request:
GET /employees
Status: 200
Time: 150ms

Useful for:
Debugging
Performance monitoring
Error tracking

Authentication Middleware
Checks:
JWT token
Cookie
Identity

Valid user? OR, Who are you?

Example:
Request: GET /employees
Authorization: Bearer ey123...

Middleware checks:
Is token valid?
Expired?
Tampered?

If invalid: 401 Unauthorized

Controller: never executes

Very important concept.

Authorization Middleware

Checks: What are you allowed to do?

Example: [Authorize(Roles = "Admin")]

User role: Employee
Required: Admin
Result: 403 Forbidden
Again: Controller never executes

Exception middleware

If app crashes:

Bad: Application crash

Better:
{
  "message": "Something went wrong"
}

Returns proper response.

CORS middleware

Checks: Can Angular app call this API?

Example: frontend.com

allowed?

Why Middleware Runs Before Controller?

Reason:
Security
Validation
Performance
Protection

Bad request should stop early.

Example:

Without middleware: DELETE /employees/5

Controller executes first.

Employee deleted.

Then auth checked.

Too late.

Correct flow:

Authentication first
Controller later

Step 6 — Routing
After middleware: Routing runs.

Routing means: Finding matching controller method.

Example:

Request: GET /employees

Controller:

[Route("employees")]

[HttpGet]
public IActionResult Get()
{
}

Internally ASP.NET builds: GET /employees → EmployeeController.Get()

Think: Dictionary lookup

Request key: GET /employees

Match found?

Execute method.

404 vs 405

404 Not Found

Example: GET /unknown

No route exists.

Result: 404 Not Found

405 Method Not Allowed

Example:

Controller: GET /employees
Request: POST /employees

Route exists.

Method wrong.

Result: 405 Method Not Allowed

Common misconception: 404 and 405 same thing

Mental Model Summary

Think:

Request
↓
Internet
↓
Nginx
↓
Kestrel
↓
Middleware starts
↓
Routing identifies endpoint
↓
Authentication
↓
Authorization
↓
Controller
↓
Database
↓
Response

Interview Note

Question: What happens when request reaches ASP.NET Core?

Good answer:
Request goes through middleware pipeline,
routing finds matching endpoint,
authentication/authorization happens,
controller executes,
service/database runs,
response returns.

1.2 — HTTP Methods & REST Semantics
Why Different HTTP Methods Exist?

Reason:
Different operations
need different meanings.

Instead of:

/employee?action=create

REST APIs use semantics.

Think:

Method itself
tells intention.
GET

Purpose:

Read/fetch data

Example:

GET /employees

Response:

Employee list

Characteristics:

Read only
Should not modify data
Safe operation

Example:

Refresh page 10 times
No employee deleted
POST

Purpose:

Create new data

Example:

POST /employees

Body:

{
  "name": "Rahim"
}

Result:

New employee created

Response:

201 Created
PUT

Purpose:

Update existing data

Example:

PUT /employees/5

Meaning:

Update employee 5
DELETE

Purpose:

Remove data

Example:

DELETE /employees/5

Response:

204 No Content

Because:

Success
No body needed
REST Semantics

Professional APIs care about:

Correct meaning
Correct status codes
Predictable behavior

Example:

GET → Ok()

POST → Created()

PUT → Ok()/NoContent()

DELETE → NoContent()
Common Mistake

Wrong:

GET /deleteEmployee/5

Bad API design.

Correct:

DELETE /employees/5
1.3 — Authentication vs Authorization

This is one of the most confusing beginner topics.

Authentication

Question:

Who are you?

Example:

User login.

Check:

Username
Password
JWT token

Valid?

Result:

User authenticated

If not:

401 Unauthorized

Important correction from learning:

401
=
Not authenticated

Think:

Identity verification
Authorization

Question:

What can you do?

Example:

Endpoint:

DELETE /employees/5

Only admin allowed.

User:

Employee

Result:

403 Forbidden

Important correction:

Authenticated
BUT not permitted

Mental model:

Authentication
=
Who are you?

Authorization
=
What can you access?
Real ERP Example

Employee:

Can view profile

Manager:

Can approve leave

Admin:

Can delete employee

Same login.

Different permissions.

Common Misconception

Wrong:

401 means no permission

Wrong.

Correct:

401
=
Not logged in

403
=
Logged in
but forbidden
Revision Summary — Part 1
Request lifecycle
=
Journey of request

Middleware
=
Checkpoints before controller

Routing
=
Find matching endpoint

404
=
Route not found

405
=
Method not allowed

GET
=
Fetch data

POST
=
Create

PUT
=
Update

DELETE
=
Remove

Authentication
=
Who are you?

Authorization
=
What can you do?

401
=
Not authenticated

403
=
No permission
