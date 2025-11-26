# Azure Load Balancer Setup (Public LB)

This project explains how I created an Azure Load Balancer with three backend virtual machines.  
I documented every step I followed, including the challenges I faced and how I solved them, so anyone can easily understand and reproduce the setup.

---

## 📌 Overview of the Setup

- **Load Balancer:** lb  
- **Backend Pool:** lb-backend  
- **Backend VMs:** vm1, vm2, vm3  
- **Health Probe:** lb-health  
- **Load Balancing Rule:** lb-rule  
- **Web File:** index.html (must be in each VM)

The Load Balancer distributes incoming web traffic (port 80) across all three VMs.

---

## Architecture

Client → Public IP → Azure Load Balancer (lb)
│
├── vm1 (in lb-backend)
├── vm2 (in lb-backend)
└── vm3 (in lb-backend)



---

# Step-by-Step Configuration

## 1️⃣ Create the Virtual Machines
- Create **3 or more VMs**
- All VMs **must be in the same region**
- All VMs **must be in the same Virtual Network (VNet)**
- Enable **RDP (port 3389)** while creating VMs  
  (needed to log in and host the webpage)

---

## 2️⃣ Configure NSG Rules (Very Important)
Go to each VM → Networking → Inbound rules:

### Add:
- **HTTP (Port 80)** → Allow  
- **HTTPS (Port 443)** → Allow  
- **RDP (Port 3389)** → Allow (already created during VM creation)

This allows web traffic to reach your VM.

---

## 3️⃣ Check Windows Firewall Inside VM
Log in to each VM → open:

**Control Panel → Windows Defender Firewall → Advanced Settings → Inbound Rules**

Make sure:
- **World Wide Web Services (HTTP – Port 80)** is Enabled  
- **World Wide Web Services (HTTPS – Port 443)** is Enabled (if needed)

I personally faced issues because this rule was disabled even though NSG was open.

---

## 4️⃣ Host a Webpage on Each VM
Inside each VM create:

**C:\inetpub\wwwroot\index.html**

This file **must be named index.html** for the web server to work by default.
**remove the extra extensions** to the html file by clicking view option on top bar.

Each VM can have a different message, for example:
- vm1 → “Hello from VM1”
- vm2 → “Hello from VM2”
- vm3 → “Hello from VM3”

This helps in testing load balancing.

---

## 5️⃣ Create the Public Load Balancer

### **Step 5.1: Create Public IP**
- Public IP Allocation: **Static only (mandatory)**  
  (Dynamic IP is not recommended for LB)

### **Step 5.2: Create the Load Balancer**
- Select the Public IP
- Type: **Public Load Balancer**
- SKU: **Standard**

---

## 6️⃣ Configure Backend Pool
- Open Load Balancer → Backend Pools  
- Choose **lb-backend**
- Add the 3 VMs (vm1, vm2, vm3)
- Select NIC level association (recommended)

---

## 7️⃣ Create Health Probe
Create a probe to check VM health:

- **Name:** lb-health  
- **Protocol:** HTTP  
- **Port:** 80  
- **Interval:** 5 seconds  
- **Unhealthy threshold:** 2

If a VM fails the probe, LB stops sending traffic.

---

## 8️⃣ Create Load Balancer Rule
Create rule for web traffic:

- **Name:** lb-rule  
- **Frontend port:** 80  
- **Backend port:** 80  
- **Backend pool:** lb-backend  
- **Health probe:** lb-health  
- **Session persistence:** None  
- **Idle timeout:** Default

This rule forwards web traffic to VMs.

---

# HTTPS (Port 443) Note — Important
If you want the LB to serve **HTTPS traffic**, you must install a valid **SSL certificate**.

Even if:
- NSG allows port 443  
- Firewall allows port 443  

You **cannot** open HTTPS without an SSL certificate.  
I personally tested this — HTTPS did not work unless certificate was installed.

---

# Testing the Load Balancer

1. Copy the **Public IP** of the LB  
2. Open browser → paste the IP  
3. Refresh multiple times  
4. You should see:
   - “Hello from VM1”
   - “Hello from VM2”
   - “Hello from VM3”

This confirms load balancing is working.

---
