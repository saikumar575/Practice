# Apache Tomcat 10 Installation and Startup Guide

This guide explains how to install Java, download Apache Tomcat 10, configure permissions, and start the Tomcat server on a Linux EC2 instance.

## Prerequisites

* Linux server (Amazon Linux, RHEL, or CentOS)
* Root or sudo access
* Internet connectivity

---

## Step 1: Update System Packages

```bash
yum update -y
```

---

## Step 2: Install Java

```bash
sudo yum install java -y
```

Verify the installation:

```bash
java --version
```

Expected output will display the installed Java version.

---

## Step 3: Download Apache Tomcat

Navigate to the `/opt` directory:

```bash
cd /opt
```

Download the latest Tomcat package:

```bash
sudo wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.55/bin/apache-tomcat-10.1.55.tar.gz
```

Verify the download:

```bash
ls
```

---

## Step 4: Extract Tomcat Archive

```bash
sudo tar -xvzf apache-tomcat-10.1.55.tar.gz
```

Verify extraction:

```bash
ls
```

---

## Step 5: Rename Tomcat Directory

```bash
sudo mv apache-tomcat-10.1.55 tomcat
```

Confirm the directory rename:

```bash
ls
```

---

## Step 6: Navigate to Tomcat Binary Directory

```bash
cd /opt/tomcat/bin
```

---

## Step 7: Grant Execute Permissions

```bash
sudo chmod +x *.sh
```

---

## Step 8: Start Tomcat Server

```bash
sudo ./startup.sh
```

Expected output:

```text
Using CATALINA_BASE:   /opt/tomcat
Using CATALINA_HOME:   /opt/tomcat
Tomcat started.
```

---

## Step 9: Access Tomcat in Browser

Open your browser and navigate to:

```text
http://<YOUR_PUBLIC_IP>:8080
```

Example:

```text
http://54.xx.xx.xx:8080
```

---

## Security Group Configuration (AWS EC2)

If Tomcat is not accessible from the browser:

1. Go to AWS EC2 Console.
2. Select your EC2 instance.
3. Open the attached Security Group.
4. Add an inbound rule:

| Type       | Protocol | Port Range | Source    |
| ---------- | -------- | ---------- | --------- |
| Custom TCP | TCP      | 8080       | 0.0.0.0/0 |

Save the rule and try accessing Tomcat again.

---

## Verify Tomcat Process

Check whether Tomcat is running:

```bash
ps -ef | grep tomcat
```

Or verify port 8080:

```bash
netstat -tulpn | grep 8080
```

---

## Stop Tomcat

```bash
cd /opt/tomcat/bin
sudo ./shutdown.sh
```

---

## Directory Structure

```text
/opt
 └── tomcat
     ├── bin
     ├── conf
     ├── lib
     ├── logs
     ├── temp
     ├── webapps
     └── work
```

---

## Installation Summary

```bash
yum update -y
sudo yum install java -y
java --version

cd /opt
sudo wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.55/bin/apache-tomcat-10.1.55.tar.gz
sudo tar -xvzf apache-tomcat-10.1.55.tar.gz
sudo mv apache-tomcat-10.1.55 tomcat

cd /opt/tomcat/bin
sudo chmod +x *.sh
sudo ./startup.sh
```

Tomcat is now installed and running on port **8080**.
