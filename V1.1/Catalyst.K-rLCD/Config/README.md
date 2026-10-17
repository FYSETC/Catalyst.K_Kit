# Catalyst.K-rLCD Raspberry Config
![alt text](../../Image/Catalyst.KRGB.png)
## Kernel Version
Please execute the following command to check the Raspberry Pi firmware version.
```
cat /etc/os-release
```
The system being queried is the Bullseye(debain11) or Trixie(debain13) version.

## Bullseye
### LCD Config
#### cmdline.txt
cmdline.txt is typically located in the /boot/firmware directory or the /boot directory. Add the following command to the command line.
```
video=HDMI-1:480x800M@60,rotate=90
```

#### config.txt
config.txt is typically located in the /boot/firmware directory or the /boot directory. Add the following command to the command line.
```
#dtoverlay=vc4-kms-v3d  # Comment out this line.
# add the following configuration line.
hdmi_force_hotplug=1
hdmi_group=2
hdmi_mode=87
hdmi_drive=1
hdmi_timings=480 0 40 64 88 800 1 10 20 35 0 0 0 53 0 31000000 6
display_hdmi_rotate=1
```

### Touch Configuration
Create the rule file using the following command:
```
sudo nano /etc/udev/rules.d/99-wch-touch.rules
```
Save the following content to the file `99-wch-touch.rules`:
```
ACTION=="add|change", KERNEL=="event*", ATTRS{name}=="wch.cn TouchScreen", ENV{LIBINPUT_CALIBRATION_MATRIX}="0 1 0 -1 0 1 0 0 1"
```
Use the following command to apply the touch configuration:
```
sudo udevadm control --reload-rules
sudo udevadm trigger
sudo reboot
```

### KlipperScreen.conf
The following configuration will turn off the display, but it will not turn off the screen backlight. Changing the configuration to something else may cause other display issues.
```
#~# screen_blanking = 300
#~# screen_blanking_printing = 300
#~# use_dpms = False
```

## Trixie
### LCD Config
#### cmdline.txt
cmdline.txt is typically located in the /boot/firmware directory or the /boot directory. Add the following command to the command line.
```
video=HDMI-A-1:480x800@60 fbcon=rotate:1
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
display_hdmi_rotate=1
disable_splash=1
max_framebuffers=0
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
  ENV{ID_INPUT_TOUCHSCREEN}="1", \
  ENV{ID_INPUT_MOUSE}="0", \
  ENV{LIBINPUT_CALIBRATION_MATRIX}="0 1 0 -1 0 1"

ACTION=="add|change", SUBSYSTEM=="input", KERNEL=="event*", \
  ATTRS{idVendor}=="27c0", ATTRS{idProduct}=="0859", \
  ENV{ID_INPUT}="1", \
  ENV{ID_INPUT_TOUCHSCREEN}="1", \
  ENV{ID_INPUT_MOUSE}="0", \
  ENV{LIBINPUT_CALIBRATION_MATRIX}="0 1 0 -1 0 1"
```

#### 99-cadwell-pointer.conf
Create the config file using the following command:
```
sudo nano /etc/X11/xorg.conf.d/99-cadwell-pointer.conf
```
Save the following content to the file `99-cadwell-pointer.conf`:
```
Section "InputClass"
    Identifier           "Cadwell touchscreen"
    MatchProduct         "Cadwell Laboratories, Inc. TouchScreen"
    MatchUSBID           "27c0:0859"
    MatchDevicePath      "/dev/input/event*"
    Driver               "libinput"
    Option "CalibrationMatrix" "0 1 0 -1 0 1 0 0 1"
    Option "TransformationMatrix" "1 0 0 0 1 0 0 0 1"
EndSection
```

#### 10-monitor.conf
Create the config file using the following command:
```
sudo nano /etc/X11/xorg.conf.d/10-monitor.conf
```
Save the following content to the file `10-monitor.conf`:
```

```

#### 99-vc4.conf
Create the config file using the following command:
```
sudo nano /etc/X11/xorg.conf.d/10-monitor.conf
```
Save the following content to the file `99-vc4.conf`:
```
Section "OutputClass"
    Identifier "vc4"
    MatchDriver "vc4"
    Driver "modesetting"
    Option "PrimaryGPU" "true"
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

### Touch Configuration

## Important
Please do not directly replace the corresponding files on your Raspberry Pi with the configuration files found in the `Example` directory.



