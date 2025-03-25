# Installing RustDesk on Ubuntu Server 24.04.02 using Docker

This guide will walk you through installing RustDesk on an Ubuntu 24.04.02 server using Docker. The process will involve setting up Docker, creating the required directory, configuring Docker Compose, and setting up the RustDesk server.

## Prerequisites:
- Ubuntu 24.04.02 server
- Root or `sudo` access
- Basic knowledge of using the terminal

---

## 1. Set up Docker’s APT repository

To install Docker on your system, first, set up Docker's official APT repository.

### Add Docker’s Official GPG Key:
```bash
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

### Add the repository to APT sources:
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

---

## 2. Install the Docker Packages

Install the Docker engine, CLI, and additional tools using the following command:

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

---

## 3. Create the Directory for Docker Configuration

In this step, you will create a directory to store the configuration for RustDesk.

### Create the directory:
```bash
sudo mkdir rustdeskdocker
```

### Change to the directory:
```bash
cd rustdeskdocker
```

---

## 4. Configure Docker Compose

### Create a `docker-compose.yml` file:
```bash
nano docker-compose.yml
```

### Paste the following Docker Compose configuration into the `docker-compose.yml` file:
```yaml
services:
  hbbs:
    container_name: hbbs
    image: rustdesk/rustdesk-server:latest
    command: hbbs
    volumes:
      - ./data:/root
    network_mode: "host"
    depends_on:
      - hbbr
    restart: unless-stopped

  hbbr:
    container_name: hbbr
    image: rustdesk/rustdesk-server:latest
    command: hbbr
    volumes:
      - ./data:/root
    network_mode: "host"
    restart: unless-stopped
```

### Save and exit the editor:
- Press **Ctrl + X** to exit.
- Press **Y** to confirm changes.
- Press **Enter** to save the file.

---

## 5. Start the Docker Containers

To start the RustDesk server, run the following command:

```bash
docker compose up -d
```

This will start both `hbbs` and `hbbr` services as background processes.

---

## 6. Verify the Docker Setup

### List the files in the directory:
```bash
ls
```

You should see a `data` folder created.

### Change into the `data` directory:
```bash
cd data
```

### List the contents of the `data` folder:
```bash
ls
```

You should see a file named `id_ed25519.pub`.

---

## 7. Retrieve the Public Key

### Display the public key:
```bash
cat id_ed25519.pub
```

You will see a key similar to the following:
```
IuBHBiIz4hQLXRhGYD9qmmDA1n54567hfdsssssddd3iY4+ME+4=
```

Copy the entire key string (from beginning to `=`).

---

## 8. Get the Server's IP Address

Run the following command to get your server's IP address:

```bash
ip a
```

Example output:
```
inet 172.16.1.99/24 brd 172.16.1.255 scope global eth0
```

Copy the IP address (e.g., `172.16.1.99`).

---

## 9. Install RustDesk on Windows

Download and install the RustDesk application on your Windows machine from the [RustDesk website](https://rustdesk.com/).

---

## 10. Configure RustDesk

### Open RustDesk and go to the ID list.
Click on the three vertical dots next to your ID number.

### Go to the "Network" section on the left side.

### Under "ID/Relay Server", enter the following details:

- **ID Server**: Enter your server's IP address (e.g., `172.16.1.99`).
- **Relay Server**: Enter your server's IP address (e.g., `172.16.1.99`).
- **Key**: Paste the public key you copied earlier.

Example:
```
ID Server: 172.16.1.99
Relay Server: 172.16.1.99
Key: IuBHBiIz4hQLXRhGYD9qmmDA1n54567hfdsssssddd3iY4+ME+4=
```

Press **OK**.

---

## 11. Verify and Connect

Your RustDesk server is now set up and ready to go online. To verify:

1. Ensure that both the **ID Server** and **Relay Server** are correctly set.
2. Check that the key is correctly entered.

If everything is configured properly, you should be able to connect to your server.

---

## 12. Congratulations!

You have successfully installed and configured **RustDesk** on your Ubuntu 24.04.02 server using Docker. Your RustDesk server is now ready to use!

---


