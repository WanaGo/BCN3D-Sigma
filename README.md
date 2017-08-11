# BCN3D-Sigma-Enhanced
Enhanced Version of BCN3D's Sigma Firmware + Display

This version of the Firmware was forked from BCN3D's offical firmware, dated 31/07/2017 Release v01-1.2.5.
https://github.com/BCN3D/BCN3DSigma-Firmware

Many changes have happened to the Display Application, along with suitable code in the Marlin to support these display changes. There are also versions for both R16 and R17 display models.

## Change Log
* Changed the colour scheme from the default Blue, to Orange, to make an obvious difference between this Enhanced version, and the stock BCN3d version.
* Converted most String objects to User Images. These were used to display the temperatures and various information in the menus and printing screens. These used to flash each time the number changed. This no longer happens, it is now smooth.
* Temperatures etc now update once a second, rather than once every 5 seconds, providing users with a faster responding experience.
* Added Z height display output to the main printing status window. This will be enhanced later.
* Added the ability to control the LED strips along the top sides of the printer. Default was a bluey/white, which was hard to take photos in, and also offered no choice to people who wanted to customise their printing experience.  
  - In theory there are 4096 colours available for the LED light strips. 
  - Colours are generated at 60Hz, using Timer0 ISR as a Software PWM source.
  - These are made with a mix of 16 colours each of Red, Green and Blue. 
  - Different intensities of each colour enable different mixes of colours, or the ability to turn the lights off all together. 
  - There is a button to randomly generate a colour
  - There is a button to enable a 'Light Show' mode, which selects a random colour, with a random interval (from 10ms to 1second) and constantly cycles. May be desirable for trade shows, or those people who want to party next to their printers :)
  - There is a button to enable a 'Light Pattern' mode, which cycles thought 90 high brightness colours, each colour staying visible for 200ms. These colours cycle and somewhat smoothly merge across the visible spectrum.
  - Colours set and colour modes are saved and recalled from EEPROM, so what you set will be shown at new power up.
  - Colours can be changed fromt he Settings menu before a print is started, or from the Utilities menu during a print, in case a colour is not right for a photo or video you want to make, you can change it mid print. 
* Added the G36 calibration command which came in the v01-1.2.5 release, into the display menu. You can now select the standard Bed Calibration, or Full Calibration, along with the G36 Bed Calibration right off the menu.
* Added the ability to update the display software from the main printers SD Card. YES! Note this is not a fast process. The average update will take an hour, maybe a little more. It updates over the serial link between the Atmel MCU and the Display, at 200kbaud. You need to do the intial upgrade to use this version before you can do this, so instructions will come later on this.
 
## Updating your stock R16 or R17 - MUST READ
### Background
Out of the factory, the stock R16 and R17 displays use the SD card on the back of the display, to hold the application the display runs. When the display is powered up, the display will load the files from the card, and store them in RAM (memory) on the display each time it starts up, and then execute the program from RAM. The displays Flash memory is largely unused. This has worked OK up till now, however BCN3D has used up all the RAM so very few updates can happen. 
So what changed in this Enhanced Version is the displays Boot Application has been changed to store and run the application from Flash, keeping RAM for variables and normal runtime operations. This then means there is a ton of headroom for updates. The display has 6 banks of 32KB of Flash, and 32KB total of RAM, plus SD card storage for graphics, fonts etc.
To enable this to work, you CANNOT simply update the display with this new program without first upgrading the displays 'Boot Application'. This is the app which sits in the first bank of Flash, which reads the SD card, puts the Application into RAM, and runs it. So we need to upgrade this small application, to instead load the SD card into the 2nd Flash Bank (if the SD card application has changed), and run it from there, freeing up a bunch of RAM. 
It is very simple to do. 
Yes, you can go back to run the stock firmware and display application if you want. The stock application will still however be run from Flash rather than RAM from this point on, but its good in a lot of ways. You can run all the stock code again if you want. 
 
### How to do the upgrade
#### Update the Boot Application on the display.
In order to upgrade the displays 'Boot Application', we need to get at the SD card in the display. This is fiddly on both R16 and R17 but especailly the R17. BCN3D never upgraded the little trap door inside the machine behind the display, when they made the R17, but the display is different and so is the location of the SD card on the display, and the type of SD card connector used.
The R16 is a push/push connector, push it eject, and push to insert, its accessible from the top of the trap door, so it is very manageable to upgrade the program in the BCN3d designed fashion.
The R17 is a latch type connector, slide it towards the top and it has a hinge at the top too, so the latch swings up from the bottom, and the card comes out from just sitting against the terminals. Reverse to put it back in. With the angle the display is on, yes you are very likely to drop the card into the guts of the printer. But, there is a way to do it.
If you are nible, by all means use the little trap door.
If you have 'man hands' or struggle to do up your own shoelaces, then you might want to go straight to this next option.

##### Trap door method
1) Unscrew the trap door inside the printer, immediately behind the display, in front of the right spool.
2) Peek in the hole
3) Make sure the printer is fully turned off, main Power and USB is unplugged.
4) Push the SD card to unlock it, and lift it out of the displays SD card connector. Take note of the orientation of the card as you remove it. So you put it back in the same way.
5) Done :) Reverse the process to reinsert, push to lock.

##### Removal of false floor method
1) Inside the machine, there is the false floor covering up the main printer controller. Remove that panel with the 4 screws. On the right side, under the right spool, is another panel. Remove the spool and spool holder (yeah you likely want to unload the filament), and then undo the screws holding in that part of the floor. There are 4 screws. This should then lift out, and expose right up to the back of the display. This should give you more access to get to the SD card on the back of the display.
2) Turn OFF the printer, and make sure the USB is also not connected to anything.
3) Carefully slide the SD card latch up, then it should swing out with the hinge at the top, allowing the card to come out. To put it back, you place the card against the socket, lower the door on top of it, and slide it down towards the bottom of the machine to lock it in place. Not rocket science but be gentle.
4) Pat yourself on the back :)

#### Load the update onto the display
1) put the SD card into your computer, and delete the files on the card. No need to format it, but you can if you like. Just be sure that it remains FAT16, anything else will not work. Deleting the files is just easier. Copy the file called "RunFlash.4xe" from the folder called "LCD 1-Time Upgrade" into the SD card. 
2) Put the SD card back into the printer, as per Step 1, just reverse the process you already did. Make sure teh card is secure and the latch is closed.
3) Power on the printer using the main power swtich. The display will likely flash, and then display something like 'Bank0 Updater Running...' followed by 'Mounting...' followed by 'Program loaded into Flashbank in xx ms' where xx is the time it took, followed by 'Power down, remove uSD and replace files before powering up again'. If you got all that, then you have upgraded the display successfully to the new Boot Application. If you got 'Drive not mounted...' or 'Program failed to load into Flashbank' then something went wrong. Either your SD card is not FAT16 anymore, or the file didnt copy correctly from your PC, etc. Try again.
4) Do a little dance, so far so good.

#### Load the Sigma Display Application
So right now, you basically have a totally blank display, but running the latest Boot Application.
Now you need to put the actual Sigma application back on it. 
At this stage, you need to do the same as you did above, but this time put different files on the SD card.
1) Remove the SD card from the back of the display, as you did already, using either method already described.
2) Copy the files to the card from the 'LCD SD Files' folder. There are say 12 files, Sigma.4xe, Sigma~1.dat etc etc d0C, d0E, d01, etc. Copy them all onto the card. 
2) Pop it in your printer, 
3) Power up the printer with the main switch, it should flash up with a quick message basically saying it has detected a new application on the SD card, and it copies it to the display, and then it runs the application. You will only see this when you change the files on the card.
4) Your display is now up to date! Now you just need to load the Marlin firmware to match. REQUIRED not OPTIONAL.
5) Power it all off, its not ready to use yet.

#### Upgrade Marlin Firmware
So now you have updated the files on your SD card, you need to upgrade Marlin so both devices are running the same version of the software. If you dont do this, the printer will not work right. There are new things on the display which Marlin will not understand, and vice versa.
There is a hex file you need to upload to the printer, over the USB cable connected to the back of the printer, into your PC.
1) Open up Cura
2) Go to the Machine menu, click 'Instal custom firmware'
3) Browse to the folder where you downloaded the updated file, and select the Marlin-v01-125-MOD.hex file. Click OK
4) If you havent already, plug in the USB to your computer/printer. I recommend having the power OFF the machine, and just rely on the power from the USB to power the controller. Not everyones computer may be OK with this, so do what you like here. I have had the printer move the steppers during an upgrade, so I prefer to leave my main power supply in the printer off. But do as you like here.
5) It should then upgrade the firmware on the main controller with the latest Marlin. 

You should now be all set. Latest Marlin - check. Latest Display Application - check. Power it up and see what happens.

#### Future upgrades using the main SD card of the printer
This is what you can now do each time there is a new update. So once you put your floor back into the machine (which you might have taken out from the upgrade above), the next time you might not want to do that. So there is a choice you can make.

You can do this by changing out the SD card files, just like we have already done above.
OR 
You can do it by loading the update from the main gcode SD card in the front of the printer. 

The choice is yours. 
Changing the SD card files in the back of the display is fast/instant, but is fiddly to access, especially on an R17 unless you take out the floor panels, but its still a bit of a contortionist act for some burly blokes :) 
Loading from the SD card in the front of the printer is slow, but easier to access, as the SD card is just the card for your gcode.
You can do either, and you can mix it up, do what ever floats your boat.

More instructions to come...
WORK IN PROGRESS

### Wish List
* Ability to change flow rate mid print, useful for dialing in new filament etc
* Display layer number during the print, to upgrade the Z height already added
* Ability to change Nozel size for calibrations, and material type
* more to add...
