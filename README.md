# Minimal CPU Power Management Scripts for Linux

__Tested on Dell XPS 13 9360 (i7-8550U) with Ubuntu 18.04__

These very simple scripts automatically switch your CPU to __powersave__ mode when the AC adapter is unplugged and to __performance__ mode when it is plugged.

First, a one-shot service is run at startup to check either if your laptop is plugged or not to choose the right CPU mode. Then, scripts to switch to one mode or the other are triggered based on AC adapter events.

These script have a minimal impact on your system performance since they are event-driven and thus they don't need to be run periodically.

## How to install

Install the dependencies

```bash
sudo apt install python3 acpi acpid cpufreq-utils
```

Go to the repository root and run

```bash
chmod +x ./install
sudo ./install
```

That's it! You don't even need to log out or reboot.

## How to uninstall

Go to the repository root and run

```bash
chmod +x ./uninstall
sudo ./uninstall
```

That's it! You don't even need to log out or reboot.

## Checking if everything works correctly

The following command gives you your current CPU governor, which can be __performance__ or __powersave__.

```bash
cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor
```

First, you can check if event-triggered scripts work. When you plug your AC adapter, the previous command should output __performance__. When you unplug it, it should output __powersave__.

Then, shut your laptop down and start on AC. After boot, the previous command should issue __performance__.

Finally, shut your laptop down again and start on battery. After boot, the previous command should issue __powersave__.

## Troubleshooting

### The scripts have no effect when plugging/unplugging

1. Run the following command:

```bash
acpi_listen
```

2. Plug and unplug your AC adapter. Events should be detected and echoed.

3. Copy-paste the event occurring when plugging the AC adapter to `<repo_root>/etc/acpi/events/switch-to-ac` (`event=<your_event>`).

4. Copy-paste the event occurring when unplugging the AC adapter to `<repo_root>/etc/acpi/events/switch-to-battery` (`event=<your_event>`).

5. Perform [installation steps](#How-to-install) again (no need to uninstall first).