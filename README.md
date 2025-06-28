# Setup-and-Use-a-Firewall-on-Linux
Building a firewall on Kali Linux (often referred to as "Kali Purple" when combining offensive and defensive security practices) involves several steps. Below is a step-by-step guide to set up a basic firewall using `iptables`, a powerful and flexible tool for configuring Linux kernel firewall rules.

 ### Step 1: Update Your System
First, ensure your system is up to date.

 ```bash
sudo apt update && sudo apt upgrade -y
```

 ### Step 2: Install `iptables`
`iptables` is usually pre-installed on Kali Linux, but you can install it if it's not already present.
 
 ```bash
sudo apt install iptables -y
```

 ### Step 3: Configure `iptables`
You'll need to create rules to allow or block traffic. Here’s a basic configuration:

1.  Flush Existing Rules:
   ```bash
   sudo iptables -F
   sudo iptables -X
   sudo iptables -t nat -F
   sudo iptables -t nat -X
   sudo iptables -t mangle -F
   sudo iptables -t mangle -X
   ```

2.  Set Default Policies:
   ```bash
   sudo iptables -P INPUT DROP
   sudo iptables -P FORWARD DROP
   sudo iptables -P OUTPUT ACCEPT
   ```

3.  Allow Loopback Interface:
   ```bash
   sudo iptables -A INPUT -i lo -j ACCEPT
   ```

4.  Allow Established and Related Connections:
   ```bash
   sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT
   ```

5.  Allow SSH (Port 22):
   ```bash
   sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
   ```

6.  Allow HTTP (Port 80) and HTTPS (Port 443):
   ```bash
   sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
   sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
   ```

7.  Allow ICMP (Ping):
   ```bash
   sudo iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT
   ```

8.  Log Dropped Packets:
   ```bash
   sudo iptables -A INPUT -j LOG --log-prefix "IPTables-Input-Dropped: " --log-level 4
   sudo iptables -A FORWARD -j LOG --log-prefix "IPTables-Forward-Dropped: " --log-level 4
   ```

 ### Step 4: Save `iptables` Rules
To ensure your rules persist across reboots, you need to save them.

1.  Install `iptables-persistent`:
   ```bash
   sudo apt install iptables-persistent -y
   ```

2.  Save Current Rules:
   ```bash
   sudo netfilter-persistent save
   ```

 ### Step 5: Verify Rules
You can verify your rules with the following command:

```bash
sudo iptables -L -v
```

 ### Step 6: Test Your Firewall
Ensure that your firewall is working as expected by testing connectivity to allowed and blocked ports.

  Additional Tools
For more advanced firewall management, you might consider using `ufw` (Uncomplicated Firewall), which is a front-end for `iptables`.

1.  Install `ufw`:
   ```bash
   sudo apt install ufw -y
   ```

2.  Enable `ufw`:
   ```bash
   sudo ufw enable
   ```

3.  Allow SSH, HTTP, and HTTPS:**
   ```bash
   sudo ufw allow ssh
   sudo ufw allow http
   sudo ufw allow https
   ```

4.  Check Status:
   ```bash
   sudo ufw status
   ```
### screenshot:- status of firewall of my kali linux PC
 ![Image](https://github.com/user-attachments/assets/9f3da1e3-2c75-471e-a039-976de35408f1)
