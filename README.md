Welcome to the team! In the world of Windows support, **Services** are the backbone of the OS. If a service fails, it’s rarely a random glitch; it's usually a domino effect involving permissions, dependencies, or network states.

When you're troubleshooting a server or a high-end workstation, think of the **Service Control Manager (SCM)** as the conductor of an orchestra. If one instrument is out of tune, the whole performance can stall.

---

## 1. Checking for Stopped Services

Your first step isn't just seeing *what* is stopped, but *why* it stopped. Windows services have different "Startup Types" (Automatic, Manual, Disabled).

* **The Quick Check:** Use `services.msc` or the PowerShell command `Get-Service | Where-Object {$_.Status -eq "Stopped" -and $_.StartType -eq "Automatic"}`. This highlights critical services that *should* be running but aren't.
* **The Event Viewer Link:** Always cross-reference a stopped service with **System Logs (Event ID 7000, 7009, or 7036)**. These will tell you if the service timed out or if there was a "Logon Failure."
* **Pro Tip:** If a service stops immediately after starting, check the **Executable Path** in the service properties. If the `.exe` is missing or the permissions on that folder have changed, the service will crash on launch.

---

## 2. Network Location Awareness (NLA) Properties

This is a sophisticated area where many junior engineers get stuck. The **Network Location Awareness** service identifies which network the computer is connected to (Public, Private, or Domain) and applies firewall rules accordingly.

* **The "Identify" Loop:** Sometimes, NLA gets stuck in an "Identifying" state. This happens if the machine cannot reach the Domain Controller (DC) to verify its location.
* **Service Dependencies on NLA:** Many services, like **DirectAccess** or even certain **SQL Server** configurations, wait for NLA to signal that the network is "Ready" before they initialize their network-facing components.
* **Troubleshooting NLA:** 1.  Ensure the **Network List Service** is also running.
2.  If the network is wrongly identified as "Public" instead of "Domain," try restarting the NLA service *after* the connection to the DC is established.

---

## 3. Checking Service Dependencies

This is the "Domino Effect" I mentioned. Most critical services rely on others to function. For example, the **Print Spooler** won't start if the **HTTP Service** or **RPC (Remote Procedure Call)** is disabled.

* **The Hierarchy:** Open the properties of any service and look at the **Dependencies** tab.
* **"This service depends on..."**: If any service in this top list is stopped, your primary service *cannot* start.
* **"The following system components depend on..."**: This shows you the "Blast Radius." If you stop this service, all of these other components will fail.


* **The RPC Factor:** Almost every network-related service depends on **Remote Procedure Call (RPC)**. If RPC is disabled (which is hard to do but possible via registry errors), Windows essentially becomes a brick.

---

## The "Expert" Workflow

When a user says "I can't access the shared drive," don't just restart the Workstation service. Follow this flow:

1. **Verify Dependencies:** Is the *Server* or *Workstation* service actually allowed to start? (Check RPC and LanmanWorkstation).
2. **Check NLA:** Does the computer realize it's on the corporate network? (Check the Network and Sharing Center).
3. **Start and Monitor:** Attempt to start the service manually while keeping the **Event Viewer** open. If it fails, the error code (e.g., Error 1068: The dependency service or group failed to start) will point you exactly where to look next.

Excellent follow-up. In a high-stakes environment, the GUI is often too slow. As an expert, you’ll find that **PowerShell** provides the surgical precision you need, while **Device Manager** acts as your hardware-to-software bridge.

Let’s break these down.

---

## 1. Using PowerShell to Start Failed Services

When a service fails to start via the GUI, it often swallows the real error message. PowerShell allows you to see the raw output and automate the recovery of multiple services at once.

### Identify and Filter

Don't just look for "Stopped" services; look for those set to **Automatic** that aren't running. These are your "True Failures."

```powershell
# Get all services that should be running but aren't
Get-Service | Where-Object { $_.Status -eq "Stopped" -and $_.StartType -eq "Automatic" }

```

### The "Start-Service" Command

To attempt a start, we use `Start-Service`. However, as an expert, you should use the `-PassThru` parameter to see the result immediately.

```powershell
# Attempt to start a specific service (e.g., Print Spooler)
Start-Service -Name "Spooler" -PassThru

```

### Bulk Recovery (The "Nuclear" Option)

If a server just rebooted and several services are hanging, you can pipe them all into a start command:

```powershell
Get-Service | Where-Object { $_.Status -eq "Stopped" -and $_.StartType -eq "Automatic" } | Start-Service

```

> **Expert Note:** If this fails, PowerShell will return a **CategoryInfo: InvalidOperation**. This usually confirms a **Dependency failure** (one of the services in the "Dependencies" tab we discussed earlier is also stopped).

---

## 2. Using Device Manager for Failed Devices

Hardware issues often manifest as service failures. If the "Print Spooler" won't start, it might be because the actual printer hardware is showing a "Code 10" in Device Manager.

### Identifying Symbols

When you open `devmgmt.msc`, look for these three visual cues:

* **Yellow Exclamation Mark (!):** The device is physically present, but the driver is failing or incompatible.
* **Red "X" or Down Arrow:** The device is manually disabled.
* **"Unknown Device":** Windows has no idea what this is—usually a chipset or proprietary driver issue.

### Critical Error Codes to Know

When you right-click a failed device and select **Properties**, look at the "Device Status" box.

* **Code 10 (This device cannot start):** Usually a driver version mismatch or a hardware communication error.
* **Code 22 (The device is disabled):** Simply right-click and select "Enable."
* **Code 43 (Windows has stopped this device):** This is often a physical hardware failure or a power-state issue.

### The Expert "Ghost" Trick

Sometimes a device is failing, but it's not even showing up because it's "disconnected" or "hidden." To see these "ghost" devices:

1. In Device Manager, click **View**.
2. Select **Show hidden devices**.
3. Look for translucent icons. This is vital for troubleshooting "duplicate" drivers or old hardware remnants that are causing conflicts with your services.

---

### The Bridge: Linking Devices to Services

If you find a failed device in Device Manager, your next step is to find the **Service Name** associated with that driver.

1. In the device Properties, go to the **Details** tab.
2. Select **Service** from the dropdown.
3. Go back to PowerShell and check the status of *that* specific service name.

In the troubleshooting lifecycle, **disabling a failed device** is often a necessary "triage" step. It allows the rest of the operating system and its dependent services to initialize without getting hung up on a hardware component that is sending bad signals or timing out.

As a Senior Engineer, you should view disabling a device as a way to isolate a fault. Here is how you handle it like a pro.

---

## 1. Disabling via Device Manager (The Standard Way)

This is the most common method when you have GUI access.

1. **Identify the Culprit:** Look for the yellow exclamation mark or the device causing system instability (e.g., a flickering display driver or a malfunctioning Wi-Fi card).
2. **The Action:** Right-click the device and select **Disable device**.
3. **The Warning:** Windows will prompt you with: *"Disabling this device will cause it to stop functioning. Do you really want to disable it?"* * **Expert Tip:** Be careful! Disabling "System Devices" (like the PCI Bus) can result in a Blue Screen of Death (BSOD) or a non-bootable system. Stick to peripheral or non-essential controllers (Audio, Network, USB).

---

## 2. Disabling via PowerShell (The Automation Way)

In a remote support scenario or when dealing with a "headless" server, you’ll use PowerShell. This is much faster than clicking through menus.

### Step A: Find the Device Instance ID

You need to target the device specifically.

```powershell
# List all devices with problems to find the target
Get-PnpDevice | Where-Object { $_.Status -ne "OK" }

```

### Step B: Disable the Device

Once you have the `InstanceId` or a unique part of the name:

```powershell
# Disable a specific device by name
Disable-PnpDevice -InstanceId "PCI\VEN_8086&DEV_15D8..." -Confirm:$false

```

> **Expert Note:** If you get an "Access Denied" error, ensure your PowerShell window is running as **Administrator**. Some core kernel devices cannot be disabled via software for security reasons.

---

## 3. Why Disable Instead of Uninstall?

This is a common question from junior techs. Here is the strategic difference:

| Action | Result | Use Case |
| --- | --- | --- |
| **Uninstall** | Removes the driver association. | Use when you want to "clean slate" a driver and let Windows reinstall it on reboot. |
| **Disable** | Keeps the driver but tells the Kernel to ignore the hardware. | Use when the hardware is **physically defective** and you want to prevent it from crashing the system. |

---

## 4. The "Last Resort": Disabling in BIOS/UEFI

If a device is so broken that it prevents Windows from even booting (a "Stop Code" BSOD during the splash screen), you must go deeper.

1. Reboot the machine and enter **BIOS/UEFI** (usually F2, F10, or Del).
2. Navigate to **Onboard Devices** or **Integrated Peripherals**.
3. Set the failing component (e.g., Integrated NIC or Onboard Audio) to **Disabled**.
4. This prevents the hardware from even being "seen" by the Windows Kernel, ensuring total isolation.

---

### When to re-enable?

Once you have updated the chipset drivers or replaced the physical hardware, you can reverse these steps.

Understood. Let’s look at the **`pnputil`** utility. This is the "gold standard" for Microsoft engineers when a driver is so corrupted or "stuck" that neither Device Manager nor PowerShell can clear it out.

When you use `pnputil`, you are interacting directly with the **Driver Store**—the protected area of the OS where Windows keeps all driver packages.

---

## Using `pnputil` to Force-Remove Drivers

Sometimes, even after disabling a device, the faulty driver file stays active in the background, causing memory leaks or "Kernel Security Check Failure" errors. Here is how you purge it.

### 1. Enumerate the Drivers

First, you need to find the "Published Name" (usually `oemXX.inf`).

```powershell
# List all third-party drivers
pnputil /enum-drivers

```

Look for the **Original Name** or **Provider Name** that matches your failing hardware (e.g., "Realtek" or "Nvidia").

### 2. The Force Deletion

If the driver is currently "in use" (even if the device is disabled), a standard uninstall will fail. You have to force it.

```powershell
# Force delete the driver package
pnputil /delete-driver oem12.inf /force

```

> **Expert Note:** The `/force` flag tells Windows to kick the driver out of the kernel memory immediately. Expect a brief system flicker if you're doing this to a display or network driver.

---

## Advanced Triage: DISM and SFC

If you’ve disabled the device and cleared the driver, but the **Service** associated with it still refuses to start (or keeps throwing "Error 2: System cannot find the file specified"), your system files might be corrupted.

### The 1-2 Punch:

1. **DISM (Deployment Image Servicing and Management):** This repairs the "Windows Image" by downloading fresh files from Microsoft’s servers.
```powershell
DISM /Online /Cleanup-Image /RestoreHealth

```


2. **SFC (System File Checker):** Once the image is healthy, SFC uses that image to repair your actual local Windows installation.
```powershell
sfc /scannow

```



---

### Summary Checklist for a New Engineer:

* **Stopped Services?** Check Dependencies first.
* **Network Issues?** Verify the NLA service status.
* **Hardware Malfunction?** Disable in Device Manager to isolate.
* **Stuck Driver?** Use `pnputil` to scrub the Driver Store.
* **Still Broken?** Run DISM/SFC to ensure the OS integrity isn't the root cause.

This is the "Bread and Butter" of a Technical Support Engineer's daily life. When the hardware is healthy and the drivers are clean, the battle moves to the **Network Stack**.

Here is your expert guide to levels 7 through 14.

---

## 07. Updating Device Drivers

Updating isn't just about getting the "latest" version; it's about stability.

* **The Best Practice:** Always download drivers directly from the manufacturer (OEM) rather than relying solely on Windows Update.
* **The Rollback:** If an update fails, use the **Roll Back Driver** button in Device Manager Properties. This is your safety net.

## 08. Verifying IP Address Assignments

Use `ipconfig /all`. Look for:

* **IPv4 Address:** Ensure it’s in the expected range (e.g., `10.x.x.x` or `192.168.x.x`).
* **169.254.x.x (APIPA):** This is a red flag. It means the machine couldn't talk to a DHCP server and assigned itself a "link-local" address. Communication will be limited.

## 09. The Ping Test

The goal is to find where the "break" is in the chain. Follow this sequence:

1. `ping 127.0.0.1` (Loopback): Tests if the NIC is alive.
2. `ping [Your_IP]`: Tests the local stack.
3. `ping [Gateway_IP]`: Tests your connection to the router/switch.
4. `ping 8.8.8.8`: Tests connectivity to the outside world.

## 10. Testing DNS Name Resolution

If you can ping `8.8.8.8` but cannot reach `google.com`, you have a DNS issue.

* **Fixing the DNS Cache:** The resolver cache might be storing a "poisoned" or outdated record.
* Command: `ipconfig /flushdns`


* **Verify DNS:** Use `nslookup google.com`. It will show you which DNS server is responding (or failing).

---

## 12. Using the Network Troubleshooter

For a new engineer, don't overlook the built-in Windows Troubleshooter (**Settings > System > Troubleshoot**).

* **What it actually does:** It resets the network adapter, clears the ARP cache, and restarts the NLA service automatically. It’s a great "First Step" while you’re gathering more info from the user.

## 13. Fixing Wi-Fi Connectivity

If a user is "Connected, no internet":

1. **Check IP:** Is it APIPA?
2. **DHCP vs. Static:** Ensure "Obtain an IP address automatically" is selected unless the corporate policy requires a Static IP.
3. **DNS Settings:** Often, manually setting DNS to `8.8.8.8` or `1.1.1.1` can bypass ISP-related outages.

---

## 14. Hyper-V Troubleshooting & Setup

Hyper-V changes how networking works because it introduces a **Virtual Switch**.

### Steps to Install Hyper-V on Windows 10/11:

1. Run PowerShell as Admin:
```powershell
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All

```


2. **Restart is required.**

### Creating a Virtual Switch:

This is the "bridge" between the VM and the physical network.

* **External Switch:** Maps to your physical NIC. Use this if the VM needs internet access.
* **Internal Switch:** Only allows communication between the Host and the VM.
* **Private Switch:** Only allows communication between VMs.

### The "Hyper-V Network" Trap:

When you create an **External Virtual Switch**, Windows creates a "Bridge Adapter." Your physical NIC will no longer have an IP; instead, the "Virtual Ethernet Adapter" handles the traffic. If you lose internet after installing Hyper-V, check the **Virtual Switch Manager** to ensure the bridge was created correctly.

---

This is one of the classic "Active Directory Desync" scenarios. As a Tier 3 engineer, you know this isn't just a "network error"—it’s a **Secure Channel failure**.

The workstation has a password (stored in the LSA secrets) that no longer matches the password for its Computer Object in the Active Directory (AD). This usually happens due to a snapshot restore, a long period of inactivity, or a metadata cleanup on the Domain Controller.

Here is the Tier 3 "Clean Rejoin" workflow to resolve the trust relationship.

---

## The Strategic Objective

We need to break the existing, broken secure channel and force the creation of a new **Kerberos ticket** and computer password.

### Step 1: Capture Local Admin Credentials

Before you touch the domain settings, **ensure you have the local administrator password**. Once you remove the machine from the domain, you will not be able to log in with your domain credentials.

* **Pro Tip:** Use `whoami` to verify you are currently a member of the local `Administrators` group.

### Step 2: Graceful Removal from the Domain

Instead of just clicking "Workgroup," we want to signal the change to the OS.

1. Open **sysdm.cpl** (System Properties).
2. Click **Change**.
3. Select **Workgroup** and type a generic name (e.g., `TEMPWORK`).
4. **Do not restart yet** if you want to be thorough.

### Step 3: AD Metadata Cleanup (The "Tier 3" Step)

To ensure a clean rejoin, you should verify the state of the object on the DC side.

1. On the Domain Controller (or via RSAT), open **Active Directory Users and Computers**.
2. Locate the problematic computer object.
3. **Right-click > Delete** the object (or "Reset Account" if you want to keep the SID, though a delete/recreate is cleaner for persistent errors).
4. **Wait for replication:** If you have multiple DCs, ensure the deletion has replicated across the site before proceeding.

### Step 4: The Local Reset

On the workstation, you need to clear the DNS and Kerberos state.

1. Open PowerShell as Administrator.
2. Run the following to clear the resolver cache and existing tickets:
```powershell
ipconfig /flushdns
klist purge -li 0x3e7  # Purges Kerberos tickets for the System account

```


3. **Restart the computer.** This clears the volatile memory and confirms the machine is now fully in a Workgroup state.

### Step 5: The Domain Rejoin

1. Log in using the **Local Administrator** account.
2. Ensure the DNS settings are pointing to your **Internal Domain Controllers** (referencing your earlier points on DNS resolution).
3. Go back to **sysdm.cpl > Change**.
4. Select **Domain** and enter the FQDN (e.g., `corp.company.com`).
5. Enter Domain Admin (or Account Operator) credentials when prompted.

### Step 6: Verification

Once the machine restarts and you log back in with a domain account, verify the Secure Channel using PowerShell:

```powershell
Test-ComputerSecureChannel -Verify -Verbose

```

If it returns **True**, the trust relationship is restored.

---

## The "Expert" Shortcut (No Reboot Method)

If you can't afford a double reboot, you can try to "Repair" the channel without leaving the domain:

1. Open PowerShell as Admin.
2. Run: `Reset-ComputerMachinePassword`
3. If that fails, use: `Test-ComputerSecureChannel -Repair -Credential (Get-Credential)`

This attempts to renegotiate the password with the DC in real-time. If it works, you've saved yourself 10 minutes of downtime!

Since you’re stepping into a Tier 3 role, understanding **Group Policy Objects (GPO)** is vital. When a machine rejoins the domain, it’s like a fresh start—but if the GPOs don't apply correctly, your user might lose their mapped drives, printers, or security permissions.

Here is the advanced guide to forcing and verifying GPO application.

---

## 1. The Force Update (`gpupdate`)

The standard command is `gpupdate /force`, but as a senior engineer, you need to understand the flags:

* **/force:** Re-applies all policy settings, even if they haven't changed.
* **/boot:** Restarts the computer after the update (necessary for software installation policies).
* **/logoff:** Logs the user out (necessary for folder redirection or user-side scripts).

## 2. The "Resultant Set of Policy" (RSoP)

When a policy isn't working, you need to see exactly what the computer *thinks* it should be doing.

* **Command:** `gpresult /r`
* **What to look for:**
* **Applied Group Policy Objects:** These are the winners.
* **The following GPOs were not applied because they were filtered out:** This tells you if there is a WMI filter or a security group permission issue.



---

## 3. Advanced Troubleshooting: The HTML Report

The `/r` flag is good for a quick look, but for a deep dive, generate an HTML report. It’s much easier to read and share with your team.

```powershell
gpresult /h C:\temp\GPO_Report.html

```

Open this file in a browser. It will show you "Winning GPOs" and exactly which setting is being overridden by another policy.

---

## 4. Common Post-Rejoin GPO Issues

After a "Workstation Trust" fix, you might encounter these two specific headaches:

### A. Slow Link Detection

If the machine is on a slow Wi-Fi connection, Windows may skip "heavy" GPOs (like software installation).

* **Fix:** You can disable Slow Link Detection in the registry or via a local policy if the  office network is undergoing maintenance.

### B. Replication Latency (The "Wait" Factor)

If you moved the computer to a new **Organizational Unit (OU)** in AD, the local machine might still be looking for policies from the old OU.

* **Fix:** Verify which Domain Controller the machine is talking to:
```powershell
echo %LOGONSERVER%

```


If that DC hasn't synced with the one where you made the changes, the GPO won't apply.

---

## Your "First Week" Technical Cheat Sheet


| Task | Command |
| --- | --- |
| **Trust Issues** | `Test-ComputerSecureChannel -Verify` |
| **Driver Scrub** | `pnputil /delete-driver oemXX.inf /force` |
| **DNS Cleanup** | `ipconfig /flushdns` |
| **GPO Audit** | `gpresult /h report.html` |
| **Service Status** | `Get-Service |
