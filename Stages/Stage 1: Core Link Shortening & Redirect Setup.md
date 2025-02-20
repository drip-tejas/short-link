## **Project Overview**

This project is a **SaaS-based link shortener** that allows users to generate **custom short links**, track engagement analytics, manage links, and utilize custom domains. It will also support **retargeting and API access** for tracking and automation.

The project is broken down into **7 stages**, each focusing on a critical part of the system:

1. **Core Link Shortening & Redirect Setup** (Current Stage) – Users can create short links, and the system handles redirects.
2. **Analytics Tracking Implementation** – Track click events, including time, device, location, and referral source.
3. **Link Management** – Change slugs, update destination links, and log previous destinations.
4. **Retargeting Functionality** – Enable tracking pixels (Facebook Pixel, Google Ads) for remarketing.
5. **API Endpoints for Analytics & Tracking** – Expose API routes to retrieve analytics and link data programmatically.
6. **Open Graph Controls & UTM Builder** – Allow customization of metadata previews for social sharing and provide a UTM parameter builder.
7. **Final Testing & Deployment** – Conduct thorough testing, fix bugs, and deploy the system to a production environment.

This document outlines **Stage 1: Core Link Shortening & Redirect Setup**.

---

## **Goal for This Stage**

The goal of this stage is to build the **core link-shortening functionality**, ensuring that:

- Users can **generate a short URL** from a long URL using a **POST** request.
- Short URLs are **stored in a PostgreSQL database**.
- When a short URL is visited via a **GET** request, the system retrieves the original URL and **redirects the user**.
- If an invalid short link is accessed, the system returns a **404 error**.
- A basic API is available for programmatic link creation.

---

## **What Needs to Be Done?**

### **1. Short URL Generation (POST /shorten)**

- Implement a **POST** endpoint (`POST /shorten`) that:
    - Accepts a **long URL** in the request body.
    - Generates a **unique alphanumeric short code** (e.g., `abc123`).
    - Stores the **original URL and short code** in a PostgreSQL database.
    - Returns a JSON response with the **generated short URL**.

**Explicit Instructions:**

✅ The system **must generate a unique short code** for each request.

✅ The short code **must be stored alongside the original URL** in the database.

✅ The response format should be:

```json
{
  "short_url": "https://short.ly/abc123"
}

```

---

### **2. Database Setup**

- Create a **PostgreSQL table** named `links` with the following columns:
    - `id` (Primary Key, Auto-incrementing)
    - `original_url` (TEXT, stores the long URL)
    - `short_code` (VARCHAR, stores the unique short code)
    - `created_at` (TIMESTAMP, defaults to current time)

**Explicit Instructions:**

✅ Ensure the `short_code` column is **indexed** for fast lookups.

✅ The system **must not allow duplicate short codes**.

---

### **3. Redirection Logic (GET /:short_code)**

- Implement a **GET** endpoint (`GET /:short_code`) that:
    - Extracts the **short code** from the URL path.
    - Looks up the original URL in the database.
    - **Redirects the user** to the original URL if found.
    - Returns a **404 error** if the short code does not exist.

**Explicit Instructions:**

✅ If the short link exists, respond with an **HTTP 301 redirect** to the original URL.

✅ If the short link does not exist, return a JSON error response:

```json
{
  "error": "Short link not found"
}

```

✅ The system **must not allow arbitrary inputs**—validate that `short_code` exists before performing a redirect.

---

### **4. Basic API for Link Shortening**

- Expose an API that allows external applications to create short links programmatically.
- The API must support the following:
    - **POST /shorten** (Create a new short link).
    - **GET /:short_code** (Redirect user to the original URL).

**Explicit Instructions:**

✅ The API **must return JSON responses** (no HTML pages).

✅ Ensure API **accepts and processes JSON payloads** correctly.

---

### **5. Testing Requirements**

- Verify that **short links are created successfully** via `POST /shorten`.
- Ensure that **visiting a short link via `GET /:short_code` correctly redirects**.
- Test **invalid short codes** (should return a 404 error).
- Validate **URL input format** (reject malformed URLs).

**Explicit Instructions:**

✅ Test using **Postman or cURL** to confirm correct API responses.

✅ Validate that short codes are **unique** and do not get overwritten.

✅ Ensure **database queries are optimized** for fast lookups.

---

## **Success Criteria**

✅ Users can **create short links** via `POST /shorten`.

✅ Short links are **stored in the database** and are **retrievable**.

✅ Users can **access a short link** via `GET /:short_code` and get **redirected** correctly.

✅ Invalid short links return a **404 error**.

✅ API endpoints function correctly **with valid JSON responses**.

---

## **Next Steps (Stage 2: Analytics Tracking Implementation)**

- Track **click events**, including:
    - Timestamp of the click.
    - Device and OS information.
    - Geolocation (country, city, region).
    - Referrer (social media, direct, email, ads).
- Store analytics in the **database** for future retrieval.
- Provide an **API endpoint (`GET /analytics/:short_code`)** to retrieve analytics data.

---

###