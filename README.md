Windows Remote Assistance allows someone you trust to connect to your computer remotely and fix issues. Here's how it works:

**Enabling Remote Assistance:**

1. **Search for Remote Assistance:** In the taskbar's search bar, type "remote assistance".
2. **Allow Connections:** Select "Allow Remote Assistance invitations to be sent from this computer" from the search results.
3. **Enable Remote Access:** In the opened window, under the "Remote" tab, check the box "Allow Remote Assistance connections to this computer" and click "OK".

**Getting Help:**

1. **Search Again:**  Repeat step 1 mentioned above.
2. **Invite Someone:** Select "Invite someone you trust to help you, or offer to help someone else".
3. **Choose Helper:** Click "Invite someone you trust to help you".
4. **Share Invitation:** There are three options:
    * **Email:** This will create an invitation file with a password that you can email to your helper.
    * **Easy Connect:** This generates a code for your helper if they're in your contacts. 
    * **Save as file:** This creates a file containing the invitation details.

**Giving Help (if someone requested it):**

1. **Search for Remote Assistance:** Repeat step 1 mentioned above.
2. **Help Someone:** Select "Invite someone you trust to help you, or offer to help someone else" and then choose "Help someone who has invited you".
3. **Connect Methods:** Choose a connection method based on how you received the invitation: Easy Connect, email attachment, or invitation file.
4. **Grant Control (optional):** If you need full control, request it after connecting and wait for permission.

**Important Note:** Only allow remote assistance from people you trust. Never give control to someone you don't know. 

For more details and visuals, you can refer to Microsoft's support page on Remote Assistance [https://support.microsoft.com/en-us/windows/solve-pc-problems-remotely-with-remote-assistance-and-easy-connect-cf384ff4-6269-d86e-bcfe-92d72ed55922](https://support.microsoft.com/en-us/windows/solve-pc-problems-remotely-with-remote-assistance-and-easy-connect-cf384ff4-6269-d86e-bcfe-92d72ed55922).



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

## Flushing the DNS Cache on Windows 10

Flushing the DNS cache can sometimes resolve internet connectivity issues by clearing outdated or incorrect information about website locations. Here's how to do it on Windows 10:

**1. Open Command Prompt:**

* Press the **Windows key + X** simultaneously. 
* Select **Command Prompt (Admin)**. You might need to provide administrator permission to proceed.

**2. Run the Flush DNS Command:**

* In the Command Prompt window, type the following command and press Enter:

```
ipconfig /flushdns
```

* You should see a message confirming that the DNS Resolver Cache was successfully flushed.

**3. (Optional) Additional Steps:**

* While not always necessary, you can also consider running these additional commands to potentially improve your internet connection:

    * **Renew IP address:**

    ```
    ipconfig /renew
    ```

    * **Release IP address (then renew):**

    ```
    ipconfig /release
    ipconfig /renew
    ```

* **Close Command Prompt:**

* Click the "X" button in the top right corner of the Command Prompt window.

**That's it!** Your DNS cache should now be flushed. You can try accessing websites again to see if the issue is resolved.


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

## Windows 10 PC Slow Performance. 

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

There are two ways to remove apps from Apps & features in Windows:

**Using Settings:**

1. Open the Start Menu.
2. Click on "Settings".
3. Click on "Apps".
4. Select "Apps & features" from the left-hand side menu.
5. Find the app you want to remove from the list that appears.
6. Click on the app and then select "Uninstall".
7. Follow any on-screen instructions to complete the uninstall process.

**Using Control Panel (Optional):**

1. Open the Start menu and type "Control Panel" in the search bar.
2. Click on "Control Panel" from the search results.
3. Go to "Programs" > "Programs and Features".
4. Right-click on the program you want to uninstall and select "Uninstall".
5. Follow any on-screen instructions to complete the uninstall process.

**Note:** In some cases, even if you've uninstalled an app, its entry might still be visible in Apps & features. If this happens, you can try right-clicking on the entry and selecting "Remove" to clean up the list. 

## Safemode

Notes: Restarting your device should be sufficient to exit you from Safe Mode back into normal mode. However, if for some reason your machine is still booting into Safe Mode on its own after restarting, try this:

Press the Windows logo key  + R.

Type msconfig in the Open box and then select OK.

Select the Boot tab.

Under Boot options, clear the Safe bootcheckbox.
