## 1. **Pipeline Projects Overview**

### 1.1 **Why Use Pipeline Projects?**
Pipeline projects are ideal for automating and securing CI/CD pipeline code. They allow you to store pipeline definitions in a version control system (SCM) and provide better scalability and flexibility for complex workflows.

---

### 1.2 **Creating a Simple "Hello World" Pipeline Job**

1. **Create a New Pipeline Job**:
   - Click on **New Item**.
   - Select **Pipeline** and click **OK**.

2. **Configure the Pipeline**:
   - In the job configuration page, scroll to **Pipeline**.
   - You can write the pipeline script directly in the **Script** text area or pull it from SCM.
   
3. **Example: "Hello World" Pipeline Script**:
   You can write a simple "Hello World" pipeline script like this:

   ```groovy
   pipeline {
       agent any

       stages {
           stage('Hello') {
               steps {
                   echo 'Hello World'
               }
           }
       }
   }
   ```

---

## 2. **Rebuild vs. Replay**

### 2.1 **Rebuild**
- **Rebuild** is used when you want to trigger the same build **without changing the pipeline script** or parameters.
- It simply re-runs the job as it was executed before.

### 2.2 **Replay**
- **Replay** allows you to modify the pipeline code and then **execute the job with those changes**.
- Useful for **troubleshooting**, as you can experiment with different changes without modifying the original code.

---

## 3. **Using the Snippet Generator in Jenkins Pipeline**

### 3.1 **Overview**
If you’re unfamiliar with how to write pipeline code, the **Snippet Generator** is a useful tool that helps you build pipeline script snippets.

- **Access Snippet Generator**:
  - In your pipeline job configuration, click on **Pipeline Syntax** in the left menu.
  - Choose **Snippet Generator**.

- **How It Works**:
  - Select a step from the list, configure it, and click **Generate Pipeline Script**.
  - The tool generates the necessary pipeline code for the selected step, which you can then copy and paste into your pipeline script.

#### Example: Using `echo` to Print a Message
1. **Select Echo Step**:
   - In the Snippet Generator, go to **Steps** and select **echo: print message**.
   - Enter the message you want to print.
   
2. **Generate the Script**:
   - Click **Generate**.
   - The generated script will look like this:

     ```groovy
     echo 'Your message here'
     ```

- **Copy and Paste the Generated Script** into your pipeline script.

---

## 4 **Pipeline Execution Mode**

By default, Jenkins pipeline execution occurs **serially**:
- Each stage will run one after another.
- A stage only runs if the previous stage was successful.

If you want to run stages in parallel or control the execution flow differently, you can modify the pipeline script accordingly.

---

## **Using Environment Variables in Jenkins Pipeline**

### **Overview:**
Jenkins Pipeline exposes environment variables through the global `env` variable. These variables are accessible from anywhere within a Jenkinsfile, making it easy to access important information during the build process.

- **Link to Documentation:** [Jenkins Pipeline - Using Environment Variables](https://www.jenkins.io/doc/book/pipeline/jenkinsfile/#using-environment-variables)

### **Examples of Default Environment Variables:**
- **`BUILD_ID`**: A unique identifier for the current build.
- **`BUILD_NUMBER`**: The number of the current build in the job’s history.
- **`BUILD_URL`**: The URL of the current build.
- **`JENKINS_URL`**: The base URL of the Jenkins instance.
- **`JOB_NAME`**: The name of the Jenkins job.
- **`WORKSPACE`**: The directory where Jenkins stores the files for the current job.

### **Sample Script for Using Environment Variables:**

#### **Jenkinsfile (Declarative Pipeline)**

```groovy
pipeline {
    agent any
    stages {
        stage('Example') {
            steps {
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
            }
        }
    }
}
```

- This simple script echoes the `BUILD_ID` and `JENKINS_URL` environment variables during the pipeline execution.

---

## **How to Parameterize a Jenkins Job**

### **What are Parameters?**
Parameters allow you to prompt users for input when triggering a job. These inputs are passed into the build and can be used to customize the behavior of the job based on the user’s selection.

### **Steps to Use Parameters in Jenkins Jobs:**

1. **Create a New Job**:
   - Start by creating a new Jenkins job (Freestyle or Pipeline).
   
2. **Enable Parameterization**:
   - Go to the **General** section of the job configuration.
   - Check the box labeled **"This project is parameterized"**.

3. **Add Parameters**:
   - Click **"Add Parameter"** to select from a variety of parameter types. Below are the available types:
     - **Boolean Parameter**: A true/false flag.
     - **Choice Parameter**: A dropdown with predefined options.
     - **Credentials Parameter**: Select credentials stored in Jenkins.
     - **File Parameter**: Allow the user to upload a file.
     - **Multi-line String Parameter**: For accepting multi-line input.
     - **Password Parameter**: For secure password input.
     - **Run Parameter**: For selecting a build to run.
     - **String Parameter**: For simple text input.

4. **Example: Using a Choice Parameter**:
   - **Name**: `ENV`
   - **Choices**: Define multiple environments (e.g., `dev`, `qa`, `prod`).
   - **Description**: "Choose the environment."

   This will allow users to choose one of the environments during the build.

5. **Triggering the Job with Parameters**:
   - When the job is triggered, Jenkins will prompt for the parameter(s).
   - You will see the **Build Parameter** option, where you can select the predefined choices (like `dev`, `qa`, or `prod`) and then start the job.

---


