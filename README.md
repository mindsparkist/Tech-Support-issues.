# Tech-Support-issues.
Here's how to troubleshoot the situation where your ethernet cable is connected but giving an APIPA address (Automatic Private IP Addressing) in Windows, while wifi works:

**1. Check the Physical Connection:**

* **Ensure the cable is securely plugged into both your computer's ethernet port and the router/modem.** Sometimes a loose connection can cause this issue. Try unplugging and replugging both ends firmly.
* **Inspect the cable for damage.** Look for any cuts, fraying, or kinks that might be affecting the signal. Try using a different ethernet cable if possible.

**2. Verify Network Adapter Settings in Windows:**

* **Open the Control Panel.** Go to "Network and Internet" -> "Network Connections".
* **Right-click on your wired ethernet adapter** and select "Properties".
* **Double-click on "Internet Protocol Version 4 (TCP/IPv4)"** to open its properties.
* **Ensure "Obtain an IP address automatically" and "Obtain DNS server address automatically" are selected.** These settings allow your computer to receive an IP address from the router/modem via DHCP (Dynamic Host Configuration Protocol).
* **Click "OK"** to save the changes.

**3. Disable Conflicting Network Adapters:**

* **In the Network Connections window,** right-click on any unused network adapters (like virtual adapters) and select "Disable". These adapters can sometimes interfere with the primary wired adapter.

**4. Restart Network Services:**

* **Right-click on the Start menu icon** and select "Command Prompt (Admin)".
* **Type the following commands one by one, pressing Enter after each:**
    * `netsh winsock reset`
    * `ipconfig /release` (releases the current IP address)
    * `ipconfig /renew` (attempts to obtain a new IP address)
* **Restart your computer** after running these commands.

**5. Update Network Adapter Drivers:**

* **Right-click on your wired ethernet adapter** in the Network Connections window and select "Properties".
* **Go to the "Driver" tab** and click "Update Driver".
* **Choose "Search automatically for updated driver software"** to let Windows search for updates. If an update is available, install it.

**6. Check Router/Modem Settings (if applicable):**

* **Consult your router/modem's manual or manufacturer's website** to see if there are any settings related to DHCP or wired network connections. Some routers might have options to enable or disable DHCP or assign specific IP addresses to devices.

**7. Disable Fast Startup (if applicable):**

* **Fast Startup** can sometimes cause network connectivity issues. To disable it:
    * Open "Control Panel" -> "Hardware and Sound" -> "Power Options".
    * Click on "Choose what the power buttons do".
    * Click on "Change settings that are currently unavailable".
    * Uncheck the box next to "Turn on fast startup (recommended)" and click "Save changes".
    * Restart your computer.
