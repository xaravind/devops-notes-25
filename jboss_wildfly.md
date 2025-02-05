---

# **WildFly 35.0.0.Final Overview**
WildFly is an open-source Java application server developed by Red Hat. It is the successor to JBoss AS (Java Application Server).

## **Ports**
- **Admin Console**: `9990`
- **Application Server**: `8080`

## **Installation Prerequisites**
- **Java JDK**: Required for WildFly to run.

### **Download WildFly**
- [WildFly Download](https://www.wildfly.org/downloads/)

---

# **Important WildFly Folders**
1. **appclient** – Contains client-side libraries.
2. **bin** – Contains executables like:
   - `standalone.bat` (for standalone mode)
   - `jboss-cli.bat` (for CLI management)
   - Other utility scripts.
3. **docs** – Documentation files.
4. **domain** – Contains configuration files for domain control:
   - `domain.xml` and `host.xml`.
5. **modules** – Stores WildFly modules and libraries.
6. **standalone** – Contains standalone-specific configurations:
   - **configuration** – Configuration files for standalone server.
   - **deployments** – Folder where applications are deployed.
   - **logs** – Log files.
7. **welcome-content** – Default content displayed by WildFly.

---

# **Starting WildFly**
1. **Navigate to the WildFly `bin` folder**:
   - `<wildfly_home>/bin/`
2. **Start WildFly in standalone mode**:
   - Execute the `standalone.bat` script.
3. **Access the Admin Console**:
   - URL: `http://localhost:9990` or `http://127.0.0.1:9990`.

---

# **Creating an Admin User**
1. **Navigate to WildFly `bin` folder**:
   - `<wildfly_home>/bin/`
2. **Run the add-user script**:
   - Execute `add-user.bat`.
   - Choose option `a` to create a management user.
   - Provide username and password.
   - Set group as `none`.
   - Select `yes` to complete.

---

# **Deploying Applications in WildFly**

### **Deployment Methods**
1. **Via GUI** (Admin Console):
   - Application deployment is enabled by default.
2. **Via File System**:
   - Copy your `.war` file to `<wildfly_home>/deployments`.
   - Restart the server to enable the application.
3. **Via CLI**:
   - Execute the following in the CLI:
     - `cp .war <wildfly_home>/deployments`.

### **Accessing the Application**
- URL: `http://localhost:8080/<app-name>/`.

---

# **Shutting Down WildFly**
- **Via Console**: Press `Ctrl + C` in the standalone prompt.
- **Via CLI**: Execute `bash-cli.bat --connect --shutdown`.
- **Via Admin Console**:
   - Go to **Runtime** → Select the server → Click **Suspend**.
   - Server can be restarted from here.

---

# **Changing Default Ports in WildFly**
1. **Locate the Configuration File**:
   - Path: `<wildfly_home_dir>/standalone/standalone.xml`.
2. **Modify Port Settings**:
   - **Backup the file** before making changes.
   - Update the following lines in `standalone.xml`:
   ```xml
   <socket-binding name="http" port="${jboss.http.port:8080}"/>
   <socket-binding name="management-http" interface="management" port="${jboss.management.http.port:9990}"/>
   ```
   - Change the port numbers for the admin and application servers as needed.

---

# **WildFly Artifact Status**

WildFly uses the following statuses for deployments:
- **Installed**: The deployment is installed but not started.
- **Deployed**: The deployment is installed and started.
- **Undeployed**: The deployment is not installed.
- **Failed**: The deployment failed to start.
- **Suspended**: The application is temporarily stopped but can be resumed later.
- **Redeploying**: The application is being updated with new code.

These statuses can be checked using the CLI tool.

---

# **Domain Control and Host Control**

WildFly’s domain and host control architecture allow for the management of multiple WildFly instances across various hosts.

## **Domain Controller**
The **Domain Controller** manages the entire domain. Its responsibilities include:
- Ensuring consistent configuration across all WildFly instances in the domain.
- Managing the lifecycle of servers across the domain.
- Providing a central management interface (preferably accessible via HTTP(S)) for other hosts.

## **Host Controller**
Each host in the domain runs a **Host Controller**, responsible for:
- Managing the configuration of WildFly servers on its host.
- Registering itself with the Domain Controller.
- Launching and managing the lifecycle of servers on its host.
- Ensuring consistency of server configurations with the domain’s policy.

---

# **Configuration and Interaction Between Domain and Host Controllers**

1. **Configuration Files**:
   - The **Host Controller** reads its configuration from `domain/configuration/host.xml`, which contains host-specific details like server listings and interface configurations.
   
2. **Communication**:
   - Host Controllers communicate with the Domain Controller for registration and management of server instances. This is configured in `host.xml`.

3. **Authentication**:
   - Host Controllers may require authentication to connect to the Domain Controller. This is also configured within the `host.xml` file.

---
