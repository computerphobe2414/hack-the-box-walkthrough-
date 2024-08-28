### Hack The Box - Greenhorn Walkthrough

#### Step 1: Initial Scanning
- **Target IP:** 10.10.11.136
- **Run an Nmap scan to discover open ports:**
  ```bash
  nmap -sC -sV -oN greenhorn_nmap 10.10.11.136
  ```
- **Results show:**
  - **Port 22 (SSH):** Open.
  - **Port 80 (HTTP):** Website running on Apache.

#### Step 2: Exploring the Website
- **Visit the website on Port 80:**
  - The homepage doesn’t reveal much.
- **Use Gobuster to find hidden directories:**
  ```bash
  gobuster dir -u http://10.10.11.136 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
  ```
- **Find the `/admin` directory.**

#### Step 3: Brute-Forcing the Admin Login
- **Discover a login page in the `/admin` directory.**
- **Use Hydra to brute-force the login credentials:**
  ```bash
  hydra -l admin -P /usr/share/wordlists/rockyou.txt http-post-form "/admin/login.php:user=^USER^&pass=^PASS^:Incorrect password"
  ```
- **Successfully log in with the obtained credentials.**

#### Step 4: Exploiting the Admin Panel
- **After logging in, explore the admin panel for vulnerable features.**
- **Find an option to upload files.**
- **Try uploading a PHP reverse shell disguised as an image.**

#### Step 5: Gaining a Reverse Shell
- **Set up a listener using Netcat:**
  ```bash
  nc -lvnp 4444
  ```
- **Execute the uploaded PHP shell to connect back to your listener.**
- **Catch the reverse shell and gain access to the machine.**

#### Step 6: Privilege Escalation
- **Enumerate the system for privilege escalation opportunities:**
  - Check for any running processes or misconfigured files.
- **Find a vulnerable service running with higher privileges.**
- **Exploit the service to gain root-level access.**

#### Step 7: Capture the Flags
- **User Flag:** Located in the user’s home directory.
- **Root Flag:** Located in the root directory.
