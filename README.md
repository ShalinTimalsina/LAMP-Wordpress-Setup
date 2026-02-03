# AWS LAMP Stack Deployment on Ubuntu Linux
**A step-by-step LAMP stack deployment to host a WordPress website on AWS EC2 using Ubuntu, Apache, MariaDB, and PHP.**

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


---

## Project Overview
This project documents deploying a **LAMP stack (Linux, Apache, MariaDB, PHP)** on **AWS EC2 (Ubuntu)** and hosting a **WordPress website**.

---

## 🎯 Objectives
- Deploy **Apache + MariaDB + PHP** on **Ubuntu EC2**
- Host **WordPress** under a proper web root
- Validate **Apache**, **PHP runtime**, and **database connectivity**

---

### Traffic Flow
1. Users access the site via **HTTP (80)** (optional **HTTPS (443)**)
2. **Apache** serves WordPress files from the web root
3. **PHP** executes WordPress code
4. **MariaDB** stores WordPress data locally on the s

---

### Architecture Diagram (ASCII)
```
                +-------------------------+
                |     Internet Users      |
                +-----------+-------------+
                            |
                            |  HTTP/HTTPS
                            v
                +-------------------------+
                |   AWS Security Group    |
                |  80, (443), 22 (My IP)  |
                +-----------+-------------+
                            |
                            v
          +----------------------------------------+
          |      EC2 Instance (Ubuntu 22.04)       |
          |                                        |
          |  +-------------+     +---------------+ |
          |  |   Apache    | --> |      PHP      | |
          |  |   (2.4.x)     |     |     (8.x)     | |
          |  +------+------+     +-------+-------+ |
          |         |                    |         |
          |         v                    v         |
          |     WordPress            MariaDB        |
          |                          (10.x)         |
          +----------------------------------------+

```
## 🏗 Architecture Overview

📂 **Architecture Diagram:**  
<p align="center">
  <img src="Architecture%20Diagram/.png" width="700">
</p>





























































