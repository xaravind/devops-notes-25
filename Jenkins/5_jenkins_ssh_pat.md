
# **Cloning a Private Git Repository in Jenkins with SSH Keys or PAT**

To securely clone a private Git repository in Jenkins, you can use **SSH keys** or a **Personal Access Token (PAT)** for authentication. Below are the steps for both methods, along with a sample pipeline configuration.

---

## **Method 1: Clone Git Repository Using SSH Keys**

### **Steps to Setup SSH Keys for Jenkins and Git (GitHub, GitLab, etc.)**

---

### **1. Generate or Use Existing SSH Key Pair**

If you haven't already created an SSH key pair for Jenkins, you'll need to generate one:

- On your Jenkins server or machine, run the following command to create an SSH key pair:

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

- When prompted for a file to save the key, press **Enter** to use the default path (`~/.ssh/id_rsa`).
- You can add a passphrase for added security, or leave it empty.
- This will generate two files:
  - **Private Key**: `~/.ssh/id_rsa` (keep this private and secure)
  - **Public Key**: `~/.ssh/id_rsa.pub` (this will be added to your Git service)

---

### **2. Store SSH Public Key in Git **

#### **GitHub**:
1. **Log in to GitHub** and go to your **profile settings**.
2. Navigate to **SSH and GPG keys**:
   - Go to **Settings** → **SSH and GPG Keys**.
   - Click on **New SSH Key**.
3. **Paste your public key**:
   - Open the `id_rsa.pub` file (public key) on your Jenkins server using a text editor:
     ```bash
     cat ~/.ssh/id_rsa.pub
     ```
   - Copy the content of the file and paste it into the **Key** field on GitHub.
   - Add a descriptive title (e.g., "Jenkins SSH Key").
4. Click **Add SSH Key**.


---

### **3. Store SSH Private Key in Jenkins**

Now that you have the **SSH public key** added to your Git service (GitHub, GitLab, etc.), you need to store the **private key** securely in Jenkins.

1. **Go to Jenkins Dashboard** → **Manage Jenkins** → **Manage Credentials**.
2. Select the appropriate **domain** (or **Global** if not using domains).
3. Click on **Add Credentials**.
   - **Kind**: Select **SSH Username with private key**.
   - **Username**: Enter the Git username for your Git service. For example, `git` for GitHub or GitLab.
   - **Private Key**: Choose **Enter directly** and paste the contents of your private key (`id_rsa`) here.
   - **ID**: Assign a unique ID to the credentials (e.g., `my-ssh-key`).
4. Click **OK** to save the credentials.

### **4. Use SSH Key in Jenkins Pipeline**
   - In your pipeline, use the `sshagent` block to authenticate with the SSH key.

### **Sample Jenkins Pipeline for SSH Authentication:**

```groovy
pipeline {
    agent any

    environment {
        // Define the SSH credentials ID and Git repository URL
        SSH_CREDENTIALS_ID = 'my-ssh-key'
        GIT_REPO_URL = 'git@github.com:yourusername/your-private-repo.git'
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    // Use SSH authentication to clone the private repository
                    sshagent([SSH_CREDENTIALS_ID]) {
                        sh 'git clone ${GIT_REPO_URL}'
                    }
                }
            }
        }
        // Add other stages like build, test, deploy, etc.
    }
}
```

---

## **Method 2: Clone Git Repository Using Personal Access Token (PAT)**

### **Steps to Setup Personal Access Token (PAT):**

1. **Generate a PAT on GitHub (or other Git services):**
   - Go to your **GitHub profile** → **Settings** → **Developer Settings** → **Personal Access Tokens**.
   - Click **Generate new token** and provide the necessary **permissions** (e.g., `repo` for full control over private repositories).
   - **Save the token** somewhere safe, as you won't be able to view it again once generated.

2. **Store PAT in Jenkins:**
   - Go to **Jenkins Dashboard** → **Manage Jenkins** → **Manage Credentials**.
   - Choose the **Global** or a specific **Domain** for the credentials.
   - Click on **Add Credentials**.
     - **Kind**: Select **Username with password**.
     - **Username**: Enter your GitHub username (or the username of your Git provider).
     - **Password**: Paste your **Personal Access Token**.
     - **ID**: Assign a unique **ID** to the credentials (e.g., `github-pat`).

3. **Use PAT in Jenkins Pipeline:**
   - In your pipeline, reference the PAT credentials to authenticate when cloning the repository.

### **Sample Jenkins Pipeline for PAT Authentication:**

```groovy
pipeline {
    agent any

    environment {
        // Define the credentials ID and Git repository URL
        GIT_CREDENTIALS_ID = 'github-pat'
        GIT_REPO_URL = 'https://github.com/yourusername/your-private-repo.git'
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    // Clone the private repository using PAT for authentication
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: '*/main']],  // Adjust the branch as needed
                        userRemoteConfigs: [[
                            url: "${GIT_REPO_URL}",
                            credentialsId: "${GIT_CREDENTIALS_ID}"
                        ]]
                    ])
                }
            }
        }
        // Additional stages (e.g., build, test, deploy) can follow
    }
}
```

---

## **Key Considerations**

- **SSH Key Authentication** is recommended for security, as it doesn’t expose your credentials in the URL.
- **Personal Access Token (PAT)** is an alternative when SSH keys aren’t feasible. PATs can be more flexible for automated systems and can be scoped to specific permissions.
- Always **secure your credentials** by storing them in Jenkins' **Credentials** store, and **never hard-code them** in pipeline scripts.

---

## **Troubleshooting**

- **"Permission denied" error**: If you encounter an error such as `Permission denied (publickey)`, ensure that:
  - The **public key** has been added to your Git service (e.g., GitHub) under **SSH Keys** in your account settings.
  - Jenkins is using the correct **SSH key** for authentication.

- **"Authentication failed" with PAT**: If using a PAT and the clone fails:
  - Ensure that the **PAT** has the necessary permissions (e.g., `repo`).
  - Verify the **credentials** are correctly configured in Jenkins.
  - Check if the URL format for the repository is correct (use HTTPS for PAT).

---

### Summary:

- **SSH Keys** provide more secure authentication for cloning repositories.
- **PAT** is another method, often simpler for setups where SSH is difficult to configure.
- Always store sensitive credentials (like SSH keys or PATs) in Jenkins' **Credentials Manager** and reference them securely in your pipeline code.

Certainly! Below is an improved structure of your notes, formatted with headlines, bullet points, and slight improvements for clarity without removing any existing content:

---

# Jenkins Shared Libraries

### **What are Shared Libraries?**
- A **library** is a collection of reusable code, such as functions and classes, that can be utilized across multiple Jenkins pipelines.
- The goal is to **avoid repetition** and improve maintainability by centralizing common functionality into libraries.
- Shared libraries in jenkins is a collection of grrovy scripts shared between different jenkins jobs.
- Shared libraries are stored in git repositories.
- Creating a Shared libraries simplifies the process of pushing source code updates for a project.
- Updating the library source code also updates the code of every project, that uses library.

### **Types of Jenkins Shared Libraries**
Jenkins supports two types of shared libraries:

1. **Global Trusted Pipeline Libraries**  
   - These libraries are available to any pipeline job running on this system.
   - **Trusted**: They run without sandbox restrictions, allowing the use of advanced features like `@Grab`.

2. **Global Untrusted Pipeline Libraries**  
   - These libraries are also available to any pipeline job running on this system.
   - **Untrusted**: They run with sandbox restrictions, meaning they cannot use certain features like `@Grab`.

---

### **Configuring Shared Libraries in Jenkins**

To set up shared libraries, follow these steps:

1. **Go to Jenkins Dashboard**  
   - Navigate to **Manage Jenkins** → **Configure System**.

2. **Add Shared Library**  
   - Under the **Global Pipeline Libraries** section, add a new library.
   - Provide the **Git repository URL** of the shared library. For example:  
     `https://github.com/xaravind/shared_lib.git`
   - **Credentials**: If it's a private repository, make sure to provide the necessary credentials.

---

### **Shared Library Folder Structure Example**

Here’s an example of the directory structure for a shared library that includes reusable variables (`vars`) with `.groovy` files:

```
shared-lib/
├── vars/
│   ├── checkProc.groovy
│   └── helloWorld.groovy
```

- **`vars/`**: This folder contains simple Groovy scripts that define reusable functions or variables.
- **`checkProc.groovy`** and **`helloWorld.groovy`** are two examples of reusable scripts.

---

### **Example Code Using Shared Libraries**

Here is a sample Jenkinsfile that demonstrates how to use a shared library:

```groovy
@Library('my-shared-library') _  // Load the shared library

pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    // Call the helloWorld function from shared library
                    helloWorld()
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Call the checkProc function from shared library
                    checkProc()
                }
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
```

### **Explanation of the Code:**

- **@Library('my-shared-library') _**:  
   This imports the shared library named `'my-shared-library'` into the pipeline. It makes the functions inside `vars/` accessible.
   
- **`helloWorld()`**:  
   A function from the `helloWorld.groovy` file inside the `vars/` directory is called in the given stages.
   
- **`checkProc()`**:  
   A function from the `checkProc.groovy` file inside the `vars/` directory is called in the given stages.

---

### **Important Notes**
- When configuring shared libraries, **ensure the repository is accessible** from Jenkins, and make sure credentials are provided if it's a private repo.
- **Global Trusted Pipeline Libraries** run without sandbox restrictions, while **Global Untrusted Pipeline Libraries** are sandboxed.
  


