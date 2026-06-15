# 🔐 Secure Static Website Hosting on AWS S3

A cloud security project demonstrating secure static website hosting on Amazon S3 with IAM least-privilege access, HTTPS enforcement, server access logging, versioning, and lifecycle management.

---

## 🏗️ Architecture

```
User Request
     │
     ▼
Amazon S3 (Static Website)
     │
     ├── Bucket Policy (HTTPS Enforcement + Public Read)
     ├── S3 Versioning (Data Protection)
     ├── IAM Role - EC2 (Least Privilege Access)
     │
     ▼
S3 Logs Bucket (Audit Trail)
     │
     └── Lifecycle Rule (Auto-delete after 30 days)
```

---

## ☁️ AWS Services Used

| Service | Purpose |
|---|---|
| Amazon S3 | Static website hosting |
| AWS IAM | Least-privilege role and policy |
| S3 Server Access Logging | Audit trail of all bucket access |
| S3 Versioning | Data protection and recovery |
| S3 Lifecycle Rules | Automated log retention management |

---

## 🔐 Security Features Implemented

### 1. HTTPS Enforcement via Bucket Policy
All non-HTTPS (HTTP) requests are explicitly denied using a bucket policy condition:
```json
"Condition": {
  "Bool": {
    "aws:SecureTransport": "false"
  }
}
```

### 2. IAM Least Privilege Policy
Created a custom IAM policy (`S3WebsiteLeastPrivilegePolicy`) that allows only the minimum required actions:
- `s3:GetObject`
- `s3:PutObject`
- `s3:ListBucket`

### 3. Server Access Logging
All access requests to the main bucket are logged to a separate dedicated S3 logging bucket (`s3-new-harsha-logs`) for audit and monitoring purposes.

### 4. S3 Versioning
Versioning is enabled on the main bucket to protect against accidental deletions and allow rollback to previous file versions.

### 5. Lifecycle Rules
A lifecycle rule automatically expires log files after **30 days** to optimize storage costs.

---

## 📁 Project Structure

```
├── index.html              # Main website homepage
├── error.html              # Custom 404 error page
├── bucket-policy.json      # S3 bucket policy (public read + HTTPS enforcement)
├── iam-policy.json         # IAM least-privilege policy
└── README.md               # Project documentation
```

---

## 🚀 Implementation Steps

1. Created S3 bucket with versioning enabled
2. Uploaded static website files (`index.html`, `error.html`)
3. Enabled static website hosting
4. Created a separate S3 logging bucket
5. Enabled server access logging pointing to logging bucket
6. Applied bucket policy for public read and HTTPS enforcement
7. Created IAM policy with least-privilege permissions
8. Created IAM role for EC2 with the least-privilege policy attached
9. Added lifecycle rule to auto-delete logs after 30 days
10. Tested versioning, logging, and website access

---

## 📋 Bucket Policy

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadForWebsite",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::s3-new-harsha/*"
    },
    {
      "Sid": "DenyNonHTTPS",
      "Effect": "Deny",
      "Principal": "*",
      "Action": "s3:*",
      "Resource": [
        "arn:aws:s3:::s3-new-harsha",
        "arn:aws:s3:::s3-new-harsha/*"
      ],
      "Condition": {
        "Bool": {
          "aws:SecureTransport": "false"
        }
      }
    }
  ]
}
```

---

## 📋 IAM Least Privilege Policy

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:PutObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::s3-new-harsha",
        "arn:aws:s3:::s3-new-harsha/*"
      ]
    }
  ]
}
```

---

## ✅ Testing Results

| Test | Result |
|---|---|
| Static website loads successfully | ✅ Pass |
| HTTP access blocked by bucket policy | ✅ Pass |
| S3 versioning shows multiple versions | ✅ Pass |
| Server access logs delivered to logs bucket | ✅ Pass |
| IAM role attached with least-privilege policy | ✅ Pass |
| Lifecycle rule created for log expiration | ✅ Pass |

---

## 💡 Key Learnings

- S3 bucket policies can enforce security controls like HTTPS-only access
- IAM least-privilege principle minimizes the attack surface
- Server access logging provides an audit trail for compliance and monitoring
- S3 versioning protects against accidental data loss
- Lifecycle rules help manage storage costs automatically
- In production, CloudFront would be added in front of S3 to serve content over HTTPS with a custom domain

---

## 👨‍💻 Author

**Vijaya Sri Harsha Anala**  
ECE Graduate | AWS Cloud Practitioner Journey  
Focused on Cloud Security & Cloud Operations

---

## 📌 Part of AWS Learning Path

`IAM` → `EC2` → `EBS` → `ELB` → `ASG` → **`S3 (This Project)`** → `RDS` → `CloudFront` → `Route 53`
