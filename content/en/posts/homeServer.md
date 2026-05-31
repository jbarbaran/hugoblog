---
title: "Home server: A mini PC, Proxmox and way too much enthusiasm"
date: 2025-09-15
draft: false
tags:
  - homelab
  - self-hosting
  - proxmox
  - mini-pc
  - home-server
categories:
  - Technology
summary: "How I set up my home server with a Beelink S12 Pro, why I chose Proxmox VE and what services I'm running on it."
---

# Building a home server with a mini PC

Having a server at home opens up a huge range of possibilities. I'd been thinking about setting one up for a while, and when I finally got started, I realised that half the process was simply deciding what to buy and what to install. So what is a home server actually good for? There are obvious advantages like cutting SaaS costs or gaining privacy, but there's one that matters more to me than all the others: *learning*.

In this post I'll walk through the process, the options I considered and why I ended up building my server around a **Beelink S12 Pro** running **Proxmox VE**.

## Why a mini PC?

The first decision is form factor. The most common options are:

- **An old PC or laptop from the cupboard.** It works, but it's usually noisy, consumes a lot of power and takes up too much space.
- **A Raspberry Pi.** Small and incredibly power-efficient, but limited in RAM and processing power for running several services at once.
- **A NAS.** A solid choice if storage is the main priority, but the price climbs quickly.
- **A mini PC.** Small, silent, low power consumption, reasonable price. Clear winner.

## The mini PCs I considered

In 2025, there are three families that make sense for a home lab:

**Intel N95 / N100** is probably the most popular choice right now for this kind of use. Very good energy efficiency, full virtualisation support and highly competitive prices. The N100 is more efficient than the N95; the N95 wins slightly on raw performance but at the cost of a bit more power draw. There are countless manufacturers building models around these chips, and the difference between them is usually minimal: what varies is the connectivity, the stock RAM and the after-sales support.

**Mac Mini** deserves a mention because it's one of the most well-known options on the market. Performance is excellent and power consumption is surprisingly low, but for me the problem is the price — clearly higher than a Chinese mini PC — and I don't need something like that to get started.

**Fanless mini PCs** (no fan). Fully passive models exist, and they're very quiet as a result. The downside is that passive cooling limits sustained performance and adds a noticeable premium to the price.

## The final decision: Beelink S12 Pro

I went with the **Beelink S12 Pro** running an **Intel N100**. The reasons were straightforward:

- Good value for money
- Very low power consumption
- 16 GB of RAM
- NVMe SSD included
- Compact size

![Beelink S12 Pro](/images/beelink.webp)

The downside is that there's no dedicated GPU, so AI workloads are limited to small models that run reasonably on CPU. It's not the most powerful mini PC on the market, but it's one of the most balanced for a home lab. My goal isn't to run enterprise infrastructure — it's to run several services reliably and efficiently.

## The operating system

With the hardware sorted, the next question was what to install. There are basically three paths:

**Linux + Docker directly** is the most popular option and the easiest to understand. You install Debian or Ubuntu Server, spin up services with Docker Compose and you're done. It works very well, but everything shares the same host: if something goes wrong at the OS level, everything goes down together.

**TrueNAS or Unraid** are interesting when the focus is a home NAS and your own personal cloud storage. That wasn't my use case.

**Proxmox VE** was something I'd never heard of before — and yet it's what I chose. It's a virtualisation platform based on Debian that lets you manage virtual machines and LXC ([LinuX Containers](https://en.wikipedia.org/wiki/LXC)) containers from a fairly comfortable web interface. The main advantage is isolation: each service lives in its own environment. If I break a container, everything else keeps running. If I want to experiment with something unusual, I do it in a throwaway VM and delete it without a trace.

It's not the simplest option to start with, but the learning curve pays for itself.

## Installing Proxmox VE

The installation process is simpler than it looks. Proxmox ships its system as an ISO image, so the first step is writing it to a USB drive.

On Windows, the most convenient tool for this is **Balena Etcher**: download the ISO from the official Proxmox website, open Etcher, select the image and the drive, and in a few minutes you have a working installer. No fuss.

With the USB drive ready, it's time to boot the Beelink from it. To do that you need to access the system's boot menu: on the S12 Pro, you do this by pressing the right key immediately after powering on (in my case **F7**, though it can vary by model). From there you select the USB drive as the boot device and the Proxmox installer loads on its own.

The installer itself is a fairly guided graphical wizard. It basically asks for:

- The disk to install the system on
- Network configuration (IP address, gateway, DNS)
- The node name
- A password for the root user

Ten minutes later the system is installed. You pull out the USB drive, the machine reboots and you can access the Proxmox web interface from any browser on your local network at `https://SERVER-IP:8006`.

![Proxmox VE installation complete](/images/proxmox-finished.webp)

From that point on, everything is managed from the browser.

## Key concepts in Proxmox VE

Before starting to deploy services, there are two concepts worth understanding properly.

### LXC: lightweight containers

LXC containers share the host system's kernel. This means lower memory usage, lower CPU overhead and almost instant startup. They're ideal for lightweight services like DNS servers, proxies or any application that doesn't need a very specific environment.

### VMs: full virtual machines

Virtual machines run completely independently, with their own kernel and operating system. In exchange, they consume more resources and take longer to start. They're the right choice when you need maximum compatibility or isolation, or when the application itself recommends it.

### What do I use?

My approach is simple:

> If something can run comfortably in an LXC, I run it in an LXC.

I only use virtual machines when they offer a clear advantage.

## How the server is organised

My current setup is fairly straightforward:

```text
proxmox-node
│
├── CT 100 - alpine-adguard
├── CT 101 - tailscale
├── CT 102 - n8n
├── CT 103 - ollama
├── CT 104 - postgresql
│
└── VM 200 - home-assistant
```

Lightweight services in LXC, more complex services in VMs. This keeps resource usage under control and makes administration simpler.

## Services I'm running

### AdGuard Home

Acts as a DNS server for my entire home network. It blocks ads at the network level, filters domains and gives visibility into the DNS traffic of every connected device. Deployed in an LXC container, it barely uses any resources.

### Home Assistant

It's always caught my interest — it's a home automation management system. Where it really shines is in automations: it also collects data from sensors and integrates with practically any smart device you have at home. In this case I used a virtual machine because that's what Home Assistant recommends.

### Tailscale

A VPN that lets me access my home network from anywhere without opening ports. Setup is very straightforward and connections are secure.

### n8n

[n8n](https://n8n.io) is a workflow automation tool, similar in concept to Zapier or Make, but one you can self-host. It lets you connect applications and services together through a visual node-based interface and build fairly complex automations without writing code. It's one of the services I've been getting the most out of lately.

### PostgreSQL

Most of the services I set up end up needing some kind of database, and I'd rather have a single centralised PostgreSQL instance than spin up a separate database per container. It's cleaner, easier to maintain and backups become much simpler.

### Ollama

With all the AI hype right now, I wanted to experiment with local AI. I've installed Ollama with a few small models to run tests. One important caveat: an Intel N100 was not designed for this. That said, it's perfectly valid for learning, running experiments and using lightweight models — all without depending on external services.

## What I might add later

As with any home lab, the list of ideas never ends. Some possibilities on my radar:

- Jellyfin
- Nextcloud
- Grafana + Prometheus
- Immich
- Nginx Proxy Manager

For now I'm trying to keep things simple and only add new services when I actually need them. And being realistic, my Beelink can't handle much more — but maybe in the future I'll upgrade to a more powerful device or even a proper NAS… who knows!

## Conclusion

Setting up a home server with a mini PC turned out to be much simpler than I expected. The Beelink S12 Pro offers plenty of power for a wide range of home services, and Proxmox VE provides the flexibility to experiment without fear of breaking anything.

The combination of the two has let me build a stable, efficient and — above all — enjoyable platform to maintain. And as tends to happen in the world of self-hosting, once you start there's always another service to try.