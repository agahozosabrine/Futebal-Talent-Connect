# ⚽ Futebol: ICT171 Cloud Server Project

![Status](https://img.shields.io/badge/status-live-brightgreen)
![Platform](https://img.shields.io/badge/platform-Azure-0078D4)
![OS](https://img.shields.io/badge/OS-Ubuntu%2022.04-E95420)
![Server](https://img.shields.io/badge/server-Apache2-D22128)
![License](https://img.shields.io/badge/license-MIT-blue)

A self built, self hosted website. No drag and drop builders, no managed platforms. Just a virtual machine, a terminal, and SSH.

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

I set out to host a working website on a server I configured myself, rather than relying on a website builder like Wix or a managed platform like WordPress.com. Everything below, from the virtual machine to the updates to the web server to the domain, was set up by hand over SSH, with no graphical desktop and no pre installed content management system.

**The stack:**

| Layer | Choice |
|---|---|
| ☁️ Hosting | Microsoft Azure (IaaS) |
| 🐧 Operating System | Ubuntu 22.04 LTS |
| 🌐 Web Server | Apache2 |
| 🔗 Domain | No-IP dynamic DNS hostname |

> **Goal:** build a working, documented server that anyone could follow to rebuild from nothing.
> **Result:** the site loads correctly at both the IP address and the domain name listed above.

---

## 🖥️ What's On the Site

**Futebol** connects talented football players and coaches with opportunities they might otherwise never have access to. The idea is simple: talent isn't rare, but exposure is. Plenty of skilled players and coaches are held back not by ability but by not knowing the right people or having a platform to be seen. This site aims to close that gap by giving them a place to be discovered.

- All content is served out of `/var/www/html/` on the server.

---

## ✅ What You'd Need Before Attempting This Yourself

Ensure you have:
- Azure for Students account
- A local terminal (Terminal.app on macOS)
- Website files ready locally, or a plan to build the site directly on the server

Note: I did not have a domain name lined up before starting. That came later, once the server was already reachable by IP. Details are under Stage 4 below.

---

## 1️⃣ Stage 1: Setting Up the Virtual Machine

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

![Azure VM list](images/azure-vm-list.png)

My server listed as Running in the Azure dashboard.

![Azure VM overview](images/azure-vm-overview.png)

Overview page confirming the OS, IP address, and resource group.

![Azure VM properties](images/azure-vm-properties.png)

Full properties view showing networking and image details.

---

## 2️⃣ Stage 2: Logging In and Updating Everything

I connected to the server using:

```bash
ssh azureuser@104.46.221.42
```

![SSH login](images/ssh-login.png)

Terminal session confirming a successful SSH connection.

With access confirmed, I brought the whole system up to date before touching anything else:

```bash
sudo apt update && sudo apt upgrade -y
```

![apt update](images/apt-update.png)

Refreshing the package index.

![apt upgrade](images/apt-upgrade.png)

Full upgrade cycle, confirming the system was current.

---

## 3️⃣ Stage 3: Getting Apache2 Running

Installed the web server:

```bash
sudo apt install apache2 -y
```

![Apache install](images/apache-install.png)

Apache2 install confirmation.

Then checked that it was actually running and set to start on boot:

```bash
sudo systemctl status apache2
```

![Apache status](images/apache-status.png)

Apache2 confirmed active and running.

Before adding anything custom, I loaded `http://104.46.221.42` in a browser to make sure the server was reachable from outside, not just responsive locally.

---

## 4️⃣ Stage 4: Getting a Domain Name

I registered the hostname futebol.ddns.net through No-IP and pointed it at my server's IP address, 104.46.221.42. Once the DNS record had propagated, the domain loaded the same content as the raw IP address.

```markdown
![DNS configuration](images/dns-config.png)
```
My No-IP dashboard, showing futebol.ddns.net pointed at my server.

```markdown
![Domain resolving](images/domain-resolving.png)
```
The domain loading correctly in a browser.

---

## 5️⃣ Stage 5: Adding HTTPS

```bash
sudo apt install certbot python3-certbot-apache -y
sudo certbot --apache -d futebol.ddns.net
```

```markdown
![Certbot output](images/certbot-output.png)
![HTTPS padlock](images/https-padlock.png)
```
Certificate issued, HTTPS confirmed working in the browser.

---

## 🛠️ Custom Script

I wrote a bash script called `health-check.sh` that pulls together several standard Linux checks into a single readable report, rather than typing each command separately every time I want to check on the server.

It reports on:
- Server uptime
- Disk usage
- Memory usage
- Whether Apache2 is currently running
- The size and file count of the website folder at `/var/www/html`
- The five most recent entries in the Apache access log

The individual commands it wraps (`df`, `free`, `uptime`, `systemctl`, `du`, `tail`) are standard Linux tools covered in the unit labs. What makes this script mine is combining them into one formatted report specific to this server, so a single command gives a full snapshot instead of running five or six separate ones.

The script itself is included in this repository as `health-check.sh`.

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

- 18 June 2026: server created on Azure
- 19 June 2026: first update pass completed, SSH access confirmed
- 18 July 2026: Apache2 installed and confirmed running
- 20 July 2026: documentation written up, screenshots added

---

Author: Agahozo Sabrine, Student Number 35764065
Built on Microsoft Azure using Ubuntu 22.04, Apache2, dynamic DNS, and SSH only administration.

## 📄 License

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file in this repository for the full text. In short: anyone is free to use, copy, modify, and share this code, provided the original license and copyright notice are included.
