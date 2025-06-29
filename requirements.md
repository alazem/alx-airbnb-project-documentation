### 📄 `requirements.md`

---

# Backend Feature Requirement Specifications

This document outlines the **technical** and **functional** specifications for the core backend features of the ALX Airbnb Clone project.

---

## 1. 🔐 User Authentication

### 🎯 Purpose

To allow users to securely sign up, log in, and manage their sessions.

### ✅ Features

* Register new users
* Log in existing users
* Log out users
* Token-based authentication

### 🧩 API Endpoints

| Method | Endpoint             | Description                        |
| ------ | -------------------- | ---------------------------------- |
| POST   | `/api/auth/register` | Register a new user                |
| POST   | `/api/auth/login`    | Authenticate user and return token |
| POST   | `/api/auth/logout`   | Invalidate token and log out       |

### 🧾 Input / Output Specifications

#### 🔸 `POST /api/auth/register`

**Input (JSON):**

```json
{
  "first_name": "John",
  "last_name": "Doe",
  "email": "john@example.com",
  "password": "securePass123"
}
```

**Validation Rules:**

* Email must be unique and valid format
* Password must be at least 8 characters

**Output (201):**

```json
{
  "message": "User registered successfully",
  "user_id": "uuid"
}
```

#### 🔸 `POST /api/auth/login`

**Input:**

```json
{
  "email": "john@example.com",
  "password": "securePass123"
}
```

**Output (200):**

```json
{
  "access_token": "jwt_token",
  "token_type": "Bearer"
}
```

### ⚙️ Performance Criteria

* Token generation time < 500ms
* Input sanitization to prevent SQLi/XSS
* Rate limiting (e.g., max 5 failed logins per 10 min)

---

## 2. 🏠 Property Management

### 🎯 Purpose

Enable hosts to list, update, and delete rental properties.

### ✅ Features

* Add a property
* Update property details
* Delete a property
* View property listings

### 🧩 API Endpoints

| Method | Endpoint               | Description        |
| ------ | ---------------------- | ------------------ |
| POST   | `/api/properties/`     | Add a new property |
| GET    | `/api/properties/`     | Get all properties |
| GET    | `/api/properties/{id}` | Get property by ID |
| PUT    | `/api/properties/{id}` | Update property    |
| DELETE | `/api/properties/{id}` | Delete property    |

### 🧾 Input / Output Specifications

#### 🔸 `POST /api/properties/`

**Input (JSON):**

```json
{
  "title": "Cozy Cottage",
  "description": "A beautiful cottage in the woods.",
  "location": "Lake Tahoe",
  "price_per_night": 100.0,
  "host_id": "uuid"
}
```

**Validation Rules:**

* Price must be a positive float
* Title and location required

**Output (201):**

```json
{
  "property_id": "uuid",
  "message": "Property listed successfully"
}
```

### ⚙️ Performance Criteria

* Max response time < 700ms
* Secure access: only property owners can update/delete
* Paginated property list endpoint

---

## 3. 📅 Booking System

### 🎯 Purpose

Allow users to book properties for specific dates.

### ✅ Features

* Book a property
* Cancel a booking
* View user bookings

### 🧩 API Endpoints

| Method | Endpoint             | Description           |
| ------ | -------------------- | --------------------- |
| POST   | `/api/bookings/`     | Create a new booking  |
| GET    | `/api/bookings/`     | Get bookings for user |
| DELETE | `/api/bookings/{id}` | Cancel a booking      |

### 🧾 Input / Output Specifications

#### 🔸 `POST /api/bookings/`

**Input (JSON):**

```json
{
  "user_id": "uuid",
  "property_id": "uuid",
  "start_date": "2025-07-01",
  "end_date": "2025-07-05"
}
```

**Validation Rules:**

* Start date must be today or later
* End date must be after start date
* Property must not be double-booked

**Output (201):**

```json
{
  "booking_id": "uuid",
  "message": "Booking confirmed"
}
```

### ⚙️ Performance Criteria

* Booking conflict check in < 300ms
* Max 3 seconds latency for complex date queries
* Only the booking user or admin can cancel

---

## ✅ Security & General Requirements

* **Authentication:** All endpoints (except register/login) require JWT.
* **Authorization:** Role-based access control (host vs. guest).
* **Error Handling:** Consistent error responses with HTTP codes.
* **Scalability:** System should handle at least 100 concurrent users.

---

