## 🔐 Bandit Level 0 → Level 1

### 🧠 Lab Description
The password for the next level is stored in a file called `readme` located in the home directory.

---

### 📖 Explanation
This level introduces basic SSH login and simple file reading in Linux. The objective is to connect to the remote server and retrieve the password from a file.

---

### 💻 Solution / Result

#### Step 1: Connect to the server
```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

Password: `bandit0`
<img width="627" height="458" alt="{F79743BE-5EB6-492D-BDD5-DE7A2717F72B}" src="https://github.com/user-attachments/assets/40d29863-d6b4-4021-82cd-fde9f654edef" />

---

#### Step 2: List files
```bash
ls
```

---

#### Step 3: Read the file
```bash
cat readme
```
<img width="683" height="179" alt="{38C53A05-4817-4DC7-BE1E-DB832C61BC44}" src="https://github.com/user-attachments/assets/5926d8d7-6f67-4f77-998a-6388d6213a24" />



📌 The `cat` command displays the file content, revealing the password for the next level.

---
### 🔄 Alternative Way
- Use `nano readme` to open the file  
- Use `vi readme` to view the file content
---

## 🔐 Bandit Level 1 → Level 2

### 🧠 Lab Description
The password for the next level is stored in a file called `-` located in the home directory.

---

### 📖 Explanation
This level focuses on handling special filenames in Linux. The filename `-` is treated as a special character (standard input/output), so it cannot be accessed directly like normal files. We need to use proper syntax to read it.

---

### 💻 Solution / Result

#### Step 1: Connect to the server
```bash
ssh bandit1@bandit.labs.overthewire.org -p 2220
```

Password: *(use password from previous level)*

---

#### Step 2: List files
```bash
ls
```

---

#### Step 3: Read the file named `-`
```bash
cat ./-
OR
cat /home/bandit1/-
```
<img width="708" height="102" alt="{8E623575-3E02-490A-854D-26D557467DE4}" src="https://github.com/user-attachments/assets/557efd33-475e-463d-8608-783605c18160" />

📌 Using `./-` tells the system to treat `-` as a filename in the current directory instead of a special symbol.

---

### 🔄 Alternative Way
```bash
cat < -
```
- Uses input redirection to read the file

```bash
less ./-
```
- Opens the file in a pager for viewing
---
## 🔐 Bandit Level 2 → Level 3

### 🧠 Lab Description
The password for the next level is stored in a file called `--spaces in this filename--` located in the home directory.

---

### 📖 Explanation
This level deals with filenames containing spaces and special characters like `--`. Such filenames cannot be accessed normally, so we must use proper quoting and syntax to read them correctly.

---

### 💻 Solution / Result

#### Step 1: Connect to the server
```bash
ssh bandit2@bandit.labs.overthewire.org -p 2220
```

Password: *(use password from previous level)*

---

#### Step 2: Read the file
```bash
cat -- "--spaces in this filename--"
```
<img width="698" height="73" alt="{DC340A70-953B-48D1-8D9A-03C4888A6690}" src="https://github.com/user-attachments/assets/fa35fd2c-ab52-4ed2-858d-f771cd160ba0" />

📌 The `--` is a special separator in Linux commands.  
It indicates the end of command options, so anything after it is treated strictly as a filename—even if it starts with `-` or `--`.

---

### 🔄 Alternative Way
```bash
cat "./--spaces in this filename--"
```

```bash
cat --spaces\ in\ this\ filename--
```
- Uses escape characters (`\`) to handle spaces in the filename
---
## 🔐 Bandit Level 3 → Level 4

### 🧠 Lab Description
The password for the next level is stored in a hidden file in the `inhere` directory.

---

### 📖 Explanation
This level introduces hidden files in Linux. Files that start with `.` are hidden and won’t appear with a normal `ls`. Using `ls -la` shows all files, including hidden ones.

---

### 💻 Solution / Result

#### Step 1: Connect to the server
```bash
ssh bandit3@bandit.labs.overthewire.org -p 2220
```

Password: *(use password from previous level)*

---

#### Step 2: Navigate to the directory
```bash
cd inhere
```

---

#### Step 3: List hidden files
```bash
ls -la
```
<img width="703" height="208" alt="{69D9ABF1-AF96-4A1E-AB33-90E3381ED484}" src="https://github.com/user-attachments/assets/4f12b929-37cd-41cc-a332-23657f1e696d" />

---

#### Step 4: Read the hidden file
```bash
cat .hidden
```

📌 `ls -la` shows all files (including hidden ones starting with `.`), allowing us to find and read the hidden file.

---

### 🔄 Alternative Way
```bash
ls -a
```
- Lists hidden files without detailed information

```bash
cat ./inhere/.hidden
```
- Reads the file using full path
---
## 🔐 Bandit Level 4 → Level 5

### 🧠 Lab Description
The password for the next level is stored in the only human-readable file in the `inhere` directory.

---

### 📖 Explanation
This level introduces the concept of identifying human-readable files among many files. Since multiple files are present, manually checking each one would be time-consuming. Instead, we can use pattern matching and commands to quickly filter and read likely files.

---

### 💻 Solution / Result

#### Step 1: Connect to the server
```bash
ssh bandit4@bandit.labs.overthewire.org -p 2220
```

Password: *(use password from previous level)*

---

#### Step 2: Navigate to the directory
```bash
cd inhere
```

---

#### Step 3: Read files efficiently
```bash
cat /home/bandit4/inhere/-file0*
```
<img width="702" height="228" alt="{C8A1B11F-4447-4D77-A77E-C6494733023A}" src="https://github.com/user-attachments/assets/e8ce492e-bbea-4467-9f0a-55ee70c11c8e" />

📌 Instead of checking each file manually, wildcard `*` helps match multiple files at once. This makes it easier to quickly locate the file containing readable text (the password).

---

### 🔄 Alternative Way
```bash
file ./*
```
- Identifies file types and helps find the human-readable file

```bash
strings ./*
```
- Extracts readable strings from files
---

## 🔐 Bandit Level 5 → Level 6

### 🧠 Lab Description
The password for the next level is stored in a file somewhere under the `inhere` directory with the following properties:
- human-readable  
- 1033 bytes in size  
- not executable  

---

### 📖 Explanation
This level focuses on filtering files based on specific properties. Instead of checking each file manually, we can use the `find` command to search for files matching the given conditions like size and type, which makes the process efficient.

---

### 💻 Solution / Result

#### Step 1: Connect to the server
```bash
ssh bandit5@bandit.labs.overthewire.org -p 2220
```

Password: *(use password from previous level)*

---

#### Step 2: Navigate to the directory
```bash
cd inhere
```

---

#### Step 3: Find the file using conditions
```bash
find . -type f -size 1033c
```

📌 This command searches for files (`-type f`) with size exactly `1033 bytes`.

---

#### Step 4: Read the file
```bash
cat ./maybehere07/.file2
```
<img width="697" height="246" alt="{440A3EDD-9BEF-4289-96F1-98986E58A77B}" src="https://github.com/user-attachments/assets/9e44509d-d132-49e4-9ff2-efbc4d6d44c9" />

📌 This file matches all conditions and contains the password.

---

### 🔄 Alternative Way
```bash
find . -type f -size 1033c -exec file {} \;
```
- Helps verify if the file is human-readable

```bash
ls -lR | grep 1033
```
## 🔐 Bandit Level 6 → Level 7

### 🧠 Lab Description
The password for the next level is stored somewhere on the server and has the following properties:
- owned by user `bandit7`  
- owned by group `bandit6`  
- 33 bytes in size  

---

### 📖 Explanation
This level introduces searching across the entire system using ownership and size filters. Since the file is not in a specific directory, we use the `find` command with user, group, and size conditions to locate it efficiently.

---

### 💻 Solution / Result

#### Step 1: Connect to the server
```bash
ssh bandit6@bandit.labs.overthewire.org -p 2220
```

Password: *(use password from previous level)*

---

#### Step 2: Search the file across the system
```bash
find / -user bandit7 -group bandit6 -type f -size 33c 2>/dev/null
```

📌 This command searches the entire system (`/`) for files matching:
- specific user and group  
- exact size (33 bytes)  
- `2>/dev/null` hides permission denied errors  

---

#### Step 3: Read the file
```bash
cat /var/lib/dpkg/info/bandit7.password
```

📌 This file contains the password for the next level.

---

### 🔄 Alternative Way
```bash
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
```
<img width="711" height="160" alt="{1FA967C3-3110-4A0D-8FBC-0CA99E7F17B1}" src="https://github.com/user-attachments/assets/2e5851ed-9b32-4264-9aea-fd6729fb1e04" />
<img width="416" height="70" alt="{A3143C3D-6DE7-4386-AC99-656EBC2C0E99}" src="https://github.com/user-attachments/assets/5cd1e1e6-7e40-4b4c-a139-cb4912b33b64" />
<img width="710" height="37" alt="{78584F0E-D3DC-4C23-B5D7-FC46A7465B07}" src="https://github.com/user-attachments/assets/abed2bf2-36d8-4fe4-8ef6-c171d13eb660" />

```bash
ls -lR / 2>/dev/null | grep bandit7
```
- Less efficient but can help identify files by ownership
- Another way to locate files by size (less efficient)
---
## 🔐 Bandit Level 7 → Level 8

### 🧠 Lab Description
The password for the next level is stored in the file `data.txt` next to the word `millionth`.

---

### 📖 Explanation
This level introduces searching within large files. Since `data.txt` is very large, manually reading it is inefficient. Instead, we use text-processing tools like `grep` to quickly find specific words and extract the required information.

---

### 💻 Solution / Result

#### Step 1: Connect to the server
```bash
ssh bandit7@bandit.labs.overthewire.org -p 2220
```

Password: *(use password from previous level)*

---

#### Step 2: Search for the keyword in the file
```bash
cat data.txt | grep "millionth"
```
<img width="713" height="101" alt="{637BD9A7-600A-43B5-8271-2CE57135CE7E}" src="https://github.com/user-attachments/assets/ebd991d1-5a74-437f-9dd0-bfac380db744" />

📌 The `grep` command searches for the word `millionth` inside `data.txt` and returns the line containing the password.

---

### 🔄 Alternative Way
```bash
grep "millionth" data.txt
```
- Directly searches without using `cat` (more efficient)

```bash
less data.txt
```
- Allows manual searching using `/millionth` inside the pager
---
## 🔐 Bandit Level 8 → Level 9

### 🧠 Lab Description
The password for the next level is stored in the file `data.txt` and is the only line of text that occurs exactly once.

---

### 📖 Explanation
This level focuses on identifying unique lines in a file. Since `uniq` only works on adjacent duplicate lines, we first sort the file so that duplicate entries are grouped together. Then, we use `uniq -u` to extract the line that appears only once.

---

### 💻 Solution / Result

#### Step 1: Connect to the server
```bash
ssh bandit8@bandit.labs.overthewire.org -p 2220
```

Password: *(use password from previous level)*

---

#### Step 2: Find the unique line
```bash
sort data.txt | uniq -u
```

📌 `sort` → sorts all lines alphabetically so duplicates come together  
📌 `uniq` → removes duplicate adjacent lines  
📌 `uniq -u` → prints only lines that appear exactly once  
<img width="694" height="90" alt="{DE936671-D624-4E69-8711-801D9C524F75}" src="https://github.com/user-attachments/assets/20040456-ff31-4550-9220-68a48f7027ca" />

---

### 🔄 Alternative Way
```bash
sort data.txt | uniq -c | grep "^ *1 "
```
- Counts occurrences and filters lines that appear only once

```bash
awk '!seen[$0]++ && count[$0]==0 {count[$0]++} END {for (line in count) if (count[line]==1) print line}' data.txt
```
- Advanced method using `awk` to find unique lines
---
## 🔐 Bandit Level 9 → Level 10

### 🧠 Lab Description
The password for the next level is stored in the file `data.txt` in one of the few human-readable strings, preceded by several `=` characters.

---

### 📖 Explanation
This level introduces extracting readable text from binary data. The file contains mostly non-readable content, so we use the `strings` command to extract human-readable parts. Then, we filter the relevant line using `grep`.

---

### 💻 Solution / Result

#### Step 1: Connect to the server
```bash
ssh bandit9@bandit.labs.overthewire.org -p 2220
```

Password: *(use password from previous level)*

---

#### Step 2: Extract readable strings and filter
```bash
cat data.txt | strings | grep "="
```
<img width="695" height="346" alt="{8904C28B-0956-4A6C-95D3-0278115AE540}" src="https://github.com/user-attachments/assets/8425e077-277d-4817-a106-ccade429af51" />

📌 `strings` → extracts human-readable text from binary files  
📌 `grep "="` → filters lines containing `=` which leads to the password  

---

### 🔄 Alternative Way
```bash
strings data.txt | grep "="
```
- More efficient (avoids unnecessary use of `cat`)

```bash
strings data.txt | grep "==="
```
- Narrows down results further by matching multiple `=` characters
