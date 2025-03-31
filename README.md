# 🚀 Employee Management System Deployment on AWS 🌍

Welcome to the **Employee Management System** project! This guide will walk you through deploying a **Spring Boot** backend, **ReactJS** frontend, and **MySQL** database on **AWS**, using **EC2**, **S3**, and **RDS** for a scalable and reliable solution. 🌟

---

## 📂 Project Structure

- **Backend**: `employeemanagmentbackend/` - Spring Boot application managing employee data.
- **Frontend**: `employeemanagement-frontend/` - ReactJS application providing the user interface.
- **Postman Collection**: `DeployOnAWSUsing-EC2-S3-RDS.postman_collection.json` - Contains API endpoints for testing.

---

## 🚀 Deployment Overview

- ✅ **Frontend (ReactJS)** → Hosted on **AWS S3** 🗂️
- ✅ **Backend (Spring Boot)** → Running on **AWS EC2** 🖥️
- ✅ **Database (MySQL)** → Managed via **AWS RDS** 💾

---

## 🛠 Prerequisites

Before you begin, ensure you have:

- ✅ **AWS Account**: Access to AWS services.
- ✅ **AWS CLI**: Installed and configured with the necessary permissions.
- ✅ **Postman**: To test API endpoints.

---

## 📌 Deployment Steps

### 🏗️ Step 1: Database Setup on AWS RDS

1. Log in to your AWS Console.
2. Navigate to **RDS** and create a **MySQL** database instance.
3. Note the **database endpoint**, **username**, and **password** for backend configuration. (Update these in `application.properties` in the backend project.)

#### 🔧 `application.properties` Configuration:
```properties
spring.datasource.url=jdbc:mysql://<rds-endpoint>:3306/employeedb
spring.datasource.username=<your-username>
spring.datasource.password=<your-password>
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.hibernate.ddl-auto=update
spring.jpa.database-platform=org.hibernate.dialect.MySQL8Dialect
```
Employee-management properties file for reference:
```sh
spring.datasource.url=jdbc:mysql://employeems.cfqm46e6iup3.ap-south-1.rds.amazonaws.com:3306/employeeMS
spring.datasource.username=admin
spring.datasource.password=12345687
spring.jpa.hibernate.ddl-auto=update
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.MySQL8Dialect
```

---

### 🖥️ Step 2: Backend Deployment on AWS EC2

1. Launch an **EC2 instance** (ensure Java is installed).
2. Transfer the Spring Boot application (`.jar` file) to the EC2 instance.
3. Configure the application to connect to the **RDS database**.
4. Start the backend application using:
   ```sh
   java -jar your-app.jar
   ```
5. Ensure your **EC2 security group** allows inbound traffic on the required ports (e.g., **8080**).

#### Connecting via SSH Client

Run this command, if necessary, to ensure your key is not publicly viewable:

```sh
chmod 400 "employeems.pem"
```

Connect to your instance using its Public DNS:

```sh
ssh -i "employeems.pem" ec2-user@ec2-65-2-75-144.ap-south-1.compute.amazonaws.com
```

#### Deploying the Application

```sh
cd backend
mvn package
scp target/app.jar ec2-user@your-ec2-instance.amazonaws.com:/home/ec2-user/
ssh ec2-user@your-ec2-instance.amazonaws.com
```

Backend API Base URL (Replace `<your-ec2-public-ip>` with your EC2 instance's public IP):
```sh
http://<your-ec2-public-ip>:8080/
```
Employee-management backend base URL for reference:
```sh
http://65.2.75.144:8080/
```

---

### 🌐 Step 3: Frontend Deployment on AWS S3

1. Build the **ReactJS application** to generate static files:
   ```sh
   npm run build
   ```
2. Create an **S3 bucket** in AWS.
3. Upload the **build files** to the S3 bucket.
4. Configure the bucket for **static website hosting**.
5. Access the frontend via the provided **S3 URL**. 🎉

Frontend URL:
```sh
http://<your-s3-bucket-name>.s3-website-<region>.amazonaws.com/
```
Employee-management frontend URL for reference:
```sh
http://employeems.aws.s3-website.ap-south-1.amazonaws.com/
```

---

## 🔍 API Testing with Postman

1. Open **Postman** and import the provided collection: `DeployOnAWSUsing-EC2-S3-RDS.postman_collection.json`.
2. Update the **base URL** with your deployed EC2 instance URL.
3. Run the API requests to test **employee management functionalities**!

---

