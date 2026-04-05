# 🛠️ Admin API – Postman Test Cases

## 🔗 Base Configuration

- **Base URL:** `http://localhost:8000`
- **Authorization:** `Bearer <admin_access_token>`
- **Headers:**
  ```json
  Content-Type: application/json
  ```

---

## 📊 Analytics

### 1. Get Dashboard Stats
- **GET** `/v1/api/analytics/dashboard`
- **Auth:** Required
- **Body:** None
- **Expected:** `200 OK`
- **Use:** Admin dashboard summary cards

---

## 🔔 Notifications

### 2. Get Notifications
- **GET** `/v1/api/notifications`
- **Auth:** Required
- **Expected:** `200 OK`

### 3. Mark Notifications as Read
- **PATCH** `/v1/api/notifications/mark-read`
- **Auth:** Required
- **Body:** `{}` (optional)
- **Expected:** `200 OK`

---

## 🐟 Fish Management

### 4. Get All Fish
- **GET** `/v1/api/fish`
- **Auth:** Optional / Required (based on server config)
- **Expected:** `200 OK`

### 5. Create Fish
- **POST** `/v1/api/fish`
- **Auth:** Admin
- **Body:**
```json
{
  "name": "Rohu",
  "category": "Freshwater",
  "pricePerKg": 220,
  "availableQty": 50,
  "description": "Fresh stock"
}
```
- **Expected:** `201 Created`

### 6. Update Fish
- **PUT** `/v1/api/fish/:id`
- **Auth:** Admin
- **Body:**
```json
{
  "name": "Rohu Premium",
  "pricePerKg": 240,
  "availableQty": 45,
  "description": "Updated stock"
}
```
- **Expected:** `200 OK`

### 7. Delete Fish
- **DELETE** `/v1/api/fish/:id`
- **Auth:** Admin
- **Expected:** `200 OK`

### 8. Toggle Stock
- **PATCH** `/v1/api/fish/:id/toggle-stock`
- **Auth:** Admin
- **Expected:** `200 OK`

---

## ⏳ Upcoming Fish

### 9. Get Upcoming Fish
- **GET** `/v1/api/upcoming-fish`
- **Auth:** Admin
- **Expected:** `200 OK`

### 10. Create Upcoming Fish
- **POST** `/v1/api/upcoming-fish`
- **Auth:** Admin
- **Body:**
```json
{
  "name": "Hilsa",
  "emoji": "🐟",
  "expectedQty": 100,
  "pricePerKg": 500,
  "expectedArrivalDate": "2026-04-10T00:00:00.000Z",
  "source": "Coastal supplier",
  "status": "pending"
}
```
- **Expected:** `201 Created`

### 11. Delete Upcoming Fish
- **DELETE** `/v1/api/upcoming-fish/:id`
- **Auth:** Admin
- **Expected:** `200 OK`

### 12. Move to Available Fish
- **POST** `/v1/api/upcoming-fish/:id/move-to-available`
- **Auth:** Admin
- **Body:**
```json
{
  "category": "Freshwater",
  "description": "Moved from upcoming stock",
  "imageUrl": "https://example.com/hilsa.jpg"
}
```
- **Expected:** `200 OK`

---

## 📦 Orders

### 13. Get Orders
- **GET** `/v1/api/orders`
- **Auth:** Admin
- **Expected:** `200 OK`

### 14. Update Order Status
- **PATCH** `/v1/api/orders/:id/status`
- **Auth:** Admin
- **Body:**
```json
{
  "status": "Delivered"
}
```
- **Expected:** `200 OK`

---

## 👥 Buyers

### 15. Get Buyers
- **GET** `/v1/api/buyers`
- **Auth:** Admin
- **Expected:** `200 OK`

### 16. Create Buyer
- **POST** `/v1/api/buyers`
- **Auth:** Admin
- **Body:**
```json
{
  "name": "Buyer A",
  "phone": "9876543210",
  "businessName": "A Seafood Store"
}
```
- **Expected:** `201 Created` or `200 OK`

### 17. Toggle Buyer Status
- **PATCH** `/v1/api/buyers/:id/toggle-status`
- **Auth:** Admin
- **Expected:** `200 OK`

---

## 💳 Payments

### 18. Get Payments
- **GET** `/v1/api/payments`
- **Auth:** Admin
- **Expected:** `200 OK`

### 19. Get Payment Summary
- **GET** `/v1/api/payments/summary`
- **Auth:** Admin
- **Expected:** `200 OK`

### 20. Mark Payment as Paid
- **PATCH** `/v1/api/payments/:id/mark-paid`
- **Auth:** Admin
- **Expected:** `200 OK`

---

## 📈 Advanced Analytics

### 21. Revenue (Daily)
- **GET** `/v1/api/analytics/revenue?period=daily`
- **Auth:** Admin
- **Expected:** `200 OK`

### 22. Revenue (Weekly)
- **GET** `/v1/api/analytics/revenue?period=weekly`
- **Auth:** Admin
- **Expected:** `200 OK`

### 23. Top Fish
- **GET** `/v1/api/analytics/top-fish`
- **Auth:** Admin
- **Expected:** `200 OK`

### 24. Top Buyers
- **GET** `/v1/api/analytics/top-buyers`
- **Auth:** Admin
- **Expected:** `200 OK`

### 25. Category Mix
- **GET** `/v1/api/analytics/category-mix`
- **Auth:** Admin
- **Expected:** `200 OK`

---

## ⚙️ Settings

### 26. Get Settings
- **GET** `/v1/api/settings`
- **Auth:** Admin
- **Expected:** `200 OK`

### 27. Update Settings
- **PUT** `/v1/api/settings`
- **Auth:** Admin
- **Body:**
```json
{
  "name": "Admin Updated"
}
```
- **Expected:** `200 OK`

---

## 🔐 Auth

### 28. Logout
- **POST** `/v1/api/auth/logout`
- **Auth:** Required
- **Expected:** `200 OK`

---

## ❌ Expected Errors

| Scenario                     | Status Code        |
|----------------------------|--------------------|
| No token                   | 401 Unauthorized   |
| Non-admin token            | 403 Forbidden      |
| Missing resource ID        | 404 Not Found      |
| Invalid request body       | 400 Bad Request    |

---

## 🚀 Notes

- Always include `Authorization` header for protected routes.
- Replace `:id` with actual resource ID.
- Use Postman environments for managing tokens efficiently.
