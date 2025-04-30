# 🏡 Airbnb Clone Backend

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

# 🧩 Airbnb Clone – Database Design

This document outlines the core database models required for building the backend of the Airbnb Clone project. These models support the features for users, properties, bookings, payments, and reviews.

---

## 1. 🧑‍💼 User Table

| Field          | Type           | Description                         |
|----------------|----------------|-------------------------------------|
| id             | UUID / Integer | Primary key                         |
| username       | String         | Unique username                     |
| email          | String         | Unique email                        |
| password_hash  | String         | Hashed password                     |
| first_name     | String         | User’s first name                   |
| last_name      | String         | User’s last name                    |
| role           | String         | `host` or `guest`                   |
| date_joined    | DateTime       | When the account was created        |
| is_active      | Boolean        | If the user account is active       |
| profile_image  | URL / String   | Optional profile picture            |

> A user can be a **host** (owns properties) or a **guest** (books properties).

---

## 2. 🏡 Property Table

| Field           | Type           | Description                               |
|-----------------|----------------|-------------------------------------------|
| id              | UUID / Integer | Primary key                               |
| owner_id        | FK → User      | The user who listed the property (host)   |
| title           | String         | Title of the listing                      |
| description     | Text           | Full description of the property          |
| address         | String         | Property address                          |
| city            | String         | City where property is located            |
| country         | String         | Country of location                       |
| price_per_night | Decimal        | Nightly rate                              |
| max_guests      | Integer        | Max number of guests                      |
| is_available    | Boolean        | Availability flag                         |
| created_at      | DateTime       | When the property was listed              |
| updated_at      | DateTime       | Last update time                          |

> A property belongs to **one host** (user).

---

## 3. 📅 Booking Table

| Field         | Type           | Description                            |
|---------------|----------------|----------------------------------------|
| id            | UUID / Integer | Primary key                            |
| user_id       | FK → User      | Guest who made the booking             |
| property_id   | FK → Property  | Property being booked                  |
| check_in      | Date           | Check-in date                          |
| check_out     | Date           | Check-out date                         |
| num_guests    | Integer        | Number of guests                       |
| status        | String         | `pending`, `confirmed`, `cancelled`    |
| created_at    | DateTime       | Booking creation time                  |
| updated_at    | DateTime       | Booking update time                    |

> A user (guest) books a property.

---

## 4. 💳 Payment Table

| Field         | Type           | Description                             |
|---------------|----------------|-----------------------------------------|
| id            | UUID / Integer | Primary key                             |
| booking_id    | FK → Booking   | Booking related to this payment         |
| user_id       | FK → User      | The guest who made the payment          |
| amount        | Decimal        | Total amount paid                       |
| status        | String         | `success`, `failed`, `pending`          |
| transaction_id| String         | External payment gateway transaction ID |
| paid_at       | DateTime       | Payment timestamp                       |

> Each payment is tied to **one booking**.

---

## 5. 📝 Review Table

| Field         | Type           | Description                             |
|---------------|----------------|-----------------------------------------|
| id            | UUID / Integer | Primary key                             |
| user_id       | FK → User      | Reviewer (guest)                        |
| property_id   | FK → Property  | Reviewed property                       |
| rating        | Integer        | 1 to 5 stars                            |
| comment       | Text           | Review content                          |
| created_at    | DateTime       | Review creation time                    |

> A guest can leave **one review per booking** (enforceable via constraints if required).

---

## 🔁 Relationships Summary

- `User` 1️⃣ ➝ 🔢 `Property` (host owns multiple properties)
- `User` 1️⃣ ➝ 🔢 `Booking` (guest can make multiple bookings)
- `User` 1️⃣ ➝ 🔢 `Review` (guest can leave reviews)
- `Property` 1️⃣ ➝ 🔢 `Booking` (a property can have many bookings)
- `Booking` 1️⃣ ➝ 1️⃣ `Payment` (each booking has one payment)
- `Property` 1️⃣ ➝ 🔢 `Review` (each property can have many reviews)

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

## 👥 Project Team Roles & Responsibilities

### 1. Backend Developer

**Responsibilities:**
- Build server-side logic and APIs.
- Connect frontend and backend.
- Handle business logic and integrations.

> May also handle architectural decisions.  
> _Source: ITRex_

---

### 2. Database Administrator (DBA)

**Responsibilities:**
- Design and optimize the database.
- Ensure data integrity and performance.
- Handle backups, migrations, and scaling.

---

### 3. DevOps Engineer

**Responsibilities:**
- Setup CI/CD, testing, and deployment.
- Manage containers and infrastructure.
- Monitor and scale backend services.

> DevOps is more of a workflow and culture.  
> _Source: ITRex, WIRED_

---

### 4. Quality Assurance (QA) Engineer

**Responsibilities:**
- Write and run test cases.
- Report and track bugs.
- Ensure system stability and correctness.

> Key to ensuring the app meets user expectations.  
> _Source: ITRex_

---

## 📈 API Documentation Overview

### REST API
- Built with Django REST Framework
- Follows OpenAPI standard

### GraphQL API
- Enables flexible queries and data fetching

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
