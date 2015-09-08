# Note
This is a GNU/Linux driver for the SiS (Silicon Integrated Systems) graphics card. It's only tested with the 662 and 671 models.
Works good and little perfomance issues in 'Linux Mint 17.2' with the 671 chip at full resolution (1280x800).
Forked from: https://gitorious.org/xf86-video-sis671/sis-671-fix

# Build & Install
#### Install necesary tools.

    sudo apt-get install autoconf automake xorg-dev xutils-dev mesa-common-dev libdrm-dev libtool 
    
#### Configure

    autoreconf -i
    ./configure --prefix=/usr --disable-static --disable-dri
    make
    sudo make install
    
#### Config 'xorg.conf'

    sudo nano /etc/X11/xorg.conf
    
Replace 'Driver' in the 'Device' section to
    
    Driver "sisimedia"

# XORG Config for [SiS] 771/671 PCIE
The following config was only tested on a 671 SiS Mirage graphic card integrated in a Fijitsu 'Siemens ESPRIMO Mobile v5535' netbook. Other configuration may apply on different graphic cards. For more info check the __Information.txt__ file.

	Section "Device"
		Identifier "Generic Video Card"
		VendorName  "Silicon Integrated Systems [SiS]"
		BoardName   "771/671 PCIE VGA Display Adapter"
		Busid "PCI:1:0:0"
		Driver "sisimedia"
		Screen 0
			Option "UseFBDev" "true"
			Option "DPMS"
			Option "ShadowFB"
			Option "MaxXFBMem"
			VideoRam 262016
			Option "RenderAccel" "true"
			Option "AllowGLXWithComposite" "true"
			Option "backingstore" "true"
			Option "AddARGBGLXVisuals" "True" 
	EndSection
 
	Section "Monitor"
		Identifier   "Configured Monitor"
		Vendorname   "Generic LCD Display"
		Modelname    "LCD Panel 1280x800"
		HorizSync 20-107
		VertRefresh 50-185

		modeline  "800x600@56" 36.0 800 824 896 1024 600 601 603 625 +hsync +vsync
		modeline  "800x600@60" 40.0 800 840 968 1056 600 601 605 628 +hsync +vsync
		modeline  "1280x768@60" 80.14 1280 1344 1480 1680 768 769 772 795 -hsync +vsync
		modeline  "1280x720@60" 74.48 1280 1336 1472 1664 720 721 724 746 -hsync +vsync
		modeline  "1280x800@60" 83.46 1280 1344 1480 1680 800 801 804 828 -hsync +vsync
	EndSection
 
	Section "Screen"
		Identifier    "Default Screen"
		Monitor       "Configured Monitor"
		Device        "Configured Video Device"
		Defaultdepth    24
		SubSection "Display"
			Depth    24
			Modes        "1280x768@60"    "1280x720@60"    "800x600@60"    "1280x800@60"    "800x600@56"
		EndSubSection
	EndSection
 
	Section "Module"
		Load "dri"
		Load "dbe" # Double-Buffering Extension
		Load "v4l" # Video for Linux
		Load "extmod"
		Load "type1"
		Load "freetype"
		Load "glx" # 3D layer
		Load "GLcore"
		Load "i2c"
		Load "bitmap"
		Load "ddc"
		Load "int10"
		Load "vbe"
		Load "speedo"
		Load "record"
	EndSection
 
	Section "DRI"
		Mode 0666
	EndSection
