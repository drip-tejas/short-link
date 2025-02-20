## **Project Overview**

This project is a **SaaS-based link shortener** that allows users to generate **custom short links**, track engagement analytics, manage links, and utilize retargeting. It will also support **API access** for tracking and automation.

### **Project Stages**

1. ✅ **Core Link Shortening & Redirect Setup** – Users can create short links, and the system handles redirects.
2. ✅ **Analytics Tracking Implementation** – Track click events, including time, device, location, and referral source.
3. **Link Management (Current Stage)** – Change slugs, update destination links, and log previous destinations.
4. **Retargeting & User Management** – User accounts with authentication, permissions, tracking pixels.
5. **API for Analytics & Link Management** – Fetch analytics, retrieve/update links via API.
6. **Open Graph Controls, UTM Builder & QR Code Generation** – QR codes (generated on link detail page, API endpoint for QR retrieval), UTM builder, metadata customization.
7. **Final Testing & Deployment** – Security, performance testing, production release.

---

## **Goal for This Stage**

The goal of this stage is to **enable link management** features, allowing users to:

- **Change the slug (shortcode)** of an existing short link.
- **Update the destination URL** for a short link, while logging previous destinations.
- Ensure **only the creator of a link** can modify it.

---

## **What Needs to Be Done?**

### **1. Changing Shortcodes (Slugs) (PUT /update-slug)**

- Allow users to **edit the slug (shortcode) of an existing link**.
- Ensure the **new slug is unique** and does not conflict with an existing one.
- When a slug is changed, the **old slug stops working** (no aliasing).

**Explicit Instructions:**

✅ Implement a `PUT /update-slug` API endpoint that:

- Accepts the **original short code and a new short code**.
- Ensures the **new short code is not already taken**.
- Updates the database **with the new short code**.
✅ If an **invalid or non-existent short code** is provided, return a `404 error`.
✅ The system **must not allow duplicate slugs**.
✅ Old slugs **must stop working immediately**.

---

### **2. Updating Destination Links (PUT /update-destination)**

- Allow users to **edit the destination (long) URL of an existing short link**.
- Store **a history of previous destination URLs** for auditing.
- Ensure **only the creator of the link** can update the destination.

**Explicit Instructions:**

✅ Implement a `PUT /update-destination` API endpoint that:

- Accepts the **short code and a new destination URL**.
- Ensures that **only the owner of the link can update it**.
- Updates the **destination URL in the database**.
- Logs **previous destination URLs** for reference.
✅ If an **invalid short code** is provided, return a `404 error`.
✅ If the **new destination URL is the same as the current one**, reject the update.
✅ Provide an API endpoint (`GET /destination-history/:short_code`) to retrieve **previous destination URLs**.

---

### **3. Database Setup for Link Updates**

- Modify the `links` table to support **slug updates**.
- Create a new `destination_history` table to log **destination URL changes**.
- Ensure **indexes are optimized** for fast retrieval.

**Explicit Instructions:**

✅ `links` table updates:

- Ensure `short_code` can be updated.
✅ `destination_history` table schema:
- `id` (Primary Key).
- `short_code` (Foreign Key linking to `links`).
- `previous_url` (TEXT, stores the old destination).
- `updated_at` (TIMESTAMP).

---

### **4. Testing Requirements**

- Ensure that **slug changes are reflected immediately**, and old slugs stop working.
- Confirm **destination updates work correctly**, and previous destinations are logged.
- Test **error handling** (e.g., duplicate slugs, invalid updates).

**Explicit Instructions:**

✅ Simulate **API requests for updating slugs and destinations**.

✅ Verify **redirects work correctly** after a slug update.

✅ Check **audit logs for destination changes** (`GET /destination-history/:short_code`).

---

## **Success Criteria**

✅ Users can **change shortcodes (slugs)**, and the old ones stop working.

✅ Users can **update destination URLs**, and previous destinations are logged.

✅ `GET /destination-history/:short_code` retrieves **previous destinations**.

✅ The system **does not allow duplicate slugs or invalid updates**.

---

## **Next Steps (Stage 4: Retargeting & User Management)**

- Implement **user authentication** using an off-the-shelf solution.
- Restrict **link management actions (update slug, update destination) to the owner**.
- Integrate **tracking pixels (Facebook Pixel, Google Ads) for retargeting**.

---