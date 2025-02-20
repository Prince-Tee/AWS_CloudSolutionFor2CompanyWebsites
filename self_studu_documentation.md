Here are additional issues you could have encountered, along with relevant platform links for t
### **🔴 Potential Issues & Troubleshooting Resources**  

1️⃣ **Permission Denied (publickey) when SSH-ing into Bastion or Web Server**  
   - Issue: Incorrect file permissions or missing key.  
   - Fix: Ensure the PEM file has the correct permissions:  
     ```bash
     chmod 400 ~/web.pem
     ```  
   - 📌 **AWS Docs:** [Troubleshooting SSH Issues](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/TroubleshootingInstancesConnecting.html)  

2️⃣ **SCP Fails Due to Incorrect Path or Key File**  
   - Issue: SCP may not work if the local path is incorrect.  
   - Fix: Double-check the file path and ensure the key exists:  
     ```bash
     ls -l ~/downloads/web.pem
     ```  
   - 📌 **Stack Overflow:** [Fixing SCP Transfer Errors](https://stackoverflow.com/questions/tagged/scp)  

3️⃣ **Bastion Host Unable to Reach Private Instances**  
   - Issue: Security Group or VPC settings may block access.  
   - Fix:  
     - Ensure the **Bastion Security Group** allows inbound SSH from your local IP.  
     - Verify the **Web Server Security Group** allows SSH from the Bastion’s **Private IP** (not Public).  
   - 📌 **AWS Docs:** [EC2 Security Groups Guide](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/security-group-rules-reference.html)  

4️⃣ **EC2 Instances Not Responding Even After SSH Connection**  
   - Issue: The instance may be in a stopped state, or there's an issue with the network.  
   - Fix: Check EC2 instance status in AWS Console and restart if necessary.  
   - 📌 **AWS Forum:** [Instance Not Responding](https://repost.aws/t/ec2-instance-not-responding-to-ssh)  

5️⃣ **Connection Timeout While SSH-ing into Private Server from Bastion**  
   - Issue: The instance is unreachable due to missing routes in the VPC.  
   - Fix:  
     - Ensure the private subnet **has a route to the Bastion Host**.  
     - Verify **NACL rules** allow inbound SSH traffic.  
   - 📌 **AWS VPC Guide:** [Troubleshooting VPC Connectivity](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_Scenarios.html)  

6️⃣ **SSH Agent Forwarding Not Working**  
   - Issue: Agent forwarding isn’t enabled or `ssh-add` failed.  
   - Fix:  
     ```bash
     ssh-add -l  # Check if key is added
     ```  
   - 📌 **Linux SSH Agent Troubleshooting:** [SSH Agent Guide](https://linux.die.net/man/1/ssh-agent)  

7️⃣ **EC2 Metadata Service Unreachable**  
   - Issue: If you’re using scripts relying on metadata (e.g., IAM role credentials), they may fail.  
   - Fix: Ensure you’re using IMDSv2 correctly:  
     ```bash
     curl -H "X-aws-ec2-metadata-token: $(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")" -v http://169.254.169.254/latest/meta-data/
     ```  
   - 📌 **AWS Metadata Guide:** [EC2 Instance Metadata](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html)  

---

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
