[![](https://img.shields.io/badge/Gitter%20Ice%20Lake-Chat-informational?style=flat&logo=gitter&logoColor=white&color=ed1965)](https://gitter.im/ICE-LAKE-HACKINTOSH-DEVELOPMENT/community)
[![](https://img.shields.io/badge/EFI-Release-informational?style=flat&logo=apple&logoColor=white&color=9debeb)](https://github.com/Baio1977/EFI-Varie-Hackintosh)
[![](https://img.shields.io/badge/Telegram-HackintoshLifeIT-informational?style=flat&logo=telegram&logoColor=white&color=5fb659)](https://t.me/HackintoshLife_it)
[![](https://img.shields.io/badge/Facebook-HackintoshLifeIT-informational?style=flat&logo=facebook&logoColor=white&color=3a4dc9)](https://www.facebook.com/hackintoshlife/)
[![](https://img.shields.io/badge/Instagram-HackintoshLifeIT-informational?style=flat&logo=instagram&logoColor=white&color=8a178a)](https://www.instagram.com/hackintoshlife.it_official/)

# Lenovo ThinkPad X1 Tablet 3°Gen Type 20KK Hackintosh 

![Lenovo](./Screenshot/1.jpg)
![Lenovo](./Screenshot/2.jpg)
 
## Specification:

- CPU: 1.6GHz Intel Core i5-8250U (Kaby Lake-R)
- Memory: 2x4GB 1,867MHz LPDDR3
- Harddrive: 256GB PCIe-NVMe M.2 SSD
- Display: 13-inch IPS-Touch Screen (3000×2000) 
- GPU: Intel UHD 620
- Camera: Front: 2MP, rear: 8MP
- WLAN: Intel dual-band 8265 Wireless 802.11ac (2 x 2) & Bluetooth 4.1
- Battery: Integrate Li-Polymer 42Wh internal battery
- Audio: Realtek HDA ALC295
- 2 x USB-C/Thunderbolt 3 Alpine Ridge (power delivery, DisplayPort, data transfer)
- NanoSIM card/microSD combo slot
- Headphone / mic combo 

## BIOS Settings:V 1.46

The bios must be properly configured prior to installing macOS.
In Security menu, set the following settings:

-  `Security > Security Chip`: must be **Disabled**
-  `Memory Protection > Execution Prevention`: must be **Enabled**
-  `Virtualization > Intel Virtualization Technology`: must be **Enabled**
-  `Virtualization > Intel VT-d Feature`: must be **Enabled**
-  `Anti-Theft > Computrace -> Current Setting`: must be **Disabled**
-  `Secure Boot > Secure Boot`: must be **Disabled**
-  `Intel SGX -> Intel SGX Control`: must be **Disabled**
-  `Device Guard`: must be **Disabled**

In Startup menu, set the following options:

-  `UEFI/Legacy Boot`: **UEFI Only**
-  `CSM Support`: **No**

In Thunderbolt menu, set the following options:

-  `Thunderbolt BIOS Assist Mode`: **UEFI Only**
-  `Wake by Thunderbolt(TM) 3`: **No**
-  `Security Level`: **No**
-  `Support in Pre Boot Environment > Thunderbolt(TM) device`: **No**

In Display menu, set the following options:
    
-  `Boot Display Device` : **LCD**
-  `Boot Time Extension` : **Disabled**	   
   
## Working:

 - Keyboard (including all Fn keys)
 - Trackpad with gestures / Trackstick
 - Battery indicator
 - Audio (Internal and headphone jack)
 - Microphone
 - Ethernet
 - GPU acceleration
 - Camera
 - Intel Wireless / Bluetooth
 - Sleep / Wake
 - Native CPU power management
 - MicroSD card reader
 - HDMI video and audio 
 - Thunderbolt JHL6540 Alpine Ridge Work whit HotPlug 
 
## Not Work:

 - Display Internal No work Bios blocked, (impossible to adapt DVMT to 128 to be able to display 3k or higher signals)

## USB Map:

![Lenovo](./Screenshot/3.png)

## Video Output:

![Lenovo](./Screenshot/4.png)

## SSDT Full Hack

![Lenovo](./Screenshot/5.png)
![Lenovo](./Screenshot/6.png)

SSDT TB3 : bundled with _DSM with real Mac device properties

```
    Scope (\)
    {
        Scope (_SB)
        {
            Scope (PCI0)
            {
                If (_OSI ("Darwin"))
                {
                    Scope (RP09)
                    {
                        Scope (PXSX)
                        {
                            Name (_STA, Zero)  // _STA: Status
                        }

                        Method (_RMV, 0, NotSerialized)  // _RMV: Removal Status
                        {
                            Return (Zero)
                        }

                        Device (UPSB)
                        {
                            Name (_ADR, Zero)  // _ADR: Address
                            OperationRegion (A1E0, PCI_Config, Zero, 0x40)
                            Field (A1E0, ByteAcc, NoLock, Preserve)
                            {
                                AVND,   32, 
                                BMIE,   3, 
                                Offset (0x18), 
                                PRIB,   8, 
                                SECB,   8, 
                                SUBB,   8, 
                                Offset (0x1E), 
                                    ,   13, 
                                MABT,   1
                            }

                            Method (_BBN, 0, NotSerialized)  // _BBN: BIOS Bus Number
                            {
                                Return (SECB) /* \_SB_.PCI0.RP09.UPSB.SECB */
                            }

                            Method (_STA, 0, NotSerialized)  // _STA: Status
                            {
                                Return (0x0F)
                            }

                            Method (_RMV, 0, NotSerialized)  // _RMV: Removal Status
                            {
                                Return (Zero)
                            }

                            Method (_DSM, 4, NotSerialized)  // _DSM: Device-Specific Method
                            {
                                If ((Arg0 == ToUUID ("a0b5b7c6-1318-441c-b0c9-fe695eaf949b") /* Unknown UUID */))
                                {
                                    Local0 = Package (0x0A)
                                        {
                                            "AAPL,slot-name", 
                                            Buffer (0x0C)
                                            {
                                                "Thunderbolt"
                                            }, 

                                            "built-in", 
                                            Buffer (One)
                                            {
                                                 0x00                                             // .
                                            }, 

                                            "model", 
                                            Buffer (0x41)
                                            {
                                                "Intel JHL6240 Alpine Ridge Thunderbolt 3 UPSB Bridge (Low Power)"
                                            }, 

                                            "device_type", 
                                            Buffer (0x0B)
                                            {
                                                "PCI Bridge"
                                            }, 

                                            "PCI-Thunderbolt", 
                                            One
                                        }
                                    DTGP (Arg0, Arg1, Arg2, Arg3, RefOf (Local0))
                                    Return (Local0)
                                }

                                Return (Zero)
                            }

                            Device (DSB0)
                            {
                                Name (_ADR, Zero)  // _ADR: Address
                                OperationRegion (A1E0, PCI_Config, Zero, 0x40)
                                Field (A1E0, ByteAcc, NoLock, Preserve)
                                {
                                    AVND,   32, 
                                    BMIE,   3, 
                                    Offset (0x18), 
                                    PRIB,   8, 
                                    SECB,   8, 
                                    SUBB,   8, 
                                    Offset (0x1E), 
                                        ,   13, 
                                    MABT,   1
                                }

                                Method (_BBN, 0, NotSerialized)  // _BBN: BIOS Bus Number
                                {
                                    Return (SECB) /* \_SB_.PCI0.RP09.UPSB.DSB0.SECB */
                                }

                                Method (_STA, 0, NotSerialized)  // _STA: Status
                                {
                                    Return (0x0F)
                                }

                                Method (_RMV, 0, NotSerialized)  // _RMV: Removal Status
                                {
                                    Return (Zero)
                                }

                                Method (_DSM, 4, NotSerialized)  // _DSM: Device-Specific Method
                                {
                                    If ((Arg0 == ToUUID ("a0b5b7c6-1318-441c-b0c9-fe695eaf949b") /* Unknown UUID */))
                                    {
                                        Local0 = Package (0x0A)
                                            {
                                                "AAPL,slot-name", 
                                                Buffer (0x0C)
                                                {
                                                    "Thunderbolt"
                                                }, 

                                                "built-in", 
                                                Buffer (One)
                                                {
                                                     0x00                                             // .
                                                }, 

                                                "model", 
                                                Buffer (0x41)
                                                {
                                                    "Intel JHL6240 Alpine Ridge Thunderbolt 3 DSB0 Bridge (Low Power)"
                                                }, 

                                                "device_type", 
                                                Buffer (0x0B)
                                                {
                                                    "PCI Bridge"
                                                }, 

                                                "PCIHotplugCapable", 
                                                Zero
                                            }
                                        DTGP (Arg0, Arg1, Arg2, Arg3, RefOf (Local0))
                                        Return (Local0)
                                    }

                                    Return (Zero)
                                }

                                Device (NHI0)
                                {
                                    Name (_ADR, Zero)  // _ADR: Address
                                    Name (_STR, Unicode ("Thunderbolt"))  // _STR: Description String
                                    Method (_RMV, 0, NotSerialized)  // _RMV: Removal Status
                                    {
                                        Return (Zero)
                                    }

                                    Method (_DSM, 4, NotSerialized)  // _DSM: Device-Specific Method
                                    {
                                        Local0 = Package (0x1B)
                                            {
                                                "AAPL,slot-name", 
                                                Buffer (0x0C)
                                                {
                                                    "Thunderbolt"
                                                }, 

                                                "name", 
                                                Buffer (0x24)
                                                {
                                                    "Alpine Ridge Thunderbolt Controller"
                                                }, 

                                                "model", 
                                                Buffer (0x3A)
                                                {
                                                    "Intel JHL6240 Alpine Ridge Thunderbolt 3 NHI0 (Low Power)"
                                                }, 

                                                "device_type", 
                                                Buffer (0x12)
                                                {
                                                    "System Peripheral"
                                                }, 

                                                "ThunderboltDROM", 
                                                Buffer (0x6A)
                                                {
                                                    /* 0000 */  0x63, 0x00, 0x06, 0xBB, 0xAD, 0xA2, 0x93, 0x78,  // c......x
                                                    /* 0008 */  0x2D, 0xEB, 0x97, 0xAB, 0x14, 0x01, 0x5D, 0x00,  // -.....].
                                                    /* 0010 */  0x01, 0x00, 0x10, 0x00, 0x01, 0x00, 0x08, 0x81,  // ........
                                                    /* 0018 */  0x80, 0x02, 0x80, 0x00, 0x00, 0x00, 0x08, 0x82,  // ........
                                                    /* 0020 */  0x90, 0x01, 0x80, 0x00, 0x00, 0x00, 0x08, 0x83,  // ........
                                                    /* 0028 */  0x80, 0x04, 0x80, 0x01, 0x00, 0x00, 0x08, 0x84,  // ........
                                                    /* 0030 */  0x90, 0x03, 0x80, 0x01, 0x00, 0x00, 0x05, 0x85,  // ........
                                                    /* 0038 */  0x09, 0x01, 0x00, 0x05, 0x86, 0x09, 0x01, 0x00,  // ........
                                                    /* 0040 */  0x02, 0x87, 0x03, 0x88, 0x20, 0x03, 0x89, 0x80,  // .... ...
                                                    /* 0048 */  0x02, 0xCA, 0x02, 0xCB, 0x08, 0x01, 0x61, 0x70,  // ......ap
                                                    /* 0050 */  0x70, 0x6C, 0x65, 0x00, 0x16, 0x02, 0x4C, 0x65,  // ple...Le
                                                    /* 0058 */  0x6E, 0x6F, 0x76, 0x6F, 0x20, 0x54, 0x68, 0x69,  // novo Thi
                                                    /* 0060 */  0x6E, 0x6B, 0x70, 0x61, 0x64, 0x20, 0x54, 0x31,  // nkpad T1
                                                    /* 0068 */  0x34, 0x00                                       // 4.
                                                }, 

                                                "ThunderboltConfig", 
                                                Buffer (0x20)
                                                {
                                                    /* 0000 */  0x00, 0x02, 0x1C, 0x00, 0x02, 0x00, 0x05, 0x03,  // ........
                                                    /* 0008 */  0x01, 0x00, 0x04, 0x00, 0x05, 0x03, 0x02, 0x00,  // ........
                                                    /* 0010 */  0x03, 0x00, 0x05, 0x03, 0x01, 0x00, 0x00, 0x00,  // ........
                                                    /* 0018 */  0x03, 0x03, 0x02, 0x00, 0x01, 0x00, 0x02, 0x00   // ........
                                                }, 

                                                "pathcr", 
                                                Buffer (0x50)
                                                {
                                                    /* 0000 */  0x04, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // ........
                                                    /* 0008 */  0x00, 0x00, 0x07, 0x00, 0x10, 0x00, 0x10, 0x00,  // ........
                                                    /* 0010 */  0x05, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // ........
                                                    /* 0018 */  0x00, 0x00, 0x07, 0x00, 0x10, 0x00, 0x10, 0x00,  // ........
                                                    /* 0020 */  0x01, 0x00, 0x00, 0x00, 0x0B, 0x00, 0x0E, 0x00,  // ........
                                                    /* 0028 */  0x0E, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // ........
                                                    /* 0030 */  0x02, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // ........
                                                    /* 0038 */  0x00, 0x00, 0x04, 0x00, 0x02, 0x00, 0x01, 0x00,  // ........
                                                    /* 0040 */  0x03, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x00,  // ........
                                                    /* 0048 */  0x00, 0x00, 0x07, 0x00, 0x02, 0x00, 0x01, 0x00   // ........
                                                }, 

                                                "linkDetails", 
                                                Buffer (0x08)
                                                {
                                                     0x08, 0x00, 0x00, 0x00, 0x03, 0x00, 0x00, 0x00   // ........
                                                }, 

                                                "TBTFlags", 
                                                Buffer (0x04)
                                                {
                                                     0x03, 0x00, 0x00, 0x00                           // ....
                                                }, 

                                                "sscOffset", 
                                                Buffer (0x02)
                                                {
                                                     0x00, 0x07                                       // ..
                                                }, 

                                                "TBTDPLowToHigh", 
                                                Buffer (0x04)
                                                {
                                                     0x01, 0x00, 0x00, 0x00                           // ....
                                                }, 

                                                "ThunderboltUUID", 
                                                ToUUID ("95e6bcfa-5a4a-5f81-b3d2-f0e4bd35cf1e") /* Unknown UUID */, 
                                                "power-save", 
                                                One, 
                                                Buffer (One)
                                                {
                                                     0x00                                             // .
                                                }
                                            }
                                        DTGP (Arg0, Arg1, Arg2, Arg3, RefOf (Local0))
                                        Return (Local0)
                                    }
                                }
                            }

                            Device (DSB1)
                            {
                                Name (_ADR, 0x00010000)  // _ADR: Address
                                Name (_SUN, One)  // _SUN: Slot User Number
                                OperationRegion (A1E0, PCI_Config, Zero, 0x40)
                                Field (A1E0, ByteAcc, NoLock, Preserve)
                                {
                                    AVND,   32, 
                                    BMIE,   3, 
                                    Offset (0x18), 
                                    PRIB,   8, 
                                    SECB,   8, 
                                    SUBB,   8, 
                                    Offset (0x1E), 
                                        ,   13, 
                                    MABT,   1
                                }

                                OperationRegion (A1E1, PCI_Config, 0xC0, 0x40)
                                Field (A1E1, ByteAcc, NoLock, Preserve)
                                {
                                    Offset (0x01), 
                                    Offset (0x02), 
                                    Offset (0x04), 
                                    Offset (0x08), 
                                    Offset (0x0A), 
                                        ,   5, 
                                    TPEN,   1, 
                                    Offset (0x0C), 
                                    SSPD,   4, 
                                        ,   16, 
                                    LACR,   1, 
                                    Offset (0x10), 
                                        ,   4, 
                                    LDIS,   1, 
                                    LRTN,   1, 
                                    Offset (0x12), 
                                    CSPD,   4, 
                                    CWDT,   6, 
                                        ,   1, 
                                    LTRN,   1, 
                                        ,   1, 
                                    LACT,   1, 
                                    Offset (0x14), 
                                    Offset (0x30), 
                                    TSPD,   4
                                }

                                OperationRegion (A1E2, PCI_Config, 0x80, 0x08)
                                Field (A1E2, ByteAcc, NoLock, Preserve)
                                {
                                    Offset (0x01), 
                                    Offset (0x02), 
                                    Offset (0x04), 
                                    PSTA,   2
                                }

                                Method (_BBN, 0, NotSerialized)  // _BBN: BIOS Bus Number
                                {
                                    Return (SECB) /* \_SB_.PCI0.RP09.UPSB.DSB1.SECB */
                                }

                                Method (_STA, 0, NotSerialized)  // _STA: Status
                                {
                                    Return (0x0F)
                                }

                                Method (_RMV, 0, NotSerialized)  // _RMV: Removal Status
                                {
                                    Return (Zero)
                                }

                                Method (_DSM, 4, NotSerialized)  // _DSM: Device-Specific Method
                                {
                                    Local0 = Package (0x06)
                                        {
                                            "AAPL,slot-name", 
                                            Buffer (0x0C)
                                            {
                                                "Thunderbolt"
                                            }, 

                                            "model", 
                                            Buffer (0x41)
                                            {
                                                "Intel JHL6240 Alpine Ridge Thunderbolt 3 DSB1 Bridge (Low Power)"
                                            }, 

                                            "device_type", 
                                            Buffer (0x0B)
                                            {
                                                "PCI Bridge"
                                            }
                                        }
                                    DTGP (Arg0, Arg1, Arg2, Arg3, RefOf (Local0))
                                    Return (Local0)
                                }

                                Device (UPS0)
                                {
                                    Name (_ADR, Zero)  // _ADR: Address
                                    OperationRegion (ARE0, PCI_Config, Zero, 0x04)
                                    Field (ARE0, ByteAcc, NoLock, Preserve)
                                    {
                                        AVND,   16
                                    }

                                    Method (_RMV, 0, NotSerialized)  // _RMV: Removal Status
                                    {
                                        Return (One)
                                    }
                                }
                            }

                            Device (DSB2)
                            {
                                Name (_ADR, 0x00020000)  // _ADR: Address
                                OperationRegion (A1E0, PCI_Config, Zero, 0x40)
                                Field (A1E0, ByteAcc, NoLock, Preserve)
                                {
                                    AVND,   32, 
                                    BMIE,   3, 
                                    Offset (0x18), 
                                    PRIB,   8, 
                                    SECB,   8, 
                                    SUBB,   8, 
                                    Offset (0x1E), 
                                        ,   13, 
                                    MABT,   1
                                }

                                Method (_BBN, 0, NotSerialized)  // _BBN: BIOS Bus Number
                                {
                                    Return (SECB) /* \_SB_.PCI0.RP09.UPSB.DSB2.SECB */
                                }

                                Method (_STA, 0, NotSerialized)  // _STA: Status
                                {
                                    Return (0x0F)
                                }

                                Method (_RMV, 0, NotSerialized)  // _RMV: Removal Status
                                {
                                    Return (Zero)
                                }

                                Method (_DSM, 4, NotSerialized)  // _DSM: Device-Specific Method
                                {
                                    If ((Arg0 == ToUUID ("a0b5b7c6-1318-441c-b0c9-fe695eaf949b") /* Unknown UUID */))
                                    {
                                        Local0 = Package (0x08)
                                            {
                                                "AAPL,slot-name", 
                                                Buffer (0x0C)
                                                {
                                                    "Thunderbolt"
                                                }, 

                                                "model", 
                                                Buffer (0x41)
                                                {
                                                    "Intel JHL6240 Alpine Ridge Thunderbolt 3 DSB2 Bridge (Low Power)"
                                                }, 

                                                "device_type", 
                                                Buffer (0x0B)
                                                {
                                                    "PCI Bridge"
                                                }, 

                                                "PCIHotplugCapable", 
                                                Zero
                                            }
                                        DTGP (Arg0, Arg1, Arg2, Arg3, RefOf (Local0))
                                        Return (Local0)
                                    }

                                    Return (Zero)
                                }

                                Device (XHC2)
                                {
                                    Name (_ADR, Zero)  // _ADR: Address
                                    Method (_PRW, 0, NotSerialized)  // _PRW: Power Resources for Wake
                                    {
                                        Return (Package (0x02)
                                        {
                                            0x69, 
                                            0x03
                                        })
                                    }

                                    Method (_DSM, 4, NotSerialized)  // _DSM: Device-Specific Method
                                    {
                                        Local0 = Package (0x10)
                                            {
                                                "AAPL,slot-name", 
                                                Buffer (0x0C)
                                                {
                                                    "Thunderbolt"
                                                }, 

                                                "built-in", 
                                                Buffer (One)
                                                {
                                                     0x00                                             // .
                                                }, 

                                                "name", 
                                                Buffer (0x20)
                                                {
                                                    "Alpine Ridge USB 3.1 Controller"
                                                }, 

                                                "model", 
                                                Buffer (0x47)
                                                {
                                                    "Intel JHL6240 Alpine Ridge Thunderbolt 3 Type C Controller (Low Power)"
                                                }, 

                                                "device_type", 
                                                Buffer (0x0F)
                                                {
                                                    "USB controller"
                                                }, 

                                                "USBBusNumber", 
                                                Zero, 
                                                "UsbCompanionControllerPresent", 
                                                One, 
                                                "AAPL,XHC-clock-id", 
                                                One
                                            }
                                        DTGP (Arg0, Arg1, Arg2, Arg3, RefOf (Local0))
                                        Return (Local0)
                                    }

                                    Device (RHUB)
                                    {
                                        Name (_ADR, Zero)  // _ADR: Address
                                        Device (SSP1)
                                        {
                                            Name (_ADR, 0x03)  // _ADR: Address
                                            Name (_UPC, Package (0x04)  // _UPC: USB Port Capabilities
                                            {
                                                0xFF, 
                                                0x0A, 
                                                Zero, 
                                                Zero
                                            })
                                            Name (_PLD, Package (0x01)  // _PLD: Physical Location of Device
                                            {
                                                ToPLD (
                                                    PLD_Revision           = 0x1,
                                                    PLD_IgnoreColor        = 0x1,
                                                    PLD_Red                = 0x0,
                                                    PLD_Green              = 0x0,
                                                    PLD_Blue               = 0x0,
                                                    PLD_Width              = 0x0,
                                                    PLD_Height             = 0x0,
                                                    PLD_UserVisible        = 0x1,
                                                    PLD_Dock               = 0x0,
                                                    PLD_Lid                = 0x0,
                                                    PLD_Panel              = "UNKNOWN",
                                                    PLD_VerticalPosition   = "UPPER",
                                                    PLD_HorizontalPosition = "LEFT",
                                                    PLD_Shape              = "UNKNOWN",
                                                    PLD_GroupOrientation   = 0x0,
                                                    PLD_GroupToken         = 0x0,
                                                    PLD_GroupPosition      = 0x0,
                                                    PLD_Bay                = 0x0,
                                                    PLD_Ejectable          = 0x0,
                                                    PLD_EjectRequired      = 0x0,
                                                    PLD_CabinetNumber      = 0x0,
                                                    PLD_CardCageNumber     = 0x0,
                                                    PLD_Reference          = 0x0,
                                                    PLD_Rotation           = 0x0,
                                                    PLD_Order              = 0x0,
                                                    PLD_VerticalOffset     = 0x0,
                                                    PLD_HorizontalOffset   = 0x0)

                                            })
                                            Method (_DSM, 4, NotSerialized)  // _DSM: Device-Specific Method
                                            {
                                                Local0 = Package (0x04)
                                                    {
                                                        "UsbCPortNumber", 
                                                        0x02, 
                                                        "UsbCompanionPortPresent", 
                                                        One
                                                    }
                                                DTGP (Arg0, Arg1, Arg2, Arg3, RefOf (Local0))
                                                Return (Local0)
                                            }
                                        }

                                        Device (SSP2)
                                        {
                                            Name (_ADR, 0x04)  // _ADR: Address
                                            Name (_UPC, Package (0x04)  // _UPC: USB Port Capabilities
                                            {
                                                0xFF, 
                                                0x0A, 
                                                Zero, 
                                                Zero
                                            })
                                            Name (_PLD, Package (0x01)  // _PLD: Physical Location of Device
                                            {
                                                ToPLD (
                                                    PLD_Revision           = 0x1,
                                                    PLD_IgnoreColor        = 0x1,
                                                    PLD_Red                = 0x0,
                                                    PLD_Green              = 0x0,
                                                    PLD_Blue               = 0x0,
                                                    PLD_Width              = 0x0,
                                                    PLD_Height             = 0x0,
                                                    PLD_UserVisible        = 0x1,
                                                    PLD_Dock               = 0x0,
                                                    PLD_Lid                = 0x0,
                                                    PLD_Panel              = "UNKNOWN",
                                                    PLD_VerticalPosition   = "UPPER",
                                                    PLD_HorizontalPosition = "LEFT",
                                                    PLD_Shape              = "UNKNOWN",
                                                    PLD_GroupOrientation   = 0x0,
                                                    PLD_GroupToken         = 0x0,
                                                    PLD_GroupPosition      = 0x0,
                                                    PLD_Bay                = 0x0,
                                                    PLD_Ejectable          = 0x0,
                                                    PLD_EjectRequired      = 0x0,
                                                    PLD_CabinetNumber      = 0x0,
                                                    PLD_CardCageNumber     = 0x0,
                                                    PLD_Reference          = 0x0,
                                                    PLD_Rotation           = 0x0,
                                                    PLD_Order              = 0x0,
                                                    PLD_VerticalOffset     = 0x0,
                                                    PLD_HorizontalOffset   = 0x0)

                                            })
                                            Method (_DSM, 4, NotSerialized)  // _DSM: Device-Specific Method
                                            {
                                                Local0 = Package (0x04)
                                                    {
                                                        "UsbCPortNumber", 
                                                        One, 
                                                        "UsbCompanionPortPresent", 
                                                        One
                                                    }
                                                DTGP (Arg0, Arg1, Arg2, Arg3, RefOf (Local0))
                                                Return (Local0)
                                            }
                                        }
                                    }
                                }
                            }

                            Device (DSB4)
                            {
                                Name (_ADR, 0x00040000)  // _ADR: Address
                                Name (_SUN, 0x02)  // _SUN: Slot User Number
                                OperationRegion (A1E0, PCI_Config, Zero, 0x40)
                                Field (A1E0, ByteAcc, NoLock, Preserve)
                                {
                                    AVND,   32, 
                                    BMIE,   3, 
                                    Offset (0x18), 
                                    PRIB,   8, 
                                    SECB,   8, 
                                    SUBB,   8, 
                                    Offset (0x1E), 
                                        ,   13, 
                                    MABT,   1
                                }

                                OperationRegion (A1E1, PCI_Config, 0xC0, 0x40)
                                Field (A1E1, ByteAcc, NoLock, Preserve)
                                {
                                    Offset (0x01), 
                                    Offset (0x02), 
                                    Offset (0x04), 
                                    Offset (0x08), 
                                    Offset (0x0A), 
                                        ,   5, 
                                    TPEN,   1, 
                                    Offset (0x0C), 
                                    SSPD,   4, 
                                        ,   16, 
                                    LACR,   1, 
                                    Offset (0x10), 
                                        ,   4, 
                                    LDIS,   1, 
                                    LRTN,   1, 
                                    Offset (0x12), 
                                    CSPD,   4, 
                                    CWDT,   6, 
                                        ,   1, 
                                    LTRN,   1, 
                                        ,   1, 
                                    LACT,   1, 
                                    Offset (0x14), 
                                    Offset (0x30), 
                                    TSPD,   4
                                }

                                OperationRegion (A1E2, PCI_Config, 0x80, 0x08)
                                Field (A1E2, ByteAcc, NoLock, Preserve)
                                {
                                    Offset (0x01), 
                                    Offset (0x02), 
                                    Offset (0x04), 
                                    PSTA,   2
                                }

                                Method (_BBN, 0, NotSerialized)  // _BBN: BIOS Bus Number
                                {
                                    Return (SECB) /* \_SB_.PCI0.RP09.UPSB.DSB4.SECB */
                                }

                                Method (_STA, 0, NotSerialized)  // _STA: Status
                                {
                                    Return (0x0F)
                                }

                                Method (_RMV, 0, NotSerialized)  // _RMV: Removal Status
                                {
                                    Return (Zero)
                                }

                                Method (_DSM, 4, NotSerialized)  // _DSM: Device-Specific Method
                                {
                                    Local0 = Package (0x06)
                                        {
                                            "AAPL,slot-name", 
                                            Buffer (0x0C)
                                            {
                                                "Thunderbolt"
                                            }, 

                                            "model", 
                                            Buffer (0x41)
                                            {
                                                "Intel JHL6240 Alpine Ridge Thunderbolt 3 DSB4 Bridge (Low Power)"
                                            }, 

                                            "device_type", 
                                            Buffer (0x0B)
                                            {
                                                "PCI Bridge"
                                            }
                                        }
                                    DTGP (Arg0, Arg1, Arg2, Arg3, RefOf (Local0))
                                    Return (Local0)
                                }

                                Device (UPS0)
                                {
                                    Name (_ADR, Zero)  // _ADR: Address
                                    OperationRegion (ARE0, PCI_Config, Zero, 0x04)
                                    Field (ARE0, ByteAcc, NoLock, Preserve)
                                    {
                                        AVND,   16
                                    }

                                    Method (_RMV, 0, NotSerialized)  // _RMV: Removal Status
                                    {
                                        Return (One)
                                    }
                                }
                            }
                        }
                    }
                }
            }
        }

        Method (DTGP, 5, NotSerialized)
        {
            If ((Arg0 == ToUUID ("a0b5b7c6-1318-441c-b0c9-fe695eaf949b") /* Unknown UUID */))
            {
                If ((Arg1 == One))
                {
                    If ((Arg2 == Zero))
                    {
                        Arg4 = Buffer (One)
                            {
                                 0x03                                             // .
                            }
                        Return (One)
                    }

                    If ((Arg2 == One))
                    {
                        Return (One)
                    }
                }
            }

            Arg4 = Buffer (One)
                {
                     0x00                                             // .
                }
            Return (Zero)
        }
    }
}
```
## YogaSMC Panel

![Lenovo](./Screenshot/7.png)

![Lenovo](./Screenshot/8.png)

![Lenovo](./Screenshot/9.png) 

## Credits

- [Apple](https://apple.com) for macOS.
- [Acidanthera](https://github.com/acidanthera) for OpenCore and all the lovely hackintosh work.
- [Dortania](https://dortania.github.io/OpenCore-Install-Guide/config-laptop.plist/icelake.html) For great and detailed guides.
- [Hackintoshlifeit](https://github.com/Hackintoshlifeit) Support group for installation and post installation.
