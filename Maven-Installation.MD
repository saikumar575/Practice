# Java & Apache Maven Setup on Amazon Linux / CentOS

This guide explains how to install **Amazon Corretto 17 (OpenJDK)** and **Apache Maven 3.9.4** on **Amazon Linux 2023**, **Amazon EC2**, or **CentOS**.

## Prerequisites

Before starting, ensure you have:

- A running Linux instance (Amazon Linux 2023, Amazon Linux 2, or CentOS)
- Root or `sudo` privileges
- An active internet connection

---

## Step 1: Update System Packages

Update all installed packages to the latest available versions.

```bash
yum update -y
```

---

## Step 2: Install Java (Amazon Corretto 17)

Apache Maven requires Java to be installed.

Install Amazon Corretto 17:

```bash
yum install java-17-amazon-corretto -y
```

Verify the installation:

```bash
java -version
```

Expected output:

```text
openjdk version "17.x.x"
OpenJDK Runtime Environment Corretto-17
```

---

## Step 3: Download Apache Maven

Navigate to your home directory and download Apache Maven **3.9.4**.

```bash
cd ~

wget https://archive.apache.org/dist/maven/maven-3/3.9.4/binaries/apache-maven-3.9.4-bin.tar.gz
```

---

## Step 4: Extract and Install Maven

Extract the downloaded archive and move it to `/opt/maven`.

```bash
tar -xvf apache-maven-3.9.4-bin.tar.gz

mv apache-maven-3.9.4 /opt/maven
```

---

## Step 5: Configure Environment Variables

Open your Bash profile.

```bash
vim ~/.bashrc
```

Add the following lines at the end of the file:

```bash
export M2_HOME=/opt/maven
export PATH=$M2_HOME/bin:$PATH
```

Save and exit.

---

## Step 6: Apply Changes

Reload the Bash configuration.

```bash
source ~/.bashrc
```

---

## Step 7: Verify Maven Installation

Run:

```bash
mvn -version
```

Expected output:

```text
Apache Maven 3.9.4
Maven home: /opt/maven
Java version: 17
```

---

# Command History

Below is the exact command sequence used during installation.

```bash
yum update -y
yum install java-17-amazon-corretto -y

cd ~

wget https://archive.apache.org/dist/maven/maven-3/3.9.4/binaries/apache-maven-3.9.4-bin.tar.gz

tar -xvf apache-maven-3.9.4-bin.tar.gz

mv apache-maven-3.9.4 /opt/maven

vim ~/.bashrc

source ~/.bashrc

mvn -version

history
```

---

# Verify Installations

Check Java:

```bash
java -version
```

Check Maven:

```bash
mvn -version
```

---

# Installed Versions

| Software | Version |
|----------|---------|
| Java | Amazon Corretto 17 |
| Apache Maven | 3.9.4 |

---

# Troubleshooting

### Maven command not found

Reload your shell:

```bash
source ~/.bashrc
```

Or verify the PATH:

```bash
echo $PATH
```

You should see:

```text
/opt/maven/bin
```

---

### Verify JAVA_HOME (Optional)

```bash
echo $JAVA_HOME
```

If required, add the following to `~/.bashrc`:

```bash
export JAVA_HOME=/usr/lib/jvm/java-17-amazon-corretto
export PATH=$JAVA_HOME/bin:$PATH
```

Then reload:

```bash
source ~/.bashrc
```

---

# References

- Apache Maven 3.9.4
- Amazon Corretto 17
- Amazon Linux 2023 Documentation

---

This documentation is provided for learning and setup purposes.
