#U-Boot Readme
##Info:
To flash the U-Boot image, use the `dd` utility directly to the main disk. 
For example, if the main disk (e.g. SD card) is `disk1` on your system, unmount any partitions and run this in a command line:
	
	$ cd U-Boot_UDOO
	$ sudo dd if=u-boot.bin of=/dev/disk1 bs=512 seek=2 skip=2



<br><br><a name="cheatsheet"></a>
#U-Boot File/Folder Cheat Sheet
###This section lists significant files and what they do, or what to do with them. I have tried to indicate what files I have changed.
Hierarchy:

*	board/				[Files specific to platforms]
	*	freescale/		[Freescale platform]
		*	mx6\_udoo/	[UDOO board]
*	include/			[Header files for UDOO]
	*	asm-arm/		[ARM]
		*	arch-mx6/	[i.MX6 architecture]
	*	configs/		[Configuration files for a number of different platforms/boards]
*	lib\_arm/			[Files specific to booting ARM systems]
*	common/
	*	This folder contains the source code for all the commands that can be used while in the U-Boot bootloader before the boot sequence. Additionally, it contains some important files used throughout the process. 
*	tools/				[Tools for compiling]

<br><br>

Files:

*	/board/
	*	freescale/
		*	mx6\_udoo/
			*	mx6_udoo.h
				* This is the main U-Boot UART switch file for both Dual-Core and Quad-Core platforms. It 
				essentially runs the entire U-Boot overarching sequence, but there is a def at the top 
				[CONFIGJ\_USE\_UART\_(x)]. Use this switch to flip between UARTs 2 and 4. 
				<br><br>

*	/include/
	*	config_aditions.h
		*	A custom added file for additional configuration options
		<br><br>
	*	asm-arm/
		*	arch-mx6/
			*	mx6.h
				*	initializes various memory locations for...everything. [added two clocks for UARTS]
				<br><br>

	*	configs/
		*	mx6dl_udoo.h
			*	(DUAL-CORE) This is the (most important file, but really the) universal U-Boot configuration 
			file for the platform. Make all changes for U-Boot here, including config options and default 
			environment variables for U-Boot
			<br><br>

		*	mx6q_udoo.h
			*	(QUAD-CORE) This is the universal U-Boot configuration file for the platform. Make all 
			changes for U-Boot here.
			<br><br>

*	/lib\_arm/
	*	board.c
		*	showrunner, executes the whole sequence
		*	insert whatever printf lines here
	<br><br>	

*	/common/
	*	console.c
		*	console multiplexer, starts the sequence, don't modify!
<br><br>
	*	cmd_udooconfig.c
		*	This is a custom-built command specifically for the UDOO. It can modify memory allocation (GPU-CPU), change the clock speed, etc. [I find that the attributes that the utility can change can be done by hand a LOT easier than the utility, which has known to cause boot stalls due to a mis-imprinted env var for the startup partiton].
	*	iomux.c
		*	CONFIG\_SERIAL_MULTI is activated here
<br><br>
	*	serial.h
		*	This file was moved here after a compiler error
<br><br>

*	/tools/
	*	[crc32.c, md5.c, sha1.c]
		*	These files were added to the 'tools' folder after a compiler error. This may or may not be an isolated issue. 
<br><br>
*	/compile.sh
	*	UDOO-tailored make-file executor
<br><br>

*	/Makefile
	*	Custom Makefile for our purposes
<br><br>
*	/MAKEALL
	*	Never use this. Ever. Don't do it. It makes EVERYTHING, and we have removed a lot of the rules and 
	dependencies. 
	
<br>
**Jake's really dumb solution to change UARTs: To change default UARTs, do a global search for UARTx and ttymxc(x-1) and imx.uart(x-1).**