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

# Question 2: DevOps Automation Script

## 🎯 Objective
Write a shell script that uses the directory structure created in **Question 1** to automate tasks such as logging, configuration reading, and script execution.

---

## 🛠 Steps

1. **Create a new script file**
   ```bash
   sudo nano /home/ec2-user/webapp/scripts/run.sh

<img width="375" height="25" alt="image" src="https://github.com/user-attachments/assets/632da32a-fbf7-4b79-a96c-1cebd0be5453" />

2. #!/bin/bash

# Load configuration
source /home/ec2-user/webapp/config/app.conf

# Log start
echo "$(date): Starting $APP_NAME on port $PORT" >> /home/ec2-user/webapp/logs/app.log

# Example action (replace with real app logic)
echo "Running $APP_NAME..."
sleep 2

# Log completion
echo "$(date): $APP_NAME stopped" >> /home/ec2-user/webapp/logs/app.log


<img width="424" height="161" alt="image" src="https://github.com/user-attachments/assets/ce65d181-9729-44b1-a841-9f7a3b5b1a95" />

Make script executable

chmod +x /home/ec2-user/webapp/scripts/run.sh


<img width="404" height="35" alt="image" src="https://github.com/user-attachments/assets/020bbbc0-9147-4e2f-9e41-a03565b45969" />


Run the script

/home/ec2-user/webapp/scripts/run.sh

<img width="389" height="218" alt="image" src="https://github.com/user-attachments/assets/b2954637-2768-477e-8b9b-e80002023784" />

Check logs

cat /home/ec2-user/webapp/logs/app.log


<img width="515" height="53" alt="image" src="https://github.com/user-attachments/assets/ae6e821c-fb6a-49bc-81d9-0f3658443f4b" />





