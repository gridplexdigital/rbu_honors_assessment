# rbu_honors_assessment

This is a solid plan. It shifts the focus slightly more towards the infrastructure configuration (Nginx/Tunneling) while keeping the coding aspect relevant.

**Correction on Scoring:**
Your breakdown sums to **80 points** (20+20+20+10+10). To reach **100**, I have added a category for **"Service Reliability & Logic Integration" (20 marks)**. This ensures they actually use your legacy code (`lib.py`) and set up a background service (Systemd/PM2), which is critical for a "deployment" test.

Here is the finalized **Grading Rubric** and the **`README.md`** file you should put in the root of the repository.

-----

### **1. Finalized Grading Rubric (100 Points)**

| Category | Task | Marks | Criteria (Checklist) |
| :--- | :--- | :--- | :--- |
| **Environment** | **Setup** | **10** | • Virtual Env created (Python `venv` or Node `modules`).<br>• `requirements.txt`/`package.json` exists.<br>• Code runs on the designated Linux/WSL distro. |
| **Database** | **Migration & Persistence** | **20** | • Data migrated from JSON to SQLite/Postgres.<br>• Schema is correct (types match).<br>• App reads/writes to DB (not the file). |
| **Development** | **REST API** | **20** | • `GET` endpoint returns JSON list.<br>• `POST` endpoint accepts JSON.<br>• HTTP Status codes are correct (200 for OK, 400 for errors). |
| **Infrastructure** | **Nginx Reverse Proxy** | **20** | • Nginx installed and running.<br>• Configuration isolates the app (App runs on localhost:8000, Nginx on :80).<br>• Direct access to port 8000 is blocked (optional bonus) or Nginx correctly forwards traffic. |
| **Network** | **Tunneling** | **10** | • Public URL provided (ngrok/Cloudflare).<br>• External request hits Nginx -\> Hits App -\> Returns Data. |
| **Reliability** | **Service & Logic** | **20** | • **Logic:** Used provided `lib.py` for calculations (didn't rewrite it).<br>• **Service:** App runs via `systemd` or `pm2` (daemonized). It does not die if the SSH session closes. |

-----

### **2. The Repository Content**

You need to create a folder, initialize git, and add these three files.

#### **File 1: `inventory.json` (The Data)**

```json
[
  {"id": 1, "name": "Wireless Mouse", "price": 25.50, "stock": 10},
  {"id": 2, "name": "Mechanical Keyboard", "price": 120.00, "stock": 5},
  {"id": 3, "name": "USB-C Hub", "price": 45.00, "stock": 20},
  {"id": 4, "name": "Monitor Stand", "price": 30.00, "stock": 8}
]
```

#### **File 2: `lib.py` (The Legacy Logic - Python Version)**

*(If the user chooses Node.js, they must port this logic or call this script via a child process—let them decide).*

```python
class PricingService:
    @staticmethod
    def calculate_final_price(base_price, quantity, tax_rate=0.18):
        """
        Legacy business logic. DO NOT MODIFY.
        Applies a bulk discount of 10% if quantity > 5.
        Adds 18% tax.
        """
        subtotal = base_price * quantity
        
        if quantity > 5:
            subtotal = subtotal * 0.90  # 10% discount
            
        total = subtotal * (1 + tax_rate)
        return round(total, 2)
```

#### **File 3: `README.md` (The Test Instructions)**

*(Copy and paste the block below into a README.md file)*

-----

# 🚀 DevOps & Backend Challenge: Legacy Migration

**Time Limit:** 3 Hours
**Total Score:** 100

## 📝 Scenario

We have a legacy inventory script that currently stores data in a static JSON file. We need to modernize this by moving it to a real database, wrapping it in a REST API, and deploying it behind a robust web server.

## 🛠 Prerequisites

  * **OS:** WSL (Ubuntu/Debian) or a Standard Linux VM.
  * **Language:** Python (FastAPI/Flask) OR Node.js (Express).
  * **Tools:** Nginx, SQLite/PostgreSQL, git.

## 📂 Repository Contents

  * `inventory.json`: Current static data.
  * `lib.py`: Contains critical business logic for pricing. **You must use this.**

-----

## 🎯 Objectives

### 1\. Environment & Database (30 Marks)

  * Setup a proper project environment (virtualenv for Python or correct package.json for Node).
  * Write a migration script to move data from `inventory.json` into a SQL database (SQLite is acceptable).
  * **Deliverable:** A database file (or schema) populated with the JSON data.

### 2\. REST API Development (20 Marks)

Create a web server (running on `localhost:8000`) with the following endpoints:

  * `GET /api/products`: Return all products from the database.
  * `POST /api/order`: Accept `{"product_id": 1, "quantity": 2}`.
      * **Constraint:** You must use the logic inside `lib.py` to calculate the price.
      * Update the stock in the database (decrement quantity).
      * Return the total price calculated by the legacy library.

### 3\. Nginx Reverse Proxy (20 Marks)

  * Install and configure **Nginx** to listen on Port 80.
  * Configure Nginx to proxy requests from `http://localhost/api/...` to your application running on `http://localhost:8000`.
  * Direct traffic should go through Nginx, not directly to the app port.

### 4\. Service Reliability (20 Marks)

  * The application must **not** run in a foreground terminal tab.
  * Create a **systemd unit file** (or use PM2) to ensure the application runs in the background and restarts automatically if the server reboots or crashes.

### 5\. Public Access (10 Marks)

  * Expose your local Nginx server to the public internet securely.
  * Use **ngrok**, **Cloudflare Tunnel**, or a similar tool.
  * **Deliverable:** A public URL (e.g., `https://xyz.ngrok.io/api/products`) that serves your API.

-----

## 📤 Submission Guidelines

1.  **Code:** detailed `server` code and `migration` scripts.
2.  **Config:** Include your Nginx configuration file (usually in `/etc/nginx/sites-available`).
3.  **Service:** Include your systemd unit file (usually in `/etc/systemd/system`).
4.  **Live Link:** Provide the active tunneling URL.

**Good Luck\!**

