

## MSI MPG B460I Gaming - hackintosh - OpenCore 0.71 - Catalina 10.15.7

###  configuration

```
MSI MPG B460I Gaming - iTX motherboard
CPU: Intel Core i9-10850K
CPU cooler: Arctic Liquid Freezer II 240
RAM: Patriot Viper Steel DDR4 64GB 3200MHz
PSU: Mars Gaming MPII550 650W

storage: Sabrent 2TB NVME
case: MetallicGear Neo Mini v2 - Silver - an iTX motherboard

total cost: 1.233 EUR / 1.490 USD
```

###  specifications

```
CPU contains an iGPU: Intel UHD Graphics 630 1536 MB
LAN: Realtek 8125B 2.5G
WiFi / Bluetooth card: Intel AX200NGW
Audio: Realtek® ALC1200
max. memory frequency supported by mobo: 2933 MHz - makes no sense to buy a faster / more expensive RAM
I/O Controller: NUVOTON NCT6687
```


###  comments

- the iGPU is driving a 34" monitor - 3440 x 1440 @ 60Hz over DispLayPort
- monitor attached via HDMI has a pink coloration after login - see FIX below
- the max refresh rate the mobo supports over HDMI is 30Hz by the specs, but still it outputs 60Hz with the fix below
- Ethernet has to be adjusted to manual 1000baseT - this motherboard has a Realtek 8125B 2.5G LAN connection
- audio works - so far the rear Stereo output has been tested
- sleep works
- USB3 finally works
- if you want to have boot chim, then plug your jack cable into the Red Line-out at the back and set 'AudioSupport' in config.plist to True (it's False now). If you prefer to use the front output of the motherboard - that corresponds to 'Headphones' in audio settings - then change the 'AudioOut' in config.plist to 3

###  fixes

video playback: VLC and other players were crashing with the default OpenCore 6.5 config

add the following to DeviceProperties/Add/PciRoot(0x0)/Pci(0x2,0x0):


| Key | Type | Value |
| ------------- | ------------- | ------------- |
| AAPL,ig-platform-id | Data | 00009B3E |
| framebuffer-con1-enable | Data | 01000000 |
| framebuffer-con1-alldata | Data | 01050900 00040000 87010000 |
| framebuffer-patch-enable | Data | 01000000 |
| framebuffer-stolenmem | Data | 00003001 |
| device-id | Data | 9B3E0000 |
| dpcd-max-link-rate | Data | 14000000 |
| enable-dpcd-max-link-rate-fix | Data | 01000000 |
| framebuffer-portcount | Data | 04000000 |
| framebuffer-con2-enable | Data | 01000000 |
| framebuffer-con2-alldata | Data | 02060900 00040000 87010000 |
| framebuffer-con3-enable | Data | 01000000 |
| framebuffer-con3-alldata | Data | 03040A00 00080000 87010000 |
| enable-hdmi20 | Data | 01000000 |
| enable-lspcon-support | Data | 01000000 |
| framebuffer-con3-has-lspcon | Data | 01000000 |
| framebuffer-con3-preferred-lspcon-mode | Data | 01000000 |

maybe it's not all needed, but have not time to dig deeper...

to fix the ping screen overe HDMI, also add one more entry:

| Key | Type | Value |
| ------------- | ------------- | ------------- |
| framebuffer-con1-type | Data | 00080000 |

more info on how to fix the pink screen [here](https://elitemacx86.com/threads/pink-screen-on-intel-hd-and-uhd-graphics-on-macos-sierra-and-later-on-desktops-clover-opencore.434/) if you have Intel HD Graphics 530 540 550 630 640 or 650


###  BIOS settings

- Secure boot OFF
- Fast boot OFF
- Intel VT-D ON (DisableIoMapper has to be True in Opencore config.plist)
- CFG Lock OFF
- SW Guard OFF
- Above 4G ON
- C1E support ON


###  Benchmarks

Cinebench 23 - 16.030 pts

Vray 5 - 13.015

Geekbench 5 - 1.288 / 10.391

NVMe R/W speed tops at 2.900 MB/s

###  Wi-Fi

using itlwm.kext + HeliPort the wi-fi is working, however speeds could be much better
itlwm.kext does not support AirDrop
over 2.4GHz: 4MB/s down and 3MB/s up
over 5GHz: 6MB/s down and 4MB/s up

###  BlueTooth

works thanks to the IntelBluetoothInjector.kext and IntelBluetoothFirmware.kext

###  not working

- Handoff / Continuity, AirDrop
- Unlock with Apple Watch - not tested
- shutdown - it turns on after a few minutes
  (Dortania Shutdown/Restart applied, but not working)
