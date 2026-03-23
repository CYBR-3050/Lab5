# **CYBR 3050 – Lab: Network Threats – Interception, Recon, and Defense**

---

## **Background**

Modern network security revolves around understanding how attackers **observe**, **enumerate**, and **interact** with systems.

In this lab, you will explore three key concepts from Chapter 6:

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
   *(Recommended: HTTP or mixed traffic captures such as “http.cap” or similar)*

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

---

## Reflection Questions

* What information does an attacker gain from a simple scan?
* Is port scanning itself a vulnerability? Why or why not? 
* Which discovered services represent the greatest risk?

---

# **Task 3 – Defense: Reducing Exposure**

## Step 1: Apply a Basic Firewall Rule

Block access to your web server port:

```bash
sudo ufw enable
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

