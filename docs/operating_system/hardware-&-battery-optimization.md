# Hardware & Battery Optimization 

An internal guide for managing, protecting, and optimizing mobile devices, hardware interfaces, and Linux environment power constraints on **Acer TravelMate (P215-52)** architectures running **Linux Mint Cinnamon**.

---

## 1. System Cleanup & Deletion Verification
Before enforcing baseline tracking configurations, ensure all experimental modules, broken code branches, and duplicate scripts generated during debugging have been safely unloaded and purged from the root filesystem.

Execute the following block to revert system alterations and remove file bloat:

```bash
# 1. Unload the experimental driver module from the active Linux kernel
sudo modprobe -r acer_wmi_battery

# 2. Erase the persistent boot configuration rules mapping
sudo rm -f /etc/modprobe.d/acer-wmi-battery.conf

# 3. Purge compiled standalone binary targets from module trees
sudo rm -f /lib/modules/$(uname -r)/kernel/drivers/platform/x86/acer-wmi-battery.ko

# 4. Refresh global system module maps and dependency bindings
sudo depmod -a

# 5. Clear script duplicates and local garbage files from the user Home directory
rm -f ~/battery_alert.sh
rm -f ~/cap_%d
```

---

## 2. Hardware Ports & Interoperability Mappings
Hardware power allocation varies by hardware node design. The **Acer TravelMate P215-52 (Core i5 10th Gen)** exposes specific electronic properties across its physical array:

| Interface Type | Visual Icon Identifier | Functional Purpose | Charging Characteristics & Risks |
| :--- | :--- | :--- | :--- |
| **USB-A (Standard)** | Standard USB Symbol ($\Psi$) | Legacy data routing, low power out. | Minimal speed; charges mobile devices only when the laptop display and processor cores are awake. |
| **USB-A (Charging)** | Battery + Lightning Bolt Icon | **Power-off Charging** profile. | Continues power out even if the machine is shut down or suspended. **Risk:** Discharging on laptop battery power will rapidly drain it. |
| **USB-C (High Speed)** | Flat Oval Geometry | Next-gen IO, dynamic power out. | Matches modern mobile phone Type-C cable profiles. High wattage, providing the fastest available charge profile. |

### Core Architectural Policy
* **Bypass Internal Racks:** Mobile phone charging should *only* occur via the laptop's USB interface if the laptop power supply is actively connected to a **wall outlet**. 
* **Avoid Double Degradation:** Using an unplugged laptop to charge external peripherals forces the laptop's internal lithium-ion storage layout through unneeded charge cycles and generates unnecessary heat, degrading hardware longevity.

---

## 3. Production Script: Battery Threshold Monitor
Since the Acer TravelMate embedded controller firmware layout drops native automated charging caps under Linux desktop environments, we protect system components using an internal system listener script.

This production-grade script listens directly to your machine's exact hardware node **`BAT1`** (the designated storage directory for this TravelMate series, replacing default `BAT0` paths) and triggers system audio/visual warnings upon reaching an **80% capacity ceiling**.

### Script Construction
Save this script exactly to `~/battery_notifier.sh`:

```bash
#!/bin/bash
# Title: battery_notifier.sh
# Purpose: Protect battery infrastructure via automated threshold alarms on BAT1 hardware channels.

while true; do
    # Target BAT1 as verified by device hardware logs
    if [ -d "/sys/class/power_supply/BAT1" ]; then
        BATTERY_LEVEL=$(cat /sys/class/power_supply/BAT1/capacity)
        STATUS=$(cat /sys/class/power_supply/BAT1/status)

        # Evaluate charge thresholds against active charging status
        if [ "$BATTERY_LEVEL" -ge 80 ] && [ "$STATUS" = "Charging" ]; then
            # Dispatch urgent visual alert notification to the desktop environment
            notify-send -u critical -i battery-full-charging "Battery Level Alert" "Battery has reached $BATTERY_LEVEL%. Unplug the phone/charger to save health!"
            
            # Play a native Linux Mint system warning chime sound
            paplay /usr/share/sounds/mint/stereo/dialog-information.ogg 2>/dev/null
        fi
    fi
    sleep 60  # Evaluate status loops every 60 seconds
done
```

### Permission Enforcement
To grant the script runtime execution rights within your system user space, run:
```bash
chmod +x ~/battery_notifier.sh
```

---

## 4. Cinnamon Desktop Environment Integration
To ensure the script persists automatically across system reboots, map it natively into your graphical environment manager:

1. Launch the **Startup Applications** utility via the Linux Mint App Menu.
2. Initialize a new item entry using the configuration below:
   * **Name:** `Battery Notification`
   * **Command:** `/home/hossain/battery_notifier.sh`
   * **Comment:** `Notify when Battery charged to 80%`
   * **Startup Delay:** `0`
3. Confirm the configuration item is **checked and enabled** in the management grid.

---

## 5. Custom Sound Asset Discovery & Configuration
If your specific hardware setup routes system alert configurations through non-standard directories, find your sound directories using the system lookup pipe below:

```bash
find /usr/share/sounds/ -type d -name "stereo" 2>/dev/null
```

### Overriding Default Audio Alerts
To replace the default system alert sound with a custom asset (e.g., a downloaded workflow completion chime), update the `paplay` path inside your script:

```bash
# Override using alternative system layouts:
paplay /usr/share/sounds/freedesktop/stereo/message.oga 2>/dev/null

# Override using a personal sound file from your user home:
paplay ~/Music/custom_alert_chime.wav 2>/dev/null
```
