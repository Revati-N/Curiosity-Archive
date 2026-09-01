## Summary
- GitHub experienced a major outage on August 17, 2026, lasting **7 hours and 47 minutes**.
- Services affected: github.com, authentication, GitHub Actions, APIs, Pull Requests, Issues, and Copilot.
This was GitHub's **second significant incident in August** (following an Actions failure on August 6).

---

## Key Details

| Field          | Info                                                                                                                 |
| -------------- | -------------------------------------------------------------------------------------------------------------------- |
| **Date**       | August 17, 2026                                                                                                      |
| **Duration**   | 7 hours 47 minutes                                                                                                   |
| **Root Cause** | Capacity failure — traffic reached a new peak, critical infrastructure in the Central US data center failed to scale |

---

## What Happened
- Traffic hit a new peak, overwhelming a critical infrastructure component in GitHub's Central US data center..
- Capacity pressure cascaded through systems, causing authentication failures and multi-service disruption..
- Recovery involved **rerouting traffic, isolating affected infrastructure, and restoring services in stages**..
- Copilot recovery took longer than other services — errors triggered a **client-side retry loop** that amplified traffic during recovery, which had to be mitigated before service could safely resume..

---

## GitHub's Response & Path Forward
- Added **3+ million CPU cores**, **120 petabytes of high-speed storage**, and significant network capacity..
- Migrating to **Azure** — currently serving ~58% of GitHub's platform load (up from 12% in May)..
- Commits/month grew from **1.4B → 2.9B** since April 2026, reflecting AI-driven demand surge..
- Implementing **retry limits, retry budgets, and variable timeouts** across services to prevent retry storms..
- Working on **isolating critical systems** and removing shared dependencies to limit blast radius..

---

## Source

[GitHub Blog — "The August 17 outage, and the work ahead"](https://github.blog/news-insights/company-news/the-august-17-outage-and-the-work-ahead/)
*Written by Vlad Fedorov (GitHub CTO)*
