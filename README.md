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

  * `inventory.json` and `categories.json`: Current static data.
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

