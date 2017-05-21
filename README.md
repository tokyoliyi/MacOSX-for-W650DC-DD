# MacOSX 10.12.5 for w650dc/dd

## About kexts
You need
For battery
+ ACPIBatteryManager.kext 
For sound card
+ AppleALC.kext with  layout id 28
For Intel graphics
+ FakePCIID_Intel_HD_Graphics.kext FakePCIID.kext IntelGraphicsFixup.kext Lilu.kext Shiki.kext
For CPU PM (need to disable XCPM if you want HWP works well in kabylake cpu)
+ NULLCPUPowerMangement.kext with FakeCPUID 0x0106e0
For USB
+ USBInjectALL.kext
For keyboard and touchpad
+ VoodooPS2Controller.kext
For Ethernet
+ IntelMausiEthernet.kext RealtekRTL8111.kext
## DSDT PATCHES
### In CLOVER DSDT PATCH
+ change GFX0 to IGPU
+ change HDAS to HDEF
+ change HECI to IMEI
### PATCH DSDT (Use Laptop Patches maintained by RehabMan)
Enable AppleLPC
+ [sys] Skylake LPC
Disable GTX950m
+ [gfx0] Disable from _REG (DSDT)
+ [gfx0] Disable/Enable on _WAK/_PTS (DSDT)
Fix instant wake
+ [usb] USB3 _PRW 0x0D Skylake (instant wake)
### FN Brightness key patch
'''
into method label _Q12 parent_label EC replace_content
begin
                    Store (0x12, P80H)
                    Store (OEM2, Local0)
                    If (LEqual (Local0, One))
                    {
                        Notify (PS2K, 0x0405)
                    }

                    If (LEqual (Local0, 0x03))
                    {
                        Notify (PS2K, 0x0406)
                    }

                    Store (0x02, OEM2)
end;
'''
## Notice
+ You need to generate and patch DSDT by yourself
+ Use ssdtPRGen.sh to generate SSDT for your CPU
+ You can use clover config from me
## Not Working
+ mic 
+ wifi





