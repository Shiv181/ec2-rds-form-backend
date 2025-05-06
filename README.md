# ec2-rds-form-backend
**Form Backend on EC2 & RDS Documentation**

---

## 1. Architecture Diagram

This solution uses a secure VPC with public and private subnets:

!\[VPC Architecture Diagram]\(Screenshot 2025-04-28 at 1.51.12 AM.png)

* **Public Subnet** hosts the EC2 instance running the Flask API (port 5000).
* **Private Subnet** hosts the RDS MySQL database, accessible **only** from the EC2.

---

## 2. Key Features

* **VPC Isolation**: EC2 in public subnet; RDS in private subnet.
* **Flask API**: Receives JSON POST at `/post-form` and writes to MySQL.
* **Environment Config**: `.env` file for credentials and endpoints.
* **Security Groups**: Tight inbound rules for both EC2 and RDS.

---

## 3. Deployment Steps

1. **VPC Setup**: Create a VPC (`10.0.0.0/16`) with two public and one private subnet.
2. **EC2 Provisioning**: Launch Ubuntu EC2 in public subnet; open port 5000 & SSH.
3. **RDS Creation**: Launch MySQL in private subnet (no public access).
4. **Flask App**: Clone, install dependencies, configure `.env`, and run on EC2.
5. **Validation**: `curl` a test POST; verify insertion in RDS table.

---

## 4. Screenshots & Outputs

### 4.1 Project Card Preview

A quick look at the project in the portfolio page:

!\[Form Backend on EC2 & RDS]\(Screenshot 2025-05-05 at 11.03.54 PM.png)

### 4.2 Sample `curl` Test

```bash
curl -X POST http://YOUR_EC2_DNS:5000/post-form \
  -H "Content-Type: application/json" \
  -d '{"name":"Test User","email":"test@example.com","subject":"Hello","message":"Testing form backend."}'
```

Expected response:

```json
{ "status": "success", "message": "Form submitted." }
```

---

## 5. Database Table Structure

```sql
CREATE TABLE IF NOT EXISTS form_submissions (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(255),
  subject VARCHAR(255),
  email VARCHAR(255),
  message TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## 6. Security Considerations

* **Lock down** EC2 SG: allow only trusted IPs on SSH and port 5000.
* **No public RDS**: keep MySQL endpoint private.
* **Use IAM roles** for production; avoid storing creds in `.env`.

---

*Document generated for the EC2 & RDS Form Backend project.*
