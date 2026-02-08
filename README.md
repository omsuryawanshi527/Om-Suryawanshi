# Om-Suryawanshi  
# AWS Intern Assignment – Image Upload System using Docker on AWS EC2  

## 1. Objective  

This project demonstrates deployment of a simple Image Upload web application using:  

- Frontend + Backend (PHP + Apache)  
- MySQL Database  
- Docker & Docker Compose  
- AWS EC2 ( Selecting Ubuntu OS)  
- Amazon S3 (Image Storage)  
- Application Load Balancer (ALB)  
- Auto Scaling Group (ASG)  
- Cost Optimization strategies  

Users can upload images through a web interface.  
Images are stored in Amazon S3 and metadata is stored in MySQL.  

---

## 2. Application Architecture  

User → Application Load Balancer → EC2 Instance(s) → Docker Containers → MySQL Database  
                                                        ↓  
                                                    Amazon S3  

Components:  
- Web Container (PHP + Apache)  
- Database (MySQL / RDS)  
- Amazon S3 for image storage  
- Docker Compose for orchestration  
- AWS EC2 for hosting  

---

## 3. Project Structure  

```text
task2-docker-app/
│
├── Dockerfile
├── docker-compose.yml
└── app/
    ├── index.php
    ├── upload.php
    └── db.php
```
---

## 4. Application Functionality  

- Upload image from browser  
- Store image in Amazon S3  
- Save image URL in MySQL  
- Display uploaded image  
- Verify database connectivity  

---

## 5. PHP Application  

### index.php  

Provides upload form.  

### upload.php  

Uploads image to S3 and stores URL in database.  

### db.php  

Handles database connection.  

---

## 6. Docker Configuration  

### Dockerfile  

Used to build the PHP web application container.

Uses PHP 8.3 with Apache.

Installs mysqli extension for MySQL connection.

Copies the app/ folder into the web server directory.

Exposes port 80 for browser access.

### docker-compose.yml
```bash
 docker-compose.yml
version: "3.9"

services:
  web:
    build: .
    container_name: php_web
    ports:
      - "80:80"
    environment:
      DB_HOST: db
      DB_USER: root
      DB_PASS: rootpass
      DB_NAME: studentdb
    depends_on:
      - db
    restart: always

  db:
    image: mysql:8.0
    container_name: mysql_db
    environment:
      MYSQL_ROOT_PASSWORD: rootpass
      MYSQL_DATABASE: studentdb
    volumes:
      - mysql_data:/var/lib/mysql
    restart: always
 volumes:
      - mysql_data
```
---

## 7. AWS EC2 Deployment Steps

### Step 1: Launch EC2
Operating System : Ubuntu

Instance Type: t2.micro

Open Ports:

22 (SSH)
80 (HTTP)

Connect:

```bash
ssh -i key.pem ec2-user@<EC2_PUBLIC_IP>
```
---

Step 2: Install Docker

```bash
sudo yum update -y
sudo yum install docker -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ec2-user
newgrp docker
```
Verify:
```bash
docker --version
docker ps
```
Step 3: Install Docker Compose
```bash
sudo curl -L https://github.com/docker/compose/releases/download/v2.27.0/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```
---
Step 4: Deploy Application

mkdir task2-docker-app

cd task2-docker-app

mkdir app

docker-compose up -d --build

Check:
docker ps

---
### 8. Access Application
From EC2:

curl http://localhost
From browser:
```bash
http://<EC2_PUBLIC_IP>
```
---
### 9. Load Balancer Configuration
Create Target Group (HTTP :80)

Register EC2

Create Application Load Balancer

Attach Target Group

Health Check Path: /

### Access app via ALB DNS OR Public IP of server.
---

## 10. Auto Scaling
Min: 1

Max: 3

Scale Out: CPU > 70%

Scale In: CPU < 30%

---

### 11. Cost Optimization
Free tier EC2

Minimal storage

Auto Scaling

Docker multi-container

Stop EC2 when idle

### 12. Troubleshooting
App not accessible
docker ps
Check Security Group port 80.


Container running but port unreachable
docker inspect php_web
ALB health check failing
docker logs php_web

### 13. Assumptions
AWS Free Tier account

Internet access

Basic Docker knowledge

Stateless application

### 14. Conclusion
This project demonstrates:

Docker installation on AWS EC2

PHP + MySQL containerized deployment

Image upload system

Amazon S3 integration

Port exposure

Auto-start containers

Load Balancer integration

Auto Scaling

Cost optimization techniques

### Author: Om Suryawanshi



---
