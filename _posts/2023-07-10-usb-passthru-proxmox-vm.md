---
layout: post
title: "Pass USB WiFi adapter into Proxmox VM"
categories: techtuts
---
> Using TP-Link TL-WN722N

<div class="centre-h2"> <img src="{{ site.baseurl }}{{ site.baseurl }}\assets\images\2023-07-10-usb-passthru-proxmox-vm\51YRuNnOOxL._AC_UF894,1000_QL80_.jpg" width="200" height="auto"> </div>

The GUI way of adding a USB device to a Proxmox VM didn't work for me when using a USB network adapter (the device id was not showing up when trying to add to the VM via the GUI), so here is a simple manual workaround.

1. Plug in your desired USB device into the physical machine you're running Proxmox on.

2. Using the CLI on the Proxmox host machine (recommended to use ssh/webGUI CLI), list all connected USB devices with `lsusb`:

    ![Alt text]({{ site.baseurl }}\assets\images\2023-07-10-usb-passthru-proxmox-vm\92c790c2-8df1-4fe9-b207-8822f3458801.png)

3. Note the ID of the desired device. In this case the `TP-Link TL-WN722`, with ID: `2357:010c`

4. Ensure the desired Proxmox VM that you want to pass the USB device to is powered off, and take note of its number (`104` in the below image):

    ![Alt text]({{ site.baseurl }}\assets\images\2023-07-10-usb-passthru-proxmox-vm\1198292a-3a93-4620-9c7f-27daa746d07e.png)

5. Still on the Proxmox host machine, run the following command to pass the USB device through to one or more of your virtual machines:

    `qm set [VM#] -usb0 host=[host-id]`

    e.g. for VM `#104` & host id `2357:010c`, I would run:

    `qm set 104 -usb0 host=2357:010c`

    **Source:** [*Proxmox documentation*](https://pve.proxmox.com/wiki/USB_Devices_in_Virtual_Machines)

6. Boot up your Proxmox VM (in my case, VM `#104`) and run `lsusb` in using the CLI. You should now see the USB device that you just passed through (`2357:010c` for me) in there!

    ![Alt text]({{ site.baseurl }}\assets\images\2023-07-10-usb-passthru-proxmox-vm\649e3f6b-48ca-4f50-a9b8-7189d64a135b.png)

## Hope this helps a few other fellow lost souls! ^^