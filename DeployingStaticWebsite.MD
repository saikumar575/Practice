# Deploying a Static Website on AWS EC2 Using Apache HTTP Server

This guide demonstrates how to install Apache (`httpd`), create a custom `index.html` page, configure permissions, and expose the website publicly through an AWS EC2 instance.

---

# Prerequisites

* AWS EC2 Instance (Amazon Linux 2023 / RHEL-based)
* Root or sudo privileges
* Public IP assigned to the EC2 instance
* Security Group access to modify inbound rules

---

# Step 1: Update System Packages

```bash
sudo yum update -y
```

---

# Step 2: Install Apache HTTP Server

```bash
sudo dnf install -y httpd
```

Verify installation:

```bash
httpd -v
```

---

# Step 3: Start and Enable Apache Service

Start the web server:

```bash
sudo systemctl start httpd
```

Enable Apache to start automatically on boot:

```bash
sudo systemctl enable httpd
```

Verify service status:

```bash
sudo systemctl status httpd
```

Expected output:

```text
active (running)
```

---

# Step 4: Create the Website Home Page

Create or edit the default Apache web page:

```bash
sudo vim /var/www/html/index.html
```

Paste the following HTML content:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TechNova Solutions</title>

    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: Arial, sans-serif;
        }

        body {
            line-height: 1.6;
            color: #333;
        }

        header {
            background: #0f172a;
            color: white;
            padding: 20px 50px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        nav a {
            color: white;
            text-decoration: none;
            margin-left: 20px;
        }

        .hero {
            background: linear-gradient(135deg, #2563eb, #0ea5e9);
            color: white;
            text-align: center;
            padding: 100px 20px;
        }

        .hero h1 {
            font-size: 3rem;
            margin-bottom: 15px;
        }

        .hero p {
            font-size: 1.2rem;
            margin-bottom: 20px;
        }

        .btn {
            display: inline-block;
            background: white;
            color: #2563eb;
            padding: 12px 25px;
            border-radius: 5px;
            text-decoration: none;
            font-weight: bold;
        }

        .section {
            padding: 60px 50px;
            text-align: center;
        }

        .services {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-top: 30px;
        }

        .card {
            background: #f5f5f5;
            padding: 25px;
            border-radius: 10px;
        }

        footer {
            background: #0f172a;
            color: white;
            text-align: center;
            padding: 20px;
        }
    </style>
</head>
<body>

<header>
    <h2>TechNova Solutions</h2>
    <nav>
        <a href="#about">About</a>
        <a href="#services">Services</a>
        <a href="#contact">Contact</a>
    </nav>
</header>

<section class="hero">
    <h1>Innovative IT Solutions</h1>
    <p>Helping businesses transform with cloud, DevOps, and software engineering.</p>
    <a href="#contact" class="btn">Get Started</a>
</section>

<section id="about" class="section">
    <h2>About Us</h2>
    <p>
        TechNova Solutions specializes in cloud infrastructure, application development,
        automation, and digital transformation services.
    </p>
</section>

<section id="services" class="section">
    <h2>Our Services</h2>

    <div class="services">
        <div class="card">
            <h3>Cloud Solutions</h3>
            <p>AWS, Azure, and Google Cloud architecture and migration services.</p>
        </div>

        <div class="card">
            <h3>DevOps</h3>
            <p>CI/CD pipelines, infrastructure automation, and container platforms.</p>
        </div>

        <div class="card">
            <h3>Software Development</h3>
            <p>Custom web and mobile application development.</p>
        </div>

        <div class="card">
            <h3>Cybersecurity</h3>
            <p>Security assessments, compliance, and cloud security best practices.</p>
        </div>
    </div>
</section>

<section id="contact" class="section">
    <h2>Contact Us</h2>
    <p>Email: info@technova.com</p>
    <p>Phone: +91 98765 43210</p>
    <p>Location: Hyderabad, India</p>
</section>

<footer>
    <p>© 2026 TechNova Solutions. All Rights Reserved.</p>
</footer>

</body>
</html>
```

Save and exit the file.

---

# Step 5: Set Proper File Permissions

```bash
sudo chmod 644 /var/www/html/index.html
```

Verify permissions:

```bash
ls -l /var/www/html/index.html
```

Expected output:

```text
-rw-r--r--
```

---

# Step 6: Configure AWS Security Group

In the AWS Management Console:

1. Navigate to **EC2 Dashboard**
2. Select your EC2 Instance
3. Open the attached Security Group
4. Click **Edit Inbound Rules**
5. Add the following rule:

| Type | Protocol | Port | Source    |
| ---- | -------- | ---- | --------- |
| HTTP | TCP      | 80   | 0.0.0.0/0 |

6. Save the rule.

---

# Step 7: Verify Apache is Listening on Port 80

```bash
sudo ss -tulnp | grep :80
```

Expected output:

```text
LISTEN 0 511 *:80
```

---

# Step 8: Access the Website

Open a browser and navigate to:

```text
http://<EC2-PUBLIC-IP>
```

Example:

```text
http://54.123.45.67
```

You should see the **TechNova Solutions** landing page.

---

# Troubleshooting

## Check Apache Service Status

```bash
sudo systemctl status httpd
```

## Restart Apache

```bash
sudo systemctl restart httpd
```

## View Apache Logs

```bash
sudo tail -f /var/log/httpd/error_log
```

## Verify Port 80 Access

```bash
sudo ss -tulnp | grep httpd
```

---

# Project Structure

```text
/var/www/html
└── index.html
```

---

# Quick Installation Summary

```bash
sudo yum update -y
sudo dnf install -y httpd

sudo systemctl start httpd
sudo systemctl enable httpd

sudo vim /var/www/html/index.html

sudo chmod 644 /var/www/html/index.html
```

Configure Security Group:

```text
HTTP | TCP | 80 | 0.0.0.0/0
```

Access:

```text
http://<EC2-PUBLIC-IP>
```

Your static website is now publicly accessible through Apache on port **80**.
