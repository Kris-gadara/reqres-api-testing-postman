# Test Data Specifications - ReqRes REST API Testing

| Data Category          | Data Set ID | Field Name          | Test Data Value      | Expected Usage / Scenario                              |
| ---------------------- | ----------- | ------------------- | -------------------- | ------------------------------------------------------ |
| **Base Configuration** | TD_ENV_01   | `baseUrl`           | `https://reqres.in`  | Global API Host URL                                    |
|                        | TD_ENV_02   | `apiKey`            | `reqres-free-v1`     | ReqRes demo API key used by the current public service |
| **User Identifiers**   | TD_USR_01   | `userId`            | `2`                  | Valid existing user ID for GET/PUT/PATCH/DELETE        |
|                        | TD_USR_02   | `invalidUserId`     | `999`                | Non-existent user ID for negative GET 404 test         |
|                        | TD_USR_03   | `specialCharUserId` | `abc!@#`             | String / Special character user ID for negative test   |
|                        | TD_USR_04   | `zeroUserId`        | `0`                  | Out of range boundary user ID test                     |
| **Pagination**         | TD_PAG_01   | `page`              | `2`                  | Standard second page user list query                   |
|                        | TD_PAG_02   | `invalidPage`       | `-1`                 | Negative page index parameter                          |
|                        | TD_PAG_03   | `excessivePage`     | `9999`               | Empty result set pagination test                       |
| **User Creation**      | TD_POST_01  | `name`              | `morpheus`           | Valid user name for POST /api/users                    |
|                        | TD_POST_02  | `job`               | `leader`             | Valid job title for POST /api/users                    |
|                        | TD_POST_03  | `name`              | `""` (Empty string)  | Edge case payload testing                              |
| **User Update**        | TD_PUT_01   | `name`              | `morpheus`           | Name field for full update (PUT)                       |
|                        | TD_PUT_02   | `updatedJob`        | `zion resident`      | Updated job field for PUT/PATCH operations             |
| **User Registration**  | TD_REG_01   | `email`             | `eve.holt@reqres.in` | Valid user email for successful registration           |
|                        | TD_REG_02   | `password`          | `pistol`             | Valid user password for registration                   |
|                        | TD_REG_03   | `email`             | `sydney@fife`        | Valid email for registration without password          |
|                        | TD_REG_04   | `email`             | `invalid.email.com`  | Malformed email string test                            |
| **User Login**         | TD_LOG_01   | `email`             | `eve.holt@reqres.in` | Valid email for successful login                       |
|                        | TD_LOG_02   | `password`          | `cityslicka`         | Valid password for successful login                    |
|                        | TD_LOG_03   | `email`             | `peter@klaven`       | Email for missing password login (Negative)            |
|                        | TD_LOG_04   | `password`          | `wrongpass123`       | Incorrect password scenario                            |
| **Resource / Unknown** | TD_RES_01   | `resourceId`        | `2`                  | Valid existing resource ID (`fuchsia rose`)            |
|                        | TD_RES_02   | `invalidResourceId` | `23`                 | Non-existent resource ID for 404 test                  |
| **Delayed Response**   | TD_DEL_01   | `delay`             | `3`                  | Delay parameter in seconds for SLA latency test        |

---

## Payload Data Snippets

### 1. User Creation Payload (POST /api/users)

```json
{
  "name": "morpheus",
  "job": "leader"
}
```

### 2. User Complete Update Payload (PUT /api/users/2)

```json
{
  "name": "morpheus",
  "job": "zion resident"
}
```

### 3. User Partial Update Payload (PATCH /api/users/2)

```json
{
  "job": "zion resident"
}
```

### 4. User Successful Registration Payload (POST /api/register)

```json
{
  "email": "eve.holt@reqres.in",
  "password": "pistol"
}
```

### 5. User Unsuccessful Registration Payload (Missing Password)

```json
{
  "email": "sydney@fife"
}
```

### 6. User Successful Login Payload (POST /api/login)

```json
{
  "email": "eve.holt@reqres.in",
  "password": "cityslicka"
}
```

### 7. User Unsuccessful Login Payload (Missing Password)

```json
{
  "email": "peter@klaven"
}
```
