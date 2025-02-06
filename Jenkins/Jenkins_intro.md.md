### Jenkins Overview
- **Jenkins** is an open-source automation server primarily used for continuous integration (CI) and continuous delivery (CD) in software deployment.
- It automates several stages of the software development lifecycle, including:
  - **Building**
  - **Testing**
  - **Deploying** applications

---

### Why "Continuous Delivery" is More Accurate than "Continuous Deployment"
- **Continuous Delivery (CD)**: Jenkins automates the delivery pipeline up to the point of production, but **manual intervention** (such as approval) is usually required before deploying to production.
- **Continuous Deployment (CD)**: Involves full automation, where code is deployed directly to production without manual approval.
- **Context**: Jenkins focuses on **Continuous Delivery**, as it automates much of the process but still allows for manual approval in some steps before production deployment.

---

### Popular CI/CD Tools
- **Jenkins**
- **GitLab CI/CD**
- **TeamCity**
- **Travis CI**
- **AWS CodePipeline**
- **Azure DevOps**
- **Octopus**

---

### Why Jenkins?
1. **Improves Quality**:
   - Automates unit tests and other testing processes.
2. **Increases Productivity**:
   - Saves time by automating repetitive tasks like builds and deployments.
3. **Reduces Risk**:
   - Minimizes human error by automating manual processes.
4. **Features**:
   - **Easy Installation**: Jenkins is simple to install and configure.
   - **Upgrades Available**: Easy to upgrade with minimal downtime.
   - **Lightweight Container Support**: Ideal for containerized environments.
   - **Advanced Security**: Offers enhanced security features.
   - **Optimized Performance**: Ensures fast performance even under load.
   - **Distributed Team Management**: Allows teams to manage Jenkins in a distributed way.
5. **Additional Features**:
   - **Open-Source**: Free and actively maintained.
   - **Good Plugin Support**: Wide variety of plugins for various integrations.
   - **Strong Community Support**: Large community for troubleshooting and best practices.
   - **Fast and Reliable**: Handles large-scale builds and deployments smoothly.
   - **Good OS Support**: Runs on various operating systems.
   - **Scripted Builds**: Customizable build scripts for complex workflows.

---

### What is CI/CD?
- **Continuous Integration (CI)**: Frequent integration of code into a shared repository with automated builds and tests.
- **Continuous Delivery (CD)**: Ensures that code is always in a deployable state, with automated deployment and testing but requires **manual approval** for production.
- **Continuous Deployment (CD)**: Fully automates testing and deployment to production, with no manual intervention required.

---

### Plugin Management in Jenkins
- **Tabs in Plugin Management**:
  - **Update**: Shows installed plugins that have updates available.
  - **Available**: Lists plugins available for installation.
  - **Install**: Displays plugins that are installed and do not need updates.
  - **Advanced**: Allows configuration of HTTP proxy, uploading plugins, and specifying plugin sources.
  
- **Important Note**: In real-world environments, plugin installation may face challenges due to proxy settings, VPNs, or network restrictions. It is advisable to configure the **HTTP Proxy** in the **Advanced** tab of Jenkins' plugin management to ensure seamless installation.

---

### How to Install Jenkins on Windows
**Prerequisites**:
- **Minimum hardware requirements**:
  - 256 MB of RAM
  - 1 GB of drive space (10 GB recommended for Docker containers)
- **Recommended hardware for small teams**:
  - 4 GB+ of RAM
  - 50 GB+ of drive space

**Download Location for Windows**:
- [Jenkins Download Page](https://www.jenkins.io/download/#downloading-jenkins)

**Installation Steps**:
1. Follow the [installation guide for Windows](https://www.jenkins.io/doc/book/installing/windows/).
2. For **Step 3**, select the option: **Run Jenkins as a local system service**.

---

### Configuring Jenkins on Windows
1. **Unblocking Jenkins**:
   - Access Jenkins at: `http://localhost:8080`
   - Retrieve the **initial Administrator password** located in the Jenkins installation directory.
     - For the default location (`C:\Program Files\Jenkins`), the password is stored in the file `C:\Program Files\Jenkins\secrets\initialAdminPassword`.

   - [Unlock Jenkins Documentation](https://www.jenkins.io/doc/book/installing/windows/#unlocking-jenkins)

2. **Customizing Jenkins with Plugins**:
   - Choose one of the following:
     - **Install Suggested Plugins**: Installs the most commonly used plugins based on typical use cases.
     - **Select Plugins to Install**: Allows manual selection of plugins to install.

3. **Create the First Administrator User**:
   - Fill out the required fields for the administrator user and click **Save and Finish**.

---

### How to Start or Stop Jenkins on Windows
1. Open **Services** and search for **Jenkins**.
2. Select **Jenkins** from the list and choose to **Start** or **Stop** the server.

---

### Restarting Jenkins via URL
- **Force Restart (not recommended during active builds)**:
  - Use: `http://[Jenkins URL]/restart`
  
- **Safe Restart (recommended)**:
  - Allows running jobs to complete before restarting: `http://[Jenkins URL]/safeRestart`

---

### Jenkins Restart Banner
- The **Jenkins Restart Banner** informs users that Jenkins will restart, typically due to system changes, plugin updates, or upgrades. This banner helps users be aware of ongoing system changes and prevents disruption to ongoing tasks.