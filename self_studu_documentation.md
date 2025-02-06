
## **🔷 Step 2: Transfer the PEM Key to the Bastion Host**
Since our servers require authentication via the PEM key (`web.pem`), you need to copy it to the Bastion Host.

1. **On Your Local Machine, Use SCP to Transfer the Key:**
   ```bash
   scp -i ~/downloads/web.pem ~/downloads/web.pem ec2-user@<BASTION_PUBLIC_IP>:~/
   ```
   Example:
   ```bash
   scp -i ~/downloads/web.pem ~/downloads/web.pem ec2-user@18.220.100.200:~/
   ```

2. **Login to the Bastion Host:**
   ```bash
   ssh -i ~/downloads/web.pem ec2-user@<BASTION_PUBLIC_IP>
   ```

3. **Set Proper Permissions for the Key on the Bastion Host:**
   ```bash
   chmod 400 ~/web.pem
   ```

---

## **🔷 Step 3: SSH into the Private Servers from the Bastion**
Now that the PEM key is on the Bastion Host, you can SSH into the private servers.

1. **Find the Private IP of the Web Server**
   - Go to **AWS Console** → **EC2 Instances**.
   - Copy the **Private IPv4 Address** of the Web Server (e.g., `10.0.2.45`).

2. **SSH into the Web Server from the Bastion Host:**
   ```bash
   ssh -i ~/web.pem ec2-user@10.0.2.45
   ```

3. **Repeat for the Nginx Server (Use its Private IP):**
   ```bash
   ssh -i ~/web.pem ec2-user@10.0.3.67
   ```

---

## **🔷 Alternative: Using SSH Agent Forwarding (No Key Copying)**
If you prefer not to copy the PEM key to the Bastion Host, you can use **SSH Agent Forwarding**.

1. **Enable SSH Agent on Your Local Machine:**
   ```bash
   eval "$(ssh-agent -s)"
   ssh-add ~/downloads/web.pem
   ```

2. **SSH into the Bastion Host with Agent Forwarding:**
   ```bash
   ssh -A -i ~/downloads/web.pem ec2-user@<BASTION_PUBLIC_IP>
   ```

3. **From the Bastion, SSH into the Web Server:**
   ```bash
   ssh ec2-user@10.0.2.45
   ```

---
you can also use
| ✅ **Use SSH Agent Forwarding** *(Optional)* | Avoid key transfer by forwarding agent | `ssh -A ec2-user@bastion_ip` |

---
