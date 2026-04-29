# 🌾 AgriCloud™ Honeypot — Agriculture Threat Intelligence System

> **Programme:** Cloud Computing, Cyber Security & Ethical Hacking  
> **Technology:** Python · Socket Library · Threading · HTML · CSS · JavaScript  
> **Objective:** Build a TCP port listener for an Agriculture IoT portal that traps and logs unauthorised connection attempts.

---

## 📌 What Is This Project?

A **Honeypot** is a cybersecurity trap — a system *intentionally designed* to lure attackers and record their behaviour without exposing any real data.

This project builds a fully functional honeypot themed as a **Smart Farm Management Portal (AgriCloud™)**. The system operates on two layers:

| Layer | Technology | What It Does |
|---|---|---|
| **Network Layer** | Python (socket, threading) | TCP listener on port 8888 — logs every incoming connection |
| **Application Layer** | HTML / CSS / JavaScript | Fake agriculture web portal that traps and tracks attackers |

When someone connects to port 8888, the Python server:
1. **Logs** their IP address and timestamp to `honeypot_log.txt`
2. **Sends** a fake "AgriTech System" banner to deceive them
3. **Closes** the connection immediately

When someone tries to log in to the web portal with wrong credentials three times:
1. Red indicator dots fill up on each attempt
2. A **SESSION QUARANTINED** overlay appears
3. They are silently redirected to a **fake farm page** where every action returns "PERMISSION DENIED"
4. All events are logged and visible on the **Threat Intelligence Dashboard**

---

## 📁 File Structure

```
honeypot-agri/
│
├── honeypot.py              ← Core Python honeypot (TCP socket on port 8888)
├── test_connection.py       ← Simulates an attacker connecting (for testing)
├── honeypot.js              ← JavaScript tracker library (logs events to localStorage)
├── honeypot_log.txt         ← Auto-generated log file (created when honeypot.py runs)
│
├── index.html               ← Landing / navigation page
├── login.html               ← 🔐 Farmer login portal (main entry point)
├── farm-details.html        ← ✅ Real farmer dashboard (shown after valid login)
├── fake-page.html           ← 🪤 Honeypot trap page (shown after 3 failed logins)
├── honeypot_dashboard.html  ← 📊 Threat intelligence dashboard (SOC monitoring view)
├── honeypot_monitoring.html ← Monitoring overlay page
├── server-data.html         ← Server telemetry display
│
└── README.md                ← This file
```

---

## 🚀 How to Run

### Prerequisites
- Python 3.x installed (no pip packages needed — standard library only)
- A modern web browser (Chrome, Firefox, or Edge)
- Port 8888 free on your machine

---

### Step 1 — Start the Python Honeypot

Open a terminal and run:

```bash
python honeypot.py
```

You will see:

```
============================================================
  AGRICULTURE HONEYPOT — THREAT INTELLIGENCE SYSTEM
============================================================
  [*] Listening on  : 0.0.0.0:8888
  [*] Logging to    : /your/path/honeypot_log.txt
  [*] Started at    : 2026-04-17 10:00:00
============================================================
  [*] Waiting for connections... (Press Ctrl+C to stop)
```

The honeypot is now live. Every connection to port 8888 will be logged.

---

### Step 2 — Simulate an Attacker (optional, for testing)

Open a **second terminal** while `honeypot.py` is running:

```bash
python test_connection.py
```

You will see what an attacker sees:

```
[*] Simulating attacker connecting to 127.0.0.1:8888
[+] Connected! Receiving response from honeypot...

--- ATTACKER SEES THIS ---

==================================================
   AGRITECH SMART FARM MANAGEMENT SYSTEM v2.1
   IoT Sensor Dashboard | Crop Monitoring Portal
   Powered by AgriCloud Infrastructure
==================================================

[!] ALERT: Unauthorized access attempt detected.
[!] ACCESS DENIED - This incident has been logged.
[!] Your IP address and timestamp have been recorded.
[!] Authorities have been notified.

--- END OF RESPONSE ---
```

Meanwhile, in the first terminal, you will see:

```
[2026-04-17 10:03:29] CONNECTION ATTEMPT | IP: 127.0.0.1            | Port: 8888 | Status: LOGGED & DENIED
```

And `honeypot_log.txt` will be updated automatically.

---

### Step 3 — Open the Web Portal

Open `login.html` in your browser. All pages share state via `localStorage` — no server needed.

```
double-click login.html
OR
open login.html in your browser
```

---

## 🔐 Login Credentials

### Demo Accounts (pre-loaded)

| Role | Farmer ID | Password | What You See |
|---|---|---|---|
| **Admin** | `admin` | `1234` | Real farm dashboard (farm-details.html) |
| **Sample Farmer** | `AGF-2024-0042` | `farm1234` | Real farm dashboard (farm-details.html) |

### Create Your Own Account

1. Click **"Register as a New Farmer"** on the login page (or open `login.html` and look for the register link)
2. Fill in your farm details — name, farm type, location, crops, irrigation method
3. Your unique **Farmer ID** will be automatically generated in the format: `AGF-YYYY-XXXX`
   - Example: `AGF-2026-4721`
4. Note down your Farmer ID and the password you set
5. Log in with these credentials from the login page

---

## 🪤 How the Honeypot Trap Works

```
User opens login.html
       │
       ├── Correct Farmer ID + Password  ──► farm-details.html  ✅ Real dashboard
       │
       └── Wrong credentials × 3 times  ──► fake-page.html  🪤 Honeypot trap!
                                              │
                                              ├── Looks IDENTICAL to real dashboard
                                              ├── Sensor values fluctuate randomly (looks live)
                                              ├── Every button click → "PERMISSION DENIED"
                                              ├── Red panel shows attacker's attempted ID
                                              └── All actions silently logged
```

### Visual Lockout Indicators

| Attempt | What the Attacker Sees |
|---|---|
| 1st wrong login | First red dot lights up |
| 2nd wrong login | Second red dot lights up |
| 3rd wrong login | Third dot lights up → SESSION QUARANTINED overlay → redirect to fake page |

---

## 📊 Threat Intelligence Dashboard

Open `honeypot_dashboard.html` in your browser to view the SOC monitoring view.

### Dashboard Features

| Panel | Description |
|---|---|
| **Total Visits** | Number of times any page was loaded |
| **Login Attempts** | Number of failed credential attempts |
| **Access Denied** | Number of 3-strike lockouts triggered |
| **Unique IPs** | Number of distinct attacker IPs detected |
| **Live Log Table** | Real-time event feed with timestamp, event type, Farmer ID used |
| **Threat Gauge** | Conic ring gauge showing MONITORING → LOW → ELEVATED → HIGH → CRITICAL |
| **Activity Chart** | Bar chart showing attack activity over the last 10 events |
| **Top Source IPs** | Ranked list of most active attacker IPs |

### Threat Level Thresholds

| Attempts | Threat Level | Gauge Colour |
|---|---|---|
| 0 | MONITORING | 🟢 Green |
| 1 | LOW | 🔵 Blue |
| 2–4 | ELEVATED | 🟡 Amber |
| 5–9 | HIGH | 🟠 Amber/Orange |
| 10+ | CRITICAL | 🔴 Red |

### Dashboard Controls

- **SIM ATTACK** — Simulates a full attack sequence (visit → attempt → denied) for demonstration
- **CLEAR** — Clears all logs and resets all counters
- **OPEN HONEYPOT MONITORING** — Opens the monitoring overlay in a new tab

---

## 🐍 Python Code Concepts Explained

| Concept | Code | What It Does |
|---|---|---|
| `socket.AF_INET` | `socket.socket(socket.AF_INET, ...)` | Use IPv4 addressing |
| `socket.SOCK_STREAM` | `socket.socket(..., socket.SOCK_STREAM)` | Use TCP (reliable, connection-based) |
| `SO_REUSEADDR` | `setsockopt(SOL_SOCKET, SO_REUSEADDR, 1)` | Allow port reuse immediately after restart |
| `server_socket.bind()` | `bind(('0.0.0.0', 8888))` | Attach socket to all interfaces on port 8888 |
| `server_socket.listen(5)` | `listen(5)` | Queue up to 5 pending connections |
| `server_socket.accept()` | `client_socket, addr = accept()` | Block and wait for incoming connection |
| `threading.Thread` | `Thread(target=handle_connection, ...)` | Handle each attacker in a separate thread |
| `daemon=True` | `Thread(..., daemon=True)` | Thread auto-dies when main process exits |
| `open(LOG_FILE, 'a')` | `open('honeypot_log.txt', 'a')` | Append to log file (never overwrites) |
| `client_socket.send()` | `send(FAKE_BANNER.encode('utf-8'))` | Send fake banner bytes to attacker |

---

## 📊 Sample Log Output

```
============================================================
  HONEYPOT SESSION STARTED: 2026-04-17 10:03:14
  Listening on 0.0.0.0:8888
============================================================
[2026-04-17 10:03:29] CONNECTION ATTEMPT | IP: 127.0.0.1            | Port: 8888 | Status: LOGGED & DENIED
[2026-04-17 10:04:15] CONNECTION ATTEMPT | IP: 192.168.1.45         | Port: 8888 | Status: LOGGED & DENIED
[2026-04-17 10:05:02] CONNECTION ATTEMPT | IP: 10.0.0.23            | Port: 8888 | Status: LOGGED & DENIED
```

Each session appends a new header block — old logs are never overwritten.

---

## 🌐 Web Pages Overview

### `login.html` — Farmer Login Portal
- Animated agricultural sunrise background (sky gradient, clouds, crop field)
- Farmer ID + password authentication
- Three-dot failed attempt indicator (dots turn red one-by-one)
- Live stats counters (visits / attempts / denied) pulled from localStorage
- Links to registration page

### `farm-details.html` — Real Farmer Dashboard ✅
Shown only after a successful login. Contains:
- 6 live IoT sensor panels: Soil Temperature, Moisture, pH, Light, Wind, Tank Level
- Crop status table with health indicators and days-to-harvest
- 7-day weather forecast strip
- Irrigation schedule with progress bars
- Season yield forecast

### `fake-page.html` — Honeypot Trap 🪤
Shown after 3 failed logins. Looks identical to the real dashboard but:
- All sensor values are randomly generated (Math.random() every 3 seconds)
- Every action button triggers: `PERMISSION DENIED — INVALID SESSION`
- Red intelligence panel at the bottom shows:
  - Attacker's attempted Farmer ID
  - Quarantine timestamp
  - "THREAT CAPTURED" status

### `honeypot_dashboard.html` — SOC Threat Intelligence Dashboard 📊
The monitoring view for security analysts. Shows:
- Real-time log table (polls localStorage every 800ms)
- Threat level gauge with conic gradient
- 10-bar activity chart
- Top source IPs panel

---

## 🧠 Learning Objectives Achieved

- ✅ **Socket Programming** — Creating TCP server sockets using Python's `socket` library
- ✅ **Port Listening** — Binding to port 8888 and accepting TCP connections
- ✅ **Threat Logging** — Recording IP addresses and timestamps to a persistent file
- ✅ **Multi-threading** — Handling simultaneous connections with `threading.Thread`
- ✅ **Deception Technology** — Sending fake banners and redirecting attackers
- ✅ **Web Security Concepts** — Login lockout, unique ID system, fake portals
- ✅ **Agriculture IoT Context** — Realistic smart farm portal as the deception theme

---

## 🛠️ Requirements

- **Python** 3.x (standard library only — `socket`, `threading`, `datetime`, `os`)
- **Browser** — Chrome, Firefox, or Edge (any modern browser)
- **VSCode** (recommended for editing and running)
- **Port 8888** — must be available (not blocked by firewall)
- No `pip install` needed

---

## ⚠️ Ethical & Legal Notice

This project is built **strictly for educational purposes** as part of a Cloud Computing, Cyber Security & Ethical Hacking programme.

> ❌ **Do NOT deploy on a public server.**  
> ❌ **Do NOT use to trap real users without authorisation.**  
> ❌ **Do NOT run on networks you do not own.**  
>
> Deploying honeypots on networks without permission is **illegal**. This system is designed to run on `localhost` for classroom demonstration only. Always obtain written permission before running any security tools on a network.

---

## 📚 References

- [Python `socket` Library Docs](https://docs.python.org/3/library/socket.html)
- [Python `threading` Library Docs](https://docs.python.org/3/library/threading.html)
- [NIST Guide to Intrusion Detection](https://www.nist.gov/)
- [OWASP — Honeypots](https://owasp.org/)
- [CISA — What is a Honeypot?](https://www.cisa.gov/)
- [MDN — Window.localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)

---

## 👨‍💻 Project Info

| Field | Details |
|---|---|
| Programme | Cloud Computing, Cyber Security & Ethical Hacking |
| Technology | Python, Socket Library, HTML/CSS/JS |
| Platform | localhost / VSCode |
| Honeypot Port | 8888 (TCP) |
| Context | Agriculture IoT — Smart Farm Management |
| Log File | `honeypot_log.txt` (auto-created) |
| State Storage | Browser `localStorage` |

---

*Made with 🌾 for Cloud Computing, Cyber Security & Ethical Hacking · AgriCloud™ Honeypot Project · 2026*
