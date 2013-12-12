---
layout: post
title: "aqemu on Ubuntu 13.10"
description: ""
category: 
tags: [Ubuntu]
---
{% include JB/setup %}

aqemu does not work very well on Ubuntu 13.10. There is a bug which could cause
a segmentation fault.

## Condition
+ Ubuntu 13.10 64bit
+ aqemu 0.8.2
+ qemu 1.5.0

## Bug
aqmeu uses an arbitrary list of qemu/kvm versions in 
Edit_Emulator_Version_Window.cpp


    // Add Items
    if( Emulators[currentRow].Get_Type() == VM::QEMU )
    {
        ui.CB_Versions->clear();

        ui.CB_Versions->addItem( tr("QEMU 0.9.0") );
        ui.CB_Versions->addItem( tr("QEMU 0.9.1") );
        ui.CB_Versions->addItem( tr("QEMU 0.10.X") );
        ui.CB_Versions->addItem( tr("QEMU 0.11.X") );
        ui.CB_Versions->addItem( tr("QEMU 0.12.X") );
        ui.CB_Versions->addItem( tr("QEMU 0.13.X") );
    }
    else if( Emulators[currentRow].Get_Type() == VM::KVM )
    {
        ui.CB_Versions->clear();

        ui.CB_Versions->addItem( tr("KVM 7X") );
        ui.CB_Versions->addItem( tr("KVM 8X") );
        ui.CB_Versions->addItem( tr("KVM 0.11.X") );
        ui.CB_Versions->addItem( tr("KVM 0.12.X") );
        ui.CB_Versions->addItem( tr("KVM 0.13.X") );
    }


## Solution
1. If your aqemu does not work already, remove `~/.config/ANDronSoft` first.
2. Run aqemu. In the "First Start Wizard", do not search Emulators and 
   directly click "next" in the "Find Emulators" page.
3. When the main window shows up, go to `File->Advanced Settings` menu.
   Change the settings as the following image.
   
   ![AdvancedSettings](/assets/images/AdvancedSettings.png)


