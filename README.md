

## MSI MPG B460I Gaming - hackintosh - OpenCore 0.95 - Ventura 13.6.1

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

###  ACPI
```
RHUB ACPI path: PCI0.XHC.RHUB
USB controller ACPI path: _SB.PCI0.XHC
```

###  comments

- the iGPU is driving a 34" monitor - 3440 x 1440 @ 60Hz over DispLayPort
- monitor attached via HDMI has a pink coloration after login - see FIX below
- the max refresh rate the mobo supports over HDMI is 30Hz by the specs, but still it outputs 60Hz with the fix below
- Ethernet has to be adjusted to manual 1000baseT - this motherboard has a Realtek 8125B 2.5G LAN connection - Update: manual adjustment not needed since I updated to OC 0.95 and Ventura5 and replaced all the .kexts
- audio works - so far the rear Stereo output has been tested
- sleep works, shutdown works
- USB3 finally works
- Handoff / Continuity, AirDrop works since I updated to OC 0.95 and Ventura
- if you want to have boot chim, then plug your jack cable into the Red Line-out at the back and set 'AudioSupport' in config.plist to True (it's False now). If you prefer to use the front output of the motherboard - that corresponds to 'Headphones' in audio settings - then change the 'AudioOut' in config.plist to 3


###  BIOS settings

- Secure boot OFF
- MSI Fast boot Off
- Fast boot OFF
- Intel VT-D is ON (DisableIoMapper has to be True in Opencore config.plist)
- CFG Lock OFF
- SW Guard OFF
- Above 4G ON
- C1E support ON


###  Benchmarks

Cinebench 23 - 16.030 pts

Vray 5 - 13.015

Geekbench 5 - 1.288 / 10.391

NVMe R/W speed tops at 2.900 MB/s

###  Wi-Fi & BlueTooth

works with:
AirportItlwm.kext
BlueToolFixup.kext
IntelBluetoothFirmware.kext
...since I updated to OC 0.95 and Ventura
however handoff  / continuity / universal keyboard does not work

###  Fixing Shutdown

3 major steps was needed:

- proper USB mapping with setting the onboard LED controller to 'internal' as suggested elsewhere

- applying the shutdown fixes found here https://dortania.github.io/OpenCore-Post-Install/usb/misc/shutdown.html
  in case of this mobo the USB contoller path in FixShutdown-USB-SSDT.aml needs to be changed from
  '\_SB.PCI0.XHC.PMEE = Zero' to '\_SB.PCI0.XHC = Zero'

- I also turned off ErP Ready in BIOS

  without these my hackintosh would turn on by itself after a few seconds or randomly during the night
