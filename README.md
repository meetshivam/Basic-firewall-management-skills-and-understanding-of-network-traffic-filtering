# Basic-firewall-management-skills-and-understanding-of-network-traffic-filtering
This project demonstrates how to configure and test basic firewall rules using UFW (Uncomplicated Firewall) on Kali Linux. The goal is to manage inbound network traffic by allowing and blocking specific ports, and verifying the behavior using Nmap.


This README is **self-explanatory**, **well-documented**, and includes **all commands**, **outputs**, and **explanations** — just like a mini project report .

---

#  Basic Firewall Configuration using UFW on Kali Linux

##  Project Description

This project demonstrates how to configure and test basic **firewall rules** using **UFW (Uncomplicated Firewall)** on **Kali Linux**.
The goal is to manage inbound network traffic by **allowing** and **blocking** specific ports, and verifying the behavior using **Nmap**.

Things to learn:

* How to check and enable UFW
* How to list and manage firewall rules
* How to block or allow specific ports
* How to test firewall rules using `nmap`
* How to simulate open ports using `netcat`

This project is part of a cybersecurity and networking exercise designed to understand how firewalls filter traffic based on rules.

---

##  Tools & Environment

| Tool                             | Purpose                        |
| -------------------------------- | ------------------------------ |
| **Kali Linux**                   | Operating System               |
| **UFW (Uncomplicated Firewall)** | Firewall configuration         |
| **Nmap**                         | Port scanning & testing        |
| **Netcat (nc)**                  | Simulate services (open ports) |

---

##  Objective

1. List current firewall rules
2. Block inbound traffic on a specific port (e.g., **Telnet: port 23**)
3. Allow inbound traffic on **SSH (port 22)**
4. Test the firewall rules using **Nmap**
5. Remove test rules and restore the original state
6. Summarize how firewall filters traffic

---

##  Step-by-Step Implementation

###  Step 1: Check if UFW is installed and running

```bash
sudo ufw status
```

**Output:**

```
Status: inactive
```

 UFW is installed but currently inactive.

---

###  Step 2: Enable UFW

```bash
sudo ufw enable
```

**Output:**

```
Firewall is active and enabled on system startup
```

Now UFW is **active**.

---

###  Step 3: List Current Rules

```bash
sudo ufw status numbered
```

**Output:**

```
Status: active
```

No rules yet.

---

###  Step 4: Add Rule to Block Inbound Traffic on Port 23 (Telnet)

```bash
sudo ufw deny 23
```

**Output:**

```
Rule added
Rule added (v6)
```

---

###  Step 5: Add Rule to Allow Inbound Traffic on Port 22 (SSH)

```bash
sudo ufw allow 22
```

**Output:**

```
Rule added
Rule added (v6)
```

---

###  Step 6: Verify Firewall Rules

```bash
sudo ufw status numbered
```

**Output:**

```
Status: active

     To                         Action      From
     --                         ------      ----
[ 1] 22                         ALLOW       Anywhere
[ 2] 23                         DENY        Anywhere
[ 3] 22 (v6)                    ALLOW       Anywhere (v6)
[ 4] 23 (v6)                    DENY        Anywhere (v6)
```



---

###  Step 7: Test with Nmap (Before Blocking)

Initially, **no service** is listening on port 23:

```bash
nmap -p 23 localhost
```

**Output:**

```
PORT   STATE  SERVICE
23/tcp closed telnet
```

Explanation:

* **Closed** = No service is running on this port.

---

###  Step 8: Simulate an Open Port Using Netcat

Let’s create a temporary listener on port 23:

```bash
sudo nc -l -p 23
```

Keep this terminal open.

Now, in another terminal, test again:

```bash
nmap -p 23 localhost
```

**Output:**

```
PORT   STATE SERVICE
23/tcp open  telnet
```

Explanation:

* **Open** = A service is listening (our netcat simulation).

---

###  Step 9: Apply the Firewall Rule and Test Again

Now that port 23 is open, let’s block it with UFW:

```bash
sudo ufw deny 23
```

Re-run the scan:

```bash
nmap -p 23 localhost
```

**Output:**

```
PORT   STATE     SERVICE
23/tcp filtered  telnet
```

 **Filtered** = Firewall is actively blocking the port.



---

###  Step 10: Cleanup (Remove Test Rule)

After testing, remove the block rule:

```bash
sudo ufw delete deny 23
```

**Output:**

```
Deleting:
 deny 23
Proceed with operation (y|n)? y
Rule deleted
Rule deleted (v6)
```

Verify again:

```bash
sudo ufw status numbered
```

**Output:**

```
[ 1] 22 ALLOW Anywhere
[ 2] 22 (v6) ALLOW Anywhere (v6)
```

Stop Netcat by pressing `Ctrl + C`.

---

##  Explanation: How UFW Firewall Filters Traffic

* **Firewall** is a network security system that filters **incoming** and **outgoing** packets.
* UFW simplifies **iptables** by allowing easy rule management.
* Each rule specifies:

  * **Action**: Allow / Deny
  * **Port**: e.g. 22 (SSH), 23 (Telnet)
  * **Direction**: Inbound / Outbound
* When a packet arrives, UFW checks:

  1. Is there a **matching rule**?
  2. If **allow**, traffic passes; if **deny**, traffic is dropped (Nmap shows “filtered”).

---

##  Final Results Summary

| Step | Action             | Command                   | Result   |
| ---- | ------------------ | ------------------------- | -------- |
| 1    | Check status       | `sudo ufw status`         | inactive |
| 2    | Enable UFW         | `sudo ufw enable`         | active   |
| 3    | Add allow rule     | `sudo ufw allow 22`       | ALLOW 22 |
| 4    | Add deny rule      | `sudo ufw deny 23`        | DENY 23  |
| 5    | Test (no service)  | `nmap -p 23 localhost`    | closed   |
| 6    | Simulate open port | `sudo nc -l -p 23`        | open     |
| 7    | Block port         | `sudo ufw deny 23`        | filtered |
| 8    | Cleanup            | `sudo ufw delete deny 23` | restored |

---



---

##  Project Structure

```
firewall-configuration-ufw/
├── README.md
├── commands/
│   ├── ufw-commands.txt
│   └── nmap-tests.txt
├── screenshots/
│   ├── ufw-status.png
│   ├── nmap-open.png
│   ├── nmap-filtered.png
├── report/
│   └── firewall-report.md
```

---

##  Key Learnings

* UFW is an easy tool to manage firewall rules.
* Ports can be **allowed**, **denied**, or **filtered**.
* **Nmap** helps verify if ports are **open**, **closed**, or **filtered**.
* **Firewalls** are crucial for **network security** and **access control**.

