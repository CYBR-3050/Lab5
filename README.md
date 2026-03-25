# **CYBR 3050 – Lab: Network Threats – Interception, Recon, and Defense**

---

## **Background**

Modern network security revolves around understanding how attackers **observe**, **enumerate**, and **interact** with systems.

In this lab, you will explore three key concepts:

* **Interception** – observing data in transit (confidentiality risk) 
* **Reconnaissance** – identifying exposed services (attack surface) 
* **Defense** – restricting access and reducing exposure 

Rather than following strict instructions, this lab encourages exploration. You are expected to investigate, interpret, and explain what you observe.

---

## **Learning Objectives**

By the end of this lab, you should be able to:

* Analyze network traffic to identify sensitive data exposure
* Perform basic reconnaissance using port scanning tools
* Evaluate system exposure and associated risks
* Implement simple defensive controls
* Connect offensive and defensive perspectives

---

# **Task 1 – Interception: Analyzing Network Traffic**

## Step 1: Obtain a Sample PCAP

1. Navigate to:

   ```
   https://www.wireshark.org/resources#sample-captures
   ```

2. Download a capture file containing web traffic
   *(Recommended: HTTP or mixed traffic captures such as “dns.cap” or similar)*

3. Open the file in **Wireshark**

```bash
wireshark
```

---

## Step 2: Explore the Traffic

Use Wireshark filters and tools to investigate:

* HTTP traffic
* DNS queries
* TCP streams

Suggested filters:

```
http
dns
tcp
```

---

## Step 3: Identify Sensitive Information

Look for:

* Credentials (usernames/passwords)
* URLs and parameters
* Session identifiers
* Any readable data in plaintext

## Step 4: Run a Wireshark Scan
You will run a live scan on wireshark. Click the blue "fin" in the top-left corner of Wireshark.

* Open a web browser in Kali
* Navigate to a site you have not been to before (on that browser)
* Stop Scan

Can you find the DNS request for your site?

## Step 5: Update your DNS Server
* Open terminal.
* Edit the file: 
```bash
sudo nano /etc/resolv.conf
```
Replace or add lines:
```bash
nameserver 8.8.8.8
nameserver 1.1.1.1
```
Save and exit (Ctrl+O, Enter, Ctrl+X). 

This is a temporary change and will revert when you restart your VM.

## Step 6: Repeat the Wireshark Scan
With the new DNS servers, repeat the wireshark scan.
* Begin new wireshark scan
* Navigate to a new website
* Stop the capture

Could you find the new DNS request?

---

## Reflection Questions

* What types of data were visible in plaintext?
* Why is HTTP particularly vulnerable to interception?
* What differences did you observe between encrypted vs unencrypted traffic?
* How does this relate to **confidentiality in the CIA triad**?

---

# **Task 2 – Reconnaissance: Scanning Your System**

## Step 1: Prepare a Local Target

Start a simple web service:

```bash
python3 -m http.server 8000
```

---

## Step 2: Perform Reconnaissance

In a new terminal, scan your system:

```bash
nmap localhost
```

Then try a more detailed scan:

```bash
nmap -sV localhost
```

---

## Step 3: Analyze Results

Identify:

* Open ports
* Services running
* Version information (if available)

## Step 4: Open Telnet
Telnet is a vulnerable service and should not be used generally. It is also a great tool for accessing remote services and is often used in malicious attacks. Spotting an open telnet connection on your network may be an indication of an attack.

```bash
sudo apt update
sudo apt install telnetd inetutils-inetd
sudo update-inetd --enable telnet
```

Verify that Telnet is open:
```bash
netstat -nltp | grep 23
```

Connect via telnet (this is silly since we are connecting to ourselves from our own computer...)
```bash
telnet localhost
```
Enter your username and password. 

After you see that you are indeed in a terminal, you can exit back to the command terminal (I know - it is redundant)
```bash
exit
```

## Step 5: Scan system again
```bash
nmap localhost
```

---

## Reflection Questions

* What information does an attacker gain from a simple scan?
* Is port scanning itself a vulnerability? Why or why not? 
* Which discovered services represent the greatest risk?

---

# **Task 3 – Defense: Reducing Exposure**

## Step 1: Apply a Basic Firewall Rule

Block access to your telnet port and local server:

```bash
sudo apt install ufw -y
sudo ufw enable
sudo ufw deny 23
sudo ufw deny 8000
```

---

## Step 2: Test the Result

* Try accessing:

```
http://localhost:8000
```
* Run another scan:

```bash
nmap localhost
```

---

## Step 3: Evaluate

* Did the service become inaccessible?
* Did the scan results change?

---

## Reflection Questions

* How does blocking a port reduce attack surface?
* What are the tradeoffs between accessibility and security?
* How do firewalls implement the idea of **controlled sharing**? 

---

# **Task 4 – Synthesis: Connecting Concepts**

## Reflection Questions

* How do interception, reconnaissance, and defense relate to each other?
* Which stage (interception vs recon) provides more useful information to an attacker? Why?
* If you were defending a real system, which control would you prioritize first?
* How do these activities reflect real-world attack workflows?

---

# **Submission Requirements**

Submit **one document** containing:

* Answers to all reflection questions
* Key observations from:

  * Packet analysis
  * Nmap scan results
  * Firewall testing
* Screenshots (optional but helpful)

---

# **Final Notes**

* This lab emphasizes **process over correctness**
* Exploration, failed attempts, and reasoning are valuable
* Do not scan systems outside your own VM or authorized environment

