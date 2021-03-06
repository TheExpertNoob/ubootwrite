ubootwrite
========
Is a simple python tool that uploads binary images to the RAM of linux systems running the U-Boot bootloader (e.g. for OpenWRT) via the serial port. It works in cases where no transfer via alternate methods (XMODEM/TFTP/BOOTM/etc.) can be made. ubootwrite has a high overhead, as the binary file is converted to ASCII and sent in 32Bit chucks, so transferring larger amounts of data can be really slow.
The original author is "pgid69" and the original source is the [OpenWRT forum](https://forum.openwrt.org/viewtopic.php?pid=183315#p183315). The initial commit is the original version found in the forum and "pgid69" states the tool is based on [brntool](https://github.com/rvalles/brntool). I "forked" the code here from https://github.com/HorstBaerbel/ubootwrite.

License
========
Basically it can only be [GPLv3](http://opensource.org/licenses/GPL-3.0), because [brntool](https://github.com/rvalles/brntool) is. See [LICENSE.md](LICENSE.md).

Overview
========
Use Python (preferably 3.0+) to run the tool: ```python ubootwrite.py [OPTIONS]```.  
**Options:**  
* ```--verbose``` - Be verbose.  
* ```--serial``` - The serial port to write to, e.g. ```--serial=/dev/ttyUSB2```. It will open it with 115200 Baud, 8 data bits, one stop bit.  
* ```--write``` - The file to transfer to RAM, e.g. ```--write=openwrt-squashfs.image```.  
* ```--addr``` - The RAM start address to write to, e.g. ```--addr=0x80050000```.  
* ```--size``` - The number of bytes to transfer, e.g. ```--size=12345```. Omit to transfer the whole file.

**An example for a full command line could be (zsun):**  
```python ubootwrite.py --serial=/dev/ttyUSB0 --write=sysupgrade.image --addr=0x80060000```  
This can take a looong time(5 hours for zsun). Be patient. Once you have the data in RAM you can copy it to flash by:  
Unprotecting flash: ```protect off all```  
Erasing the sectors: ```erase 9f020000 +${filesize}``` (all in hex)  
Copying the data to flash: ```cp.b 80060000 9f020000 ${filesize}``` (all in hex)  

FAQ
========
**Q:** I'm on linux and I can not access the serial port somehow...  
**A:** You might need to root: ```sudo bash```

Dependencies
========
* python
* python-serial

I found a bug or have a suggestion
========
The best way to report a bug or suggest something is to post an issue on GitHub. Try to make it simple, but descriptive and add ALL the information needed to REPRODUCE the bug. **"Does not work" is not enough!** If you can not compile, please state your system, compiler version, etc! You can also contact me via email if you want to.
