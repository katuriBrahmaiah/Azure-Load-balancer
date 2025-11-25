# Azure Load Balancer Setup

This repository contains the basic configuration and explanation of how I created an Azure Load Balancer and connected backend virtual machines for load distribution.

## 📌 Overview
The Azure Load Balancer is used to distribute incoming traffic between multiple virtual machines (VMs) to ensure high availability and reliability.

## 🏗 Architecture
- 1 Load Balancer (Public)
- Backend Pool with 3 Virtual Machines
- Health Probe (HTTP/Port 80)
- Load Balancing Rule (Port 80 → Backend VMs)
- Inbound NAT Rules (Optional)

## ⚙️ Configuration Steps I Followed
1. Created a **Public IP** for the LB  
2. Created an **Azure Load Balancer**
3. Added **Backend Pool** and attached VMs  
4. Configured **Health Probe**  
5. Created **Load Balancing Rule**  
6. Tested the load distribution using browser requests

## 🧪 Testing
- Hosted a simple webpage on both VMs  
- Accessed the LB Public IP  
- Requests were served by both VMs (round-robin)


## 🎯 Purpose of This Repository
This repo is mainly for:
- Showcasing my Azure hands-on work  
- Learning documentation practices  
- Keeping track of my cloud projects  


