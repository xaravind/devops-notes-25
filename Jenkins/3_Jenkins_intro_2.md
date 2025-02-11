### **Jenkins Folder Structure**

In **Windows OS**, Jenkins stores all its information and configuration in the following path:

```
C:\ProgramData\Jenkins\.jenkins\
```

#### **Folder Breakdown:**

1. **`workspace`**:
   - Contains all the information about **configured jobs**.
   - Stores the workspace and build artifacts for each job.

2. **`secrets`**:
   - Stores **configured secrets** such as credentials and tokens.
   - Ensures sensitive data is handled securely.

3. **`plugins`**:
   - Contains all **installed plugins**, both manually installed and automatically by Jenkins.
   - Each plugin has its own directory, including configuration files and resources.

4. **`nodes`**:
   - Contains information about **configured nodes** (master or agents).
   - Stores node configuration details.

5. **`logs`**:
   - Stores **log files** related to Jenkins operations.
   - Includes logs from agents (if configured) and task logs.
   - You can also access Jenkins logs through the **Jenkins GUI**:
     - **Manage Jenkins → System Log → All Jenkins Logs**.
     - The logs provide detailed information on Jenkins system activities, job executions, user authentication, plugin activity, and internal Jenkins events.

#### **Important Jenkins Configuration File**:

- **`config.xml`**: 
   - The **primary configuration file** for Jenkins.
   - Located in `JENKINS_HOME/config.xml`.
   - Contains key configurations related to global settings, security, and plugin configurations.
   - **Important**: Always **backup** the `config.xml` file before modifying it.

---

### **What If a Jenkins User Forgets Their Password?**

#### **1. Resetting the Password (Admin Access Required)**

**Via Jenkins GUI** (for Admins):
   - Navigate to **Manage Jenkins → Security → Users**.
   - Find and select the user whose password needs to be reset.
   - In the **Security** section, reset the password:
     - Enter the new password and confirm it.
     - Click **Save** to apply the changes.

**User Must Reset Their Own Password**:
   - After the admin resets the password, the user must reset it themselves:
   
   **Steps for the User**:
   - Log in to Jenkins.
   - Click on the **username** at the top right corner.
   - Go to **Security**.
   - Enter and confirm the **new password**.
   - Click **Save**.

#### **2. File-Based Password Reset (Not Recommended)**

- **Location of User Configuration File**:
  - User configurations are stored in:  
    `JENKINS_HOME/users/<user_name>/config.xml`
  
  **Warning**: 
  - **Modifying** the `config.xml` file directly is **not recommended** as it can corrupt the user configuration. 
  - Instead, you can **delete the user's folder**, but this will result in the loss of their settings and data.

#### **3. External Authentication Systems (LDAP/Active Directory)**

- If Jenkins is using **external authentication** (e.g., **LDAP** or **Active Directory**), passwords are not stored in Jenkins.
- Users must reset their password via the external authentication provider (e.g., AD/LDAP system).
- Contact the system administrator for assistance with password resets in the external system.

#### **4. Deleting and Re-Creating the User (Last Resort)**
- If the normal user cannot be reset due to limited access or external authentication, you could:

- Delete the user (as an admin).
- Recreate the user: Admin can create a new user with the same username, ensuring the user can log in again (though this may cause loss of previous settings for the user).


---
### **What To Do If Jenkins Admin Forgets Their Password**

If you forget your Jenkins admin password, follow these steps to regain access.

#### **1. Backup Jenkins Configuration**
- Navigate to Jenkins Home Directory:
  - Path: `C:\ProgramData\Jenkins\.jenkins\`
- Take a backup of the `config.xml` file in case anything goes wrong during the process.

#### **2. Disable Security Temporarily**
- Open the `config.xml` file in a text editor.
- Locate the following line:
  ```xml
  <useSecurity>true</useSecurity>
  ```
- Change the value from `true` to `false`:
  ```xml
  <useSecurity>false</useSecurity>
  ```
- Save the changes to `config.xml`.

#### **3. Restart Jenkins**
- Restart Jenkins to apply the changes.
- Access Jenkins via the web browser. You should be able to log in without authentication.

#### **4. Modify Security Settings**
- In Jenkins, go to **Manage Jenkins**.
- Click on **Configure Global Security** under the Security section.
  
  **Security Realm:**
  - Change the **Security Realm** from `None` to **Jenkins’ own user database**.
  - Save the changes.

  **Authorization:**
  - Change the **Authorization Strategy** from **Anyone can do anything** to either **Matrix-based security** or **Project-based Matrix Authorization Strategy**.
  - Uncheck **Anonymous user** to remove administrative privileges from anonymous users.
  - Click **Save**.

#### **5. Add New Admin User**
- Click on the **Add user** button.
- Create a new admin user with the desired credentials.
  - Assign the **Administrator** role to this user.
  
#### **6. Change Admin User Password**
- In the **Manage Jenkins** section, go to **Manage Users**.
- Select the newly created admin user and change the password as needed.
- Save the changes.

#### **7. Restore Security Settings**
- After restarting Jenkins, verify that the newly created admin user has full administrative privileges.
- Return to the `config.xml` file.
- Change the `<useSecurity>` setting back to `true`:
  ```xml
  <useSecurity>true</useSecurity>
  ```
- Save the file.

#### **8. Final Restart**
- Restart Jenkins one more time to finalize all changes.
- Confirm that the system is secure, and the admin user has the proper permissions.

These steps should help restore administrative access to Jenkins while keeping the system secure.

---



### **Ways to Trigger Jenkins Jobs**

#### 1. **Trigger Builds Remotely**  
   Allows triggering Jenkins builds remotely via a predefined URL, which is useful for scripts or external systems like SCM hooks.

   **Steps to Create an API Token**:
   - Click on your **username/admin name** at the top right corner.
   - Go to **Security**.
   - Under **API Token**, click **Add New Token**.
   - Copy the token (you can only view it once during creation).
   - Click **Save**.

   **Usage**:  
   You can trigger a build remotely using the following URL:
   ```
   JENKINS_URL/job/<job_name>/build?token=<TOKEN_NAME>
   ```
   Alternatively, for builds with parameters:
   ```
   JENKINS_URL/job/<job_name>/buildWithParameters?token=<TOKEN_NAME>
   ```
### 1.3 **Build Trigger Automatically**
A real-life scenario could be triggering jobs automatically, such as after a deployment is completed.

- **Trigger Jobs Remotely with Scripts**:
  - You can use the **remote trigger** mechanism with URLs and tokens to trigger jobs programmatically. This is useful for scenarios like:
    - Testing teams triggering jobs after deployments.
  - Configure the job with an **Authentication Token**.
  - Remote teams can then use the token to trigger the job remotely via scripts or webhooks.

---

#### 2. **Build After Other Projects are Built (Upstream and Downstream)**  
   Configure a job to trigger after other projects are built. This is useful for running tests after a build, for example.

   - **Steps**:
     - In the job configuration, under **Build Triggers**, select **Build after other projects are built**.
     - Specify the **job name** of the project to watch.
     - Choose one of the options:
       - **Trigger only if the build is stable**
       - **Trigger even if the build is unstable**
       - **Trigger even if the build fails**
       - **Always trigger, even if the build is aborted**
---


###  **Build After Other Projects**
You can configure Jenkins to trigger a job after another job completes.

- **Create Multiple Jobs**:
  - For example, create three jobs: **Job1**, **Job2**, and **Job3**.
  
- **Configure Downstream Jobs**:
  - For **Job1**, configure it to trigger **Job2** under the **Build Triggers** section. (Job2 is the downstream job).
  - For **Job2**, configure it to trigger **Job3** similarly.
  
- **Result**:  
  - When **Job1** is triggered, it will automatically trigger **Job2**, and once **Job2** finishes, it will trigger **Job3**.

---

#### 3. **Build Periodically**  
   Configure Jenkins to trigger a build at regular intervals using **cron syntax**.

   **Cron Syntax**: 
   ```
   MINUTE HOUR DOM MONTH DOW
   ```

   - **MINUTE**: 0–59 (minute of the hour)
   - **HOUR**: 0–23 (hour of the day)
   - **DOM**: 1–31 (day of the month)
   - **MONTH**: 1–12 (month)
   - **DOW**: 0–6 (day of the week, where 0=Sunday)

   Example:
   - `H/5 * * * *` (Polls every 5 minutes)
   - `H 4 * * 1-5` (Polls at 4 AM, Monday to Friday)
#### **Build Periodically / Poll SCM Trigger**
To trigger a Jenkins job periodically or based on changes in source control, follow these steps:

- **Create a New Job**:
  - Go to Jenkins dashboard and click on **New Item**.
  - Select **Freestyle project** and name your job.

- **Configure Build Periodically**:
  - Go to **Configure** of your job.
  - Under **Build Triggers**, check the box for **Build Periodically**.
  - Under **Schedule**, configure the schedule using cron syntax. Example:

    ```
    * * 8 2 6
    ```
    Example: `* * 8 2 6` will trigger the job every minute, every hour, on Saturday, 8th February.



#### 4. **GitHub Hook Trigger for Git SCM Polling**  
   Trigger builds when Jenkins receives a GitHub push hook. Jenkins checks whether the hook came from the GitHub repository defined in the **SCM/Git** section of the job.

   **Note**: This requires the **GitHub plugin** to be installed and the GitHub webhook to be configured.

#### 5. **Poll SCM**  
   - Jenkins checks for changes in the source code repository at regular intervals. If changes are detected, Jenkins triggers a build.
   - The polling schedule is configured using **cron syntax**.
   - **Advantages**: Ideal for continuous integration, but can be **resource-intensive** and may cause **delays** depending on the polling interval.

---

#### **How to Change the Default Port Number of Jenkins**

If you need to change the default port number Jenkins uses (8080), follow these steps:

- Go to the Jenkins installation directory:
  - Path: `C:\Program Files\Jenkins`

- Before making any changes, **take a backup** of the `jenkins.xml` file to ensure you can restore the original settings if needed.

- Open the `jenkins.xml` file in a text editor (e.g., Notepad++ or Visual Studio Code).

- Search for the following line in the file:
  ```
  --httpPort=8080
  ```
- Replace `8080` with your desired port number, for example:
  ```
  --httpPort=9090
  ```

- After modifying the port number, save the `jenkins.xml` file.

- Restart Jenkins for the changes to take effect.
- After Jenkins restarts, it will be accessible on the new port you specified (e.g., `http://localhost:9090`).

---

