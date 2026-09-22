# Catalyst.K ScreenRGB Raspberry Config
## Display Config
### cmdline.txt
cmdline.txt is typically located in the /boot/firmware directory or the /boot directory. Add the following command to the command line.
```
video=HDMI-1:480x800M@60,rotate=90
```

### config.txt
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

## Klipper
## Touch Configuration
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

## KlipperScreen.conf
The following configuration will turn off the display, but it will not turn off the screen backlight. Changing the configuration to something else may cause other display issues.
```
#~# screen_blanking = 300
#~# screen_blanking_printing = 300
#~# use_dpms = False
```

## Important
Please do not directly replace the corresponding files on your Raspberry Pi with the configuration files found in the `Example` directory.



