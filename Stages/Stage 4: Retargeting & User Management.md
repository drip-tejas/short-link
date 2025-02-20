## **Project Overview**

This project is a **SaaS-based link shortener** that allows users to generate **custom short links**, track engagement analytics, manage links, and utilize retargeting. It also supports **API access** for tracking and automation.

### **Project Stages**

1. ✅ **Core Link Shortening & Redirect Setup** – Users can create short links, and the system handles redirects.
2. ✅ **Analytics Tracking Implementation** – Track click events, including time, device, location, and referral source.
3. ✅ **Link Management** – Change slugs, update destination links, and log previous destinations.
4. **Retargeting & User Management** (**Current Stage**) – User authentication & permissions, tracking pixels for remarketing.
5. **API for Analytics & Link Management** – Fetch analytics, retrieve/update links via API.
6. **Open Graph Controls, UTM Builder & QR Code Generation** – QR codes (generated on link detail page, API endpoint for QR retrieval), UTM builder, metadata customization.
7. **Final Testing & Deployment** – Security, performance testing, production release.

---

## **Goal for This Stage**

The goal of this stage is to implement **user authentication and retargeting functionality**, ensuring that:

- **Users must log in** to manage their links (edit slug, update destination).
- **Only link owners can modify their links**.
- **Tracking pixels (Facebook Pixel, Google Ads, etc.) can be embedded** for retargeting users who click on links.
- **Authentication UI is built**, allowing users to register, log in, and manage sessions.

---

## **What Needs to Be Done?**

### **1. Implement User Authentication (Sign-up, Login, Logout)**

- Use an **off-the-shelf authentication solution** for Express.js.
- Support **email + password login** (OAuth/social login can be future scope).
- Issue **JWT tokens** for authentication.

**Explicit Instructions:**

✅ Implement `POST /signup` API to allow new users to register.

✅ Implement `POST /login` API for authentication, returning a JWT token.

✅ Implement `POST /logout` API to invalidate the session.

✅ Store user data securely (hashed passwords, no plaintext storage).

✅ Protect all **link modification endpoints (update slug, update destination)** so only the **owner of the link** can modify it.

---

### **2. UI: Authentication Pages**

Create the **frontend authentication UI** for user access and session management.

🔹 **Sign-up Page**

- Fields: **Email, Password, Confirm Password**.
- Validation: **Ensure email is unique**, password meets complexity rules.
- Button: **"Create Account"** → Calls `POST /signup`.

🔹 **Login Page**

- Fields: **Email, Password**.
- Button: **"Login"** → Calls `POST /login`, stores JWT token for authentication.

🔹 **Forgot Password Page (Optional)**

- Allows users to **reset their password** via email.

🔹 **Logout Button**

- Calls `POST /logout`, clears JWT token, redirects user to login.

🔹 **Authentication Handling**

- Ensure logged-in users **stay authenticated** across page refreshes.
- Redirect unauthenticated users to **login** when accessing restricted pages.

---

### **3. Restrict Link Management to Owners**

- Modify the `links` table to **associate links with user accounts**.
- Ensure only the **creator of a short link** can edit it.

**Explicit Instructions:**

✅ Add a `user_id` column to the `links` table to track link ownership.

✅ Modify `PUT /update-slug` and `PUT /update-destination` to **require authentication**.

✅ If a user attempts to modify a link **they do not own**, return a `403 Forbidden` error.

---

### **4. Implement Tracking Pixels for Retargeting**

- Allow users to **embed tracking pixels** (Facebook Pixel, Google Ads, etc.) in their short links.
- When a user **creates or edits a short link**, they can attach a **tracking pixel ID**.
- When someone **clicks the link**, the tracking pixel script should execute before redirecting.

**Explicit Instructions:**

✅ Modify `POST /shorten` and `PUT /update-destination` to allow users to **attach a tracking pixel ID**.

✅ Modify `GET /:short_code` to **inject the tracking pixel before redirecting**.

✅ Supported tracking pixels:

- **Facebook Pixel** (`fbclid`)
- **Google Ads Pixel** (`gclid`)
✅ Ensure **tracking pixels do not slow down the redirect** (async execution).

---

### **5. Database Setup for User Management & Retargeting**

- Modify the `links` table to store:
    - `user_id` (Foreign Key linking to the `users` table).
    - `tracking_pixel_id` (Stores pixel ID for retargeting).
- Create a **users table** for authentication.

**Explicit Instructions:**

✅ `users` table schema:

- `id` (Primary Key).
- `email` (Unique, required).
- `password_hash` (Hashed password).
- `created_at` (Timestamp).
✅ Update `links` table to include `user_id` and `tracking_pixel_id`.
✅ Ensure `tracking_pixel_id` is optional (not all links will use retargeting).

---

### **6. Testing Requirements**

- Verify **user authentication** (signup, login, logout) works correctly.
- Ensure **only link owners can edit their links**.
- Test that **tracking pixels fire correctly before redirection**.
- Simulate **unauthorized API requests** to ensure authentication security.

**Explicit Instructions:**

✅ Test JWT authentication using **Postman or cURL**.

✅ Attempt **unauthorized edits** and confirm the system blocks them.

✅ Click a link and verify **tracking pixels fire before redirecting**.

✅ Ensure **redirect speed is not significantly affected** by tracking pixel execution.

✅ Validate authentication UI flows (successful login, session persistence, logout).

---

## **Success Criteria**

✅ Users can **sign up, log in, and manage their links securely**.

✅ Only link **owners can edit their links**.

✅ Tracking pixels **fire before redirecting** for retargeting purposes.

✅ The system **blocks unauthorized edits** (403 Forbidden error).

✅ Authentication UI **is fully functional and integrated with the backend**.

---

## **Next Steps (Stage 5: API for Analytics & Link Management)**

- Expose **API endpoints** to fetch analytics for a given short link.
- Provide API methods to **retrieve and update links programmatically**.
- Ensure **authentication is required** for API access.

---