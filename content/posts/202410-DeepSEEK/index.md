---
date: '2025-02-03T22:18:13+05:00'
title: 'DeepSeek-R1 on Raspberry Pi 5: Open-Source AI Without a GPU'
categories: ["Embedded", "LLMs"]
tags: [ "RPI"]
---
# Running DeepSeek-R1 1.5B on Raspberry Pi 5 (CPU-Only)

## Technical Insights
### Why Can We Run This on Raspberry Pi 5?
Thanks to open-source advancements, we can now run large-scale AI models on small devices like the Raspberry Pi 5. Key factors enabling this include:
- **Optimized lightweight models**: DeepSeek-R1 1.5B is built efficiently to run on limited hardware.
- **ARM64 Support**: Modern AI frameworks support ARM-based architectures, enabling their use on RPi5.
- **Open-source software**: Platforms like Ollama make AI deployment accessible to all.

### Performance Considerations
- Running this model on an RPi5 **without a GPU** will be CPU-intensive.
- Consider **reducing active processes** to free up memory.
- If performance lags, use a **lighter model** or **external processing (cloud inference).**
- **No GPU acceleration was used** in this setup, meaning all computations rely solely on the CPU, which may affect inference speeds.

---

This guide covers the installation and execution of DeepSeek-R1 1.5B on a Raspberry Pi 5, following the steps demonstrated in your images.

## Prerequisites
- **Raspberry Pi 5** (ARM64 architecture, more powerful than previous versions)
- **Debian-based Linux installed**
- **An internet connection**
- **At least 4GB RAM recommended** for smooth operation


## Step 1: Log in to Your Raspberry Pi
Upon booting, log in using your credentials:

```shell
raspberrypi login: hisam
Password: ******
```

*Example login screen:*
![Login Screen](/mnt/data/rpi7.png)
![](/rpi2.png)

## Step 2: Install Curl
Curl is required to fetch the installation script. Run:

```shell
sudo apt install curl
```

If it's already installed, you'll see:

```shell
curl is already the newest version...
```

*Example output:*
![Curl Installation](/mnt/data/rpi6.png)
![](/rpi3.png)

## Step 3: Install Ollama
Ollama is the runtime needed to execute DeepSeek models.

```shell
curl -fsSL https://ollama.com/install.sh | sh
```

This will download and install Ollama.

*Example installation screen:*
![](/rpi4.png)

## Step 4: Enable and Start Ollama Service
After installation, Ollama sets up a system service.

```shell
ollama
```

The output will indicate success:

```shell
>>> Creating ollama user...
>>> Enabling and starting ollama service...
>>> The Ollama API is now available at 127.0.0.1:11434.
```

*Example setup screen:*
![](/rpi5.png)

## Step 5: Pull and Run DeepSeek-R1 1.5B
Now, pull and run the model:

```shell
ollama run deepseek-r1:1.5b
```

This will download the model, which is about **1.1 GB** in size.

*Example download screen:*
![](/rpi6.png)

Once downloaded, the model is ready to run.

## Step 6: Execute DeepSeek-R1 1.5B
Run the model and start interacting:

```shell
ollama run deepseek-r1:1.5b
```

You should see a prompt where you can start typing queries:

```shell
>>> Hey!
Hello! How can I assist you today? 😊
```

*Example interaction:*
![](/rpi7.png)

## Final Setup Image
![](/rpi9.png)

## Video Demonstration
[Watch Video](https://github.com/hissamshar/hissamshar.github.io/raw/refs/heads/master/static/rpi8.mp4)

## Conclusion
You have successfully installed and executed DeepSeek-R1 1.5B on your Raspberry Pi 5. This demonstrates the power of **open-source AI**, making it possible to run advanced models on small-scale devices. If you encounter performance issues, consider optimizing your setup or offloading computations.

Happy coding!


