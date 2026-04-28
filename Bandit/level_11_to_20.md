
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
