# AWS LAMP Stack Deployment on Ubuntu Linux
**A step-by-step LAMP stack deployment on AWS EC2 using Ubuntu, Apache, MariaDB, PHP, and WordPress, designed with public and private subnets and tested using round-robin load balancing.**

---
<p align="center">
  <img src="https://img.shields.io/badge/AWS-EC2-orange?logo=amazonaws&style=for-the-badge" />
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Apache-red?logo=apache&style=for-the-badge" />
  <img src="https://img.shields.io/badge/MariaDB-blue?logo=mariadb&style=for-the-badge" />
  <img src="https://img.shields.io/badge/PHP-777BB4?logo=php&style=for-the-badge" />
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Ubuntu-E95420?logo=ubuntu&style=for-the-badge" />
  <img src="https://img.shields.io/badge/WordPress-21759B?logo=wordpress&style=for-the-badge" />
  <img src="https://img.shields.io/badge/DevOps-Practices-success?style=for-the-badge" />
</p>



---

## Table of Contents

- [Project Overview](#project-overview)
- [Objectives](#objectives)
- [Architecture Overview](#-architecture-overview)
  - [Traffic Flow](#traffic-flow)
- [AWS Setup Images](#aws-setup-images)
- [AWS Resources Used](#-aws-resources-used)
- [Security Group Design Detailed](#-security-group-design-detailed)
- [NAT Gateway Role in Security](#-nat-gateway-role-in-security)
- [Temporary Bastion Access Clarification](#temporary-bastion-access-clarification)
- [Final Security Summary](#final-security-summary)
- [Access Strategy Bastion Style](#-access-strategy-bastion-style)
- [Apache Server Configuration](#%EF%B8%8F-apache-server-configuration)
- [Nginx Load Balancer Configuration](#%EF%B8%8F-nginx-load-balancer-configuration)
- [Validation](#validation)

---

## Project Overview

This project demonstrates a **production-style AWS architecture** using a custom VPC where:

- **Nginx** acts as a **Reverse Proxy + Load Balancer** in port `80`
- **Two Apache web servers** run in **private subnets** in port `8080`
- Traffic flows securely from **Internet → Nginx → Apache servers**
- **Round-Robin load balancing** distributes requests evenly
- Backend servers are **not publicly accessible**

This setup is intentionally designed for **hands-on practice**, **cloud networking understanding**, and **DevOps fundamentals**.

---

## Objectives

- Understand **public vs private subnet architecture**
- Implement **Nginx reverse proxy & load balancing**
- Secure backend servers using **Security Group → Security Group rules**
- Practice **Linux server administration**
- Gain experience with **real AWS networking components**

---

## 🏗 Architecture Overview

📂 **Architecture Diagram:**  
<p align="center">
  <img src="Architecture%20Diagram/Nginx-Apache-Lb-Architecture-Diagram.png" width="700">
</p>



### Traffic Flow

```
Internet
   ↓
Elastic IP
   ↓
Nginx Load Balancer (Public Subnet)
   ↓
Private IP Communication
   ↓
Apache App Server 1 (Private Subnet)
   ↓
Apache App Server 2 (Private Subnet)
```

---

























































