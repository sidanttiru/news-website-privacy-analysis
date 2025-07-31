# news-website-privacy-analysis
An analysis of third-party data tracking and security dependencies on major news websites, such as the BBC, Daily Mail, and The Guardian.

# Comparative Analysis of Third-Party Data Exposure on News Websites

**Author:** Sidant Tiru
**Date:** July 2025
**Tools Used:** Burp Suite, Android Studio (for emulated mobile environment), Browser Developer Tools

---

## 1. Executive Summary

This project analyzes the network traffic of three major UK news websites—BBC News, The Guardian, and the Daily Mail—to quantify their use of third-party services and identify potential privacy and security implications. The analysis revealed a broad spectrum of data privacy practices, from BBC News, which featured no third-party requests, to the Daily Mail, where 30% of network requests were to third parties. A significant finding was the Daily Mail's critical dependency on the Zephr API for paywall and user experience management, which introduces a potential third-party security risk to its user data and platform stability.

---

## 2. Introduction & Objectives

In the modern digital landscape, news consumption inevitably involves sharing data. However, the extent and nature of this data sharing are often unclear to the end-user. This project aimed to make these practices transparent by accomplishing the following:

* **Objective 1:** Quantify the volume of third-party HTTP requests on selected news websites.
* **Objective 2:** Identify the key third-party services being utilized.
* **Objective 3:** Analyze the potential risks associated with these third-party dependencies.

---

## 3. Methodology

The analysis was conducted by simulating a user session on the mobile version of each news website.

1.  **Environment Setup:** An emulated mobile device was configured in Android Studio to provide a clean environment for each test.
2.  **Traffic Interception:** All network traffic from the emulated device was routed through **Burp Suite**, which was used to log and inspect every HTTP/S request.
3.  **User Simulation:** For each site, the ~15 minute session involved:
    * Navigating to the main homepage.
    * Observing idle traffic.
    * Loading additional content on the main homepage.
    * Clicking on a headline article to view its content.
    * Clicking on interactive elements within the article (share button, image links, etc).
4.  **Data Analysis:** Third-party requests were identified as any request made to a domain other than the primary news site's domain (e.g., `bbc.com`). The volume was calculated as: `(Total 3rd-Party Requests / Total Requests) * 100%`.

---

## 4. Findings & Analysis

The analysis revealed distinct privacy and data-sharing strategies for each platform.

| News Website   | Total Requests | 3rd-Party Requests | 3rd-Party Data Volume | Key Observations                                                                |
| :------------- | :------------- | :----------------- | :-------------------- | :------------------------------------------------------------------------------ |
| **BBC News** | 9              | 0                  | **0%** | Operates a clean, first-party-only environment. No tracking or analytics found. |
| **The Guardian** | 12             | 3                  | **25%** | Utilizes Google Analytics for basic visitor tracking, a standard industry practice. |
| **Daily Mail** | 20             | 6                  | **30%** | Heavy reliance on Google Analytics and the Zephr/Zuora platform.                |

### Deep Dive: The Daily Mail & The Zephr API

The most significant finding was the Daily Mail's deep integration with **Zephr** (a Zuora company), a subscription and paywall management service.

* **Function:** Zephr acts as an intelligent gatekeeper. When a user interacts with the site, requests are sent to the Zephr API, which makes real-time decisions about what content to show, whether to display a paywall, or what ads to serve.
* **Security Implication:** This creates a **critical third-party dependency**. The Daily Mail has effectively outsourced a core part of its user experience and security logic. A vulnerability, data breach, or outage at Zephr could directly impact the Daily Mail's users and operations, potentially exposing user activity data or disrupting access to the site. This highlights a significant supply chain risk.

---

## 5. Conclusion

This analysis demonstrates that not all news websites handle user data equally. While some platforms like the BBC prioritize a first-party experience, others embed third-party services at the core of their architecture. The case of the Daily Mail and Zephr illustrates that the use of third-party services goes beyond simple analytics and can introduce significant security and operational risks. Users should be aware that their data privacy and security are often dependent on a chain of companies, not just the website they are visiting.

---

## Appendix: Evidence

*See the `logs/` directory for screenshots of the network traffic logs captured during analysis.*
```
