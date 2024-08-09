# Local AI

## Introduction

Local AI is a term used to describe the use of AI models on a local device, such as a smartphone or a computer. This is in contrast to cloud-based AI, where the AI model is hosted
on a remote server and accessed over the internet. Local AI has several advantages, including improved privacy and reduced latency. However, it also has some limitations, such as
limited processing power and storage space.

### Install AMD drivers (ROCm)

```Bash
sudo pacman -S --needed rocm-hip-sdk rocm-opencl-sdk rocm-hip-runtime hip-runtime-amd miopen-hip rocm-opencl-runtime
```

### OpenCL Image support

The latest ROCm versions now includes OpenCL Image Support used by GPGPU accelerated software such as Darktable. ROCm with the AMDGPU open source graphics driver are all that is
required. AMDGPU PRO is not required.

```Bash
❯ /opt/rocm/bin/clinfo | grep -i "image support"
  Image support:				 Yes
```

## LMStudio

LM Studio is a desktop application for running local LLMs on your computer.
Install from AUR:

```Bash
yay -S --needed lmstudio-appimage
```

## Jan AI

Jan is a ChatGPT-alternative that runs 100% offline on your Desktop. Our goal is to make it easy for a person to download and run LLMs and use AI with full control and privacy.
Install from AUR:

```Bash
yay -S --needed jan-bin
```

## Ollama

Ollama is a free and open-source AI assistant that runs locally on your device. It can perform a wide range of tasks, such as answering questions, setting reminders, and playing
music.

### Install from an official repository:

```Bash
sudo pacman -S --needed ollama
sudo systemctl enable --now ollama.service
```

### Install from script:

```Bash
curl -fsSL https://ollama.com/install.sh | sh
```

### Install from source:

Ollama is distributed as a self-contained binary. Download it to a directory in your PATH:

```bash
sudo curl -L https://ollama.com/download/ollama-linux-amd64 -o /usr/bin/ollama
sudo chmod +x /usr/bin/ollama
```

### Adding Ollama as a startup service (recommended)

Create a user for Ollama:

```bash
sudo useradd -r -s /bin/false -m -d /usr/share/ollama ollama
```

Create a service file in `/etc/systemd/system/ollama.service`:

```ini
[Unit]
Description=Ollama Service
After=network-online.target

[Service]
ExecStart=/usr/bin/ollama serve
User=ollama
Group=ollama
Restart=always
RestartSec=3

[Install]
WantedBy=default.target
```

Then start the service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now ollama
```

### AMD Radeon GPU support

While AMD has contributed the `amdgpu` driver upstream to the official linux kernel source, the version is older and may not support all ROCm features. We recommend you install the
latest driver from [AMD Official Website](https://www.amd.com/en/support/linux-drivers) for best support of your Radeon GPU.

## Update

Update ollama by running the install script again:

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

Or by downloading the ollama binary:

```bash
sudo curl -L https://ollama.com/download/ollama-linux-amd64 -o /usr/bin/ollama
sudo chmod +x /usr/bin/ollama
```

## Installing specific versions

Use `OLLAMA_VERSION` environment variable with the install script to install a specific version of Ollama, including pre-releases. You can find the version numbers in
the [releases page](https://github.com/ollama/ollama/releases).

For example:

```
curl -fsSL https://ollama.com/install.sh | OLLAMA_VERSION=0.1.32 sh
```

## Viewing logs

To view logs of Ollama running as a startup service, run:

```bash
journalctl -e -u ollama
```

## Uninstall

Remove the ollama service:

```bash
sudo systemctl stop ollama
sudo systemctl disable ollama
sudo rm /etc/systemd/system/ollama.service
```

Remove the ollama binary from your bin directory (either `/usr/local/bin`, `/usr/bin`, or `/bin`):

```bash
sudo rm $(which ollama)
```

Remove the downloaded models and Ollama service user and group:

```bash
sudo rm -r /usr/share/ollama
sudo userdel ollama
sudo groupdel ollama
```
