# Session 1 — ASP.NET Core Fundamentals

## 1.1 Request Lifecycle
Frontend request:
```txt
GET /employees
```

Flow:
1. Browser/Frontend creates HTTP request
2. DNS resolves server IP
3. Request travels through internet
4. Nginx/Reverse proxy receives request
5. ASP.NET Core (Kestrel) receives request
6. Middleware runs
7. Routing finds endpoint
8. Authentication/Authorization (if required)
9. Controller method executes
10. Database/service call
11. Response returned

## Routing
Routing matches:
- URL
- HTTP method

Example:
```csharp
[Route("employees")]
public class EmployeeController
{
    [HttpGet]
    public IActionResult Get()
    {
        return Ok();
    }
}
```

Valid:
```txt
GET /employees
```

Invalid:
```txt
POST /employees
```
Response:
`405 Method Not Allowed`

404 = URL not found

## HTTP Methods
GET = read data
POST = create new data
PUT = update existing data
DELETE = remove data

REST example:
```txt
GET /employees
POST /employees
PUT /employees/5
DELETE /employees/5
```

## Status Codes
200 = success
400 = invalid request data
401 = not authenticated
403 = authenticated but forbidden
404 = resource not found
405 = wrong HTTP method
500 = server crash/bug

Examples:
Invalid input:
```json
{
  "name": "",
  "salary": -5000
}
```
Return:
`400 Bad Request`

Employee not found:
`404 Not Found`

Not logged in:
`401 Unauthorized`

Logged in but no permission:
`403 Forbidden`

## Authentication vs Authorization
Authentication:
"Who are you?"

Authorization:
"Are you allowed?"

401:
No login/token

403:
Logged in but permission denied
