
- **Database** stores users, listings, bookings, transactions, reviews.  
- **mysql db** holds images and uploads.  
- **Third-Party APIs** (e.g., Stripe) manage secure payment flows.

---

## Key Workflows

### User Registration & Authentication
1. Client sends registration payload (name, email, password/SSO token).
2. Auth Service hashes password & creates user record.
3. System assigns default role (**Guest** or **Host**).
4. On login, JWT issued → included in headers for subsequent requests.

---

### Property Listing Management
1. Host submits property details + images.
2. API validates required fields & uploads media to the mysql  database.
3. Property Service persists metadata (title, location, price).
4. Listings become **searchable** by Guests.

---

### Booking Lifecycle
1. Guest selects property & dates.
2. Booking Service checks **availability**.
3. If free → creates booking as `pending`.
4. Payment Gateway invoked (capture or pre-authorize funds).
5. On success, booking → `confirmed` & notification sent to Host & Guest.
6. After stay completion, status → `completed`.

---

### Payments
- **Guest Payment:** charged during booking creation.
- **Host Payout:** triggered after stay confirmation.
- **Refunds:** booking canceled → gateway handles reversal.

---

### Reviews & Ratings
1. Guest receives prompt post-checkout.
2. Review Service links review to the **booking ID**.
3. Host can view & optionally respond.
4. Ratings aggregated into property score.

---

### Notifications
- Triggered by events (booking created, canceled, payment processed).
- Sent through:
  - **Email** (via SendGrid/Mailgun).
  - **In-App** popups or real-time socket events.

  ## Diagrams links
  - https://drive.google.com/file/d/1ZKujF6RAFiXBnnS06EoOEIYg5q6IBbLH/view?usp=drive_link