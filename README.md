# Understanding & Capturing the WPA2 Handshake: A Lab Guide

**Author:** Sayed Muhammad Subayyal (SB)  
**Date:** September 2025  

---

## Abstract
This lab demonstrates how to capture and verify a WPA2 handshake in a controlled environment.  
The WPA2 handshake is crucial for Wi-Fi security—it’s the cryptographic process by which a client proves knowledge of the correct password. Capturing it (ethically, on networks you own) is essential for learning how wireless authentication works, its vulnerabilities, and how to better secure networks.

---

## 1. Introduction
Wireless networks secured with WPA2 are widely used, but they are not immune to attacks.  
An attacker who successfully captures a WPA2 handshake can attempt offline password cracking.  
This guide teaches:

- How to put your wireless adapter into monitor mode  
- How to capture the handshake using tools like `airodump-ng`  
- How to verify the handshake in Wireshark  
- What steps to take if capture fails  
- And how to defend against such attacks

> **Ethical Notice:** Only perform this on networks you own or have explicit permission to test. Unauthorized interception of network traffic is illegal and unethical.

---

## 2. Lab Setup & Requirements

| Item | Details |
|------|---------|
| **Hardware** | Wi-Fi adapter that supports monitor mode and packet injection |
| **Software / OS** | Linux distribution (e.g. Kali Linux), `aircrack-ng` suite, `Wireshark` |
| **Permissions** | Local admin / sudo privileges to change interface modes |
| **Environment** | You own the network or have explicit permissions |

---

## 3. Step-by-Step Guide

### Step 1: Stop Network Manager  

sudo systemctl stop NetworkManager
### Step 2: Enable Monitor Mode on Wireless Adapter
Replace wlan0 with your adapter name if different.


sudo ip link set wlan0 down
sudo iw dev wlan0 set type monitor
sudo ip link set wlan0 up
### Step 3:Lock Adapter to Target Channel
Use the channel that the target network is broadcasting on.


sudo iwconfig wlan0 channel <channel_number>
### Step 4: Capture the Handshake

sudo airodump-ng wlan0 -c <channel_number> --bssid <Target_BSSID> -w capture wlan0
-c <channel> → locks the scan to a specific channel

--bssid <Target_BSSID> → focuses only on that network

-w capture → saves packets to a capture file
Wait for a client to connect (or reconnect) to the targeted Wi-Fi. In the terminal, you should see a message like:

css

[ WPA handshake: <MAC address> ]
Step 5: Verify the Handshake with Wireshark
Open the .cap file in Wireshark.

Apply the display filter:
eapol
You should see all four EAPOL messages (Message 1/2/3/4) between the client & router.
If all four are present, the handshake is valid.

## 4.  Troubleshooting
Issue	Possible Cause	Suggestion
No handshake detected	No client connecting / no traffic	Force a client to disconnect & reconnect, or wait more
Weak signal / noise interference	Distance or physical obstacles	Move closer to AP, reduce interference
Adapter not supporting monitor mode	Hardware limitation	Use a compatible Wi-Fi adapter
Wrong channel or BSSID	BSSID or channel misconfiguration	Double-check using iwlist or airmon-ng scanning

## 5.  Security Implications & Defense
Once a handshake is captured, an attacker can attempt password cracking (dictionary / brute force).

Strong network passwords (long, complex) slow down or prevent such attacks.

Enabling WPA3, where supported, provides stronger protection.

Use client isolation / disable broadcast of SSID if possible.

Monitor for unauthorized capture attempts or rogue APs.

## 6.  Conclusion
This lab shows how capturing a WPA2 handshake works in real terms and why it’s a critical step for anyone interested in wireless security.
Beyond just learning the mechanics, the real value is in understanding how small misconfigurations or weak credentials can lead to substantial vulnerabilities.

References
aircrack-ng documentation

Wireshark user guide

WPA2 protocol specification

Ethical Hacking / Wireless Security resources
