## **Project Overview**

This project is a **SaaS-based link shortener** that allows users to generate **custom short links**, track engagement analytics, manage links, and utilize retargeting. It also supports **API access** for tracking and automation.

### **Project Stages**

1. ✅ **Core Link Shortening & Redirect Setup** – Users can create short links, and the system handles redirects.
2. ✅ **Analytics Tracking Implementation** – Track click events, including time, device, location, and referral source.
3. ✅ **Link Management** – Change slugs, update destination links, and log previous destinations.
4. ✅ **Retargeting & User Management** – User authentication & permissions, tracking pixels for remarketing.
5. ✅ **API for Analytics & Link Management** – Fetch analytics, retrieve/update links via API.
6. **Open Graph Controls & QR Code Generation (with QR Analytics)** (**Current Stage**) – Implement social media metadata customization and QR code generation.
7. **Final Testing & Deployment** – Security, performance testing, production release.

---

## **Goal for This Stage**

The goal of this stage is to enhance **link sharing and offline engagement** by implementing:

- **Open Graph metadata controls** for customizing social media previews.
- **QR code generation** for each short link, allowing users to share links offline.
- **QR scan tracking & analytics** to monitor QR code performance.
- **Build the Link Detail Page UI** so users can **view analytics, manage link settings, and download QR codes**.

---

## **What Needs to Be Done?**

### **1. Open Graph Metadata Customization**

- Allow users to **set custom Open Graph metadata** for their short links.
- Metadata should include:
    - **Title** (`og:title`).
    - **Description** (`og:description`).
    - **Image URL** (`og:image`).
- When a short link is shared on social media (Facebook, Twitter, LinkedIn), the **custom metadata should be displayed** instead of the default.

**Explicit Instructions:**

✅ Modify the `POST /shorten` and `PUT /update-destination` APIs to allow users to **set Open Graph metadata**.

✅ Store metadata fields in the database alongside each short link.

✅ Modify `GET /:short_code` to **return Open Graph metadata** when accessed by a social media crawler.

✅ Ensure **metadata updates reflect immediately**.

✅ Validate image URLs to ensure **they are publicly accessible**.

---

### **2. QR Code Generation**

- Automatically generate a **QR code for each short link** upon creation.
- Display the QR code on the **Link Detail Page**.
- Provide an **API endpoint to retrieve the QR code** in PNG/SVG format.

**Explicit Instructions:**

✅ Modify `POST /shorten` to **generate a QR code upon link creation**.

✅ Store the **QR code image URL** in the database.

✅ Implement an API (`GET /qr/:short_code`) to retrieve the **QR code for a given link**.

✅ Ensure QR codes are **optimized for fast loading**.

✅ Support **PNG & SVG formats** for different use cases.

---

### **3. QR Scan Tracking & Analytics**

- Track **QR scans** separately from regular short link clicks.
- When a QR code is scanned, log:
    - **Total Scans** → Count every scan event.
    - **Time of Scan** → Timestamp of each scan.
    - **Device & OS** → Identify whether the scan was done via iOS, Android, etc.
    - **Location (Country, City, Region)** → Extracted via IP-based geolocation.
    - **Referrer** → If available, track the scan source.

**Explicit Instructions:**

✅ Modify `GET /:short_code` to detect QR scans (e.g., by adding a `qr=1` query param).

✅ Store scan events in a new `qr_scans` table for tracking.

✅ Implement a `GET /qr-analytics/:short_code` API to **retrieve scan analytics**.

✅ Ensure scan logging **does not slow down redirections** (async tracking).

---

### **4. UI: Link Detail Page (With QR Analytics)**

Create the **frontend Link Detail Page UI** to manage individual links and their settings.

🔹 **Overview Section**

- Displays the **short URL** and **destination URL**.
- Shows **total clicks and key analytics summary**.

🔹 **Analytics Section**

- Clicks breakdown by:
    - **Time (Today, Last 7 Days, Last 30 Days)**.
    - **Location (Top Countries & Cities)**.
    - **Device Type (Mobile, Desktop, Tablet)**.
    - **Referral Sources (Social Media, Direct, Email, etc.)**.

🔹 **QR Code Section**

- Displays **QR code preview** for the short link.
- **Download QR code button** (PNG/SVG options).
- **QR Scan Analytics** → Shows total scans, scan locations, device breakdown.

🔹 **Edit Link Section**

- **Slug Change Input** → Users can modify the short code.
- **Destination URL Update Input** → Users can change the destination link.
- **Table of Previous Destinations** → Shows past URLs for reference.

🔹 **Open Graph Controls**

- Input fields for **title, description, and image URL**.
- **Preview button** to show how the link will appear on social media.

🔹 **Buttons / Actions**

- **"Save Changes"** → Updates the slug, destination, or metadata.
- **"Delete Link"** → Removes the short link (confirmation required).
- **"Copy Short URL"** → Copies link to clipboard.

---

### **5. Database Setup for Open Graph, QR Codes & QR Analytics**

- Modify the `links` table to store:
    - `og_title` (TEXT).
    - `og_description` (TEXT).
    - `og_image_url` (TEXT).
    - `qr_code_url` (TEXT).
- Create a new `qr_scans` table:

| Column | Type | Description |
| --- | --- | --- |
| `id` | `SERIAL` | Primary key |
| `short_code` | `VARCHAR(10)` | Foreign key linking to `links` table |
| `timestamp` | `TIMESTAMP` | Time of scan |
| `device_type` | `TEXT` | Mobile, Tablet, Desktop |
| `os` | `TEXT` | iOS, Android, Windows, etc. |
| `country` | `TEXT` | User’s country (from IP) |
| `city` | `TEXT` | User’s city (from IP) |
| `referrer` | `TEXT` | Source of scan, if available |

**Explicit Instructions:**

✅ Store QR analytics separately from regular clicks.

✅ Optimize queries to **quickly fetch scan data for reports**.

✅ Ensure **QR analytics are logged asynchronously** to prevent slow redirects.

---

### **6. Testing Requirements**

- Verify that **Open Graph metadata is correctly rendered** when sharing links on social media.
- Ensure **QR codes are generated and retrievable via API**.
- Test **QR scan tracking and analytics retrieval**.

**Explicit Instructions:**

✅ Use Facebook & Twitter debugging tools to **verify Open Graph previews**.

✅ Scan QR codes to **verify correct redirects & analytics tracking**.

✅ Test `GET /qr-analytics/:short_code` API for **accurate scan reporting**.

---

## **Success Criteria**

✅ Users can **set custom Open Graph metadata** for social sharing.

✅ Open Graph metadata is **correctly rendered on social media previews**.

✅ QR codes are **generated, downloadable, and trackable**.

✅ QR scans are **logged and retrievable via API**.

✅ **Link Detail Page UI is fully functional**, displaying analytics, QR codes, and edit options.

---