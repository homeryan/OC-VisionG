# Intel i7-10700K Gigabyte Z490 VisionG Hackintosh

Fork from https://github.com/samuel21119/Intel-i9-10900-Gigabyte-Z490-Vision-G-Hackintosh

Tested working version:

macOS Big Sur 11.6.1

## Bootloader

**[OpenCore](https://github.com/acidanthera/OpenCorePkg) 0.7.4**

## Working

- **USB**: All ports working, USB map is shown below

- **Ethernet**: Intel I225-V 2.5Gb

  - Add **dk.e1000=0** to boot-args, mentioned [here](https://dortania.github.io/OpenCore-Install-Guide/config.plist/comet-lake.html#nvram)

  - Add device property:

  ```
  <key>PciRoot(0x0)/Pci(0x1C,0x1)/Pci(0x0,0x0)</key>
  <dict>
  	<key>device-id</key>
  		<data>
  		8hWGgA==
  		</data>
  </dict>
  ```

- **Ethernet**: Intel 82574L 1Gb

  - This NIC is natively supported. However the different subvendor ID prevents the system from loading the driver. So you need to use Linux to flash the EEPROM. Boot into Live Ubuntu. Use ```ip a``` to find out the Ethernet Adapter. In my case, it's ```enp8s0```. Then run the following command:

    ```
    sudo apt install ethtool
    sudo ethtool -E enp8s0 magic 0x10D38086 offset 0x16 value 0x00
    sudo ethtool -E enp8s0 magic 0x10D38086 offset 0x17 value 0x00
    sudo ethtool -E enp8s0 magic 0x10D38086 offset 0x1A value 0xF6
    sudo ethtool -E enp8s0 magic 0x10D38086 offset 0x16 value 0x00
    sudo ethtool -E enp8s0 magic 0x10D38086 offset 0x17 value 0x00
    sudo ethtool -E enp8s0 magic 0x10D38086 offset 0x18 value 0x86
    sudo ethtool -E enp8s0 magic 0x10D38086 offset 0x19 value 0x80
    ```

    Reboot the PC.

  - **Note that the subvendor ID will be reverted back if you cut off the power supply. So prepare a permanent Ubuntu flash drive.**

- Add device property:

- Native NVRAM

- Sleep/Wake

- Reboot/Shutdown

## BIOS Setting

- Disable

  - CSM Support
  - Secure Boot
  - Re-Size BAR Support
  - Settings -> Super IO Configuration -> Serial Port: Disabled

- Enable

  - VT-x

  - VT-d (Enable **DisableIoMapper** in config.plist)

  - Above 4G Decoding

  - Internal Graphics

  - DVMT Pre-Allocated: 64M

  - Hyper-Threading

    

## Credits

- [Apple](https://www.apple.com/) : Awesome macOS
- [Acidanthera](https://github.com/acidanthera) : OpencorePkg, kexts, tools etc.
- [Dortania](https://github.com/dortania) : Opencore guide
- https://github.com/SchmockLord/Hackintosh-Intel-i9-10900k-Gigabyte-Z490-Vision-D
- https://www.tonymacx86.com/threads/ohchangs-build-gigabyte-z590-vision-g-i7-10700k-amd-rx580.310986/page-18#post-2283363: Intel I225-V Ethernet Adapter
- https://dortania.github.io/OpenCore-Install-Guide/ktext.html#ethernet: Vendor Id of Intel 82574L
- https://github.com/BrushXue/EP2C602-4LD16-E5-2667v2-Hackintosh: Intel 82574L Ethernet Adapter
- https://www.tonymacx86.com/threads/guide-asrock-rack-ep2c602.289060/: Intel 82574 Ethernet Adapter