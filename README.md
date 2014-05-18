cups-epilog
===========
Original source: http://mtm.cba.mit.edu/cups/cups-epilog/src/cups-epilog.c

CUPS driver for the Epilog Laser engraver  

 Description:  
 Epilog laser engraver  
  
 The Epilog laser engraver comes with a windows printer driver. This works  
 well with Corel Draw, and that is about it. There are other windows  
 applications, like inkscape, but these rasterise the image before sending to  
 the windows printer driver, so there is no way to use them to vector cut!  
  
 The cups-epilog app is a cups backend, so build and link/copy to  
 /usr/lib/cups/backend/epilog. It allows you to print postscript to the laser  
 and both raster and cut. It works well with inkscape.  
  
 With this linux driver, vector cutting is recognised by any line or curve in  
 100% red (1.0 0.0 0.0 setrgbcolor).  
  
 Create printers using epilog://host/Legend/options where host is the  
 hostname or IP of the epilog engraver. The options are as follows. This  
 allows you to make a printer for each different type of material.  
 af	Auto focus (0=no, 1=yes)  
 r	Resolution 75-1200  
 rs	Raster speed 1-100  
 rp	Raster power 0-100  
 vs	Vector speed 1-100  
 vp	Vector power 1-100  
 vf	Vector frequency 10-5000  
 sc	Photograph screen size in pizels, 0=threshold, +ve=line, -ve=spot, used  
      in mono mode, default 8.  
 rm	Raster mode mono/grey/colour  
  
 The mono raster mode uses a line or dot screen on any grey levels or  
 colours. This can be controlled with the sc parameter. The default is 8,  
 which makes a nice fine line screen on 600dpi engraving. At 600/1200 dpi,  
 the image is also lightened to allow for the size of the laser point.  
  
 The grey raster mode maps the grey level to power level. The power level is  
 scaled to the raster power setting (unlike the windows driver which is  
 always 100% in 3D mode).  
  
 In colour mode, the primary and secondary colours are processed as separate  
 passes, using the grey level of the colour as a power level. The power level  
 is scaled to the raster power setting. Note that red is 100% red, and non  
 100% green and blue, etc, so 50% red, 0% green/blue is not counted as red,  
 but counts as "grey". 100% red, and 50% green/blue counts as red, half  
 power. This means you can make distinct raster areas of the page so that you  
 do not waste time moving the head over blank space between them.  
  
 Epolog cups driver  
 Uses gs to rasterise the postscript input.  
 URI is epilog://host/Legend/options  
 E.g. epilog://host/Legend/rp=100/rs=100/vp=100/vs=10/vf=5000/rm=mono/flip/af  
 Options are as follows, use / to separate :-  
 rp   Raster power  
 rs   Raster speed  
 vp   Vector power  
 vs   Vector speed  
 vf   Vector frequency  
 w    Default image width (pt)  
 h    Default image height (pt)  
 sc   Screen (lpi = res/screen, 0=simple threshold)  
 r    Resolution (dpi)  
 af   Auto focus  
 rm   Raster mode (mono/grey/colour)  
 flip X flip (for reverse cut)  
 Raster modes:-  
 mono Screen applied to grey levels  
 grey Grey levels are power (scaled to raster power setting)  
 colour       Each colour grey/red/green/blue/cyan/magenta/yellow plotted  
 separately, lightness=power  
  
  
 Installation:  
 gcc -o epilog `cups-config --cflags` cups-epilog.c `cups-config --libs`  
 http://www.cups.org/documentation.php/api-overview.html  

 Manual testing can be accomplished through execution akin to:  
 $ export DEVICE_URI="epilog://epilog-mini/Legend/rp=100/rs=100/vp=100/vs=10/vf=5000/rm=grey"  
 # ./epilog job user title copies options  
 $ ./epilog 123 jdoe test 1 options < hello-world.ps  
  
