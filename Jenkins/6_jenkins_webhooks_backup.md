# Using GitHub Webhook Actions in Jenkins

## Prerequisites
- Install the **Git Plugin** in Jenkins.

---

## Configure Jenkins Job for Webhook

### **For Freestyle Project**
1. **Create a New Job:**
   - Go to **Jenkins Dashboard** → **New Item** → Select **Freestyle Project**.

2. **Configure Source Code Management:**
   - Under **Source Code Management**, select **Git** and enter the repository URL.

3. **Enable Webhook Trigger:**
   - Under **Build Triggers**, check **"GitHub hook trigger for GITScm polling"**.
   - This ensures that when Jenkins receives a GitHub push hook, the GitHub Plugin verifies if the hook matches the defined Git repository in the **SCM/Git** section of the job.

---

### **For Pipeline Project**
1. **Create a New Job:**
   - Go to **Jenkins Dashboard** → **New Item** → Select **Pipeline Project**.

2. **Configure Source Code Management:**
   - Under **Pipeline** → Select **Pipeline script from SCM**.
   - Select **SCM as Git**.
   - Enter the repository URL.
   - Ensure the `Jenkinsfile` is stored in the repository.

3. **Sample Jenkinsfile Pipeline:**
   ```groovy
   pipeline {
       agent any

       stages {
           stage('Checkout') {
               steps {
                   git branch: 'main', url: 'https://github.com/your-repo.git'
               }
           }
           stage('Build') {
               steps {
                   echo 'Building the project...'
               }
           }
       }
   }
   ```

---

## **Configure Webhook in GitHub**
1. **Navigate to GitHub Webhook Settings:**
   - Go to your **GitHub repository** → **Settings** → **Webhooks**.

2. **Add a New Webhook:**
   - Click **Add Webhook**.
   - Set the **Payload URL** to:
     ```
     http://your-jenkins-url/github-webhook/
     ```
     **Example:**
     ```
     http://jenkins.example.com/github-webhook/
     ```
   - Set **Content Type** to `application/json`.

3. **Select Trigger Events:**
   - Recommended: **Just the push event**.
   - Alternatively, select **individual events** as needed.

4. **Save the Webhook:**
   - Click **Add Webhook**.

---

## **Test the Webhook**
1. Push a commit to the repository.
2. Check **Jenkins Console Output** to verify that Jenkins was triggered successfully.

---

