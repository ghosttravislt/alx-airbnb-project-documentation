# **Project Requirements**

This document outlines the key requirements for the property rental platform.  
The requirements are categorized into **Core Functionalities**, **Technical Requirements**, and **Non-Functional Requirements**.

---

## 🏠 **Core Functionalities**

### **1. User Management**
- Users can create accounts and log in securely.  
- The system supports multiple roles: **Guest**, **Host**, and **Admin**.  
- Hosts can manage their property listings.  
- Guests can browse and book properties.

### **2. Property Management**
- Hosts can **add, edit, and delete** property listings.  
- Each property includes details such as name, description, location, price per night, and availability.  
- Guests can **view property details** and read reviews before booking.

### **3. Booking Management**
- Guests can **book available properties** for specific dates.  
- Bookings store information such as property, user, start date, end date, and total price.  
- Hosts can view all bookings related to their properties.  
- Guests can view their booking history and current reservations.

### **4. Payment System**
- Guests can make secure payments for confirmed bookings.  
- Multiple payment methods are supported (e.g., credit card, PayPal, mobile money).  
- Payment details are linked to the associated booking.

### **5. Reviews & Ratings**
- Guests can submit reviews and ratings after their stay.  
- Each review includes a rating (1–5) and is tied to both the property and the user.  
- Hosts can view feedback on their properties.

### **6. Messaging System**
- Users can send and receive messages within the platform (e.g., guests communicating with hosts).  
- Each message stores sender, recipient, body, and timestamp.  
- Ensures private and organized communication between users.

---

## ⚙️ **Technical Requirements**

### **1. Database**
- PostgreSQL (or any relational database) with a fully normalized schema (up to **Third Normal Form**).  
- Foreign key constraints ensure **referential integrity** across all entities.  
- Support for UUIDs as primary keys for scalability and uniqueness.

### **2. Backend**
- Implemented using **Node.js**, **Express**, or similar frameworks.  
- Follows RESTful API principles for handling CRUD operations.  
- Secure password hashing using algorithms like **bcrypt**.  
- Validation of input data before database transactions.

### **3. Frontend (Optional for full-stack implementation)**
- Built with **React**, **Next.js**, or **Vue** for interactive UI.  
- Integration with backend APIs for dynamic data fetching.  
- Form validation and responsive design for all device sizes.

### **4. Security**
- Implements authentication and authorization (e.g., JWT-based).  
- Protects user data with encryption and secure session handling.  
- Prevents SQL injection and cross-site scripting (XSS).

---

## 🚀 **Non-Functional Requirements**

### **1. Performance**
- The system should respond to API requests within 2 seconds under normal load.  
- Database queries must be optimized using proper indexing and joins.

### **2. Scalability**
- The application should support a growing number of users, properties, and bookings.  
- Designed with modularity to allow future feature expansion (e.g., notifications, discounts).

### **3. Reliability**
- Ensures data consistency across all transactions.  
- Provides backup and recovery procedures for database resilience.

### **4. Usability**
- Intuitive and user-friendly interface for both hosts and guests.  
- Clear navigation and responsive layout for mobile and desktop.

### **5. Maintainability**
- Codebase follows consistent structure and documentation.  
- Uses environment-based configuration for easy deployment.

### **6. Security & Privacy**
- User passwords and sensitive data must never be stored in plain text.  
- Access control enforces role-based permissions for users.

---

## ✅ **Summary**
This project’s requirements ensure a **robust, secure, and user-friendly** property rental system that models real-world use cases effectively.  
It meets database normalization standards (3NF), supports essential user workflows, and maintains strong technical and non-functional standards.
