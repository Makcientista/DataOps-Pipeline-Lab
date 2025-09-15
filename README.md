### **Git Configuration**

Create a new repository or clone an existing one and make your first commit to version control your project..

1.  **Clone the repository:** 
    ```bash
    git clone https://github.com/Makcientista/DataOps-Pipeline-Lab.git
    ```
    *Or, if you prefer to create a new one:*
    ```bash
    git init
    ```

2.  **Access the project folder:**
    ```bash
    cd DataOps-Pipeline-Lab
    ```

3.  **Add and save files (`commit`):**
    ```bash
    git add .
    git commit -m "Environment setup commands in the README."
    ```

4.  **Push changes to the remote repository:**
    ```bash
    git push origin main
    ```

5.  **Check commit history:**
    ```bash
    git log
    ```

6.  **Manage branches for new features:**
    ```bash
    git branch dev
    git checkout dev
    git merge main
    ```

---
### **Python Virtual Environment (`venv`)**

 Set up a virtual environment to isolate your project dependencies.


1.  **Create the virtual environment:**
    ```bash
    python -m venv venv
    ```

1.0 **Activate virtual env**
    ---bash
    .\venv\Scripts\activate
    ---

    
2.  **Generate the dependencies file (`requirements.txt`):**
    ```bash
    pip freeze > requirements.txt
    ```

3.  **Install dependencies (if `requirements.txt` already exists):**
    ```bash
    pip install -r requirements.txt
    ```

---

### **PostgreSQL on WSL**

Follow these steps to install and configure PostgreSQL on Windows Subsystem for Linux (WSL).

#### **Installation and Startup**

1.  **Update system packages:**
    ```bash
    sudo apt update && sudo apt upgrade -y
    ```

2.  **Install PostgreSQL:**
    ```bash
    sudo apt install postgresql postgresql-contrib -y
    ```

3.  **Start the database service:**
    ```bash
    sudo service postgresql start
    ```

#### **User and Database Configuration**

1.  **Access PostgreSQL interactive terminal (`psql`):**
    ```bash
    sudo -u postgres psql
    ```

2.  **Access PostgreSQL interactive terminal `makiesse`:**
    ```sql
    CREATE USER makiesse WITH SUPERUSER CREATEDB CREATEROLE LOGIN ENCRYPTED PASSWORD '123456789';
    ```

3.  **Create database `analise_funcionarios` and set owner:**
    ```sql
    CREATE DATABASE analise_funcionarios OWNER makiesse;
    ```

4.  **Grant privileges to the new database:**
    ```sql
    GRANT ALL PRIVILEGES ON DATABASE analise_funcionarios TO makiesse;
    ```

#### **External Access Configuration (Windows)**

Allow DBeaver, Python, and other tools on Windows to connect to the database running on WSL.

1.  **Find the configuration file:**
    ```bash
    sudo -u postgres psql -c "SHOW config_file;"
    ```

2.  **Edit `postgresql.conf`:**
    * Open the file in the text editor:
        ```bash
        sudo nano /etc/postgresql/16/main/postgresql.conf
        ```
    * Change:
     `#listen_addresses = 'localhost'` 
    
    to:
        ```bash
        listen_addresses = '*'
        ```

3.  **Edit `pg_hba.conf`:**
    * Open the file:
        ```bash
        sudo nano /etc/postgresql/16/main/pg_hba.conf
        ```
    * Add this line at the end to allow connections from any IP:
        ```
        host    all             all             0.0.0.0/0                 md5
        ```

4.  **Restart PostgreSQL service to apply changes:**
    ```bash
    sudo service postgresql restart
    ```

#### **Windows Firewall Port Release**

Enable port 5432 for PostgreSQL in Windows Firewall to allow connections.

1.  Open Windows **Firewall with Advanced Security**.
2.  In the left panel, click **Inbound Rules**, and then click **New Rule**... in the right panel. 
3. **Follow the wizard:**

* **Rule Type:** Select **Port**.

* **Protocol and Ports:** Choose **TCP** and enter **5432** in "Specific local ports".

* **Action:** Select **Allow the connection**.

* **Profile:** Check all (**Domain**, **Private**, **Public**).

* **Name:** Give it a name, e.g., `PostgreSQL_WSL`, and finish.

