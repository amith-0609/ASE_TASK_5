# BankingJWT — Skill Experiment 4

This is a ready-to-run implementation of the uploaded **SOA Programming and Microservices — Skill Experiment 4: Token-Based Authentication for Banking API**.

The source document specifies:
- Java 17
- Spring Web
- Spring Security
- Spring Boot DevTools
- JJWT 0.12.6
- `POST /login`
- protected `GET /account/details`
- valid, invalid, no-token, and expired-token tests.

## Requirements

Install:
1. Java 17
2. VS Code
3. Extension: Extension Pack for Java
4. Maven (or use the Maven wrapper if you add one)
5. Postman

## Open in VS Code

1. Extract the ZIP.
2. VS Code → File → Open Folder.
3. Select the extracted `BankingJWT` folder.
4. Wait for Maven/Java dependencies to finish loading.

## Run

Open:
`src/main/java/com/klh/bankingjwt/BankingJwtApplication.java`

Run the `main()` method.

The API starts on:
`http://localhost:8080`

## Test 1 — Login

In Postman:

POST
`http://localhost:8080/login`

Body → raw → JSON:

```json
{
  "username": "admin",
  "password": "1234"
}
```

Click Send.

A long JWT string should be returned.

## Test 2 — Valid token

GET
`http://localhost:8080/account/details`

Postman:
Authorization → Bearer Token → paste the JWT from Test 1.

Expected response:

`Account Number: 1234567890, Balance: ₹50,000`

## Test 3 — Invalid token

Use the same GET request, but set Bearer Token to:

`this_is_an_invalid_token`

Expected result:
`401 Unauthorized`

## Test 4 — No token

Authorization → No Auth.

Send:

GET
`http://localhost:8080/account/details`

Expected result:
`401 Unauthorized`

## Test 5 — Expired token

Open `JwtUtil.java`.

Temporarily change:

```java
.expiration(new Date(System.currentTimeMillis() + 600000))
```

to:

```java
.expiration(new Date(System.currentTimeMillis() + 1000))
```

Save and restart the application.

1. POST `/login` again.
2. Copy the new token.
3. Wait 2–3 seconds.
4. GET `/account/details` with that token.

Expected result:
`401 Unauthorized`

After the experiment, change `1000` back to `600000` and restart the application.

## Postman collection

The `postman/BankingJWT-Skill-Experiment-4.postman_collection.json` file contains the five requests.

## Important

This project is for a classroom demonstration. The username/password and JWT secret are intentionally simple because they follow the experiment. Do not use these demo credentials or a hard-coded secret in a real banking application.
