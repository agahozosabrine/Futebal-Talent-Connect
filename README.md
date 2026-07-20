# Futebol – ICT171 Cloud Server Project

**ICT171 Assignment – 2026**
**Student Name:** Agahozo Sabrine
**Student Number:** 35764065

**VM Name:** agahozo
**Username:** azureuser
**Public IP:** 104.46.221.42
**Domain:** [https://futebol.ddns.net](https://futebol.ddns.net)

---

## Project Overview

This project demonstrates the deployment of a website using **Microsoft Azure IaaS**, Ubuntu 22.04 LTS, and the Apache2 web server, configured via SSH. A dynamic DNS domain (futebol.ddns.net) points to the server's public IP so the site is reachable by name as well as by IP address.

**Purpose:** To build, configure, and document a working cloud web server from scratch, demonstrating core sysadmin skills — provisioning, remote administration, package management, and web server configuration.
**Outcome:** A functional website accessible at both futebol.ddns.net and 104.46.221.42.

---

## Before Starting

Make sure you have:
- An Azure for Students (or equivalent) account
- A dynamic DNS domain (e.g. from noip.com or duckdns.org)
- A local terminal (Terminal.app / PowerShell) with SSH available
- Website files ready locally, or a plan to build the site directly on the server

---

## Phase 1 – Azure VM Setup

Created a virtual machine in the Azure portal:

- **VM Name:** agahozo
- **OS:** Ubuntu 22.04 LTS
- **Size:** Standard B2ats v2 (2 vCPUs, 1 GiB RAM)
- **Region:** Japan East
- **Resource Group:** ICT171A

![Azure VM list](images/azure-vm-list.png)
*The `agahozo` VM listed in Azure's Compute infrastructure dashboard, status: Running.*

![Azure VM overview](images/azure-vm-overview.png)
*VM overview confirming OS (Ubuntu 22.04), public IP (104.46.221.42), and resource group (ICT171A).*

![Azure VM properties](images/azure-vm-properties.png)
*Full VM properties: networking (private IP 10.1.1.4, vnet/subnet), size, and source image details (Canonical, Ubuntu 22.04 LTS).*

---

## Phase 2 – Connect via SSH and Update the System

Connected to the VM using SSH:

```bash
ssh azureuser@104.46.221.42
```

![SSH login](images/ssh-login.png)
*Successful SSH connection, showing system load, memory usage, and IP address confirmation.*

Updated the package index and upgraded installed packages:

```bash
sudo apt update && sudo apt upgrade -y
```

![apt update output](images/apt-update.png)
*Running `sudo apt update` to refresh package lists.*

![apt upgrade output](images/apt-upgrade.png)
*Full update and upgrade cycle — system confirmed fully up to date.*

---

## Phase 3 – Install and Verify Apache2

Installed the Apache2 web server:

```bash
sudo apt install apache2 -y
```

![Apache install](images/apache-install.png)
*Apache2 installation — confirmed already at latest version (2.4.52).*

Verified the service is active and enabled on boot:

```bash
sudo systemctl status apache2
```

![Apache status](images/apache-status.png)
*Apache2 service active (running), started automatically by systemd.*

Tested the default page in a browser at `http://104.46.221.42` to confirm the server was reachable before adding custom content.

---

## Phase 4 – Domain Setup (Dynamic DNS)

*(Document your provider here — e.g. No-IP / DuckDNS)*

- Registered the hostname **futebol.ddns.net**
- Pointed the A record to the VM's public IP: **104.46.221.42**
- Verified propagation by browsing to https://futebol.ddns.net

```markdown
![DNS configuration](images/dns-config.png)
```
*(Screenshot of your dynamic DNS dashboard showing the hostname mapped to 104.46.221.42)*

```markdown
![Domain resolving](images/domain-resolving.png)
```
*(Screenshot of futebol.ddns.net loading successfully in the browser)*

---

## Phase 5 – Enable HTTPS with Let's Encrypt

*(Fill in once completed)*

```bash
sudo apt install certbot python3-certbot-apache -y
sudo certbot --apache -d futebol.ddns.net
```

```markdown
![Certbot output](images/certbot-output.png)
![HTTPS padlock](images/https-padlock.png)
```

---

## Phase 6 – Website Content

*(Describe what the site contains — pages, structure, any customisation)*

```markdown
![Website homepage](images/website-homepage.png)
```

---

## Final Verification

- ✅ `http://104.46.221.42` — Apache serving the site
- ⬜ `https://futebol.ddns.net` — *(update once DNS/SSL confirmed)*

---

## Scripting Component

*(Describe your custom script here: what it does, why it's useful, and what you modified from any reference source)*

```markdown
![Script output](images/script-output.png)
```

---

## Development Timeline

- **18 June 2026** — VM created on Azure
- **19 June 2026** — Initial system update and SSH access confirmed
- **18 July 2026** — Apache2 installed and verified running
- **20 July 2026** — System re-verified, documentation started

---

**Project by Agahozo Sabrine – 35764065**
**Azure IaaS | Ubuntu 22.04 | Apache2 | Dynamic DNS | SSH-based deployment**

## License

*(Add license info here, e.g. MIT — see LICENSE file)*
