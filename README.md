# 9343_NVME

Read **EVERYTHING** before attempting, especially if you haven't flashed or modified firmware before. All of this is at your own risk and you are responsible if your system dies, catches fire, or your cat stages a coup.
This will add basic support for NVME drives on the Dell XPS 13, model 9343, and likely other laptops sporting unpatched Intel BootGuard.
There are very likely some small issues with this method, or at least missing features one may expect when compared to systems shipped with full NVME support.
Short of exploiting the write protect on this system, you'll neeed to disassemble and flash with a programmer. Not sure if anyone has done this, but if you know how to without disassembly of your system, you can use that method instead. 
With some basic knowledge of how to remove the bottom panel, and a Google search on using a SOIC clip, you should be able to manage this modification. 

This *Shouldn't*, but may change or break existing features.
This *Will* neuter Intel BootGuard.
Works on other systems, but will require a different method on systems with updated BootGuard PEI. Worst case scenario you can flash your dump back if it doesn't work.

**You will need:**
T5 Torx driver
Philips #0 or similar sized driver
SOIC8 programming clip
SOIC8 to DIP8 adapter
SPI programmer (a CH341a for about $10 on Amazon works great for this, and comes with the clip and adapter.)
A BIOS dump utility of your choice. This one works well for Windows - https://github.com/nofeletru/UsbAsp-flash/releases
UEFITool Old Engine https://github.com/LongSoft/UEFITool/releases/tag/0.28.0
NvmExpressDXE_Small.ffs (credit to Ethaniel at Win-Raid for creating this, link to the relevant forum - https://www.win-raid.com/t3557f13-Small-NvmExpressDxe-driver.html)

*there are alternatives to this flash method should you not have this on hand, or have other suitable options available to you, and know what you're doing.
Example A20 BIOS and the necessary UEFI DXE driver are included in this repo.*

**Steps:**
1) Remove the bottom cover of your system. https://www.ifixit.com/Guide/Dell+XPS+13+Back+Cover+Replacement/88927
  1A) Disconnect the main battery connector from the board. The CMOS battery can remain connected.
2) Move the tape covering the Winbond flash chip out of the way, and attach your clip. https://www.ifixit.com/Teardown/Dell+XPS+13+Teardown/36157#s80987
3) Dump your BIOS, save it under any name and file extension, (eg. "dump.rom".)
  3A) Dump your BIOS twice more and save the files again with new names (eg. "dump2.rom", "dump3.rom"
  3B) Upload each to http://onlinemd5.com/ and make sure they are identical, or hash locally.
4) Open image in UEFITool.
5) Ctrl+F > Text tab > "bootguard" > Ok
6) Double click "...found in User Interface section..." > Right click "BootGuardDXE" > Replace as is... > select NvmExpressDxe_Small
7) Ctrl+S > save your rom with a new name (eg. "nvme.rom")
8) Flash your modified BIOS back to the board.
9) Remove the clip, connect the main battery, and press power. You will likely need to press power a few times, As the system will reboot a few times initially. Don't panic, just keep trying the power button and waiting for the screen to power on. If you never get successful boot after about two minutes, try removing both the main and RTC batteries for 30 seconds, then connecting them back and retrying. If replacing bootguard doesn't work, remove it and place the nvme driver at the end of the DXE section. Failing this, flash your old BIOS back.
10) If it boots and you can install and boot your OS of choice off of the new NVME drive in GPT/UEFI mode, then congrats! Put the tape back in place, make sure both batteries are connected, and put the lower cover back on.
