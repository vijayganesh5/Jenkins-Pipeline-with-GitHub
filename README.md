# 🚀 Jenkins GitHub Webhook Integration – Auto Build & Email Notification

## 🧠 **Objective**

The goal of this task is to:

1. Create a **simple script file** and push it to your **GitHub repository**.
2. Connect your **GitHub repo** to a **Jenkins project**.
3. Whenever you make a new commit to GitHub, Jenkins should **automatically trigger a build** (using a webhook).
4. After the build completes, you’ll receive the **build result via email** 📧.

---

## ⚙️ **Tech Stack Used**

* ☁️ **AWS EC2 (Ubuntu)** – For hosting Jenkins
* 🧰 **GitHub** – For storing your project/script
* ⚙️ **Jenkins** – For automation, webhook, and email notifications

---

## 🧩 **Prerequisites**

Before starting, make sure you have:
✅ Jenkins installed and running on your AWS EC2 Ubuntu instance
✅ Port **8080** open in the EC2 Security Group
✅ A **GitHub account and repository**
✅ **Java** installed on EC2
✅ Internet connectivity for EC2 (for Jenkins to connect with GitHub & email server)

---

## 🪜 **Steps to Perform**

### 1️⃣ **Create a Simple Script File**

Create a basic shell script named `build.sh`:

```bash
#!/bin/bash
echo "✅ Jenkins build triggered successfully!"
date
```

Make it executable:

```bash
chmod +x build.sh
```

---

### 2️⃣ **Push Script to GitHub Repository**

* Create a new repo on GitHub (example: `jenkins-webhook-demo`)
* Initialize and push your script:

  ```bash
  git init
  git add build.sh
  git commit -m "Initial commit"
  git branch -M main
  git remote add origin https://github.com/<your-username>/jenkins-webhook-demo.git
  git push -u origin main
  ```

---

### 3️⃣ **Install Required Jenkins Plugins**

In Jenkins Dashboard:

* Go to **Manage Jenkins → Plugins → Available Plugins**
* Search and install:

  * ✅ Git plugin
  * ✅ GitHub Integration plugin
  * ✅ Email Extension Plugin
* Restart Jenkins after installation.

---

### 4️⃣ **Configure Email Notification in Jenkins**

1. Go to **Manage Jenkins → System**
2. Scroll to **Extended E-mail Notification** section
3. Configure:

   * **SMTP Server:** `smtp.gmail.com`
   * Enable **Use SMTP Authentication**
   * Add your Gmail credentials (or App Password if 2FA is on)
   * Default user e-mail suffix: `@gmail.com`
4. Click **Apply and Save**
5. Test the email configuration ✅

---

### 5️⃣ **Create Jenkins Project and Connect GitHub**

1. On Jenkins Dashboard → click **New Item**
2. Enter name (e.g., *GitHub-Auto-Build*)
3. Choose **Freestyle project** → click OK

Then configure:

* Under **Source Code Management → Git**, enter your repo URL:

  ```
  https://github.com/<your-username>/jenkins-webhook-demo.git
  ```
* Add credentials if needed (GitHub personal access token).

---

### 6️⃣ **Set Up GitHub Webhook for Auto Build**

#### 🧩 In GitHub:

1. Go to your repository → **Settings → Webhooks → Add webhook**
2. Enter Payload URL:

   ```
   http://<your-ec2-public-ip>:8080/github-webhook/
   ```
3. Content type: `application/json`
4. Choose “Just the push event”
5. Click **Add Webhook**

#### ⚙️ In Jenkins:

* In your project, go to **Configure → Build Triggers**
* Check ✅ **GitHub hook trigger for GITScm polling**
* Under **Build Steps**, choose **Execute Shell** and enter:

  ```bash
  bash build.sh
  ```
* Under **Post-build Actions → Editable Email Notification**, configure:

  * Recipient email
  * Subject:

    ```
    Jenkins Build Result: $BUILD_STATUS
    ```
  * Body:

    ```
    Jenkins build for project "$PROJECT_NAME" completed with status: $BUILD_STATUS
    View logs: $BUILD_URL/console
    ```
* Click **Apply and Save**

---

### 7️⃣ **Test the Setup**

1. Make a change to your `build.sh` file, for example:

   ```bash
   echo "Build triggered from GitHub commit!"
   ```
2. Commit and push the change:

   ```bash
   git add .
   git commit -m "Testing webhook trigger"
   git push
   ```
3. Jenkins should **automatically start a new build** 🎯
4. Once the build completes, check your email — you should receive a **build result message** 📬

---

## 🧹 **Cleanup (Optional)**

To clean up Jenkins and free EC2 resources:

```bash
sudo systemctl stop jenkins
sudo apt remove --purge jenkins -y
sudo apt autoremove -y
```

Or terminate your EC2 instance from AWS Console.

---

## 🧾 **Summary**

✅ Created a simple script and pushed to GitHub
✅ Connected GitHub repo to Jenkins
✅ Configured Webhook for auto build trigger
✅ Set up Jenkins email notification
✅ Successfully received build result via email 🎉

---

## 🎯 **End Result**

You now have a **fully automated Jenkins CI setup** — every GitHub commit triggers a new Jenkins build, and you’re instantly notified by email! 🚀

---

## 🎉 Output Link with Screenshots:
- https://docs.google.com/document/d/15s654e3M8yJNFQtFWjxeX_jiNAjRjwkk35YI2BRPx9w/edit?usp=sharing

---
