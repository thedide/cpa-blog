---
title: "Deploy Streamlit App to Cloud"
date: 2025-03-15T23:21:39-07:00

categories:
  - Technology
tags:
  - Streamlit app
  - AWS
  - Azure
  - GCP
  - Google cloud
---

# Deploying a Streamlit App on AWS, GCP, and Azure

Streamlit makes it easy to build interactive web applications in Python, but deploying them to a cloud platform like **AWS, GCP, or Azure** requires some setup. In this guide, we'll cover how to deploy your Streamlit app on each of these cloud providers and discuss best practices for performance, security, and cost optimization.

---

## 1. Deploying on AWS (Amazon Web Services)

AWS provides multiple options for deploying a Streamlit app, including **EC2**, **Elastic Beanstalk**, and **AWS Lambda with API Gateway**. Here, we'll focus on deploying with **EC2**, a flexible and widely used option.

### Using **EC2**

1. **Create an EC2 Instance:**
   - Go to the **AWS EC2 Console** → Launch an instance.
   - Select **Ubuntu 22.04 LTS** (or any preferred OS).
   - Choose an instance type (e.g., `t2.micro` for free tier, or `t3.medium` for better performance).
   - Configure **security group** to allow inbound traffic on **port 8501** (default for Streamlit).
   - Launch the instance and connect via SSH.

2. **Install dependencies on EC2:**
   ```bash
   sudo apt update && sudo apt install -y python3-pip
   pip3 install streamlit
   ```

3. **Upload your Streamlit app (via SCP or Git):**
   ```bash
   scp -i your-key.pem app.py ubuntu@your-ec2-ip:/home/ubuntu/
   ```
   Or, clone from GitHub:
   ```bash
   git clone https://github.com/your-repo.git
   ```

4. **Run the Streamlit app:**
   ```bash
   streamlit run app.py --server.port 8501 --server.address 0.0.0.0
   ```

5. **Access your app** using `http://your-ec2-ip:8501/`.

To keep it running persistently, use:
   ```bash
   nohup streamlit run app.py &
   ```

### Best Practices for AWS Deployment

- Use **Elastic IP** to ensure a fixed public IP for your instance.
- Set up **Auto Scaling** if you expect traffic spikes.
- Use **AWS RDS** instead of local databases for better reliability.
- Consider deploying with **AWS Elastic Beanstalk** for automated scaling and monitoring.

---

<script async src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"></script>
<!-- cpa -->
<ins class="adsbygoogle"
     style="display:block"
     data-ad-client="ca-pub-2843564932689995"
     data-ad-slot="3526097725"
     data-ad-format="auto"
     data-full-width-responsive="true"></ins>
<script>
     (adsbygoogle = window.adsbygoogle || []).push({});
</script>

## 2. Deploying on GCP (Google Cloud Platform)

Google Cloud Platform provides multiple ways to deploy a Streamlit app, including **Compute Engine**, **Cloud Run**, and **App Engine**. We'll cover **Cloud Run**, which is a fully managed, serverless platform.

### Using **Cloud Run** (Serverless)

1. **Install Google Cloud SDK & authenticate:**
   ```bash
   gcloud auth login
   gcloud config set project YOUR_PROJECT_ID
   ```

2. **Create a `Dockerfile` for your app:**
   ```dockerfile
   FROM python:3.9
   WORKDIR /app
   COPY requirements.txt ./
   RUN pip install -r requirements.txt
   COPY . .
   CMD streamlit run app.py --server.port $PORT --server.address 0.0.0.0
   ```

3. **Build and push the Docker image:**
   ```bash
   gcloud builds submit --tag gcr.io/YOUR_PROJECT_ID/streamlit-app
   ```

4. **Deploy to Cloud Run:**
   ```bash
   gcloud run deploy streamlit-app --image gcr.io/YOUR_PROJECT_ID/streamlit-app --platform managed --allow-unauthenticated --region us-central1
   ```

5. **Get the URL** and access your app!
   ```bash
   gcloud run services describe streamlit-app --region us-central1 --format 'value(status.url)'
   ```

### Best Practices for GCP Deployment

- Use **Google Cloud Storage** to store static files efficiently.
- Optimize costs by setting up **scaling limits** in Cloud Run.
- Use **Cloud SQL** for database management instead of local storage.
- Consider **Google App Engine** for simpler deployment with auto-scaling.

---

## 3. Deploying on Azure

Azure provides multiple deployment options such as **Virtual Machines (VMs)**, **Azure App Service**, and **Azure Functions**. Here, we'll focus on deploying with **Azure App Service**.

### Using **Azure App Service**

1. **Install Azure CLI and login:**
   ```bash
   az login
   az group create --name streamlit-rg --location eastus
   ```

2. **Create an Azure Web App:**
   ```bash
   az webapp up --name streamlit-app --resource-group streamlit-rg --runtime "PYTHON:3.9"
   ```

3. **Deploy your code using GitHub Actions or ZIP deployment:**
   ```bash
   az webapp deployment source config-zip --resource-group streamlit-rg --name streamlit-app --src app.zip
   ```

4. **Set startup command for Streamlit:**
   ```bash
   az webapp config set --resource-group streamlit-rg --name streamlit-app --startup-file "streamlit run app.py --server.port 8000"
   ```

5. **Access your app** at `https://streamlit-app.azurewebsites.net`.

### Best Practices for Azure Deployment

- Use **Azure Blob Storage** to handle static files.
- Set up **Auto Scaling** to optimize resource utilization.
- Consider using **Azure Kubernetes Service (AKS)** for better scalability.
- Monitor your app using **Azure Monitor** for performance tracking.

---

## Wrap up

Each cloud provider offers different ways to deploy a **Streamlit app**:
- **AWS EC2** provides full control over the instance.
- **GCP Cloud Run** offers a serverless, scalable deployment.
- **Azure App Service** simplifies deployment with managed hosting.

### Choosing the Right Cloud Provider
| Feature          | AWS EC2            | GCP Cloud Run       | Azure App Service    |
|-----------------|--------------------|---------------------|----------------------|
| **Ease of Use** | Medium             | Easy (Serverless)   | Easy                 |
| **Scalability** | High (Manual Setup) | High (Auto-Scales) | High (Auto-Scales)   |
| **Cost**       | Pay for Instance    | Pay per Request     | Pay per Usage        |
| **Best For**   | Full Control        | Serverless Apps     | Managed Hosting      |

