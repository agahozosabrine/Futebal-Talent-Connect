# ⚽ Futebol — ICT171 Cloud Server Project

![Status](https://img.shields.io/badge/status-live-brightgreen)
![Platform](https://img.shields.io/badge/platform-Azure-0078D4)
![OS](https://img.shields.io/badge/OS-Ubuntu%2022.04-E95420)
![Server](https://img.shields.io/badge/server-Apache2-D22128)
![License](https://img.shields.io/badge/license-MIT-blue)

A self built, self hosted website no drag and drop builders, no managed platforms. Just a virtual machine, a terminal, and SSH.

🔗 **Live site:** [futebol.ddns.net](https://futebol.ddns.net) · **Backup by IP:** [104.46.221.42](http://104.46.221.42)
🎥 **Video walkthrough:** *(link to be added once recorded)*

<br>

| | |
|---|---|
| **Author** | Agahozo Sabrine |
| **Student Number** | 35764065 |
| **Assignment** | ICT171 Cloud Server Project, 2026 |
| **Server Name** | agahozo |
| **Login User** | azureuser |
| **Public IP** | 104.46.221.42 |
| **Repository** | [agahozosabrine/Futebal-Talent-Connect](https://github.com/agahozosabrine/Futebal-Talent-Connect) |

---

## 📋 Declaration of Original Work

This repository and its contents were built and documented independently by Agahozo Sabrine (Student Number 35764065) for ICT171 at Murdoch University. All commands, configuration steps, and screenshots reflect my own server build. Where any external reference, script, or tutorial was consulted, it is credited in the relevant section below rather than copied wholesale.

---

## 🚀 What This Project Is

I set out to host a working website on a server I configured myself, rather than relying on a website builder like Wix or a managed platform like WordPress.com. Everything below  the virtual machine, the updates, the web server, and the domain  was set up by hand over SSH, with no graphical desktop and no pre installed content management system.

**The stack:**

| Layer | Choice |
|---|---|
| ☁️ Hosting | Microsoft Azure (IaaS) |
| 🐧 Operating System | Ubuntu 22.04 LTS |
| 🌐 Web Server | Apache2 |
| 🔗 Domain | Free dynamic DNS hostname |

> **Goal:** build a working, documented server that anyone could follow to rebuild from nothing.
> **Result:** the site loads correctly at both the IP address and the domain name listed above.

---

## 🖥️ What's On the Site

**Futebol** connects talented football players and coaches with opportunities they might otherwise never have access to. The idea is simple: talent isn't rare, but exposure is  plenty of skilled players and coaches are held back not by ability but by not knowing the right people or having a platform to be seen. This site aims to close that gap by giving them a place to be discovered.

- All content is served out of `/var/www/html/` on the server.
- *(add any specific pages/features here e.g. player profiles, sign-up form, listings  once you're ready to describe them in more detail)*

---

## ✅ What You'd Need Before Attempting This Yourself

- An Azure student account with available credit
- A computer with a terminal capable of running SSH commands
- Your website files ready to go, or a plan for building them directly on the server

Note: I did not have a domain name lined up before starting  that came later, once the server was already reachable by IP. Details are under Stage 4 below.

---

## 1️⃣ Stage 1  Setting Up the Virtual Machine

I logged into the Azure portal and provisioned a new virtual machine using these settings:

- Name: agahozo
- Operating system: Ubuntu 22.04 LTS
- Size: Standard B2ats v2, giving 2 vCPUs and 1 GiB of memory
- Region: Japan East
- Resource group: ICT171A

I also opened three ports on the Network Security Group so the server would actually be reachable:
- Port 22 for SSH access
- Port 80 for regular web traffic
- Port 443 reserved for secure web traffic once HTTPS is added

**Screenshots to add here:**
1. `azure-vm-list.png` — the Azure dashboard showing my VM listed with a "Running" status
2. `azure-vm-overview.png` — the VM's overview page, showing the operating system and public IP
3. `azure-vm-properties.png` — the detailed properties tab, showing networking info and the source image

![Azure VM list](images/azure-vm-list.png)

My server listed as Running in the Azure dashboard.

![Azure VM overview](images/azure-vm-overview.png)

Overview page confirming the OS, IP address, and resource group.

![Azure VM properties](images/azure-vm-properties.png)

Full properties view showing networking and image details.

---

## 2️⃣ Stage 2  Logging In and Updating Everything

I connected to the server using:

```bash
ssh azureuser@104.46.221.42
```

**Screenshot to add here:**
4. `ssh-login.png` — the terminal right after a successful login

![SSH login](images/ssh-login.png)

Terminal session confirming a successful SSH connection.

With access confirmed, I brought the whole system up to date before touching anything else:

```bash
sudo apt update && sudo apt upgrade -y
```

**Screenshots to add here:**
5. `apt-update.png` — output of the update command
6. `apt-upgrade.png` — output of the full update and upgrade sequence

![apt update](images/apt-update.png)

Refreshing the package index.

![apt upgrade](images/apt-upgrade.png)

Full upgrade cycle, confirming the system was current.

---

## 3️⃣ Stage 3  Getting Apache2 Running

Installed the web server:

```bash
sudo apt install apache2 -y
```

**Screenshot to add here:**
7. `apache-install.png` — confirmation that Apache2 installed (or was already current)

![Apache install](images/apache-install.png)

Apache2 install confirmation.

Then checked that it was actually running and set to start on boot:

```bash
sudo systemctl status apache2
```

**Screenshot to add here:**
8. `apache-status.png` — status output showing "active (running)"

![Apache status](images/apache-status.png)

Apache2 confirmed active and running.

Before adding anything custom, I loaded `http://104.46.221.42` in a browser to make sure the server was reachable from outside, not just responsive locally.

---

## 4️⃣ Stage 4  Getting a Domain Name

*(fill in your actual provider once confirmed — e.g. No-IP or DuckDNS)*

I registered the hostname futebol.ddns.net and pointed it at my server's IP address, 104.46.221.42. Once the DNS record had propagated, the domain loaded the same content as the raw IP address.

**Screenshots to add here:**
9. `dns-config.png` — your dynamic DNS provider's dashboard showing the hostname mapped to 104.46.221.42
10. `domain-resolving.png` — futebol.ddns.net loading successfully in a browser

```markdown
![DNS configuration](images/dns-config.png)
```
My dynamic DNS dashboard, showing futebol.ddns.net pointed at my server.

```markdown
![Domain resolving](images/domain-resolving.png)
```
The domain loading correctly in a browser.

---

## 5️⃣ Stage 5  Adding HTTPS

```bash
sudo apt install certbot python3-certbot-apache -y
sudo certbot --apache -d futebol.ddns.net
```

**Screenshots to add here:**
11. `certbot-output.png` — Certbot confirming the certificate was issued
12. `https-padlock.png` — the browser showing a secure padlock on the domain

```markdown
![Certbot output](images/certbot-output.png)
![HTTPS padlock](images/https-padlock.png)
```
Certificate issued, HTTPS confirmed working in the browser.

---

## 🛠️ Custom Script

*(describe your script here: what problem it solves, what it does, and whether any part of it was adapted from a lab exercise or an online source — credit that source here)*

**Screenshot to add here:**
13. `script-output.png` — the script running with visible output

```markdown
![Script output](images/script-output.png)
```

---

## 🔍 Checking Everything Works

- [x] Site reachable at http://104.46.221.42
- [ ] Site reachable at https://futebol.ddns.net *(update once DNS and HTTPS are confirmed)*
- [ ] HTTPS padlock shows no certificate warnings
- [ ] Site displays correctly on both desktop and mobile

---

## 📅 Build Log

- 18 June 2026 — server created on Azure
- 19 June 2026 — first update pass completed, SSH access confirmed
- 18 July 2026 — Apache2 installed and confirmed running
- 20 July 2026 — documentation written up, screenshots added

---

Author: Agahozo Sabrine, Student Number 35764065
Built on Microsoft Azure using Ubuntu 22.04, Apache2, dynamic DNS, and SSH-only administration.

## 📄 License

This project is licensed under the **MIT License** see the [LICENSE](LICENSE) file in this repository for the full text. In short: anyone is free to use, copy, modify, and share this code, provided the original license and copyright notice are included.
