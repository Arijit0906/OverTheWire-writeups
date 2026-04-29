
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
