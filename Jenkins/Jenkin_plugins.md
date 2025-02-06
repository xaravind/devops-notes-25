Jenkins Guide
Jenkins Plugins
Jenkins plugins are extensions that enhance Jenkins' functionality.

Plugin Installation (GUI)
There are two ways to install plugins:

1. Automatic Installation (.jpi - Jenkins Plugins)
Login to Jenkins GUI.
Go to Dashboard → Manage Jenkins → Plugins.
Select Available Plugins.
Search for the required plugin.
Select the checkbox → Click Install.
Tick the checkbox to Restart Jenkins.
2. Manual Installation (.hpi - Hudson Plugins)
Download plugins from:
Official Jenkins Plugins
Jenkins GUI → Available Plugins
Choose the required version and download.
Upload the .hpi file manually in Jenkins:
Dashboard → Manage Jenkins → Plugins → Advanced Settings → Upload Plugin → Deploy.
Check the Deployment Progress and restart if needed.
Uninstall Plugins
Dashboard → Manage Jenkins → Plugins.
Select the (X) option next to the plugin.
Click YES to confirm.
Update Plugins
Dashboard → Manage Jenkins → Plugins → Updates.
Tick the checkboxes for plugins to update.
Click Update (next to the search box).
Restart Jenkins after updating.
⚠️ Note: In real-time environments, avoid direct updates. First, test updates in a local setup before deploying them to production.

User Management in Jenkins
Create Users
Dashboard → Manage Jenkins → Users.
Click + Create User.
Enter Name, Password, Email.
Click Create User.
Assign Permissions
Dashboard → Manage Jenkins → Security.
Define access control in the Authorization section.
Choose Matrix-based Security.
Click Add User → Enter User ID.
Assign necessary permissions (minimum: Overall Read).
User Storage Options:

Jenkins’ Own User Database – Suitable for small teams.
LDAP (Lightweight Directory Access Protocol) – For enterprise-level authentication.
Authorization Strategies
Project-based Matrix Authorization Strategy
Assign role-based access control (RBAC) based on credentials, overall access, agent, etc.
Matrix Authorization Strategy
Configure fine-grained permissions, like starting builds, configuring items, and deleting jobs.
Jenkins Jobs/Projects
A Job/Project in Jenkins consists of tasks like building, testing, and deploying code.

Types of Jenkins Projects
Maven Project – Automates builds using POM files.
Pipeline – Manages workflows across multiple build agents.
Multi-configuration Project – Supports multiple environments/configurations.
Folder – Groups items under separate namespaces.
Multibranch Pipeline – Creates pipelines for each SCM branch.
Organization Folder – Scans repositories to create multibranch subfolders.
Creating a Simple Job/Project
Steps:
Dashboard → New Item → Enter Job Name.
Select an item type (e.g., Freestyle Project) → Click OK.
Configure the job:
General – Add project description.
Source Code Management – Add repository URLs.
Build Triggers – Define automated build conditions.
Build Environment – Set up pre-build configurations.
Build Steps – Define tasks like compiling and testing.
Post-Build Actions – Add actions like artifact archiving or notifications.
Click Save.
Creating a Freestyle Job/Project
Dashboard → New Item → Enter Job Name.
Select Freestyle Project → Click OK.
In the configuration, go to Build Steps → Click Add Build Step.
Select Execute Windows Batch Command.
Type dir → Click Save.
Go to Dashboard, select the job.
Click Build Now.
Check job status and output.