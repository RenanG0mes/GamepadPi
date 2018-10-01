# GamepadPi
A functional emulation device made with Raspberry Pi Zero 


**Materials used**
	
	- Raspberry Pi Zero W
	- Copper clad board (if you don't have access to a cnc milling machine you can probably improvise with one of these already perforated prototyping boards)
	- 12 tactile switches (6mm x 6 mm)
	- 2x20 male header
	- 2x20 female header
	- 8 screws (M2 x 5mm) (and maybe M2 nuts)
	
**Equipment Used**
	
	- 3D printer and filament
	- PCB cnc milling machine
	- PCB soldering equipment
	- Some sandpaper
	
	
**Instructions**

	1 - Printing the gamepad case
	
	The 3D CAD files can be found in the folder "3D CAD files", the drawings were made using Fusion 360 software and the files are avaliable in .f3d and .step. Use your preferred slicer to generate the files for you 3D printer.
	There is a second option which is a smaller, wooden made, version of the case, files can be found in this same folder.
	
	2 - Gamepad PCB board
	
	The gerber files to mill the board can be foun in the folder "PCB board". If you don't have access to a cnc milling machine you can probably improvise with one of these already perforated prototyping boards, but you're going to have to figure out how to place buttons at the right distances one to another.
	The files for the smaller version of the game pad can be found here too.
	The tactile switches should be soldered at one of the sides of the board and the female header sould be soldered to the opposite side.
	Jumper cables were used to connect the shouder buttons to the board.
	The male header should be soldered to the Raspberry Pi Zero.
	Pictures of the soldering can be found at the "General pictures" folder
	
 	3 - Mounting
 
 	Use the screws to fix the Raspberry Pi to the botton of the case and the PCB board to the top of the case (don't forget to put the buttons first). Dependind on the kind of screws you are using, if you can't get the threads of the screws inside de plastic you can use some instant glue to glue nuts into the holes of the fixing points and then attach the screws to the nuts.
 	After mounting it all you should put the top and botton together, the two headers should connect.
 	As my 3D printing job was not very good, it was necessary to use sandpaper to smooth some imperfections so the two pieces os the case could snap together and the buttons work correctly.
	
	4 - Installing and configuring the system
		
		(Instruction for Ubuntu distribution)
		
		4.1 - Installing retropie
		Download retropie image from https://retropie.org.uk/download/
		
		Extract: 
			gunzip retropie-4.X.X-rpi2_rpi3.img.gz  (X correnponds to the version you downloaded)
			
		4.2 - Discover the SD card mount point
		Run "lsblk" to see which devices are currently connected to your machine.
		Insert the SD card into an SD card reader, then connect the reader to your computer.
		Run "lsblk" again. The new device that has appeared is your SD card.
		If any partitions on the SD card have been mounted, unmount them all with "umount", for example "umount /dev/sdX1"
		
		4.3 - Copy the image to SD card using "dd" command.
			"sudo dd bs=4M if=NAMEOFTHERETROPIEIMAGE.img of=/dev/sdX status=progress conv=fsync"
			
		4.4	- Run "sync" and remove
		Run "sync". This will ensure that the write cache is flushed and that it is safe to unmount your SD card.
		Remove the SD card from the card reader.
		
		4.5 (optional) - Enable SSH and connect to Wi-Fi
		If you have a micro usb to usb adapter, you can connect a keyboard and mouse and skip this part, if you don't we're gonna have to enable SSH and connect to a wi-fi network so we will be able to control our raspberry pi through another machine.
		
		To enable SSH create an empty file, WITH NO EXTENSION in the boot directory called "ssh".
		
		To connect to a wi-fi network create a file named "wpa_supplicant.conf", the content of this file should be as it follows (change your country and network credentials):
		
		country=BR
		ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
		update_config=1
		network={
			ssid="MyWiFiNetwork"
			psk="YourPassword"
			key_mgmt=WPA-PSK
		}
		
		Now your raspberry pi will connet to the wi-fi network and you can control it via SSH.
		
		Install adafruit's retrogame library running the above commands:
		
		cd
		curl https://raw.githubusercontent.com/adafruit/Raspberry-Pi-Installer-Scripts/master/retrogame.sh >retrogame.sh
		sudo bash retrogame.sh
		
		Select "PiGRRL 2"
		
		More info can be found at: https://learn.adafruit.com/retro-gaming-with-raspberry-pi?view=all#adding-controls-software
		
		Use nano to edit /boot/retrogame.cfg
		The file retrogame.cfg should be as it follows:
		
		# Sample configuration file for retrogame.
		# Really minimal syntax, typically two elements per line w/space delimiter:
		# 1) a key name (from keyTable.h; shortened from /usr/include/linux/input.h).
		# 2) a GPIO pin number; when grounded, will simulate corresponding keypress.
		# Uses Broadcom pin numbers for GPIO.
		# If first element is GND, the corresponding pin (or pins, multiple can be
		# given) is a LOW-level output; an extra ground pin for connecting buttons.
		# A '#' character indicates a comment to end-of-line.
		# File can be edited "live," no need to restart retrogame!

		# Here's a pin configuration for the PiGRRL 2 project:

		LEFT      21  # Joypad left
		RIGHT     20  # Joypad right
		UP        16  # Joypad up
		DOWN      12  # Joypad down
		LEFTCTRL  25  # 'A' button
		LEFTALT   24  # 'B' button
		Z         23  # 'X' button
		X         26  # 'Y' button
		SPACE     19  # 'Select' button
		ENTER     13  # 'Start' button
		A         06  # Left shoulder button
		S         05  # Right shoulder button
		ESC       17  # Exit ROM; PiTFT Button 1
		1         22  # PiTFT Button 2
		2         23  # PiTFT Button 3
		3         23  # PiTFT Button 4

		# For configurations with few buttons (e.g. Cupcade), a key can be followed
		# by multiple pin numbers.  When those pins are all held for a few seconds,
		# this will generate the corresponding keypress (e.g. ESC to exit ROM).
		# Only ONE such combo is supported within the file though; later entries
		# will override earlier.

		
