# BalanceBridge: Wii Balance Board Controller for Windows & Unity
_Modernizing the Wii Balance Board for Windows, Unity, and Steam._

This project makes the Wii Balance Board usable as a modern controller on Windows 11, with integration paths for Unity and Steam.  
See [How to Bridge a Wii Balance Board](Wii-Balance-Board-Bridge.md) for the full Raspberry Pi setup guide.  

[^1]


---

## Troubleshooting
1. Unable to connect the Wii Balance Board via Bluetooth in Windows 11  
   - Disable Bluetooth on your laptop or PC.  Note that you may need to disable the existing bluetooth adapter in Device Manager.
   - Install a [CSR4.0 Mini USB Bluetooth Adapter Wireless Dongle](https://www.amazon.com/dp/B07KC39CCL?ref=ppx_yo2ov_dt_b_fed_asin_title).  Note that you may need to update the drivers for the new device.
   - Connect the Balance Board to the new Bluetooth adapter per API instructions.
2. Still unable to connect to the Wii Balance Board via Bluetooth in Windows 11 after trying everything else.
   - If the Balance Board still refuses to connect, the workaround is to bypass Windows’ limitations by bridging it through a Raspberry Pi. See the Guide.  Here is how to bridge the Wii Balance Board: [Guide](Wii-Balance-Board-Bridge.md)

---

## License & Copyright
© 2025 [Bryan Price](mailto:bryansp_ms@hotmail.com?subject=Wii%20Balance%20Board)  
This work is licensed under the [Creative Commons Attribution 4.0 International License](https://creativecommons.org/licenses/by/4.0/).  
You are free to share and adapt this material for any purpose, even commercially, provided you give appropriate credit to Bryan Price as the original author.



## Footnotes
[^1]: This project is made possible by Ishachar and others before me that have worked on the Wii Balance Board:  https://github.com/lshachar/WiiBalanceWalker
