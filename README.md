# Airbnb Clone Backend

## 🚀 Objective

The backend of the Airbnb Clone project provides a solid and scalable system to manage:
- User interactions
- Property listings
- Bookings
- Payment processing

The goal is to replicate Airbnb’s core backend functionality.

---

## 🏆 Project Goals

- **User Management**: Secure registration, login, and profile management.
- **Property Management**: Create, update, and retrieve listings.
- **Booking System**: Make and manage property reservations.
- **Payment Processing**: Handle and record transactions.
- **Review System**: Leave and manage property reviews.
- **Data Optimization**: Fast data access through indexing and caching.

---

## 🛠️ Features Overview

### 1. API Documentation
- **OpenAPI Standard**: Clear and structured docs.
- **Django REST Framework**: For CRUD operations.
- **GraphQL**: For flexible data queries.

### 2. User Authentication
- **Endpoints**:  
  `/users/`, `/users/{user_id}/`  
- **Features**: Register, authenticate, manage profiles.

### 3. Property Management
- **Endpoints**:  
  `/properties/`, `/properties/{property_id}/`  
- **Features**: Create, update, delete, retrieve listings.

### 4. Booking System
- **Endpoints**:  
  `/bookings/`, `/bookings/{booking_id}/`  
- **Features**: Make, update, cancel bookings.

### 5. Payment Processing
- **Endpoint**:  
  `/payments/`  
- **Feature**: Handle booking payments.

### 6. Review System
- **Endpoints**:  
  `/reviews/`, `/reviews/{review_id}/`  
- **Features**: Post, update, delete reviews.

### 7. Database Optimizations
- **Indexing**: For faster queries.
- **Caching**: Using Redis to reduce DB load.

---

## ⚙️ Technology Stack

- **Backend**: Django
- **API**: Django REST Framework, GraphQL
- **Database**: PostgreSQL
- **Async Tasks**: Celery
- **Caching**: Redis
- **Containers**: Docker
- **CI/CD**: Automated testing and deployment

---

## 👥 Team Roles

- **Backend Developer**: APIs, models, logic.
- **Database Admin**: Schema design, indexing.
- **DevOps Engineer**: Deployment, scaling, monitoring.
- **QA Engineer**: Testing and quality assurance.

---

## 📈 API Documentation Overview

### REST API
- Uses Django REST Framework
- Documented with OpenAPI

### GraphQL
- Supports flexible queries and mutations

---

## 📌 Endpoints Overview

### Users
- `GET /users/` - List all users  
- `POST /users/` - Register a new user  
- `GET /users/{user_id}/` - Retrieve a user  
- `PUT /users/{user_id}/` - Update a user  
- `DELETE /users/{user_id}/` - Delete a user  

### Properties
- `GET /properties/` - List all properties  
- `POST /properties/` - Create a property  
- `GET /properties/{property_id}/` - Get property details  
- `PUT /properties/{property_id}/` - Update a property  
- `DELETE /properties/{property_id}/` - Delete a property  

### Bookings
- `GET /bookings/` - List all bookings  
- `POST /bookings/` - Make a booking  
- `GET /bookings/{booking_id}/` - Get booking details  
- `PUT /bookings/{booking_id}/` - Update a booking  
- `DELETE /bookings/{booking_id}/` - Cancel a booking  

### Payments
- `POST /payments/` - Process a payment  

### Reviews
- `GET /reviews/` - List all reviews  
- `POST /reviews/` - Create a review  
- `GET /reviews/{review_id}/` - Get a review  
- `PUT /reviews/{review_id}/` - Update a review  
- `DELETE /reviews/{review_id}/` - Delete a review  

---

## 📂 License

This project is licensed under the MIT License.
