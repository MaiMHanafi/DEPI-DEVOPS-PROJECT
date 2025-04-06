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
  
# 🔐 Gmail App Password Setup for Postfix

To send emails using Gmail SMTP via Postfix, you **must use a Gmail App Password**. Google **does not allow** using your normal Gmail password with external tools like Postfix, especially when 2-Step Verification (2FA) is enabled.

---

## ✅ Why Do You Need an App Password?

Google considers tools like Postfix as "less secure apps" if they try to log in with your Gmail password.  
Instead, **App Passwords** provide a secure, limited-access method for third-party tools.

---

## 🔹 Step-by-Step Guide to Generate and Use a Gmail App Password

### 1️⃣ Enable 2-Step Verification
- Visit: [https://myaccount.google.com/security](https://myaccount.google.com/security)
- Under **"Signing in to Google"**, enable **2-Step Verification** if it's not already active.

### 2️⃣ Generate an App Password
- Visit: [https://myaccount.google.com/apppasswords](https://myaccount.google.com/apppasswords)
- You'll be asked to log in again and verify with 2FA.
- Choose the following:
  - **App**: Mail
  - **Device**: Other (enter a custom name like `Postfix Server`)
- Click **Generate**
- A **16-character password** will be shown (example: `abcd efgh ijkl mnop`)

> ⚠️ **Copy this password**. You’ll only see it once.

---

### 3️⃣ Create the Postfix Authentication File

On your target server, run:
```bash
sudo nano /etc/postfix/sasl_passwd
```

Add the following line:
```
[smtp.gmail.com]:587 your_email@gmail.com:your_app_password
```
Replace `your_email@gmail.com` with your actual Gmail address.  
Replace `your_app_password` with the 16-character App Password (without spaces).

---

### 4️⃣ Secure and Compile the File

Run these commands to secure and compile the authentication file:

```bash
sudo postmap /etc/postfix/sasl_passwd
sudo chown root:root /etc/postfix/sasl_passwd /etc/postfix/sasl_passwd.db
sudo chmod 0600 /etc/postfix/sasl_passwd /etc/postfix/sasl_passwd.db
```

---

✅ You're now ready to send secure emails using Gmail SMTP from your server!

---
_______________________________________________________________________________________________________________________________
_______________________________________________________________________________________________________________________________
## 🛡️ Security Tips

- **Never hardcode passwords** directly into playbooks or roles.
- Use **Ansible Vault** to securely encrypt sensitive variables like `smtp_password`:
```bash
ansible-vault encrypt vars.yml
```

---

Let me know if you'd like this content auto-added to your repo or linked from your `README.md`.


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
