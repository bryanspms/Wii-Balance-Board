# 📝 How to Bridge a Wii Balance Board to Modern Bluetooth LE Using a Raspberry Pi

This guide explains how to use a Raspberry Pi as a bridge between the **Wii Balance Board** (which uses classic Bluetooth) and modern devices that expect **Bluetooth Low Energy (BLE)**. The approach preserves the original hardware while making it usable with Windows 11, phones, or VR setups.

---

## 1. Hardware You’ll Need
- **Wii Balance Board** (with batteries installed)  
- **Raspberry Pi Zero W** (or Pi 3/4 with built‑in Bluetooth)  
- **MicroSD card** (8GB+ recommended)  
- **Micro‑USB power supply**  
- (Optional) **3D‑printed or custom enclosure** for portability  

---

## 2. Prepare the Raspberry Pi
1. Flash **Raspberry Pi OS Lite** onto the microSD card using [Raspberry Pi Imager](https://www.raspberrypi.com/software/).  
2. Boot the Pi and run updates:  
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```  
3. Enable Bluetooth services:  
   ```bash
   sudo systemctl enable bluetooth
   sudo systemctl start bluetooth
   ```  
4. Install required libraries:  
   ```bash
   sudo apt install python3-pip bluetooth bluez python3-bluez -y
   pip3 install bleak
   ```

---

## 3. Pair the Wii Balance Board (Classic Bluetooth)
1. Put the Balance Board in pairing mode by pressing the **red sync button** inside the battery compartment.  
2. On the Pi, scan for devices:  
   ```bash
   bluetoothctl
   scan on
   ```  
   Look for a device named **Nintendo RVL-WBC-01**.  
3. Pair and trust it:  
   ```bash
   pair XX:XX:XX:XX:XX:XX
   trust XX:XX:XX:XX:XX:XX
   connect XX:XX:XX:XX:XX:XX
   ```  
   (Replace with your board’s MAC address.)

---

## 4. Read Sensor Data
The Balance Board sends weight data from four corner sensors. A simple Python script can capture it:

```python
import cwiid, time

print("Press sync button on Balance Board now...")
bb = cwiid.Wiimote()  # Connects to the board
bb.rpt_mode = cwiid.RPT_BALANCE | cwiid.RPT_BTN

while True:
    sensors = bb.state['balance']
    print(sensors)  # Dictionary with kg values from each corner
    time.sleep(0.5)
```

---

## 5. Re‑Broadcast as BLE HID
To make the Pi act like a modern BLE device:
1. Use **BlueZ experimental features** to create a GATT server.  
2. With Python’s `bleak` library, advertise a custom BLE service (e.g., “BalanceBoardService”).  
3. Map the sensor data into BLE characteristics (e.g., total weight, X/Y balance shift).  

Example (simplified GATT server skeleton):

```python
from bleak import BleakGATTServer

# Define custom UUIDs for weight and balance
WEIGHT_UUID = "12345678-1234-5678-1234-56789abcdef0"
BALANCE_UUID = "12345678-1234-5678-1234-56789abcdef1"

# Update characteristics with sensor data in your loop
```

This way, phones or PCs can connect to the Pi as if it were a modern BLE fitness scale or joystick.

---

## 6. Test the Bridge
- On your phone or PC, scan for Bluetooth devices.  
- You should see your Pi advertising as “BalanceBoardBridge” (or whatever name you set).  
- Connect and verify that weight/balance data is streaming.  

---

## 7. Make It Portable
- Add your Python script to autostart:  
   ```bash
   sudo nano /etc/rc.local
   ```
   Insert before `exit 0`:  
   ```bash
   python3 /home/pi/balance_bridge.py &
   ```  
- Place the Pi and a USB power bank in a small enclosure.  

---

## ⚖️ Closing Notes
- This approach **preserves the original hardware** — no soldering or irreversible mods.  
- You can later refine the BLE profile to mimic a **standard HID joystick**, making it plug‑and‑play with emulators or VR apps.  
- If desired, you can expand the script into a full bridge that automatically connects and re‑broadcasts without manual intervention.
