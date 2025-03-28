# Cisco Dynamic NAT Project

## 📌 Overview
This project demonstrates **Dynamic NAT (Network Address Translation)** in **Cisco Packet Tracer** using an **ISP Router** to connect a private network to the internet. It involves configuring a Customer Edge (CE) Router to translate private IP addresses into a pool of public IP addresses before accessing external networks.

## 🖥️ Network Topology

### **Devices Used:**
- **PC0, PC1** (End-user devices)
- **Switch** (Layer 2 switch for LAN connectivity)
- **CE Router (Customer Edge)** (2911 Router)
- **ISP Router** (2911 Router)
- **Cloud (Internet)** (Cloud-PT)

### **Connections:**
| Device       | Interface   | Connected To | Interface   |
|-------------|------------|--------------|------------|
| PC0         | Fa0        | Switch       | Fa0/1      |
| PC1         | Fa0        | Switch       | Fa0/2      |
| Switch      | Fa0/3      | CE Router    | Gig0/0     |
| CE Router   | Gig0/1     | ISP Router   | Gig0/0     |
| ISP Router  | Gig0/1     | Cloud        | Eth6       |

## ⚙️ Configuration Steps

### **Step 1: Configure PCs**
Assign **Static IP Addresses** to PC0 and PC1:
```plaintext
PC0 IP: 192.168.1.2/24
Gateway: 192.168.1.1

PC1 IP: 192.168.1.3/24
Gateway: 192.168.1.1
```

### **Step 2: Configure CE Router**
```bash
# Configure interfaces
configure terminal
interface GigabitEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown
 exit

interface GigabitEthernet0/1
 ip address 10.0.0.2 255.255.255.252
 no shutdown
 exit

# Enable NAT
ip nat pool NAT_POOL 203.0.113.100 203.0.113.110 netmask 255.255.255.240
access-list 1 permit 192.168.1.0 0.0.0.255
ip nat inside source list 1 pool NAT_POOL overload

# Define NAT Interfaces
interface GigabitEthernet0/0
 ip nat inside
 exit

interface GigabitEthernet0/1
 ip nat outside
 exit

# Set default route
ip route 0.0.0.0 0.0.0.0 10.0.0.1
exit
```

### **Step 3: Configure ISP Router**
```bash
configure terminal
interface GigabitEthernet0/0
 ip address 10.0.0.1 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet0/1
 ip address 8.8.8.1 255.255.255.0
 no shutdown
 exit

# Set default route to Cloud (Internet)
ip route 0.0.0.0 0.0.0.0 8.8.8.2
exit
```

### **Step 4: Configure Cloud (Internet)**
1. Click on **Cloud** > Go to **Config**.
2. Select **Ethernet6** > Choose **Cable**.
3. Assign **8.8.8.2/24** to the **Cloud Interface**.

## 🛠️ Testing Connectivity
1. Open the **Command Prompt** on **PC0 or PC1**.
2. **Ping the Internet (8.8.8.2)**:
   ```bash
   ping 8.8.8.2
   ```
3. If the ping is successful, NAT is working correctly!

## 🎯 Key Features
- **Dynamic NAT with Overload (PAT)** to translate multiple private IPs into a pool of public IPs.
- **ISP Router Simulation** to connect to the Internet.
- **Real-world Networking Scenario** for enterprise setups.

## 📂 Files Included
- `Cisco-Dynamic-NAT.pkt` - Cisco Packet Tracer project file.

## 📢 Contributing
Feel free to **fork** this repository, raise issues, or contribute by submitting pull requests.

## 📜 License
This project is open-source and available under the **MIT License**.

---

🚀 **Happy Networking!**
