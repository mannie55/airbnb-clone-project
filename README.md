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
## Team Roles

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


## 🧰 Technology Stack

### 1. **Django**
   - **Purpose**: A web framework for building RESTful APIs and handling server-side logic.
   - **Description**: Django is used to manage the backend logic, user authentication, data processing, and rendering API responses. It provides tools for easily creating and managing the database, routing URLs to views, and interacting with the database using Django's ORM.

### 2. **Django REST Framework (DRF)**
   - **Purpose**: A toolkit for building Web APIs in Django.
   - **Description**: DRF is used to simplify the process of building RESTful APIs in Django. It provides classes for serialization (turning data into JSON format), authentication, permissions, and viewsets for handling CRUD operations efficiently.

### 3. **GraphQL**
   - **Purpose**: A query language for APIs and runtime for executing those queries by using a type system.
   - **Description**: GraphQL provides a flexible and efficient way to interact with data. Instead of having multiple endpoints for different queries, GraphQL allows clients to request only the data they need, which can reduce the number of API requests and improve performance.

### 4. **PostgreSQL**
   - **Purpose**: A relational database management system.
   - **Description**: PostgreSQL is used to store data in the application. It is a powerful, open-source object-relational database system known for its robustness, scalability, and support for advanced data types and queries.

### 5. **Celery**
   - **Purpose**: An asynchronous task queue/job queue system.
   - **Description**: Celery is used to handle background tasks such as sending emails, processing payments, or generating reports asynchronously, which helps keep the main application responsive and performant.

### 6. **Redis**
   - **Purpose**: A powerful, in-memory data structure store.
   - **Description**: Redis is used for caching, reducing database load, and speeding up data retrieval. It helps improve performance by storing frequently accessed data in memory.

### 7. **Docker**
   - **Purpose**: A platform for developing, shipping, and running applications inside containers.
   - **Description**: Docker is used to containerize the application, making it easier to manage, scale, and deploy across different environments. It ensures the application runs the same way regardless of where it's deployed.

### 8. **CI/CD (Continuous Integration/Continuous Deployment)**
   - **Purpose**: Automating the process of testing, building, and deploying applications.
   - **Description**: CI/CD tools are used to automate the software development lifecycle. With CI/CD pipelines, tests are run automatically on every code change, and deployment to production is streamlined, reducing human error and improving software quality.

---

# 🧩 Airbnb Clone – Database Design

This document outlines the core database models required for building the backend of the Airbnb Clone project. These models support the features for users, properties, bookings, payments, and reviews.

---

## Database Design

### 1. 🧑‍💼 User Table

| Field         | Type               | Description                               |
|---------------|--------------------|-------------------------------------------|
| user_id       | UUID               | Primary key (Indexed)                     |
| first_name    | String (VARCHAR)   | User’s first name (Required)              |
| last_name     | String (VARCHAR)   | User’s last name (Required)               |
| email         | String (VARCHAR)   | email address (UNIQUE, Required)          |
| password_hash | String (VARCHAR)   | Hashed password (Required)                |
| phone_number  | String (VARCHAR)   | Optional phone number                     |
| role          | Enum               | `guest`, `host`, or `admin` (Required)    |
| created_at    | Timestamp          | Time when account was created             |


> A user can be a **host** (owns properties) or a **guest** (books properties).

---

### 2. 🏡 Property Table

| Field           | Type               | Description                                   |
|----------------|--------------------|-----------------------------------------------|
| property_id     | UUID / Integer     | Primary key (Indexed)                         |
| owner_id        | UUID (FK → User)   | Refers to the user who listed the property    |
| name            | String (VARCHAR)   | Title of the listing (Required)               |
| description     | Text               | Full description of the property (Required)   |
| location        | String (VARCHAR)   | Property address (Required)                   |
| price_per_night | Decimal            | Nightly rate (Required)                       |
| is_available    | Boolean            | True if property is currently available       |
| created_at      | Timestamp          | When the property was listed                  |
| updated_at      | Timestamp          | Last time the property was updated            |

> A property belongs to **one host** (user).

---

### 3. 📅 Booking Table

|| Field         | Type               | Description                                        |
|---------------|---------------------|----------------------------------------------------|
| booking_id    | UUID / Integer      | Primary key (Indexed)                              |
| user_id       | UUID (FK → User)    | Guest who made the booking                         |
| property_id   | UUID (FK → Property)| Property being booked                              |
| check_in      | Date                | Check-in date (Required)                           |
| check_out     | Date                | Check-out date (Required)                          |
| total_price   | Decimal             | Total booking price (Required)                     |
| status        | Enum                | `pending`, `confirmed`, or `cancelled` (Required)  |
| created_at    | Timestamp           | When the booking was created                       |
| updated_at    | Timestamp           | When the booking was last updated                  |


> A user (guest) books a property.

---

### 4. 💳 Payment Table
| Field          | Type               | Description                                         |
|----------------|--------------------|-----------------------------------------------------|
| payment_id     | UUID / Integer     | Primary key (Indexed)                               |
| booking_id     | UUID (FK → Booking)| Booking associated with this payment                |
| user_id        | UUID (FK → User)   | Guest who made the payment                          |
| amount         | Decimal            | Total amount paid                                   |
| payment_date   | Timestamp          | Date and time when payment was made                 |
| payment_method | Enum               | `credit_card`, `paypal`, or `stripe` (Required)     |


> Each payment is tied to **one booking**.

---

### 5. 📝 Review Table

| Field        | Type                | Description                                  |
|--------------|---------------------|----------------------------------------------|
| review_id    | UUID / Integer      | Primary key (Indexed)                        |
| user_id      | UUID (FK → User)    | Guest who wrote the review                   |
| property_id  | UUID (FK → Property)| Property being reviewed                      |
| rating       | Integer             | Star rating from 1 to 5 (Required)           |
| comment      | Text                | Written review content (Required)            |
| created_at   | Timestamp           | When the review was created                  |

> A guest can leave **one review per booking** (enforceable via constraints if required).

---

### 6. 💬 Message Table 

| Field         | Type               | Description                                     |
|---------------|--------------------|-------------------------------------------------|
| message_id    | UUID / Integer     | Primary key (Indexed)                           |
| sender_id     | UUID (FK → User)   | User who sent the message                       |
| recipient_id  | UUID (FK → User)   | User who received the message                   |
| message_body  | Text               | Content of the message (Required)               |
| sent_at       | Timestamp          | Date and time when the message was sent         |

> A message is sent by one user **sender** and received by another user **recipient**.
---

### 🔁 Relationships Summary

- `User` 1️⃣ ➝ 🔢 `Property` (host owns multiple properties)
- `User` 1️⃣ ➝ 🔢 `Booking` (guest can make multiple bookings)
- `User` 1️⃣ ➝ 🔢 `Review` (guest can leave reviews)
- `Property` 1️⃣ ➝ 🔢 `Booking` (a property can have many bookings)
- `Booking` 1️⃣ ➝ 1️⃣ `Payment` (each booking has one payment)
- `Property` 1️⃣ ➝ 🔢 `Review` (each property can have many reviews)


---

## 🛠️ Feature Breakdown


### 2. User Management
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

## 📈 API Documentation Overview

### REST API
- Built with Django REST Framework
- Follows OpenAPI standard

### GraphQL API
- Enables flexible queries and data fetching

---

## CI/CD Pipeline

A **CI/CD pipeline** (Continuous Integration and Continuous Deployment) is an automated process that helps developers build, test, and deploy code changes more efficiently and reliably. 

### Why It's Important

- **Automation**: It reduces manual work by automating repetitive tasks like testing and deployment.
- **Early Bug Detection**: Tests run automatically, helping catch errors early in the development cycle.
- **Faster Delivery**: Changes can be deployed to production more quickly and with confidence.
- **Consistency**: Ensures code is always deployed in a consistent and reproducible way.

### Tools Used

- **GitHub Actions**: Automates workflows like running tests and deploying code whenever changes are pushed to the repository.
- **Docker**: Containerizes the application to ensure it runs the same way across all environments.
- **Docker Hub** or **GitHub Packages**: Used to store and distribute Docker images.
- **Heroku**, **AWS**, or **DigitalOcean**: Platforms where the final deployment can happen.

---

## 🔐 API Security

### 1. **Authentication**
   - **Purpose**: Ensures that users are who they claim to be.
   - **Explanation**: Authentication is implemented to verify the identity of users accessing the system. In this project, we use token-based authentication (e.g., JWT or OAuth) to securely authenticate users during login and other interactions.
   - **Why it’s Crucial**: Without proper authentication, unauthorized users could gain access to sensitive data or perform actions they shouldn't have access to, like creating bookings or making payments.

### 2. **Authorization**
   - **Purpose**: Determines what an authenticated user is allowed to do.
   - **Explanation**: Authorization controls user access to resources based on their role or permissions. For example, only users with the role of "admin" can delete properties, while only property owners can edit their own listings.
   - **Why it’s Crucial**: Authorization ensures that users can only perform actions or access resources they are allowed to. This is important for preventing unauthorized access and ensuring data integrity.

### 3. **Rate Limiting**
   - **Purpose**: Controls the number of requests a user can make to the API in a given time period.
   - **Explanation**: Rate limiting is implemented to prevent abuse and protect the system from malicious activities, such as DDoS attacks or brute-force login attempts. It ensures that users don’t overwhelm the system by making too many requests in a short period.
   - **Why it’s Crucial**: By implementing rate limiting, we protect the system from excessive load and ensure fair access for all users, preventing the backend from becoming slow or unresponsive.

### 4. **Data Encryption**
   - **Purpose**: Protects data in transit and at rest.
   - **Explanation**: Encryption is used to protect sensitive data, such as user passwords and payment details. All communication between the client and server is encrypted using HTTPS to ensure data is secure in transit. Sensitive data stored in the database is also encrypted.
   - **Why it’s Crucial**: Encryption protects user privacy by ensuring that sensitive information (e.g., passwords, financial data) cannot be read by unauthorized parties. This is essential for maintaining trust and compliance with data protection regulations.

### 5. **Input Validation and Sanitization**
   - **Purpose**: Prevents malicious data from entering the system.
   - **Explanation**: Input validation and sanitization are implemented to ensure that only valid data is accepted from users and that any potentially harmful data (e.g., SQL injection or cross-site scripting (XSS) attacks) is filtered out.
   - **Why it’s Crucial**: Validating and sanitizing input helps prevent attackers from exploiting the application’s input forms to execute malicious code that could compromise the system’s security.

### 6. **Logging and Monitoring**
   - **Purpose**: Tracks suspicious activities and potential security breaches.
   - **Explanation**: Logs are generated for all key activities, including user logins, payment transactions, and API requests. These logs are monitored for unusual behavior, such as repeated failed login attempts, which might indicate an attempted attack.
   - **Why it’s Crucial**: Monitoring and logging help detect security threats early, allowing for a rapid response to potential attacks. It also ensures that the system complies with security auditing requirements.

### 7. **Secure Payment Processing**
   - **Purpose**: Ensures that financial transactions are processed safely.
   - **Explanation**: Payment processing is handled through secure third-party payment gateways (e.g., Stripe or PayPal) to ensure that sensitive payment information is not exposed to the backend servers. These gateways comply with the Payment Card Industry Data Security Standard (PCI-DSS).
   - **Why it’s Crucial**: Securing payment data is vital to protect users' financial information and prevent fraud. Insecure payment systems could lead to financial losses and damage to the company's reputation.

### 8. **Session Management**
   - **Purpose**: Manages user sessions securely.
   - **Explanation**: Secure session management techniques are used to handle user sessions, such as using secure, HttpOnly cookies to store session tokens and implementing session timeouts to limit exposure in case a session is hijacked.
   - **Why it’s Crucial**: Proper session management ensures that user sessions are terminated when they are no longer needed, reducing the risk of unauthorized access due to session hijacking or misuse.

---

## 📂 License

This project is licensed under the MIT License.
