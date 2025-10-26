# 🧾 Backend Requirement Specifications

This document outlines the detailed backend requirements for three key features of the property rental and booking system.

---

## 1. 🧩 User Authentication System

### 1.1 Overview
The authentication module handles user registration, login, and authorization using JWT tokens.  
It ensures that only verified users access protected resources.

---

### 1.2 API Endpoints

| Method | Endpoint | Description |
|---------|-----------|-------------|
| `POST` | `/api/auth/register` | Register a new user (guest or host) |
| `POST` | `/api/auth/login` | Authenticate user and issue JWT token |
| `GET` | `/api/auth/verify/:token` | Verify email after registration |
| `POST` | `/api/auth/logout` | Invalidate current session token |

---

### 1.3 Input / Output Specifications

#### **POST /api/auth/register**

**Request**
```json
{
  "full_name": "John Doe",
  "email": "john@example.com",
  "password": "SecretPass123",
  "role": "guest"
}
```

**Response (201 Created)**
```json
{
  "message": "User registered successfully. Verification email sent.",
  "user_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
}
```

---

#### **POST /api/auth/login**

**Request**
```json
{
  "email": "john@example.com",
  "password": "SecretPass123"
}
```

**Response (200 OK)**
```json
{
  "access_token": "jwt.token.value",
  "expires_in": 3600
}
```

---

### 1.4 Validation Rules
- **Email:** Must be unique and valid format.  
- **Password:** Minimum 8 characters, includes uppercase letter and number.  
- **Role:** Must be `guest` or `host`.  
- **Full name:** Only alphabets and spaces (2–50 characters).  

---

### 1.5 Performance & Security
- Response time ≤ **500ms** for login/registration.  
- Passwords hashed using **bcrypt (≥12 salt rounds)**.  
- JWT tokens expire after **1 hour**.  
- Rate limiting: **5 login attempts/min per IP**.  

---

## 2. 🏠 Property Management System

### 2.1 Overview
Hosts can create, update, and delete property listings.  
Guests can view and filter available properties.

---

### 2.2 API Endpoints

| Method | Endpoint | Description |
|---------|-----------|-------------|
| `POST` | `/api/properties` | Add new property (Host only) |
| `GET` | `/api/properties` | Retrieve all properties (with filters) |
| `GET` | `/api/properties/:id` | Get property details |
| `PUT` | `/api/properties/:id` | Update property details |
| `DELETE` | `/api/properties/:id` | Delete property listing |

---

### 2.3 Input / Output Specifications

#### **POST /api/properties**

**Request**
```json
{
  "title": "Modern Apartment in Accra",
  "description": "2-bedroom apartment with ocean view",
  "price_per_night": 250.00,
  "location": "Accra, Ghana",
  "host_id": "a3e92d8a-9ef7-4e1a-9a88-45b5d1f9a3a1",
  "images": ["image1.jpg", "image2.jpg"]
}
```

**Response**
```json
{
  "message": "Property created successfully",
  "property_id": "b8a31c7f-0c11-4cfb-bb77-087d8906c845"
}
```

---

### 2.4 Validation Rules
- **Title:** 10–100 characters.  
- **Price per night:** Must be > 0.  
- **Location:** Required.  
- **Images:** At least one valid URL.  
- **Host ID:** Must exist in the users table and have `role = host`.  

---

### 2.5 Performance & Security
- Retrieval time ≤ **300ms** for up to 1,000 listings.  
- Frequently accessed listings cached using Redis/in-memory cache.  
- Only authenticated hosts can modify/delete their own listings.  
- Use **UUIDs** for all property IDs.  

---

## 3. 🛏 Booking System

### 3.1 Overview
Handles property booking, payment processing, and automatic notifications for both guests and hosts.

---

### 3.2 API Endpoints

| Method | Endpoint | Description |
|---------|-----------|-------------|
| `POST` | `/api/bookings` | Create a new booking |
| `GET` | `/api/bookings/:id` | Get booking details |
| `GET` | `/api/bookings/user/:id` | Get all bookings for a user |
| `PATCH` | `/api/bookings/:id/cancel` | Cancel a booking |

---

### 3.3 Input / Output Specifications

#### **POST /api/bookings**

**Request**
```json
{
  "guest_id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "property_id": "b8a31c7f-0c11-4cfb-bb77-087d8906c845",
  "check_in": "2025-11-01",
  "check_out": "2025-11-04",
  "payment_method": "credit_card"
}
```

**Response**
```json
{
  "booking_id": "fba1d2b3-2d98-42a5-8ad3-90ff32b7418c",
  "status": "confirmed",
  "message": "Booking successful"
}
```

---

### 3.4 Validation Rules
- `check_in` < `check_out`, both must be future dates.  
- `property_id` must exist and be available.  
- `guest_id` must exist and be active.  
- `payment_method` must be one of: `credit_card`, `mobile_money`, or `paypal`.  

---

### 3.5 Performance & Security
- Booking confirmation within **≤ 2 seconds**, including payment processing.  
- Prevent double-booking via **transactional locking**.  
- Store transactions with **UUID** keys.  
- Auto-send notifications upon booking or cancellation.  
- Handle **100+ concurrent booking requests** safely.  

---

## ✅ Summary Table

| Feature | Main Endpoint | Response Time | Security Highlights |
|----------|----------------|----------------|---------------------|
| **User Authentication** | `/api/auth/register` | ≤ 500ms | JWT, bcrypt, rate limit |
| **Property Management** | `/api/properties` | ≤ 300ms | Role-based access, UUIDs |
| **Booking System** | `/api/bookings` | ≤ 2s | Locking, transactional safety, notifications |

---
