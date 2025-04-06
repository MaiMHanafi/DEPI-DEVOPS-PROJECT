# 📬 Ansible Role: Postfix Gmail SMTP Setup

This Ansible role installs and configures **Postfix** to send emails using **Gmail SMTP**. It sets up authentication, manages configuration files, and ensures the correct firewall rules are in place.

---

## 📁 Role Structure

```
roles/
└── postfix_gmail/
    ├── tasks/
    │   └── main.yml
    ├── templates/
    │   └── postfix_main.cf.j2
    └── README.md
```

---

## 🔧 Requirements

- Target system must be **Debian/Ubuntu-based**.
- **UFW** (Uncomplicated Firewall) should be installed to manage port rules.
- A valid **Gmail account** with **App Password** (if 2FA is enabled) is required.

---

## 🔐 Role Variables

| Variable        | Description                              | Example                    |
|----------------|------------------------------------------|----------------------------|
| `smtp_email`    | Gmail address used to send emails        | `your.email@gmail.com`     |
| `smtp_password` | Gmail App Password for authentication    | `yourapppassword`          |

Define these in your playbook or inventory:

```yaml
smtp_email: your.email@gmail.com
smtp_password: your_app_password
```

---

## 📜 Tasks Summary

- Install required packages: `postfix` and `mailutils`
- Ensure `/etc/postfix` configuration directory exists
- Deploy a Postfix `main.cf` configuration file via Jinja2 template
- Configure SMTP authentication with `sasl_passwd`
- Secure and compile the password file using `postmap`
- Restart Postfix to apply changes
- Open port 587 via `ufw` for SMTP communication

---

## 📂 Template File: `postfix_main.cf.j2`

This file should contain your Postfix configuration using Jinja2 templating syntax. Example settings may include:

```
relayhost = [smtp.gmail.com]:587
smtp_use_tls = yes
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_sasl_security_options = noanonymous
smtp_tls_CAfile = /etc/ssl/certs/ca-certificates.crt
```

---

## 🔒 Security Note

Avoid hardcoding sensitive values (like passwords) in your playbooks. Use **Ansible Vault** to securely encrypt secrets:

```bash
ansible-vault encrypt vars.yml
```

---

## ✅ Tested On

- Ubuntu 20.04  
- Ubuntu 22.04
