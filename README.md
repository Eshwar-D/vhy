# API Route Test Cases

Use these as manual smoke tests in Postman, Insomnia, or curl.

## Base Setup
- Base URL: `http://localhost:8000`
- Auth header when needed: `Authorization: Bearer <accessToken>`
- Cookie auth also works for routes that read `accessToken` from cookies.

## 1) Health Check
- Method: `GET`
- URL: `/health`
- Auth: none
- Expected: `200 OK`
- Expected body: `{ "status": "OK", "message": "Server started" }`

## 2) Auth

### Register
- Method: `POST`
- URL: `/v1/api/auth/register`
- Body:
```json
{
  "phonenumber": "9876543210",
  "name": "Test User",
  "role": "Shop"
}
```
- Expected: `200` or `201`
- Expected: user created or already exists message

### Login
- Method: `POST`
- URL: `/v1/api/auth/login`
- Body:
```json
{
  "phonenumber": "9876543210"
}
```
- Expected: `200 OK`
- Expected: OTP flow started

### Verify OTP
- Method: `POST`
- URL: `/v1/api/auth/verify`
- Body:
```json
{
  "phonenumber": "9876543210",
  "otp": "123456"
}
```
- Expected: `200 OK`
- Expected: access/refresh token set

### Refresh Token
- Method: `POST`
- URL: `/v1/api/auth/refresh`
- Auth: refresh cookie
- Expected: `200 OK`

### Logout
- Method: `POST`
- URL: `/v1/api/auth/logout`
- Auth: access token or cookie
- Expected: `200 OK`

### Me
- Method: `GET`
- URL: `/v1/api/auth/me`
- Auth: access token
- Expected: `200 OK`

### Update Profile
- Method: `PUT`
- URL: `/v1/api/auth/update-profile`
- Auth: access token
- Body:
```json
{
  "name": "Updated User",
  "location": {
    "lat": 12.9716,
    "lng": 77.5946
  }
}
```
- Expected: `200 OK`

## 3) User

### Get Current User
- Method: `GET`
- URL: `/v1/api/user/me`
- Auth: access token
- Expected: `200 OK`

### Get User By ID
- Method: `GET`
- URL: `/v1/api/user/<userId>`
- Auth: access token
- Expected: `200 OK`

### Update User
- Method: `PUT`
- URL: `/v1/api/user/update`
- Auth: access token
- Body:
```json
{
  "name": "Updated Name",
  "location": {
    "lat": 12.9716,
    "lng": 77.5946
  }
}
```
- Expected: `200 OK`

### Get User By Role
- Method: `GET`
- URL: `/v1/api/user/role/Shop`
- Auth: access token
- Expected: `200 OK`

## 4) Shops

### List Shops
- Method: `GET`
- URL: `/api/shops`
- Auth: none
- Expected: `200 OK`

### Nearby Shops
- Method: `GET`
- URL: `/api/shops/nearby?lat=12.9716&lng=77.5946&radius=10`
- Auth: none
- Expected: `200 OK`

### Shop By ID
- Method: `GET`
- URL: `/api/shops/<shopId>`
- Auth: none
- Expected: `200 OK` or `404` if not found

## 5) Listings

### Get All Listings
- Method: `GET`
- URL: `/v1/api/list/getlist`
- Auth: access token
- Expected: `200 OK`

### Get Listings By Mandi ID
- Method: `GET`
- URL: `/v1/api/list/mandi/<mandiId>`
- Auth: access token
- Expected: `200 OK` or `404` if empty

### Get Listings By Seller ID
- Method: `GET`
- URL: `/v1/api/list/seller/<sellerId>`
- Auth: access token
- Expected: `200 OK` or `404` if empty

### Create Listing
- Method: `POST`
- URL: `/v1/api/list/create`
- Auth: access token
- Body example:
```json
{
  "fishName": "Rohu",
  "quantity": 25,
  "pricePerKg": 220,
  "freshnessStatus": "Fresh",
  "description": "Fresh catch",
  "image": "https://example.com/rohu.jpg",
  "location": {
    "type": "Point",
    "coordinates": [77.5946, 12.9716]
  }
}
```
- Expected: `201 Created`

## 6) Fish

### List Fish
- Method: `GET`
- URL: `/api/fish`
- Auth: none
- Expected: `200 OK`

### Fish By ID
- Method: `GET`
- URL: `/api/fish/<fishId>`
- Auth: none
- Expected: `200 OK`

### Create Fish
- Method: `POST`
- URL: `/api/fish`
- Auth: admin token
- Body example:
```json
{
  "name": "Rohu",
  "category": "Freshwater",
  "pricePerKg": 220,
  "availableQty": 50,
  "description": "Fresh stock"
}
```
- Expected: `201 Created`

### Toggle Stock
- Method: `PATCH`
- URL: `/api/fish/<fishId>/toggle-stock`
- Auth: admin token
- Expected: `200 OK`

## 7) Upcoming Fish

### List Upcoming Fish
- Method: `GET`
- URL: `/api/upcoming-fish`
- Auth: admin token
- Expected: `200 OK`

### Create Upcoming Fish
- Method: `POST`
- URL: `/api/upcoming-fish`
- Auth: admin token
- Body example:
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
- Expected: `201 Created`

### Move To Available
- Method: `POST`
- URL: `/api/upcoming-fish/<id>/move-to-available`
- Auth: admin token
- Expected: `200 OK`

## 8) Orders

### Summary
- Method: `GET`
- URL: `/api/orders/summary`
- Auth: admin token
- Expected: `200 OK`

### List Orders
- Method: `GET`
- URL: `/api/orders`
- Auth: access token
- Expected: `200 OK`

### Create Order
- Method: `POST`
- URL: `/api/orders`
- Auth: buyer token
- Body example:
```json
{
  "fishId": "<fishId>",
  "qtyKg": 2,
  "notes": "Deliver in morning"
}
```
- Expected: `201 Created`

### Get Order By ID
- Method: `GET`
- URL: `/api/orders/<orderId>`
- Auth: access token
- Expected: `200 OK`

### Update Order Status
- Method: `PATCH`
- URL: `/api/orders/<orderId>/status`
- Auth: admin token
- Body example:
```json
{
  "status": "Delivered"
}
```
- Expected: `200 OK`

## 9) Buyers

### List Buyers
- Method: `GET`
- URL: `/api/buyers`
- Auth: admin token
- Expected: `200 OK`

### Buyer By ID
- Method: `GET`
- URL: `/api/buyers/<buyerId>`
- Auth: admin token
- Expected: `200 OK`

### Create Buyer
- Method: `POST`
- URL: `/api/buyers`
- Auth: admin token
- Expected: `200` or `201`

## 10) Payments

### Summary
- Method: `GET`
- URL: `/api/payments/summary`
- Auth: admin token
- Expected: `200 OK`

### List Payments
- Method: `GET`
- URL: `/api/payments`
- Auth: admin token
- Expected: `200 OK`

### Mark Paid
- Method: `PATCH`
- URL: `/api/payments/<paymentId>/mark-paid`
- Auth: admin token
- Expected: `200 OK`

## 11) Analytics

### Dashboard
- Method: `GET`
- URL: `/api/analytics/dashboard`
- Auth: admin token
- Expected: `200 OK`

### Revenue
- Method: `GET`
- URL: `/api/analytics/revenue?period=daily`
- Auth: admin token
- Expected: `200 OK`

### Top Fish
- Method: `GET`
- URL: `/api/analytics/top-fish`
- Auth: admin token
- Expected: `200 OK`

### Top Buyers
- Method: `GET`
- URL: `/api/analytics/top-buyers`
- Auth: admin token
- Expected: `200 OK`

### Category Mix
- Method: `GET`
- URL: `/api/analytics/category-mix`
- Auth: admin token
- Expected: `200 OK`

## 12) Notifications

### List Notifications
- Method: `GET`
- URL: `/api/notifications`
- Auth: admin token
- Expected: `200 OK`

### Mark Read
- Method: `PATCH`
- URL: `/api/notifications/mark-read`
- Auth: admin token
- Expected: `200 OK`

## 13) Settings

### Get Settings
- Method: `GET`
- URL: `/api/settings`
- Auth: admin token
- Expected: `200 OK`

### Update Settings
- Method: `PUT`
- URL: `/api/settings`
- Auth: admin token
- Body example:
```json
{
  "name": "Admin Updated"
}
```
- Expected: `200 OK`

## Quick Negative Tests
- Call a protected route without token
- Expected: `401 Unauthorized`

- Call an admin-only route with non-admin token
- Expected: `403 Forbidden`

- Send invalid body to login/register/update routes
- Expected: `400 Bad Request`

- Request a missing resource by ID
- Expected: `404 Not Found`

## Admin Routes For Postman

Use an admin access token for all routes below unless noted otherwise.

### Route Prefix Mapping
- Frontend route: `GET /v1/api/analytics/dashboard` -> Backend route: `GET /api/analytics/dashboard`
- Frontend route: `GET /v1/api/notifications` -> Backend route: `GET /api/notifications`
- Frontend route: `PATCH /v1/api/notifications/mark-read` -> Backend route: `PATCH /api/notifications/mark-read`
- Frontend route: `GET /v1/api/fish` -> Backend route: `GET /api/fish`
- Frontend route: `POST /v1/api/fish` -> Backend route: `POST /api/fish`
- Frontend route: `PUT /v1/api/fish/:id` -> Backend route: `PUT /api/fish/:id`
- Frontend route: `DELETE /v1/api/fish/:id` -> Backend route: `DELETE /api/fish/:id`
- Frontend route: `PATCH /v1/api/fish/:id/toggle-stock` -> Backend route: `PATCH /api/fish/:id/toggle-stock`
- Frontend route: `GET /v1/api/upcoming-fish` -> Backend route: `GET /api/upcoming-fish`
- Frontend route: `POST /v1/api/upcoming-fish` -> Backend route: `POST /api/upcoming-fish`
- Frontend route: `DELETE /v1/api/upcoming-fish/:id` -> Backend route: `DELETE /api/upcoming-fish/:id`
- Frontend route: `POST /v1/api/upcoming-fish/:id/move-to-available` -> Backend route: `POST /api/upcoming-fish/:id/move-to-available`
- Frontend route: `GET /v1/api/orders` -> Backend route: `GET /api/orders`
- Frontend route: `PATCH /v1/api/orders/:id/status` -> Backend route: `PATCH /api/orders/:id/status`
- Frontend route: `GET /v1/api/buyers` -> Backend route: `GET /api/buyers`
- Frontend route: `POST /v1/api/buyers` -> Backend route: `POST /api/buyers`
- Frontend route: `PATCH /v1/api/buyers/:id/toggle-status` -> Backend route: `PATCH /api/buyers/:id/toggle-status`
- Frontend route: `GET /v1/api/payments` -> Backend route: `GET /api/payments`
- Frontend route: `GET /v1/api/payments/summary` -> Backend route: `GET /api/payments/summary`
- Frontend route: `PATCH /v1/api/payments/:id/mark-paid` -> Backend route: `PATCH /api/payments/:id/mark-paid`
- Frontend route: `GET /v1/api/analytics/revenue?period=daily|weekly` -> Backend route: `GET /api/analytics/revenue?period=daily|weekly`
- Frontend route: `GET /v1/api/analytics/top-fish` -> Backend route: `GET /api/analytics/top-fish`
- Frontend route: `GET /v1/api/analytics/top-buyers` -> Backend route: `GET /api/analytics/top-buyers`
- Frontend route: `GET /v1/api/analytics/category-mix` -> Backend route: `GET /api/analytics/category-mix`
- Frontend route: `GET /v1/api/settings` -> Backend route: `GET /api/settings`
- Frontend route: `PUT /v1/api/settings` -> Backend route: `PUT /api/settings`
- Frontend route: `POST /v1/api/auth/logout` -> Backend route: `POST /v1/api/auth/logout`

### Common Headers
- `Authorization: Bearer <admin_access_token>`
- `Content-Type: application/json`

### Admin Smoke Test Order

1. `GET /api/analytics/dashboard`
2. `GET /api/orders`
3. `GET /api/buyers`
4. `GET /api/payments/summary`
5. `GET /api/settings`
6. `GET /api/notifications`
7. `GET /api/fish`
8. `GET /api/upcoming-fish`

### Example Requests

#### GET /api/analytics/dashboard
- Auth: admin token
- Expected: `200 OK`
- Example response: dashboard totals and summary cards

#### GET /api/notifications
- Auth: admin token
- Expected: `200 OK`
- Example response: notification list

#### PATCH /api/notifications/mark-read
- Auth: admin token
- Body:
```json
{}
```
- Expected: `200 OK`
- Example response: all notifications marked read

#### GET /api/fish
- Auth: admin token or none depending on server rules
- Expected: `200 OK`
- Example response: fish array

#### POST /api/fish
- Auth: admin token
- Body:
```json
{
  "name": "Rohu",
  "category": "Freshwater",
  "pricePerKg": 220,
  "availableQty": 50,
  "description": "Fresh stock"
}
```
- Expected: `201 Created`

#### PUT /api/fish/:id
- Auth: admin token
- Body:
```json
{
  "name": "Rohu Premium",
  "pricePerKg": 240,
  "availableQty": 45
}
```
- Expected: `200 OK`

#### DELETE /api/fish/:id
- Auth: admin token
- Expected: `200 OK`

#### PATCH /api/fish/:id/toggle-stock
- Auth: admin token
- Expected: `200 OK`

#### GET /api/upcoming-fish
- Auth: admin token
- Expected: `200 OK`

#### POST /api/upcoming-fish
- Auth: admin token
- Body:
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
- Expected: `201 Created`

#### DELETE /api/upcoming-fish/:id
- Auth: admin token
- Expected: `200 OK`

#### POST /api/upcoming-fish/:id/move-to-available
- Auth: admin token
- Body example:
```json
{
  "category": "Freshwater",
  "description": "Moved from upcoming stock",
  "imageUrl": "https://example.com/hilsa.jpg"
}
```
- Expected: `200 OK`

#### GET /api/orders
- Auth: admin token
- Expected: `200 OK`

#### PATCH /api/orders/:id/status
- Auth: admin token
- Body:
```json
{
  "status": "Delivered"
}
```
- Expected: `200 OK`

#### GET /api/buyers
- Auth: admin token
- Expected: `200 OK`

#### POST /api/buyers
- Auth: admin token
- Body example:
```json
{
  "name": "Buyer A",
  "phone": "9876543210",
  "businessName": "A Seafood Store"
}
```
- Expected: `200` or `201`

#### PATCH /api/buyers/:id/toggle-status
- Auth: admin token
- Expected: `200 OK`

#### GET /api/payments
- Auth: admin token
- Expected: `200 OK`

#### GET /api/payments/summary
- Auth: admin token
- Expected: `200 OK`

#### PATCH /api/payments/:id/mark-paid
- Auth: admin token
- Expected: `200 OK`

#### GET /api/analytics/revenue?period=daily
- Auth: admin token
- Expected: `200 OK`

#### GET /api/analytics/top-fish
- Auth: admin token
- Expected: `200 OK`

#### GET /api/analytics/top-buyers
- Auth: admin token
- Expected: `200 OK`

#### GET /api/analytics/category-mix
- Auth: admin token
- Expected: `200 OK`

#### GET /api/settings
- Auth: admin token
- Expected: `200 OK`

#### PUT /api/settings
- Auth: admin token
- Body example:
```json
{
  "name": "Admin Updated"
}
```
- Expected: `200 OK`

#### POST /v1/api/auth/logout
- Auth: access token or cookie
- Expected: `200 OK`

### Expected Errors
- No token on admin route: `401 Unauthorized`
- Non-admin token on admin route: `403 Forbidden`
- Missing resource ID: `404 Not Found`
- Bad request body: `400 Bad Request`
