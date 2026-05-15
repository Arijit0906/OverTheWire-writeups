
## 🔐 Bandit Level 11 → Level 12

### 🧠 Lab Description
The password for the next level is stored in the file `data.txt`, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions.

---

### 📖 Explanation
This level introduces the ROT13 cipher, a simple letter substitution technique where each letter is rotated by 13 positions in the alphabet. For example:
- a ↔ n  
- b ↔ o  
- c ↔ p  

To decode this, we use the `tr` (translate) command in Linux, which replaces characters from one set with another.

---

### 💻 Solution / Result

#### Step 1: Connect to the server
```bash
ssh bandit11@bandit.labs.overthewire.org -p 2220
```

Password: *(use password from previous level)*

---

#### Step 2: Decode the ROT13 text
```bash
cat data.txt | tr "A-Za-z" "N-ZA-Mn-za-m"
```
<img width="701" height="143" alt="image" src="https://github.com/user-attachments/assets/b159ded6-f98c-41dc-a2a6-9d0b62c3290e" />

📌 `tr` → translates characters from one set to another  
📌 `"A-Za-z"` → all uppercase and lowercase letters  
📌 `"N-ZA-Mn-za-m"` → shifted alphabet for ROT13 decoding  

---

### 🔄 Alternative Way
```bash
tr "A-Za-z" "N-ZA-Mn-za-m" < data.txt
```

```bash
cat data.txt | rot13
```
- If `rot13` utility is available on the system

---
## 🔐 Bandit Level 12 → Level 13

### 🧠 Lab Description
The password for the next level is stored in the file `data.txt`, which is a hexdump of a file that has been repeatedly compressed.

---

### 📖 Explanation
This level introduces working with encoded and repeatedly compressed files. The given file is a hexdump, so it must first be converted back to binary. Then, multiple layers of compression (gzip, bzip2, tar) need to be identified and extracted step-by-step using appropriate tools.

---
<img width="989" height="665" alt="image" src="https://github.com/user-attachments/assets/78c9153d-0c67-4e58-b785-282964ffeb12" />

### 💻 Solution / Result

#### Step 1: Create a temporary working directory
```bash
mktemp -d
cd /tmp/<generated-folder>
```

#### Step 2: Copy the file
```bash
cp ~/data.txt .
```

#### Step 3: Convert hexdump to binary
```bash
xxd -r data.txt > file
```

📌 `xxd -r` → converts hexdump back to original binary format  

---

#### Step 4: Identify file type
```bash
file file
```

📌 `file` → detects file type (gzip, bzip2, tar, etc.)

---

#### Step 5: Repeatedly decompress/extract

```bash
mv file file.gz
gunzip file.gz
```

```bash
mv file file.bz2
bunzip2 file.bz2
```

```bash
mv file file.gz
gunzip file.gz
```

```bash
tar -xf file
tar -xf data5.bin
```

```bash
mv data6.bin data6.bz2
bunzip2 data6.bz2
```

```bash
tar -xf data6
```

```bash
mv data8.bin data8.gz
gunzip data8.gz
```

---

#### Step 6: Read the final file
```bash
cat data8
```
<img width="1135" height="812" alt="image" src="https://github.com/user-attachments/assets/844e5687-92ef-4e47-a456-e9be44bc6be6" />
<img width="1320" height="86" alt="image" src="https://github.com/user-attachments/assets/a55ff358-8ebd-41fd-bfeb-e6c7e38a853c" />
<img width="1278" height="178" alt="image" src="https://github.com/user-attachments/assets/2779eb53-18a1-47c1-85a2-3a462e0ec27d" />

📌 Final output reveals the password.

---

### 🧾 Key Commands Learned
- `mktemp -d` → creates a secure temporary directory  
- `cp` → copies files  
- `xxd -r` → converts hexdump to binary  
- `file` → identifies file type  
- `mv` → renames files  
- `gunzip` → decompresses `.gz` files  
- `bunzip2` → decompresses `.bz2` files  
- `tar -xf` → extracts archive files  
- `cat` → displays file content  

---

### 🔄 Alternative Way
Instead of manually renaming each file, you can repeatedly run:
```bash
file <filename>
```
and apply the correct extraction command based on the detected type.

---

### ✅ Result
Successfully extracted multiple layers of compression to retrieve the final password.


## 🔐 Bandit Level 13 → Level 14

### 🧠 Lab Description
The password for the next level is stored in `/etc/bandit_pass/bandit14` and can only be read by user `bandit14`. Instead of a password, a private SSH key is provided for authentication.

---

### 📖 Explanation
This level introduces SSH key-based authentication. Instead of using a password, we use a private key (`sshkey.private`) to log in as another user. Initially, I tried using the key directly inside the current Bandit session, but the error messages made it clear that localhost login restrictions were preventing it. After understanding the issue, I transferred the key to my local machine and used it from there.

This level also helped me understand why proper file permissions are important for SSH keys.

---

### 💻 Solution / Result

#### Step 1: Connect to the server
```bash
ssh bandit13@bandit.labs.overthewire.org -p 2220
```

Password:
```bash
FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn
```

---

#### Step 2: Copy the private key to local machine
```bash
scp -P 2220 bandit13@bandit.labs.overthewire.org:sshkey.private .
```

📌 `scp` (Secure Copy Protocol) is used to securely transfer files between systems.

---

#### Step 3: Set correct permissions
```bash
chmod 600 sshkey.private
```

📌 `chmod 600` allows only the owner to read/write the file.  
SSH rejects private keys with open permissions for security reasons.
<img width="1794" height="201" alt="image" src="https://github.com/user-attachments/assets/e3ee46c6-d61a-441a-8f12-a279384c81c7" />


---

#### Step 4: Login using the SSH key
```bash
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```
<img width="1720" height="577" alt="image" src="https://github.com/user-attachments/assets/e053e5c9-dcd7-4f51-9c65-b2c4a2773c10" />

📌 `-i` specifies the identity file (private key) used for authentication.

---

#### Step 5: Read the password
```bash
cat /etc/bandit_pass/bandit14
```
<img width="892" height="156" alt="image" src="https://github.com/user-attachments/assets/b1ebd36f-07f6-4258-baa8-7a2d57b57a53" />

---

### 🧾 Key Commands Learned
- `scp` → securely transfers files between systems  
- `chmod 600` → restricts file permissions for security  
- `ssh -i` → logs in using a private SSH key  
- `cat` → reads file contents  

---

### 🔄 Alternative Way
```bash
sftp -P 2220 bandit13@bandit.labs.overthewire.org
```
- Can also be used to transfer the private key file securely.

---

### ✅ Result
Successfully used SSH key-based authentication to log in as `bandit14` and retrieve the password for the next level.
---
## 🔐 Bandit Level 14 → Level 15

### 🧠 Lab Description
The password for the next level can be retrieved by submitting the password of the current level to port `30000` on `localhost`.

---

### 📖 Explanation
This level introduces basic network communication using tools like `nc` (Netcat) and `telnet`. The task is to connect to a service running locally on port `30000` and send the current level’s password to receive the next one.

As mentioned in the Bandit introduction, passwords are stored in `/etc/bandit_pass/`, but each file is only readable by its respective user.

---

### 💻 Solution / Result

#### Step 1: Connect to the server
```bash
ssh bandit14@bandit.labs.overthewire.org -p 2220
```

Password:
```bash
MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS
```

---

#### Step 2: Read the current password
```bash
cd /etc/bandit_pass/
cat bandit14
```
<img width="1479" height="354" alt="image" src="https://github.com/user-attachments/assets/7cd7aed1-9943-4f34-96da-08e10151ec8b" />

---

#### Step 3: Send the password to port 30000
```bash
echo "MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS" | nc localhost 30000
```
<img width="464" height="205" alt="{576142CB-8D15-421C-9D82-7FFF9D5CEC08}" src="https://github.com/user-attachments/assets/80d893fc-910b-4710-91ed-8760a584f67c" />

📌 `nc` (Netcat) is used to connect to network services and send/receive data.  
📌 `localhost` refers to the current machine itself.

---

### 🧾 Key Commands Learned
- `nc` → connects to TCP/UDP services and transfers data  
- `echo` → prints text/output  
- `cat` → displays file content  
- `localhost` → refers to the current system  

---

### 🔄 Alternative Way
```bash
telnet localhost 30000
```

After connecting, manually paste the password.

📌 `telnet` can also establish a TCP connection to the service.

---

### ✅ Result
Successfully connected to the local service on port `30000` and retrieved the password for the next level.
---

## 🔐 Bandit Level 15 → Level 16

### 🧠 Lab Description
The password for the next level can be retrieved by submitting the password of the current level to port `30001` on `localhost` using SSL/TLS encryption.

---

### 📖 Explanation
This level introduces encrypted network communication using SSL/TLS. In the previous level, a normal TCP connection was enough, but here the service requires a secure encrypted connection. For this, `ncat` with the `--ssl` option is used.

---

### 💻 Solution / Result

#### Step 1: Connect to the server
```bash
ssh bandit15@bandit.labs.overthewire.org -p 2220
```

Password:
```bash
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

---

#### Step 2: Connect securely to port 30001
```bash
ncat --ssl localhost 30001
```
<img width="855" height="151" alt="image" src="https://github.com/user-attachments/assets/fd9dcc8b-97d4-427e-a813-6547169b6b76" />

📌 `ncat` is an improved version of Netcat used for network communication.  
📌 `--ssl` enables SSL/TLS encryption for secure communication.  
📌 `localhost` refers to the current machine.

---

#### Step 3: Submit the password
After connecting, paste the current level password:
```bash
8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo
```

📌 The server responds with the password for the next level.

---

### 🧾 Key Commands Learned
- `ncat` → advanced networking utility for TCP/UDP communication  
- `--ssl` → enables encrypted SSL/TLS communication  
- `ssh` → securely connects to remote systems  

---

### 🔄 Alternative Way
```bash
openssl s_client -connect localhost:30001
```

📌 `openssl s_client` can also establish SSL/TLS connections manually.

---

### ✅ Result
Successfully established an SSL/TLS encrypted connection to port `30001` and retrieved the password for the next level.
