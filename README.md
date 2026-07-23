⚽ Futebol: ICT171 Cloud Server Project

![Status](https://img.shields.io/badge/status-live-brightgreen)
![Platform](https://img.shields.io/badge/platform-Azure-0078D4)
![OS](https://img.shields.io/badge/OS-Ubuntu%2022.04-E95420)
![Server](https://img.shields.io/badge/server-Apache2-D22128)
![License](https://img.shields.io/badge/license-MIT-blue)

A self built, self hosted website. No drag and drop builders, no managed platforms. Just a virtual machine, a terminal, and SSH.

🔗 **Live site:** [futebol.ddns.net](https://futebol.ddns.net) · **Backup by IP:** [104.46.221.42](http://104.46.221.42)
🎥 **Video walkthrough:** *(link to be added once recorded )*

<br>

| | |
|---|---|
| **Author** | Agahozo Sabrine |
| **Student Number** | 35764065 |
| **Assignment** | ICT171 Cloud Server Project, 2026 |
| **Server Name** | agahozo |
| **Login User** | azureuser |
| **Public IP** | 104.46.221.42 |
| **Domain** | [https://futebol.ddns.net](https://futebol.ddns.net) |
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

## ✅ Prerequisites: What You'd Need Before Attempting This Yourself

Before starting, make sure you have the following ready:

1. **A Cloud Account:** An active Microsoft Azure account (I used an Azure for Students account).
2. **A Local Terminal:** Any terminal or command-line interface will do — I used Terminal.app on macOS, but PowerShell or Git Bash on Windows works the same way.
3. **Website Files (optional):** Either have your HTML/CSS/JS ready locally, or plan to build the site directly on the server. I built mine locally and uploaded it afterwards.
4. **Basic CLI Comfort:** You should be comfortable navigating directories and connecting to a remote machine — nothing more advanced than that.

Note: I did not have a domain name lined up before starting. That came later, once the server was already reachable by IP. Details are under Stage 4 below.

---

## 1️⃣ Stage 1: Provisioning the Virtual Machine

The first step is standing up the actual cloud hardware.

1. Logged into the **Azure Portal** and searched for **Virtual Machines**.
2. Clicked **Create** → **Azure Virtual Machine** and configured it with these settings:
   - Name: `agahozo`
   - Operating system: **Ubuntu Server 22.04 LTS**
   - Size: **Standard B2ats v2** (2 vCPUs, 1 GiB memory)
   - Region: **Japan East**
   - Resource group: **ICT171A**
3. Under **Administrator account**, I chose password-based authentication and set a strong password, keeping it safe for the SSH login step later.
4. Under **Networking**, I opened three inbound ports on the Network Security Group so the server would actually be reachable from outside:

| Service | Port | Protocol | Why it's needed |
|---|---|---|---|
| SSH | 22 | TCP | Lets me connect to and manage the server remotely |
| HTTP | 80 | TCP | Regular, unencrypted web traffic |
| HTTPS | 443 | TCP | Secure web traffic, used once SSL/TLS is added |

Without these rules open, the server would be unreachable both for SSH and for anyone trying to visit the site.

<img width="936" height="326" alt="Screenshot 2026-07-23 at 16 56 52" src="https://github.com/user-attachments/assets/f667ac12-805e-4b90-aba5-91b4fcb9c08e" />

My server listed as Running in the Azure dashboard.

<img width="1454" height="539" alt="Screenshot 2026-07-20 at 11 30 17" src="https://github.com/user-attachments/assets/7c390b4c-c315-499b-b943-7163b14d2603" />

Overview page confirming the OS, IP address, and resource group.

<img width="971" height="716" alt="Screenshot 2026-07-20 at 11 30 51" src="https://github.com/user-attachments/assets/49eef142-72e8-401b-ab5a-3f2e08bcec91" />

Full properties view showing networking and image details.

---

## 2️⃣ Stage 2: Connecting and Updating the Server

With the VM running, the next step is logging in and making sure the system is fully patched before installing anything.

1. **Connect over SSH** from the local terminal:

```bash
ssh azureuser@104.46.221.42
```

   - The first time you connect, you'll see a warning that the authenticity of the host can't be established, asking if you want to continue connecting. Typing `yes` and pressing Enter accepts the server's fingerprint and continues.
   - When prompted for the password, nothing will appear on screen as you type — no dots, no asterisks. This is normal Linux behaviour to stop anyone from seeing the password length over your shoulder. Just type it and press Enter.

<img width="623" height="523" alt="Screenshot 2026-07-23 at 16 58 24" src="https://github.com/user-attachments/assets/b7dd78af-61ca-4843-ac8c-df7d13b1cefb" />

Terminal session confirming a successful SSH connection.

2. **Update and upgrade the system** before touching anything else:

```bash
sudo apt update && sudo apt upgrade -y
```

   `apt update` refreshes the local package index so the system knows what the latest versions are, and `apt upgrade -y` installs those updates without asking for confirmation on each one.

<img width="1470" height="956" alt="Screenshot 2026-07-23 at 17 01 57" src="https://github.com/user-attachments/assets/c62f8935-69eb-4c03-b317-a4117691a6fb" />

Refreshing the package index.

<img width="574" height="208" alt="Screenshot 2026-07-23 at 17 03 20" src="https://github.com/user-attachments/assets/37fcfb32-aed4-4623-8ea3-122c43e267ee" />

Full upgrade cycle, confirming the system was current.

---

## 3️⃣ Stage 3: Installing and Running Apache2

1. **Install the web server:**

```bash
sudo apt install apache2 -y
```

<img width="1439" height="163" alt="Screenshot 2026-07-23 at 17 04 48" src="https://github.com/user-attachments/assets/41076da7-b0d9-46ff-a95f-0419a712a5b6" />

Apache2 install confirmation.

2. **Start Apache and enable it on boot**, then check its status:

```bash
sudo systemctl start apache2
sudo systemctl enable apache2
sudo systemctl status apache2
```

   `start` launches the service immediately, `enable` makes sure Apache comes back up automatically after a reboot, and `status` confirms it's active and running.

<img width="564" height="353" alt="Screenshot 2026-07-23 at 17 39 24" src="https://github.com/user-attachments/assets/e4dec985-5a18-487c-a8d7-8a8c4fa4cdbb" />

Apache2 status check.

<img width="640" height="357" alt="Screenshot 2026-07-23 at 17 05 36" src="https://github.com/user-attachments/assets/9d7b9478-c48b-4b7d-b00d-2c02895dba9b" />

Apache2 confirmed active and running.

3. **Confirm the server is publicly reachable** by loading `http://104.46.221.42` in a browser — not just checking that it works locally. If everything is set up correctly, you'll be greeted by the default **Apache2 Ubuntu Default Page**, confirming the server is fully serving web traffic before any custom files are added.

<img width="1011" height="595" alt="Screenshot 2026-07-23 at 20 32 04" src="https://github.com/user-attachments/assets/5869fa3d-2416-4987-b959-a4402fe24053" />


Default Apache2 Ubuntu page, confirming the server was live before I uploaded my own site.

4. **Upload the website files.** Apache serves files from `/var/www/html/` by default, which is owned by `root`, so I gave my own user ownership of that folder before uploading anything:

```bash
sudo chown -R azureuser:azureuser /var/www/html
```

   From there I used an SFTP client (FileZilla) to drag and drop my HTML, CSS, and JavaScript files from my computer straight into `/var/www/html/`, replacing the default `index.html` with my own site.

---

## 4️⃣ Stage 4: Getting a Domain Name

Remembering a raw IP address like `104.46.221.42` isn't practical, so I mapped it to a proper hostname using No-IP's free dynamic DNS service.

1. Created a free account at [No-IP.com](https://www.no-ip.com/).
2. Clicked **Create Hostname**, chose the name `futebol`, and picked the free `.ddns.net` extension.
3. Set the **Record Type** to **A (Host)** and pasted in my Azure VM's public IP address, `104.46.221.42`.
4. Saved the hostname and waited for the DNS record to propagate.

Once propagation was complete, `futebol.ddns.net` loaded the exact same content as the raw IP address.

<img width="1158" height="238" alt="Screenshot 2026-07-23 at 17 22 50" src="https://github.com/user-attachments/assets/56558c77-7aa1-44df-82e0-7a97abc808ef" />

My No-IP dashboard, showing futebol.ddns.net pointed at my server.

<img width="1270" height="806" alt="Screenshot 2026-07-23 at 17 24 51" src="https://github.com/user-attachments/assets/559efd13-12e4-4b4f-8da7-4cae14428f7c" />

The domain loading correctly in a browser.

---

## 5️⃣ Stage 5: Securing the Site with HTTPS

Running over plain HTTP means all traffic between the browser and the server is unencrypted. To fix this, I used Certbot to install a free SSL/TLS certificate from Let's Encrypt.

1. **Confirm port 443 is open** in the Azure Network Security Group, alongside the existing SSH (22) and HTTP (80) rules — without it, Certbot's validation and HTTPS traffic itself would fail.

2. **Install Certbot and its Apache plugin:**

```bash
sudo apt install certbot python3-certbot-apache -y
```

3. **Request and install the certificate:**

```bash
sudo certbot --apache -d futebol.ddns.net
```

   During this step, Certbot asks a few quick questions:
   - An email address, used for urgent renewal and security notices.
   - Agreement to the Let's Encrypt Terms of Service.
   - Whether to share that email with the EFF (optional, yes/no).
   - Whether to redirect all HTTP traffic to HTTPS automatically — I selected the redirect option so every visitor lands on the secure version of the site.

4. **Test that auto-renewal works.** Let's Encrypt certificates expire after 90 days, but Certbot sets up a background timer to renew them automatically. A dry run confirms this is working without actually issuing a new certificate:

```bash
sudo certbot renew --dry-run
```

<img width="572" height="378" alt="Screenshot 2026-07-23 at 17 08 36" src="https://github.com/user-attachments/assets/57d24b6a-e17e-406d-aa05-3df7855a2c98" />

Installing Certbot and requesting the certificate.

<img width="1218" height="842" alt="Screenshot 2026-07-23 at 17 32 04" src="https://github.com/user-attachments/assets/bca8ca7f-b1fc-41a1-84eb-78175c3a4c24" />

Certificate issued, HTTPS confirmed working in the browser.

---

## 📜 Scripting Component

To meet the interactive scripting requirements of the project, the platform utilizes custom client side JavaScript (`assets/js/main.js` / inline scripts) to process user interactions, handle dynamic filtering, and manage web applications dynamically in the browser.

### 💡 Script Functionality & Implementation
1. **Dynamic Content Filtering:**
   * **Purpose:** Allows scouts and users to filter player profiles by position (e.g., Forward, Midfielder, Defender), location and age group in realtime.
   * **Implementation:** Uses JavaScript Event Listeners (`addEventListener('change', ...)`) and DOM manipulation to re render player cards dynamically without reloading the server page.

2. **Form Validation & Data Handling:**
   * **Purpose:** Validates input data on player registration and contact forms prior to submission.
   * **Implementation:** Verifies email formatting, checks required field parameters, and provides immediate visual feedback/error handling directly in the browser DOM.

### 📸 Verifiable Output
* **Real time DOM Updates:** JavaScript dynamically updates element class lists and DOM nodes upon filter execution.
* **Console Verification:** Open the browser Developer Tools (`F12` -> Console) on `https://futebol.ddns.net/players.html` to view active JS event logs during search and filter operations.

---

## 🔍 Checking Everything Works

- [x] Site reachable at http://104.46.221.42
- [x] Site reachable at https://futebol.ddns.net
- [x] HTTPS padlock shows no certificate warnings
- [x] Site displays correctly on both desktop and mobile

---

## 🚀 Conclusion & Next Steps

Building, deploying, and securing this server from scratch covered the full stack of cloud fundamentals:

- **Virtualization & Cloud Management:** Provisioning and configuring a Linux VM in Microsoft Azure.
- **Web Server Administration:** Deploying and managing Apache2 to serve live traffic.
- **Secure File Operations:** Setting correct directory ownership and transferring files safely via SFTP.
- **Network Security & Cryptography:** Opening the right ports and implementing automated TLS/SSL encryption with Let's Encrypt.

The server is now live, documented, and ready to keep growing as the Futebol platform is built out further.

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
