# Linux_assignment

# Question 1: Set Up Your DevOps Project Structure

## 🎯 Objective
Create a complete project directory from scratch, apply correct permissions, and set ownership.  
The structure built here will be used directly by the script in **Question 2**.

## 🛠 Steps

**1. Create directories in one command**

**Command :**

mkdir -p /home/ec2-user/webapp/scripts /home/ec2-user/webapp/logs /home/ec2-user/webapp/config

<img width="682" height="88" alt="image" src="https://github.com/user-attachments/assets/cfcdbdf3-5263-4280-bb07-d0f7f7bf3ec1" />

**2. Create app.conf with content**

**Command :**

cat > /home/ec2-user/webapp/config/app.conf

APP_NAME=WebApp

PORT=8080

Saved with Ctrl+D

<img width="496" height="107" alt="image" src="https://github.com/user-attachments/assets/10973725-ccd2-44f6-ad92-eaa908e05c40" />

**3. Create empty log file and Confirm size is 0**

**Command :**

touch /home/ec2-user/webapp/logs/app.log

ls -l /home/ec2-user/webapp/logs/app.log

<img width="357" height="76" alt="image" src="https://github.com/user-attachments/assets/50e03a39-aafa-4cd6-ad5f-d53cd66f28a1" />

**4. Set permissions**

**Command :**
chmod 755 /home/ec2-user/webapp/scripts && chmod 644 /home/ec2-user/webapp/config/app.conf


**755 (scripts/):**

Owner: read, write, execute

Group: read, execute

Others: read, execute

→ Owner can fully manage scripts; others can run but not modify.

**644 (app.conf):**

Owner: read, write

Group: read

Others: read

→ Only owner can edit config; everyone can read. No Execute permission given to anyone.

<img width="664" height="160" alt="image" src="https://github.com/user-attachments/assets/fd2d144f-d161-47a7-9197-fab72627e6c2" />

**5 and 6. Change ownership recursively and Verify structure**

**Command :**

sudo chown -R root:root /home/ec2-user/webapp/

ls -lR /home/ec2-user/webapp/

<img width="443" height="229" alt="image" src="https://github.com/user-attachments/assets/8bfd6563-374a-46dc-a19c-ea7ee1214244" />

# Question 2: Write an Interactive Log Script

## 🎯 Objective
Using the `webapp/` structure from Question 1, create a bash script that:
- Prompts the user for input
- Reads a configuration file
- Writes timestamped log entries  

The log entries generated here will be used as input for **Question 3**.

## 🛠 Steps

1. **Navigate into the scripts directory**

**Command :**

   cd /home/ec2-user/webapp/scripts/
   vim log_user.sh
   
<img width="375" height="25" alt="image" src="https://github.com/user-attachments/assets/632da32a-fbf7-4b79-a96c-1cebd0be5453" />

**2. Press i to enter insert mode and add the following content:**

#!/bin/bash

**# Prompt user for name**

read -p "Enter your name: " username

**# Display config file**

cat /home/ec2-user/webapp/config/app.conf

**# Append log entry with timestamp**

echo "Login: $username Date: $(date)" >> /home/ec2-user/webapp/logs/app.log

**# Show full log file**

cat /home/ec2-user/webapp/logs/app.log

Press Esc, then type :wq to save and quit

<img width="424" height="161" alt="image" src="https://github.com/user-attachments/assets/ce65d181-9729-44b1-a841-9f7a3b5b1a95" />

**3.Give execute permission**

**Command :**

chmod +x log_user.sh

<img width="404" height="35" alt="image" src="https://github.com/user-attachments/assets/020bbbc0-9147-4e2f-9e41-a03565b45969" />

**4.Run the script three times with different names**

**Command :**

./log_user.sh

<img width="389" height="218" alt="image" src="https://github.com/user-attachments/assets/b2954637-2768-477e-8b9b-e80002023784" />

**5.Check the log file**

**Command :**

cat /home/ec2-user/webapp/logs/app.log

<img width="515" height="53" alt="image" src="https://github.com/user-attachments/assets/ae6e821c-fb6a-49bc-81d9-0f3658443f4b" />

# Question 3: User Management and File Permission Control

## 🎯 Objective
Create 4 Linux users.  
- **devuser1** and **devuser2** → write access to `log_user.sh`  
- **devuser3** and **devuser4** → read-only access  

Use Linux groups and file permissions (`chmod`) to enforce this.

---

## 🛠 Steps

1. **Create the writers group**

**Command :**

sudo groupadd writers

sudo useradd -m devuser1

sudo useradd -m devuser2

sudo useradd -m devuser3

sudo useradd -m devuser4

<img width="433" height="65" alt="image" src="https://github.com/user-attachments/assets/4bf7a6be-cf0f-4b88-b4c6-193f6960382c" />

**2. Add devuser1 and devuser2 to writers group and Change group ownership of the script log_user.sh from root to writers**

**Command :**

sudo usermod -aG writers devuser1 && sudo usermod -aG writers devuser2

sudo chown root:writers /home/ec2-user/webapp/scripts/log_user.sh

<img width="680" height="26" alt="image" src="https://github.com/user-attachments/assets/567aefff-fa81-4171-965b-eb8ea97e5d6d" />

**3. Set permissions of the script log_user.sh to 664 and verify the permission set for log_user.sh**

**Command :**

sudo chmod 664 /home/ec2-user/webapp/scripts/log_user.sh

ls -l /home/ec2-user/webapp/scripts/log_user.sh

Owner (root) → read & write

Group (writers(devuser1, devuser2)) → read & write

Others (devuser3, devuser4) → read only


<img width="601" height="40" alt="image" src="https://github.com/user-attachments/assets/6799b347-68d5-4814-b18d-f36b34935272" />

✅ Test Access

**4. Write access (devuser1/devuser2):**

**Command :**

su - devuser1

echo "# devuser1 test" >> /home/ec2-user/webapp/scripts/log_user.sh



<img width="479" height="295" alt="image" src="https://github.com/user-attachments/assets/ea7ed070-fff8-4b86-a9a6-ca9a245230a0" />

<img width="428" height="202" alt="image" src="https://github.com/user-attachments/assets/29335dd3-25d3-4b32-b531-eea2c9c617e4" />


**5. Read-only access (devuser3/devuser4):**

**Command :**

su - devuser3

cat /home/ec2-user/webapp/scripts/log_user.sh

<img width="487" height="266" alt="image" src="https://github.com/user-attachments/assets/06085d1c-098d-4ad9-b535-c610c3faf61d" />

<img width="449" height="243" alt="image" src="https://github.com/user-attachments/assets/4b7dfca0-2c5c-43b1-a706-c3c129c61818" />

**(Permission denied since only read access provided for devuser3 and devuser4)**

echo "# devuser3 test" >> /home/ec2-user/webapp/scripts/log_user.sh

<img width="461" height="39" alt="image" src="https://github.com/user-attachments/assets/b2b40239-bc81-40f2-ada7-55280905620c" />










