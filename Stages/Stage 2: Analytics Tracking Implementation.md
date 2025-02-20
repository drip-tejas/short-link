## **Project Overview**

This project is a **SaaS-based link shortener** that allows users to generate **custom short links**, track engagement analytics, manage links, and utilize custom domains. It also supports **retargeting and API access** for tracking and automation.

### **Project Stages**

1. ✅ **Core Link Shortening & Redirect Setup** – Users can create short links, and the system handles redirects.
2. **Analytics Tracking Implementation** (**Current Stage**) – Track click events, including time, device, location, and referral source.
3. **Link Management** – Change slugs, update destination links, and log previous destinations.
4. **Retargeting & User Management** – User accounts with authentication, permissions, tracking pixels.
5. **API for Analytics & Link Management** – Fetch analytics, retrieve/update links via API.
6. **Open Graph Controls, UTM Builder & QR Code Generation** – QR codes (generated on link detail page, API endpoint for QR retrieval), UTM builder, metadata customization.
7. **Final Testing & Deployment** – Security, performance testing, production release.

---

## **Goal for This Stage**

The goal of this stage is to implement **click tracking** to collect analytics on short link usage. The system must:

- Capture **click events** whenever a short link is accessed (`GET /:short_code`).
- Store **click analytics** in a database for future reporting.
- Ensure analytics are **accurate and scalable** under high traffic.

---

## **What Needs to Be Done?**

### **1. Track Click Events (GET /:short_code Update)**

- When a user visits a short link (`GET /:short_code`), record the following data:
    - **Timestamp** (date and time of the click).
    - **Device Type** (mobile, tablet, desktop).
    - **Operating System** (Windows, macOS, Linux, Android, iOS).
    - **Browser** (Chrome, Firefox, Safari, Edge, etc.).
    - **Geolocation** (country, state, city, region) – **Extracted from IP address**.
    - **Referral Source** (social media, direct, email, ad campaign).
    - **Click Type** (organic, paid, social).
    - **Language** (browser language settings).
- Store this data in a **new database table (`click_events`)** for analytics.

**Explicit Instructions:**

✅ Modify the existing `GET /:short_code` endpoint to **record click data before redirecting**.

✅ Ensure analytics **do not delay redirection** (asynchronous processing).

✅ Use **IP-based geolocation (MaxMind or similar API)** for location tracking.

✅ Store data **efficiently** (optimize queries for high traffic).

---

### **2. Database Setup for Analytics**

- Create a **PostgreSQL table (`click_events`)** to store tracking data.
- Required columns:
    - `id` (Primary Key, Auto-incrementing).
    - `short_code` (Foreign Key linking to `links` table).
    - `timestamp` (When the link was clicked).
    - `device_type` (Mobile, Desktop, Tablet).
    - `os` (Windows, macOS, Linux, Android, iOS).
    - `browser` (Chrome, Firefox, Safari, Edge, etc.).
    - `country`, `state`, `city`, `region` (Extracted from IP).
    - `referral_source` (Social media, email, direct, etc.).
    - `click_type` (Organic, Paid, Social).
    - `language` (Browser language).

**Explicit Instructions:**

✅ Ensure `short_code` is indexed for **fast lookups**.

✅ Store **timestamps in UTC** for consistency.

✅ Optimize for **high-volume inserts** (batch processing if necessary).

---

### **3. Referral Source & Device Tracking**

- Extract **referrer URL** from `req.headers.referer` to determine how the user found the link.
- Extract **user-agent details** to determine **device type, OS, and browser**.
- Store parsed data in the `click_events` table.

**Explicit Instructions:**

✅ Do **not** store full User-Agent strings (parse and store only relevant details).

✅ If the referrer is **empty**, classify as **"Direct" traffic**.

---

### **4. IP-Based Geolocation Tracking**

- Use an **IP Geolocation API** (MaxMind, ipinfo.io, etc.) to determine:
    - **Country, state, city, region** of the visitor.
- **DO NOT store raw IP addresses** (privacy concerns).
- Cache geolocation responses to **minimize API calls and costs**.

**Explicit Instructions:**

✅ The system **must not store users’ actual IPs** for privacy reasons.

✅ Geolocation API **must be fast** (ensure minimal impact on response time).

✅ If an IP lookup **fails**, store the location as `"Unknown"`.

---

### **5. API Endpoint for Analytics Retrieval (GET /analytics/:short_code)**

- Create an API endpoint `GET /analytics/:short_code` to retrieve analytics data for a given short link.
- The response should include:
    - **Total Clicks**
    - **Clicks by Time Period** (Today, Last 7 Days, Last 30 Days)
    - **Top Countries & Cities**
    - **Device & OS Distribution**
    - **Referral Sources Breakdown**

**Explicit Instructions:**

✅ **Aggregate data efficiently** (use SQL GROUP BY for optimized queries).

✅ The API response should be **in JSON format** (no HTML).

✅ Ensure **fast performance** for analytics queries (optimize indexes).

---

### **6. Testing Requirements**

- Test **click tracking accuracy** (ensure all recorded data is correct).
- Verify **referrer and user-agent parsing** (correctly classifies devices, browsers, and traffic sources).
- Test **geolocation lookup speed and accuracy**.
- Ensure `GET /analytics/:short_code` returns **correct aggregated analytics**.

**Explicit Instructions:**

✅ Simulate **different browsers, devices, and referrers** for testing.

✅ Perform **load testing** (ensure analytics collection does not slow down redirections).

✅ Test API response times **under high traffic loads**.

---

## **Success Criteria**

✅ Short link clicks **are tracked in the database**.

✅ Analytics data includes **timestamp, device, OS, browser, location, and referral source**.

✅ `GET /analytics/:short_code` returns **accurate aggregated analytics**.

✅ Click tracking **does not delay redirections** (async processing).

✅ System **handles high traffic efficiently** (optimized queries & indexing).

---

## **Next Steps (Stage 3: Custom Domain Support & Link Management)**

- Allow users to **set up and use custom domains** for short links.
- Enable **updating shortcodes (slugs)** for existing links.
- Implement **destination URL updates**, with previous destinations logged.

---

### **Does this breakdown match what you need?** 🚀 If yes, I’ll move on to structuring **Stage 3: Custom Domain Support & Link Management.**