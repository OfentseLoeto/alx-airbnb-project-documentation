# alx-airbnb-project-documentation for the project's features and functionalities!


## Key Features

### 1. User Management
- **Registration & Authentication**
  - Register as Guest or Host.
  - Passwords hashed securely (bcrypt).
  - OAuth (Google, Facebook) supported.
  - Role-Based Access Control (Guest · Host · Admin).
- **Session Handling**
  - JWTs for stateless sessions.
- **Profile Management**
  - Update names, phone, profile image.
  - Store preferences (currency, notifications).

---

### 2. Property Listings
- Hosts can **create, edit, and delete** listings.
- Required metadata:
  - Title, description, location, nightly price, amenities.
  - Date availability validation.
  - Upload multiple property images.
- Guests can:
  - **Search** by city, region, or keyword.
  - **Filter** by price, amenities, number of guests.
  - View paginated results for performance.

---

### 3. Booking System
- **Create Bookings**
  - Guests request dates; system validates availability.
  - Bookings start in `pending` status.
- **Lifecycle Management**
  - Status transitions: pending → confirmed → completed/canceled.
- **User Dashboards**
  - Guests: view active & past bookings.
  - Hosts: view property bookings.
- Full CRUD endpoints for easy client integration.

---

### 4. Payment Processing
- Integration with **Stripe** or **PayPal** for secure transactions.
- **Guest Payments**
  - Capture payment during booking.
- **Host Payouts**
  - Automatic transfers after completion.
- **Payment History**
  - Track receipts, refunds, and audit logs.
- Notifications for **payment confirmation** and **payout completion**.

---

### 5. Reviews & Ratings
- Guests submit reviews **only after completed stays**.
- Rating validation (1–5).
- Hosts may respond to reviews.
- Display average ratings on listing detail pages.

---

### 6. Notifications
- **Event-Based Triggers**
  - Booking confirmations, cancellations, payment updates, new messages.
- **Delivery Channels**
  - Email (SendGrid, Mailgun).
  - In-app real-time notifications (WebSockets).

---

### 7. Admin Dashboard
- Secure, admin-only panel.
 - Manage:
  - Users (roles, suspensions).
  - Properties (approve, remove).
  - Bookings (override, dispute resolution).
  - Payment audits & logs.

  ### Vist the Diagram liks:
  - https://drive.google.com/file/d/1i_2cdWC9vmXkOjZHs9QDLxCY1-yUvmf6/view?usp=drive_link