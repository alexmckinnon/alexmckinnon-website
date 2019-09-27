---
title: The Day That Windows Died
date: "2019-08-01"
template: "post"
draft: false
slug: "/posts/the-day-that-windows-died/"
category: "Software"
tags:
  - "Software"
  - "Windows"
description: "I recently replaced my laptop hard drive with an SSD... and re-installed Windows. I was looking forward to an SSD upgrade and ideally a graceful migration. Windows had other plans. "
---


I recently replaced my laptop hard drive with an SSD... and re-installed Windows. I was looking forward to an SSD upgrade and ideally a graceful migration. Windows had other plans. 

### Initial State

The list of issues with the laptop before the upgrade:

1. Dual boot (Windows 10 and Fedora 26)
2. More software and services installed/running then I know of or need
3. Remnants of original Windows user name
4. Chaotic file management
5. Issues with wireless and TAP adapter
6. Wifi speed issues

Most of the issues were self-inflicted: 

1. Dual-boot wasn't needed as I never use Fedora anymore.
2. I installed too many things over the years that are no longer needed but I never cleaned things up.
3. I renamed the Windows user but the original name seems to be heavily tied to things and remains.
4. I had unorganized files and projects scattered in multiple locations.
5. I had constant issues with the wireless adapter requiring repair after each boot, as well as issues with the TAP adapter that needed to be re-installed almost every time I needed to connect to a VPN for work. 
6. My wifi speed was half of what I got over a wired connection (around 35mbps compared to 80mbps). 

I did resolve the wifi speed issue by removing the SmartByte application that came pre-installed by Dell on the laptop. The plan for the remaining issues: ensure files are backed up and do a factory restore.

### Round One

1. **Back up files**. Backed up all files to USB (note for future Alex: do backups better)
2. **Reinstall Windows**. Used Windows Recovery tool to remove everything and reinstall Windows
3. **Set up laptop**: 
   - removed unwanted factory installed software
   - configured desktop, taskbar, start menu, etc
   - installed applications
   - took notes throughout process to refine setup and reference in the future

#### Troubled Waters Ahead

I encountered issues installing one application - it required .Net 4.7.2. I couldn't install 4.7.2 due to 'not supported on this OS' error. 

It looks like the Windows 10 version I reset to is well behind and needs a lot of updates. This did not go well. 

#### Updates Betrayel 

After a number of attempts to update Windows unsuccessfully I found some helpful info regarding running the Windows Update troubleshooter. I ran that, it fixed things. Lets try again.... 

Blue Screen

The update failed. Unable to launch Windows. Keeps going into Automatic Repair on boot. Cannot log in to do any repair. Invalid password.

(╯°□°）╯︵ ┻━┻

The solution here seems to be: use Windows installation disk to repair or restore.

### Round Two

I was planning on upgrading to an SSD fairly soon, so seeing as I may need to reset the laptop again now seems like a good time.

1. **Create bootable Windows USB.** I used a different laptop to create a bootable USB using the latest Windows 10 iso. This required modifying the browser user agent to be able to download Windows from Microsoft's website.
2. **Buy SSD**. I live in a small city (pop. 90,000) with limited choices for buying parts in-store. Luckily the local BestBuy has the SSD I was planning on getting and it's on sale.
3. **Install SSD**. This was easy:
   - Remove bottom panel (2 screws)
   - Remove hard drive (4 screws + 1 cable)
   - Remove hard drive from caddy (4 screws)
   - Do reverse with new drive
4. **Install Windows**. Inserted USB. Started laptop. Installed Windows
5. **Set up laptop, again**. Only a week went by between round one and round two and I hadn't done anything meaningful, so I just went through the process of installing all the applications and setting everything up again.

#### Takeaways

1. **Back up your data**. I managed back up my files onto a USB drive at the start and I had most of the important files synced to cloud storage. This was just luck - I did not have reliable backups though.
2. **Keep things clean**. Don't install it if you don't need it. Uninstall it if you don't need it anymore. Keep filesystem organized. Remove unneeded files (specifically temp files). 
3. **Delete Factory Bloatware**. I didn't realize how much they sneak in there until seeing the difference between installing Windows 10 using the Microsoft iso compared to the factory restore. I don't need any of that.
4. **Windows USB**. Keep a bootable Windows USB on hand to use for recovery/repair if needed.

