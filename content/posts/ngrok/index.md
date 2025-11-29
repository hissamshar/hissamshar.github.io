---
date: '2025-08-14T19:58:22+05:00'
title: 'Ngrok: Expose Localhost to the Internet'
categories: ["Linux", "Local Host"]
tags: [ "Tunneling"]
draft: false
---

## 1. Introduction

Sometimes you need to share a local application with the outside world, maybe to demo your project, test a webhook, or allow a teammate to access your development server.

Normally, you’d need a public IP, port forwarding, or a cloud server. **ngrok** removes all that complexity by creating a **secure tunnel** from the internet directly to your machine, giving you a public URL instantly.

---

## 2. Prerequisites

Before we begin, make sure you have:

- A terminal
- A [free ngrok account](https://ngrok.com/) (for authentication token)
- A running local service (e.g., a Python HTTP server, web app, or API)

---

## 3. Installing ngrok on Linux

We’ll cover installation for **Arch Linux**, **Debian/Ubuntu**, and **Fedora**.

### 3.1 Arch Linux
> This package is not in the official repos, install it from the [AUR](https://aur.archlinux.org/packages/ngrok):

```bash
yay -S ngrok
```

---

### 3.2 Debian / Ubuntu

```bash
sudo apt update
sudo apt install snapd
sudo snap install ngrok
```

Alternatively, download the binary from the [ngrok downloads page](https://ngrok.com/download).

---

### 3.3 Fedora

```bash
sudo dnf install snapd
sudo ln -s /var/lib/snapd/snap /snap
sudo snap install ngrok
```

---

## 4. Authenticating ngrok

Once installed, you need to connect it to your account so you can use custom domains, longer session times, and access the dashboard.

1. Sign in to [ngrok dashboard](https://dashboard.ngrok.com/). -> **Your AuthToken**
![](/ngrok1.png)
2. Copy your **AuthToken**.
3. Run:
    ```bash
    ngrok config add-authtoken <YOUR_TOKEN>
    ```
    You will see:
    **Authtoken saved to configuration file: ~/.config/ngrok/ngrok.yml**


---

## 5. Exposing a Local Service

For example, if your local web server is running on port **8080**:

```bash
ngrok http 8080
```

You’ll see output like:

![](/ngrok2.png)

Now you can share the HTTPS URL with anyone. It wil be similar to: 
**https://random.string.ngrok-free.app**

![](/ngrok3.png)

---

## 6. Adding Basic Security

You can protect your tunnel with a simple username and password:

```bash
ngrok http --basic-auth="user:password" 8080

```

![](/ngrok4.png)

Anyone visiting the public link will need credentials.

---
## 7. The Inspector 

One of ngrok's most powerful features is its built-in web interface, accessible at **http://127.0.0.1:4040**. This interface lets you inspect every single request that comes through your tunnel in real-time. You can see headers, request bodies, and response details, and even replay requests with a single click—an absolute lifesaver for debugging webhooks.

![](/ngrok5.png)
## 8. Common Use Cases

- **Webhook testing** — Connect services like GitHub, Stripe, or Twilio to your local environment.
- **Temporary demos** — Share work-in-progress with clients without deployment.
- **Remote device access** — SSH into a Raspberry Pi without changing router settings.

---

## 9. Conclusion

In just a few commands, you’ve learned how to:

- Install ngrok on popular Linux distros
- Authenticate your installation
- Share a local service securely

From here, you can explore ngrok’s advanced features like static domains, IP allowlists, and traffic inspection.
