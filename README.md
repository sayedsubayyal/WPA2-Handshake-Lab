# WPA2-Handshake-Lab
Educational lab for WPA2 handshake capture and verification
# WPA2-Handshake-Lab
Educational lab for WPA2 handshake capture and verification

# WPA2 Handshake Capture Lab Guide 🔐

**Author:** Sayed Muhammad Subayyal  
**Purpose:** Educational lab practice for ethical cybersecurity learning.  
**Note:** This guide uses **your own Wi-Fi network**. Do NOT attempt on networks you do not own.

---

## Step 1: Stop Network Manager
Prevent automatic network management from interrupting sniffing.

```bash
sudo systemctl stop NetworkManager
Step 2: Put Adapter into Monitor Mode
Switch your Wi-Fi card from normal browsing to sniffing mode.

bash
Copy
Edit
sudo ip link set wlan0 down
sudo iw dev wlan0 set type monitor
sudo ip link set wlan0 up
Step 3: Lock Adapter to Target Channel
Ensure your adapter stays on the correct Wi-Fi channel.

bash
Copy
Edit
sudo iwconfig wlan0 channel 6
Step 4: Capture the Handshake
Start sniffing traffic on the target network.

bash
Copy
Edit
sudo airodump-ng wlan0 -c 6 --bssid <Target_BSSID> -w capture
wlan0 → Your Wi-Fi interface in monitor mode

-c 6 → Locks to channel 6

--bssid → Target Wi-Fi MAC address

-w capture → Saves output to a file named capture

📌 Wait for a device to connect/reconnect.
✅ Look for: [WPA handshake: <BSSID>] in the terminal.

Step 5: Verify Handshake in Wireshark 🔍
Open the .cap capture file in Wireshark.

Apply the filter: eapol

Check for 4 handshake messages:

Message 1 (104)

Message 2 (204)

Message 3 (304)

Message 4 (404)

📌 Seeing all 4 → Handshake successfully captured.
