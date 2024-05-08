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

Here are some troubleshooting steps you can try to improve the performance of a slow Windows 10 PC:

**Basic Checks:**

1. **Restart your computer:** This may seem obvious, but a simple restart can often clear temporary files and resolve minor software glitches.
2. **Check for Updates:** Make sure you have the latest Windows 10 updates installed. These updates often include performance improvements and bug fixes. Here's how:
    * Click Start > Settings > Update & Security > Windows Update.
    * Click "Check for updates" and install any available updates.

**Reduce Startup Programs and Background Processes:**

3. **Disable unnecessary startup programs:** Many programs automatically launch at startup, which can slow down your boot time and overall performance. Here's how to manage them:
    * Press Ctrl + Shift + Esc to open Task Manager.
    * Click on the "Startup" tab.
    * Right-click on any program you don't want to launch at startup and select "Disable."

4. **Close background processes:** Some applications might run in the background even if you're not actively using them. Here's how to close them:
    * Open Task Manager (Ctrl + Shift + Esc).
    * Click on the "Processes" tab.
    * Look for any programs you're not using and consuming high CPU or memory usage.
    * Right-click on the program and select "End task."

**Manage Storage Space:**

5. **Clean up disk space:** A cluttered hard drive can slow down your computer. Use the Disk Cleanup tool to remove unnecessary files like temporary files, downloaded content, and old system files. Here's how:
    * Open File Explorer.
    * Right-click on your main hard drive (usually C:) and select "Properties."
    * Click on "Disk Cleanup" under the "General" tab.
    * Select the files you want to delete and click "Clean up system files" (if prompted).

6. **Consider upgrading storage:** If your hard drive is nearly full, consider upgrading to a solid-state drive (SSD). SSDs are significantly faster than traditional hard disk drives (HDDs) and can dramatically improve performance.

**Optimize Performance:**

7. **Adjust performance settings:** Windows 10 offers various performance settings you can adjust to optimize your PC for speed or appearance. Here's how:
    * Open Control Panel (Search for "control panel" in the Start menu).
    * Go to "Hardware and Sound" -> "Power Options."
    * Choose a power plan that prioritizes performance (e.g., "High performance").

8. **Disable visual effects:** Turning off some visual effects can improve performance, especially on older computers. Here's how:
    * Open Control Panel (Search for "control panel" in the Start menu).
    * Go to "System and Security" -> "System."
    * Click on "Advanced system settings" in the left pane.
    * Under the "Performance" tab, click on "Settings."
    * Choose "Adjust for best performance" and click "Apply" and "OK."

**Advanced Measures:**

9. **Check for malware:** Malware infections can significantly slow down your computer. Run a scan with your antivirus software to detect and remove any potential threats.

10. **Defragment your hard drive (HDD only):** If you're still using a traditional HDD, defragging it can help improve performance by rearranging fragmented files. However, this is generally not necessary for SSDs. Here's how to defragment:
    * Open File Explorer.
    * Right-click on your main hard drive (usually C:) and select "Properties."
    * Click on "Tools" and then "Optimize and defragment the drive."
    * Choose your drive and click "Optimize."

11. **Update device drivers:** Outdated device drivers can also cause performance issues. Update your graphics card driver, network adapter driver, and other essential drivers to the latest versions. You can usually find them on the manufacturer's website.

12. **Perform a system restore (if applicable):** If the performance issue started recently, consider performing a system restore to a point when your computer was running smoothly. This can revert system settings and installed programs to an earlier state.

**Remember:** 

* Try these steps one at a time and restart your computer after each step to see if the issue persists.
* Not all steps might be necessary for your specific situation. 
* If none of these solutions work, consider seeking help from a computer technician or contacting Microsoft support.

