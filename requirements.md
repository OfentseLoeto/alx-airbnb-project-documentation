1. User Authentication & Management
1.1 Scope

Provide secure registration, login, and session handling for Guests, Hosts, and Admins.

1.2 API Endpoints
Endpoint	             Method	    Description
/api/v1/auth/register	 POST	      Register a new user
/api/v1/auth/login	   POST	      Authenticate user & issue JWT
/api/v1/auth/logout	   POST	      Invalidate active token (optional blacklist)
/api/v1/users/me	     GET	      Fetch current user profile
/api/v1/users/me	     PATCH	    Update profile details (photo, phone, preferences)

1.3 Input / Output

POST /auth/register – Request JSON

{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "password": "StrongP@ssw0rd",
  "role": "host"      // or "guest"
}

Response (201 Created)

{
  "id": "uuid",
  "name": "Jane Doe",
  "email": "jane@example.com",
  "role": "host",
  "createdAt": "2025-09-08T18:00:00Z",
  "token": "eyJhbGci..."
}

1.4 Validation Rules

- Email: RFC 5322 compliant, must be unique.

- Password: min 8 chars, 1 uppercase, 1 number, 1 special.

- Role: only guest or host (admins seeded manually).

- Return 422 Unprocessable Entity if validation fails.

1.5 Performance & Security

- Passwords hashed with bcrypt/argon2 (≥12 rounds).

- JWT expiry 24h, refresh tokens optional.

- Rate-limit login attempts (e.g., 5/minute/IP).

- Aim for <150 ms response for auth endpoints under normal load.

2. Property Management
2.1 Scope

Enable hosts to create, update, and manage property listings, including photos and amenities.

2.2 API Endpoints
Endpoint	                 Method	   Description
/api/v1/properties	       POST	     Create a new property
/api/v1/properties	       GET	     Get paginated list of properties (search/filter)
/api/v1/properties/{id}	   GET	     Get property details
/api/v1/properties/{id}	   PATCH	   Update property
/api/v1/properties/{id}	   DELETE	   Delete property
2.3 Input / Output

POST /properties – Request JSON

{
  "title": "Modern Loft",
  "description": "Stylish apartment in downtown.",
  "location": "Cape Town, South Africa",
  "pricePerNight": 850,
  "maxGuests": 4,
  "amenities": ["wifi", "pool", "parking"],
  "availability": [
    { "from": "2025-09-10", "to": "2025-09-20" }
  ],
  "images": ["https://cdn.cloudinary.com/property123-1.jpg"]
}
Reasponse (201 Created)

{
  "id": "uuid",
  "title": "Modern Loft",
  "hostId": "uuid",
  "pricePerNight": 850,
  "status": "active",
  "createdAt": "2025-09-08T18:00:00Z"
}

2.4 Validation Rules

- Title: ≤ 150 characters.

- Price: positive numeric, local currency.

- Location: string (city, country).

- Images: must be valid URLs if provided.

- Host must be authenticated (role=host).

2.5 Performance Criteria

- Index location, pricePerNight columns for fast search (<300 ms).

- Use pagination (limit 20 per page default).

- Cache frequently accessed property details via Redis.

3. Booking System
3.1 Scope

Allow guests to reserve properties, manage booking status, and avoid date conflicts.

3.2 API Endpoints
Endpoint	                      Method	     Description
/api/v1/bookings	              POST	       Create a booking
/api/v1/bookings	              GET	         List user’s bookings
/api/v1/bookings/{id}	          GET	         Get booking details
/api/v1/bookings/{id}/cancel	  POST	       Cancel booking
/api/v1/bookings/{id}/confirm	  POST	       Host confirms booking
3.3 Input / Output

POST /bookings – Request JSON

{
  "propertyId": "uuid",
  "checkIn": "2025-09-15",
  "checkOut": "2025-09-20",
  "guests": 2,
  "paymentMethod": "stripe"
}
Response (201 Created)

{
  "id": "uuid",
  "propertyId": "uuid",
  "guestId": "uuid",
  "status": "pending",
  "totalPrice": 4250,
  "createdAt": "2025-09-08T18:10:00Z"
}

3.4 Validation Rules

- checkIn < checkOut.

- Dates must be available (no overlap).

- Guests ≤ property’s maxGuests.

- Payment details valid & processed before confirmation.

3.5 Performance & Reliability

- Use DB transactions for atomic booking + payment.

- Lock property date ranges to prevent race conditions.

- Cache property availability snapshot to reduce repeated DB scans.

- Target <500 ms booking confirmation under peak load.
