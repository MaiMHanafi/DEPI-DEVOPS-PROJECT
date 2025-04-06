# ⚙️ Ansible Role: Jenkins and Development Environment Setup

This Ansible role automates the setup of a **complete CI/CD and development environment**. It installs and configures Jenkins, Docker, Docker Compose, Ansible, Node.js, and other essential packages on a **Debian-based Linux system**.

---

## 📋 Overview

This role handles:

- System updates and package installation
- Jenkins setup and repository configuration
- Docker and Docker Compose installation
- Adding the Jenkins user to the Docker group
- Sudoers file update for passwordless sudo
- Jenkins service management

---

## 🧱 Role Structure

```
roles/
└── jenkins_setup/
    ├── tasks/
    │   └── main.yml
    └── README.md (this file)
```

---

## ✅ Requirements

- Debian-based system (Ubuntu 20.04, 22.04, etc.)
- Internet access (to download Jenkins, Docker Compose, etc.)
- Run as a user with `sudo` privileges

---

## 📦 Packages Installed

- `git`
- `openjdk-17-jdk`
- `docker.io`
- `docker-compose`
- `ansible`
- `nodejs`
- `npm`
- `jenkins`

---

## 🔧 Tasks Summary

### 1️⃣ System Preparation
- Update and upgrade the APT cache
- Install required system packages

### 2️⃣ Docker & Docker Compose
- Install Docker
- Download Docker Compose from the official GitHub release based on system architecture

### 3️⃣ Jenkins Installation
- Add Jenkins APT repository key and source list
- Update the package cache
- Install Jenkins

### 4️⃣ Jenkins Configuration
- Enable and start the Jenkins service
- Add the Jenkins user to the `docker` group for container access
- Restart Jenkins service

### 5️⃣ Sudo Configuration
- Ensure `ubuntu` and `jenkins` users can run `sudo` commands without a password
- Validated using `visudo` to prevent syntax errors

---

## 🛡️ Security Notes

- Passwordless `sudo` should be used with caution and limited to trusted users.
- Use a firewall (e.g., `ufw`) to control access to Jenkins (default port: `8080`).
- Remember to **secure Jenkins** post-installation (admin user setup, plugins, credentials management).

---

## 🧪 Tested On

- ✅ Ubuntu 20.04
- ✅ Ubuntu 22.04

---

## 🚀 Usage

Include this role in your playbook like this:

```yaml
- hosts: target_servers
  become: yes
  roles:
    - jenkins_setup
```

---

Let me know if you’d like a `defaults/main.yml` or `handlers/main.yml` created for role flexibility!
