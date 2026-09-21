# Catalyst.K ScreenMIPI Raspberry Config
## Kernel Version
Please execute the following command to check the Raspberry Pi firmware version.
```
cat /etc/os-release
```
The system being queried is the Bullseye or Trixie version.

## Bullseye
###  Display Config
#### cmdline.txt
cmdline.txt is typically located in the /boot/firmware directory or the /boot directory. Add the following command to the command line.
```
usbhid.quirks=0x27c0:0x0859:0x00001408 dwc_otg.fiq_fsm_enable=0 video=HDMI-A-1:480x800@60 fbcon=rotate:1
```

#### config.txt
config.txt is typically located in the /boot/firmware directory or the /boot directory. Add the following command to the command line.
```
dtoverlay=vc4-kms-v3d  # ensure this line of configuration is set.
# add the following configuration line.
hdmi_force_hotplug=1
hdmi_group=2
hdmi_mode=87
hdmi_drive=1
hdmi_timings=480 0 40 64 88 800 1 10 20 35 0 0 0 53 0 31000000 6
display_hdmi_rotate=2
```

### Touch Configuration
#### 99-wch-touch.rules
Create the rule file using the following command:
```
sudo nano /etc/udev/rules.d/99-wch-touch.rules
```
Save the following content to the file `99-wch-touch.rules`:
```
ACTION=="add|change", SUBSYSTEM=="input", KERNEL=="event*", \
  ATTRS{name}=="Cadwell Laboratories, Inc. TouchScreen", \
  ENV{ID_INPUT}="1", \
  ENV{ID_INPUT_MOUSE}="1"
```

#### 99-cadwell-pointer.conf
Create the config file using the following command:
```
sudo nano /etc/X11/xorg.conf.d/99-cadwell-pointer.conf
```
Save the following content to the file `99-cadwell-pointer.conf`:
```
Section "InputClass"
    Identifier           "Cadwell abs evdev"
    MatchProduct         "Cadwell Laboratories, Inc. TouchScreen"
    MatchUSBID           "27c0:0859"
    MatchDevicePath      "/dev/input/event*"
    Driver               "evdev"
    Option "Ignore"      "off"
    Option "Mode"        "Absolute"
    Option "SwapAxes"    "off"
    Option "InvertX"     "off"
    Option "InvertY"     "off"
    Option "TransformationMatrix" "1 0 0 0 1 0 0 0 1"
EndSection
```

#### 10-monitor.conf
Create the config file using the following command:
```
sudo nano /etc/X11/xorg.conf.d/10-monitor.conf
```
Save the following content to the file `10-monitor.conf:
```
Section "Device"
    Identifier "Card0"
    Driver     "modesetting"
EndSection

Section "Monitor"
    Identifier "HDMI-1"
    ModeLine   "480x800" 30.00 480 528 576 656 800 810 820 914 -hsync +vsync
    Option     "PreferredMode" "480x800"
    Option     "Rotate"        "right"
EndSection

Section "Monitor"
    Identifier "HDMI-A-1"
    ModeLine   "480x800" 30.00 480 528 576 656 800 810 820 914 -hsync +vsync
    Option     "PreferredMode" "480x800"
    Option     "Rotate"        "right"
EndSection

Section "Screen"
    Identifier "Screen0"
    Device     "Card0"
    Monitor    "HDMI-1"
    SubSection "Display"
        Modes "480x800"
    EndSubSection
EndSection
```

Use the following command to apply the touch configuration:
```
sudo udevadm control --reload-rules
sudo udevadm trigger
sudo reboot
```

#### KlipperScreen.conf
The following configuration prevents the screen from turning off and is the recommended way to work.
```
#~# screen_blanking = off
#~# screen_blanking_printing = off
#~# use_dpms = False
```

The following configuration will turn off the display, but it will not turn off the screen backlight.
```
#~# screen_blanking = 300
#~# screen_blanking_printing = 300
#~# use_dpms = False
```

The following configuration turns off the display and the screen backlight, but there is a time lag when waking the device via touch.
```
#~# screen_blanking = 300
#~# screen_blanking_printing = 300
#~# use_dpms: True
```

## Trixie
###  Display Config
#### cmdline.txt
cmdline.txt is typically located in the /boot/firmware directory or the /boot directory. Add the following command to the command line.
```
usbhid.quirks=0x27c0:0x0859:0x00001408 dwc_otg.fiq_fsm_enable=0 video=HDMI-A-1:480x800@60 fbcon=rotate:1
```

#### config.txt
config.txt is typically located in the /boot/firmware directory or the /boot directory. Add the following command to the command line.
```
dtoverlay=vc4-kms-v3d  # ensure this line of configuration is set.
# add the following configuration line.
hdmi_force_hotplug=1
hdmi_group=2
hdmi_mode=87
hdmi_drive=1
hdmi_timings=480 0 40 64 88 800 1 10 20 35 0 0 0 53 0 31000000 6
display_hdmi_rotate=2
```

### Touch Configuration
#### 99-wch-touch.rules
Create the rule file using the following command:
```
sudo nano /etc/udev/rules.d/99-wch-touch.rules
```
Save the following content to the file `99-wch-touch.rules`:
```
ACTION=="add|change", SUBSYSTEM=="input", KERNEL=="event*", \
  ATTRS{name}=="Cadwell Laboratories, Inc. TouchScreen", \
  ENV{ID_INPUT}="1", \
  ENV{ID_INPUT_MOUSE}="1"

ACTION=="add|change", SUBSYSTEM=="input", KERNEL=="event*", \
  ATTRS{idVendor}=="27c0", ATTRS{idProduct}=="0859", \
  ENV{ID_INPUT}="1", \
  ENV{ID_INPUT_MOUSE}="1"
```

#### 99-cadwell-pointer.conf
Create the config file using the following command:
```
sudo nano /etc/X11/xorg.conf.d/99-cadwell-pointer.conf
```
Save the following content to the file `99-cadwell-pointer.conf`:
```
Section "InputClass"
    Identifier           "Cadwell abs evdev"
    MatchProduct         "Cadwell Laboratories, Inc. TouchScreen"
    MatchUSBID           "27c0:0859"
    MatchDevicePath      "/dev/input/event*"
    Driver               "evdev"
    Option "Ignore"      "off"
    Option "Mode"        "Absolute"
    Option "SwapAxes"    "off"
    Option "InvertX"     "off"
    Option "InvertY"     "off"
    Option "TransformationMatrix" "1 0 0 0 1 0 0 0 1"
EndSection

```

#### 10-monitor.conf
Create the config file using the following command:
```
sudo nano /etc/X11/xorg.conf.d/10-monitor.conf
```
Save the following content to the file `10-monitor.conf:
```
Section "Device"
    Identifier "Card0"
    Driver     "modesetting"
EndSection

Section "Monitor"
    Identifier "HDMI-1"
    ModeLine   "480x800" 30.00 480 528 576 656 800 810 820 914 -hsync +vsync
    Option     "PreferredMode" "480x800"
    Option     "Rotate"        "right"
EndSection

Section "Monitor"
    Identifier "HDMI-1"
    ModeLine   "480x800" 30.00 480 528 576 656 800 810 820 914 -hsync +vsync
    Option     "PreferredMode" "480x800"
    Option     "Rotate"        "right"
EndSection

Section "Screen"
    Identifier "Screen0"
    Device     "Card0"
    Monitor    "HDMI-1"
    SubSection "Display"
        Modes "480x800"
    EndSubSection
EndSection
```

Use the following command to apply the touch configuration:
```
sudo udevadm control --reload-rules
sudo udevadm trigger
sudo reboot
```

#### KlipperScreen.conf
The following configuration prevents the screen from turning off and is the recommended way to work.
```
#~# screen_blanking = off
#~# screen_blanking_printing = off
#~# use_dpms = False
```

The following configuration will turn off the display, but it will not turn off the screen backlight.
```
#~# screen_blanking = 300
#~# screen_blanking_printing = 300
#~# use_dpms = False
```

The following configuration turns off the display and the screen backlight, but there is a time lag when waking the device via touch.
```
#~# screen_blanking = 300
#~# screen_blanking_printing = 300
#~# use_dpms: True
```

## Important
Please do not directly replace the corresponding files on your Raspberry Pi with the configuration files found in the `Example` directory.



