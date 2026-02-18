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

