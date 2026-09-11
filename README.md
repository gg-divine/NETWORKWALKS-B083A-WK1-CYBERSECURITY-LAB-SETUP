# NETWORKWALKS-B083A-WK1-CYBERSECURITY-LAB-SETUP
Virtual Lab created with VirtualBox and Kali Linux for testing.
</div>

<p align="center">
  <img src="https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Ver-Virtualbox%20v7.2-0070C0?style=flat-square&labelColor=000000" />
  <img src="https://img.shields.io/badge/Kali%20Linux-v2026.2-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Skill-Linux-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Network-10.0.0.0%2F24-238F89?style=flat-square&labelColor=000000" />
  <img src="https://img.shields.io/badge/Penetration%20Testing-C00000?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Skill-Virtualization-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/GitHub-404040?style=flat-square&labelColor=0070C0&logo=github&logoColor=white" />
  <img src="https://img.shields.io/badge/Kali%20Linux-404040?style=flat-square&labelColor=C00000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/NetworkWalks-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Ethical%20Hacking-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Divine%20Oses%20Oyedoh%20-C00000?style=flat-square" />
</p>

---

## Project Overview

The project deals with the building of a *virtual cybersecurity and penetration-testing laboratory** with Kali Linux, on VirtualBox.

The objective of the lab is to create an isolated environment where the fundamental procedures of cybersecurity can be carried out safely.

---


## What I Set Out to Do

Get VirtualBox installed and configured properly
Bring in Kali Linux as my main attack/testing box
Build a private NAT Network so the lab stays isolated
Get Kali talking to that network with a fixed IP
Confirm connectivity and DNS actually work
Snapshot the VM once everything was clean
Add a Windows 10 machine as a realistic target
Add an Android 9 virtual device to poke at mobile-side testing
Write all of it down so I (or anyone else) can rebuild it later


---

## Purpose of the Lab

The lab is meant for the following activites:
-Packet analysis
-Port scanning
-Web security testing
-Network Reconnaissance
-Security tool experimentation
-Exploitation practice

## How the Lab is Laid out 

![](VirtualBox_kali_08_09_2026_11_14_02.png)

| 🧩 Component       | ⚙️ Configuration   |
| ------------------ | ------------------  |
| 🖥️ Host OS         | macOS Ventura      |
| 🧠 Host RAM        | 16 GB              |
| ⚡ Processor       | Intel Core i7      |
| 🧰 Hypervisor      | VirtualBox 7.2  |
| 🐉 Security OS     | Kali Linux 2026.2  |
| 🧠 Kali RAM        | 8192 MB            |
| 🌐 Virtual Network | NAT Network        |
| 📡 Network Address | 10.0.0.0/24        |
| 🐧 Kali IP Address | 10.0.0.2/24        |
| 🚪 Default Gateway | 10.0.0.1           |
| 🌍 DNS Server      | 8.8.8.8            |
| 🔮 Future VM Range | 10.0.0.3–10.0.0.99 |

---

# 🪜 Setting Everything Up

## Step 1 — Get 7-Zip Installed

Kali Linux is often distributed as a .7z archive, so I needed 7-Zip first just to unpack the VM files.

## Step 2 — Install VirtualBox

Straightforward install — VirtualBox is what everything else in this lab runs on top of.

## Step 3 — Build the NAT Network

Instead of using a plain NAT setup, I created a dedicated NAT Network inside VirtualBox:

Network Name: NatNetwork
IPv4 Prefix:  10.0.0.0/24
DHCP:         Enabled
IPv6:         Disabled

![](Screenshot_2026-09-09_at_5.18.29_PM.png)

The reason I went with a NAT Network rather than standard NAT is that it lets multiple VMs on the same network talk to each other, while still giving each one a way out to the internet. That's exactly what I need once I start adding attacker and target machines to the same environment.

## Step 4 — Bring In Kali Linux

I grabbed the Kali Linux VM straight from the official site and imported it into VirtualBox, then set the network adapter like this:

Adapter 1
Attached to: NAT Network
Network:     NatNetwork
Adapter Type: Intel PRO/1000 MT Desktop
Allocated resources:
text
RAM: 8192 MB

![](VirtualBox_kali_08_09_2026_11_14_02.png)

I also set up a shared folder between the host and the Kali VM, mainly so I can move files back and forth without messing around with USB drives or network shares.

## Step 5 — Sort Out Kali's Network Configuration

Next, I checked Kali's network settings and locked in a fixed IP so I wouldn't have to hunt it down every time I booted the VM:

text
IP Address: 10.0.0.2
Subnet Mask: 255.255.255.0
Gateway: 10.0.0.1
DNS: 8.8.8.8

Having a predictable IP address makes documentation and future exercises a lot easier — I always know exactly where Kali lives on the network.

## Step 6 — Snapshot the Clean State

Once everything was working, I took a snapshot:
text
Clean Kali - Network Setup
This gives me a known-good baseline. If a future exercise breaks something, I can just roll back instead of rebuilding from scratch.

# Adding Windows 10 as a Target Machine

To make the lab feel more like a real environment, I added a Windows 10 virtual machine alongside Kali. The point of this box isn't to attack anything external — it's to give myself a realistic, disposable target that behaves like an actual endpoint on a corporate-style network, so I can practice things like enumeration, service discovery, and exploitation against a system I fully control.

I downloaded a Windows 10 evaluation image from Microsoft's own developer site (Microsoft provides these specifically for testing purposes) and imported it into VirtualBox in much the same way as the Kali VM.

# Adding an Android 9 Virtual Device for Mobile Testing

Mobile security is a different beast from desktop or server testing, so I wanted a way to experiment with that side of things too. Rather than running Android through VirtualBox directly — which isn't really built for mobile OS images — I used an Android-x86 build (Android 9 "Pie") that's specifically packaged to run as a regular VM.

# Things That Went Wrong (and How I Fixed Them)
Keeping a record of problems is honestly one of the more useful parts of a project like this — future me (or anyone following along) will hit the same snags.

## Problem 1 — Internet Stopped Working After Setting a Static IP
After I manually configured Kali's IPv4 settings, internet access dropped depending on how NetworkManager was handling things. My workaround was:

'''bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0 '''

I then restarted the connection (a reboot works too) and re-tested connectivity.

## Problem 2 - Lack of knowledge on the download of Windows 10 and Android 9 on VirtualBoss
My workaround was:
I went on YouTube to find appropriate videos that fit perfectly.

# Tools & Resources

7-Zip: https://7-zip.org/download.html
VirtualBox: https://virtualbox.org/wiki/Downloads
Kali Linux: https://kali.org/get-kali
Windows 10 (evaluation images): Microsoft's official developer/evaluation center
Android-x86: the official Android-x86 project release page

# Author 
Divine Oses-Oyedoh Cybersecurity Professional B083
LinkedIn www.linkedin.com/in/divine-oses-oyedoh-b543283a0

# Project Information
Program: Cybersecurity at Networkwalks | Week: 01 | Project: Cybersecurity & Pentesting Lab Setup | Repository: GitHub
