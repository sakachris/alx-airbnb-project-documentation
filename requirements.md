
# 📌 Backend Requirement Specifications

## 📋 Overview

This document outlines the **API endpoints**, **input/output specifications**, **validation rules**, and **performance criteria** for the core backend modules of a property booking platform:

- **User Authentication**
- **Property Management**
- **Booking System**
- **Payments**
- **Reviews and Ratings**
- **Admin Panel APIs**

---

## 🔐 1. User Authentication

### 📘 Description
Handles user registration, login, logout, and token-based authentication using JWT.

### 📄 Endpoints
| Method | Endpoint              | Description            |
|--------|-----------------------|------------------------|
| POST   | `/api/auth/register`  | Register a new user    |
| POST   | `/api/auth/login`     | Log in and get token   |
| POST   | `/api/auth/logout`    | Invalidate token       |
| GET    | `/api/auth/profile`   | Get user profile       |

### 🔠 Input/Output Specifications
**POST `/api/auth/register`**
```json
{
  "username": "johndoe",
  "email": "john@example.com",
  "password": "StrongP@ssw0rd"
}
```
**Output:**
```json
{
  "id": 1,
  "username": "johndoe",
  "email": "john@example.com",
  "token": "JWT-TOKEN-HERE"
}
```

**POST `/api/auth/login`**
```json
{
  "email": "john@example.com",
  "password": "StrongP@ssw0rd"
}
```
**Output:**
```json
{
  "token": "JWT-TOKEN-HERE"
}
```

### ✅ Validation Rules
- Unique email and username
- Password >= 8 chars, 1 digit, 1 symbol

### 🚀 Performance
- Response time < 300ms
- Secure password hashing (bcrypt)

---

## 🏠 2. Property Management

### 📘 Description
Hosts can manage property listings (CRUD).

### 📄 Endpoints
| Method | Endpoint                | Description                  |
|--------|-------------------------|------------------------------|
| GET    | `/api/properties/`      | List properties              |
| GET    | `/api/properties/{id}`  | Property details             |
| POST   | `/api/properties/`      | Create new property          |
| PUT    | `/api/properties/{id}`  | Update property              |
| DELETE | `/api/properties/{id}`  | Delete property              |

### 🔠 Input/Output
**POST `/api/properties/`**
```json
{
  "title": "Ocean View Apartment",
  "location": "Mombasa",
  "price_per_night": 75,
  "max_guests": 4,
  "amenities": ["WiFi", "Kitchen"],
  "images": ["img1.jpg"]
}
```
**Output:**
```json
{
  "id": 101,
  "title": "Ocean View Apartment",
  "host": "johndoe"
}
```

### ✅ Validation Rules
- Positive price
- Required title, location, image

### 🚀 Performance
- Paginated listing (limit/offset)
- Media hosted on CDN/S3

---

## 🗓️ 3. Booking System

### 📘 Description
Guests can book available properties.

### 📄 Endpoints
| Method | Endpoint                    | Description             |
|--------|-----------------------------|-------------------------|
| POST   | `/api/bookings/`            | Create booking          |
| GET    | `/api/bookings/`            | List bookings           |
| GET    | `/api/bookings/{id}/`       | Booking details         |
| DELETE | `/api/bookings/{id}/cancel` | Cancel a booking        |

### 🔠 Input/Output
**POST `/api/bookings/`**
```json
{
  "property_id": 101,
  "check_in": "2025-06-01",
  "check_out": "2025-06-05",
  "guests": 2,
  "payment_token": "tok_visa_1234"
}
```
**Output:**
```json
{
  "id": 1001,
  "property": "Ocean View Apartment",
  "status": "confirmed"
}
```

### ✅ Validation Rules
- Valid date range
- Guest count <= max_guests
- No overlapping bookings

### 🚀 Performance
- Availability check < 300ms
- Booking create < 500ms

---

## 💳 4. Payment Integration

### 📘 Description
Integrates with Stripe/PayPal for secure transactions.

### 📄 Endpoints
| Method | Endpoint               | Description              |
|--------|------------------------|--------------------------|
| POST   | `/api/payments/verify` | Verify payment token     |
| POST   | `/api/payments/webhook`| Handle payment webhooks  |

### ✅ Validation Rules
- Token must be valid and match booking
- Prevent double payments

### 🚀 Performance
- Webhook response < 200ms
- Async retry logic on failures

---

## ⭐ 5. Reviews and Ratings

### 📘 Description
Guests can review and rate properties after their stay.

### 📄 Endpoints
| Method | Endpoint               | Description         |
|--------|------------------------|---------------------|
| POST   | `/api/reviews/`        | Add review          |
| GET    | `/api/reviews/{id}`    | Get reviews         |
| DELETE | `/api/reviews/{id}`    | Delete own review   |

### 🔠 Input
```json
{
  "property_id": 101,
  "rating": 4,
  "comment": "Great location!"
}
```

### ✅ Validation Rules
- One review per guest per stay
- Rating: 1-5 only

---

## 📈 6. Admin Panel APIs

### 📘 Description
Admins manage users, bookings, properties, and reviews.

### 📄 Endpoints
| Method | Endpoint              | Description              |
|--------|-----------------------|--------------------------|
| GET    | `/api/admin/users/`   | List all users           |
| DELETE | `/api/admin/users/{id}` | Deactivate user        |
| GET    | `/api/admin/stats/`   | Platform statistics      |

### 🚀 Performance
- Caching for stats endpoints
- Paginated lists for large data sets
