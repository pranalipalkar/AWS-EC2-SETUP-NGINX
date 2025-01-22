# AWS-EC2-SETUP-NGINX

---

# 🚀 Hosting a Website on Nginx Server on AWS EC2 (Amazon Linux)

This guide will help you set up an **Nginx** web server on an **Amazon EC2 instance** running **Amazon Linux**. By the end of this tutorial, you’ll have a simple website hosted and accessible via your public IP address.

## 📝 Prerequisites

Before you begin, ensure you have the following:
1. ✅ An **AWS account** and access to the **AWS Management Console**.
2. ✅ A **running EC2 instance** (preferably **Amazon Linux 2**).
3. ✅ SSH access to the EC2 instance using a **.pem private key**.
4. ✅ **Default VPC** and **Security Group** set up in your AWS account.
5. ✅ **Security Group** with only **HTTP (port 80)** and **HTTPS (port 443)** open for inbound traffic.

If you don't have an EC2 instance yet, you can create one with the **Amazon Linux 2** AMI in the **default VPC**.

---

## 🛠️ Step-by-Step Guide

### Step 1: Connect to Your EC2 Instance via SSH

1. Open your terminal.
2. Run the following command to connect to your EC2 instance (replace `your-key.pem` with your private key file and `your-ec2-public-ip` with your EC2 instance’s public IP address):

```bash
ssh -i "your-key.pem" ec2-user@your-ec2-public-ip
```

For **Ubuntu**-based instances, use `ubuntu` as the username:

```bash
ssh -i "your-key.pem" ubuntu@your-ec2-public-ip
```

---

### Step 2: Update Your EC2 Instance

Once connected, update the instance’s package list:

```bash
sudo yum update -y
```

---

### Step 3: Install Nginx

Run the following command to install **Nginx**:

```bash
sudo amazon-linux-extras install nginx1 -y
sudo yum install nginx -y
```

---

### Step 4: Start Nginx

Start the **Nginx** service:

```bash
sudo systemctl start nginx
```

Enable it to start automatically when your EC2 instance reboots:

```bash
sudo systemctl enable nginx
```

---

### Step 5: Verify Nginx is Running

Check the status of Nginx:

```bash
sudo systemctl status nginx
```

You should see a message like `Active: active (running)` if Nginx is up and running.

---

### Step 6: Adjust Security Group to Allow HTTP and HTTPS Traffic

To only allow **HTTP** (port 80) and **HTTPS** (port 443) traffic, follow these steps:

1. Go to the **AWS Management Console**.
2. Navigate to **EC2** > **Security Groups** in the **Networking & Security** section.
3. Select the **Security Group** associated with your EC2 instance (this is usually the default security group if you’re using the default VPC).
4. Click **Inbound rules** and then **Edit inbound rules**.
5. Add the following inbound rules:
   - **Type**: HTTP, **Port Range**: 80, **Source**: Anywhere (0.0.0.0/0)
   - **Type**: HTTPS, **Port Range**: 443, **Source**: Anywhere (0.0.0.0/0)

6. Remove any unnecessary inbound rules, such as **SSH** (port 22), if you're only allowing **HTTP** and **HTTPS** traffic.
7. Click **Save rules**.

**Important:** If you have an active SSH session, ensure port **22** (SSH) remains open for continued access.

---

### Step 7: Edit the Default `index.html`

By default, Nginx serves content from the directory `/usr/share/nginx/html`. Let’s edit the `index.html` file located there.

1. Navigate to the **Nginx HTML directory**:

```bash
cd /usr/share/nginx/html
```

2. Edit the `index.html` file using the **`vi` editor**:

```bash
sudo vi index.html
```

3. Press `i` to enter **insert mode** in **vi**.
4. Replace the content of `index.html` with your custom HTML or a simple message like:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>My Awesome Website</title>
</head>
<body>
  <h1>Welcome to My Website Hosted on Nginx! 🚀</h1>
  <p>This is a simple webpage hosted on an Amazon EC2 instance with Nginx.</p>
</body>
</html>
```

5. After editing, press `Esc` to exit insert mode.
6. To save the changes and exit **vi**, type `:wq` and press `Enter`.

---

### Step 8: Restart Nginx

After editing the `index.html`, restart Nginx to apply the changes:

```bash
sudo systemctl restart nginx
```

---

### Step 9: Access Your Website

1. Open your **web browser**.
2. Type your EC2 instance's **public IP address** into the address bar:

```text
http://your-ec2-public-ip
```

You should now see the webpage with the message "Welcome to My Website Hosted on Nginx! 🚀".

---

## ✅ Conclusion

🎉 **Congratulations!** You’ve successfully set up an **Nginx web server** on your **Amazon Linux EC2 instance** with the default security group and only allowed **HTTP (port 80)** and **HTTPS (port 443)** traffic. Your website is now accessible from the internet via your EC2 instance's public IP.

---

### 🚨 Troubleshooting Tips

- **If Nginx isn’t working**, check the EC2 instance’s security group to make sure **port 80 (HTTP)** and **port 443 (HTTPS)** are open.
- **If you can’t connect via SSH**, verify that your **.pem file** has the correct permissions (`chmod 400 your-key.pem`).
- **If your website isn’t loading** over HTTPS, ensure that you’ve configured SSL certificates (self-signed or via Let's Encrypt) for HTTPS. This guide covers HTTP traffic only.

---

