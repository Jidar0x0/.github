<div align="center">
  <img src="assets/jidar-logo.png" alt="Jidar Logo" width="220" />
  <br><br>
  
  [![Status](https://img.shields.io/badge/Status-Private_Beta-blue.svg?style=for-the-badge&color=000000)](#)
  [![Security](https://img.shields.io/badge/Security-Zero_Trust-success.svg?style=for-the-badge&color=2ea043)](#)
  [![AI Powered](https://img.shields.io/badge/Powered_by-AI-8A2BE2.svg?style=for-the-badge&color=6e40c9)](#)
</div>

---

## 👁️ What is Jidar?

> [!IMPORTANT]  
> **Jidar** is an innovative cybersecurity platform engineering the next evolution of **Security Information and Event Management (SIEM)**. By integrating advanced Artificial Intelligence, we empower organizations to detect, analyze, and respond to threats with unprecedented speed and accuracy. 

Our Virtual SecOps platform is specifically tailored for Startups and SMEs, providing enterprise-grade security without the overhead. 

---

## 🔌 Jidar Official SDKs

To securely feed data into our AI engines, we provide lightweight, production-ready security event ingestion libraries. These SDKs collect security events from your backend (failed logins, suspicious API requests, etc.) and securely transmit them to the Jidar platform.

> [!NOTE]  
> *Detection, alerting, and automated responses happen entirely in the Jidar backend. The SDKs are strictly zero-overhead data shippers.*

### 📦 Supported Ecosystems

| SDK Package | Min Version | Status |
| :--- | :--- | :--- |
| 🟢 **TypeScript / Node.js** | Node 18+ | ✅ Reference Implementation |
| 🐍 **Python** | Python 3.8+ | ✅ Production Pilot |
| 🐘 **PHP** | PHP 8.1+ | ✅ Production Pilot |
| 🔵 **Go** | Go 1.21+ | ✅ Production Pilot |
| ☕ **Java** | JDK 11+ | ✅ Production Pilot |
| 🟣 **.NET** | .NET 8+ | ✅ Production Pilot |

---

## ⚡ Rock-Solid Reliability Guarantees

Integrating a security tool should never break your app. Our SDKs are built on a strict reliability contract:

*   ⏱️ **Non-blocking & Auto-flush:** Capture methods return instantly. Events are batched (default: 20) and flushed every 5 seconds.
*   🛡️ **Bounded Queue & No-crash:** Max 1000 events / 10 MB in memory. Internal SDK errors are swallowed and will *never* propagate to your application.
*   🔁 **Circuit Breaker & Retry:** Exponential backoff with jitter. Stops sending after 5 consecutive failures, with a 5-minute cooldown.
*   🛑 **Graceful Shutdown:** Ensures pending events are flushed before application exit.

---

## 🔒 Security & Data Privacy (Zero-Leakage)

We operate on a strict Zero-Trust model. Jidar SDKs automatically sanitize all metadata *before* it leaves your infrastructure:

*   **Auto-Redaction:** 23 sensitive keys (passwords, tokens, cookies, API keys) are automatically stripped.
*   **Deep Inspection:** Case-insensitive matching and recursive sanitization up to 5 levels deep.
*   **Immutable Operations:** Your original application data structures are never mutated.

---

## 🏗️ Architecture Overview
```text
┌─────────────────────────────────────────────────────┐
│                   Your Backend                      │
│                                                     │
│   login_handler():                                  │
│     if auth_failed:                                 │
│       jidar.capture_failed_login(email, ip, reason) │
└─────────────────┼───────────────────────────────────┘
                  ▼
┌─────────────────────────────────────────────────────┐
│              Jidar SDK Pipeline                     │
│  ┌──────────┐  ┌──────────┐  ┌───────────────────┐  │
│  │Sanitizer │→ │ Bounded  │→ │  Batched HTTP     │  │
│  │(strip    │  │ Queue    │  │  POST /api/logs/  │  │
│  │ secrets) │  │ (1K/10MB)│  │  + retry / break  │  │
│  └──────────┘  └──────────┘  └───────────────────┘  │
└─────────────────┼───────────────────────────────────┘
                  ▼
┌─────────────────────────────────────────────────────┐
│        Jidar AI Platform (api.jidar0x0.com)         │
│     Detection → Alerting → Response → Dashboard     │
└─────────────────────────────────────────────────────┘
