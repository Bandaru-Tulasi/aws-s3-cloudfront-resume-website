# 🚀 AWS Static Resume Website — S3 + CloudFront + OAC (No Custom Domain)

This project demonstrates how to deploy a secure, scalable **static resume website** using Amazon **S3**, **CloudFront CDN**, and **Origin Access Control (OAC)** — without purchasing a domain or using ACM certificates.

This setup uses the default **CloudFront-provided HTTPS URL**, which is completely free and production-ready.

---
## 🎯 Why This Project Matters

This project demonstrates real-world AWS architecture skills, including:

- Designing **secure S3 access using OAC**
- Implementing **CDN-based HTTPS delivery** without extra cost
- Applying **least-privilege IAM policies**
- Understanding **modern AWS best practices** (OAC over OAI)

This is a production-grade static hosting solution commonly used for resumes, documentation sites, and landing pages.

## 📌 Architecture Overview

```text
+-----------------------+
User (Browser) ---> | Amazon CloudFront |
+----------+------------+
|
v
+------------------------+
| S3 Private Bucket |
| (Static Resume Files) |
+------------------------+
^
|
+------------------+
| Origin Access |
| Control (OAC) |
+------------------+
```

---

## 🧰 Technologies Used

- **Amazon S3** (private bucket storing website files)
- **CloudFront CDN** (fast global delivery)
- **Origin Access Control (OAC)** (secure CloudFront → S3 access)
- **AWS IAM** (permissions + bucket policies)

---

# 🛠️ Project Setup — Step-by-Step

## 1️⃣ Create S3 Bucket

- Bucket name: `tulsi-resume-site`
- Region: `us-east-1`
- **Block Public Access = ON**
- Upload `index.html` (your resume website)
- Disable Static Website Hosting (required for OAC)

---

## 2️⃣ Create CloudFront Distribution

Configure CloudFront:

### ✔ Origin Domain:
tulsi-resume-site.s3.us-east-1.amazonaws.com

### ✔ Origin Access:
Enable **Origin Access Control (OAC)**

### ✔ Viewer Protocol Policy:
Redirect HTTP to HTTPS

### ✔ Default Root Object:
index.html

### ✔ Price Class:
North America & Europe (optional)

---

## 3️⃣ Attach OAC and Update S3 Bucket Policy

Use this policy (replace `<ACCOUNT-ID>` and `<DISTRIBUTION-ID>`):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": { "Service": "cloudfront.amazonaws.com" },
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::tulsi-resume-site/*",
      "Condition": {
        "StringEquals": {
          "AWS:SourceArn": "arn:aws:cloudfront::<ACCOUNT-ID>:distribution/<DISTRIBUTION-ID>"
        }
      }
    }
  ]
}

This ensures only CloudFront can access your S3 bucket.

4️⃣ Invalidate CloudFront Cache
Whenever you update your website:
`/*`
This refreshes all cached content globally.

🌍 Live Website URL
Your resume is publicly available at:

https://d1oa4kn4bwt9vg.cloudfront.net/
This is the default CloudFront domain and includes FREE HTTPS automatically — no ACM certificate needed.

## 📄 Files in This Project

```
index.html   # Resume webpage
README.md    # Project documentation
```
⭐ Future Enhancements (Optional)

You can extend this project by adding:

Custom domain (Route 53)

ACM SSL certificate

Contact form using API Gateway + DynamoDB + Lambda

CI/CD deployment pipeline using GitHub Actions or CodePipeline
