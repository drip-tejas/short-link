## **Project Overview**

This project is a **SaaS-based link shortener** that allows users to generate **custom short links**, track engagement analytics, manage links, and utilize retargeting. It also supports **API access** for tracking and automation.

### **Project Stages**

1. ✅ **Core Link Shortening & Redirect Setup** – Users can create short links, and the system handles redirects.
2. ✅ **Analytics Tracking Implementation** – Track click events, including time, device, location, and referral source.
3. ✅ **Link Management** – Change slugs, update destination links, and log previous destinations.
4. ✅ **Retargeting & User Management** – User authentication & permissions, tracking pixels for remarketing.
5. **API for Analytics & Link Management** (**Current Stage**) – Expose API endpoints to retrieve analytics and manage links programmatically.
6. **Open Graph Controls, UTM Builder & QR Code Generation** – QR codes (generated on link detail page, API endpoint for QR retrieval), UTM builder, metadata customization.
7. **Final Testing & Deployment** – Security, performance testing, production release.

---

## **Goal for This Stage**

The goal of this stage is to **provide API endpoints** that allow users to:

- **Retrieve analytics data** for a given short link.
- **Manage links programmatically** (fetch, update, and delete links via API).
- **Ensure all API endpoints require authentication** to protect data.
- **Build the Dashboard UI** to allow users to **view and manage their links** from the frontend.

---

## **What Needs to Be Done?**

### **1. API Endpoint: Retrieve Analytics (`GET /analytics/:short_code`)**

- Allow users to **fetch analytics data** for a given short link.
- The response should include:
    - **Total Clicks**.
    - **Clicks by Time Period** (Today, Last 7 Days, Last 30 Days).
    - **Top Countries & Cities**.
    - **Device & OS Distribution**.
    - **Referral Sources Breakdown**.

**Explicit Instructions:**

✅ Implement a `GET /analytics/:short_code` API that:

- Accepts a **short code** as a parameter.
- Aggregates data from the `click_events` table.
- Returns analytics **in a structured JSON response**.
✅ Ensure **only the link owner** can access analytics.
✅ Optimize queries to **return analytics quickly** (indexing, caching if needed).

---

### **2. API Endpoint: Fetch All User Links (`GET /links`)**

- Allow users to retrieve **all their short links** in one request.
- The response should include:
    - **Short URL**.
    - **Original URL**.
    - **Click Count**.
    - **Created At Timestamp**.

**Explicit Instructions:**

✅ Implement a `GET /links` API that:

- Requires authentication.
- Retrieves **only links owned by the user**.
- Returns a JSON array of links with relevant metadata.

---

### **3. API Endpoint: Fetch a Single Link (`GET /link/:short_code`)**

- Allow users to retrieve **detailed data** for a specific short link.
- Response includes:
    - **Short URL**.
    - **Original URL**.
    - **Click Count**.
    - **Created At Timestamp**.
    - **Analytics Summary** (optional).

**Explicit Instructions:**

✅ Implement a `GET /link/:short_code` API that:

- Accepts a **short code** as a parameter.
- Returns **link details & analytics summary** in a structured JSON response.
✅ Ensure **only the link owner** can access the data.

---

### **4. API Endpoint: Update a Link (`PUT /link/:short_code`)**

- Allow users to **update a link’s slug or destination URL** via API.
- Users can **change either or both** fields in a single request.
- Updates should be **validated and restricted to the owner**.

**Explicit Instructions:**

✅ Implement a `PUT /link/:short_code` API that:

- Accepts **new slug and/or destination URL** in the request body.
- Ensures **only the owner of the link** can make updates.
- Logs previous destinations (if the URL is changed).
- Rejects requests if the **new slug is already taken**.
✅ If **no changes are provided**, return an error.

---

### **5. API Endpoint: Delete a Link (`DELETE /link/:short_code`)**

- Allow users to **delete their own short links** via API.
- Deleting a link should:
    - **Remove the link from the database**.
    - **Delete associated analytics data**.
    - **Invalidate the short URL permanently**.

**Explicit Instructions:**

✅ Implement a `DELETE /link/:short_code` API that:

- Accepts a **short code** as a parameter.
- Ensures **only the link owner** can delete it.
- **Permanently removes the link & analytics** from the database.
✅ If the short code **does not exist**, return a `404 error`.
✅ Deleted short codes **must not be re-used**.

---

### **6. API Authentication & Security**

- All API endpoints must require **authentication via JWT tokens**.
- API requests must be **rate-limited** to prevent abuse.
- Use **CORS settings** to restrict unauthorized access.

**Explicit Instructions:**

✅ **Require authentication** for all endpoints (`Authorization: Bearer <token>`).

✅ Implement **rate limiting** (e.g., max 100 requests per minute per user).

✅ Ensure API responses **do not expose sensitive data**.

✅ Restrict API usage to **allowed domains** using CORS settings.

---

### **7. UI: Dashboard / List Page**

Create the **frontend Dashboard UI** for managing links and viewing analytics.

🔹 **Dashboard Overview**

- Displays **total clicks, top-performing links, and general stats**.

🔹 **List of Links**

- Shows all links created by the user with:
    - **Short URL**.
    - **Destination URL**.
    - **Click Count**.
    - **Creation Date**.
- **Actions per link:** Edit, Delete, View Analytics.

🔹 **Filters & Search**

- Allow filtering by **date range, click count, or keyword search**.

🔹 **Buttons / Interactions**

- **"Create New Link"** → Redirects to link creation page.
- **"View Details"** → Opens the **Link Detail Page** (Stage 6).
- **"Delete"** → Prompts confirmation before deleting.

---

### **8. Testing Requirements**

- Test API authentication (ensure unauthorized requests are blocked).
- Verify that **only the owner** can retrieve, update, and delete their links.
- Ensure **analytics retrieval is fast and accurate**.
- Test **dashboard UI functionality** (link list, filters, actions).

**Explicit Instructions:**

✅ Use **Postman or cURL** to test API requests.

✅ Attempt **unauthorized API calls** and ensure they are blocked.

✅ Test **rate-limiting functionality** to prevent abuse.

✅ Check **dashboard UI updates in real time** when new links are added or modified.

---

## **Success Criteria**

✅ Users can **retrieve analytics for their links** via API.

✅ Users can **fetch all their links** in one request.

✅ Users can **update or delete their links programmatically**.

✅ API requests **are secured with authentication & rate limiting**.

✅ **Dashboard UI is implemented**, allowing users to view and manage links.

---

## **Next Steps (Stage 6: Open Graph Controls, UTM Builder & QR Code Generation)**

- Implement **Open Graph metadata customization** for social sharing.
- Create a **UTM parameter builder** for campaign tracking.
- Generate **QR codes for short links** (display in UI + API endpoint for retrieval).

---