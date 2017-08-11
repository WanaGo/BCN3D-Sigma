# BCN3D-Sigma-Enhanced
Enhanced Version of BCN3D's Sigma Firmware + Display

This version of the Firmware was forked from BCN3D's offical firmware, dated 31/07/2017 Release v01-1.2.5.

Many changes have happened to the Display firmware, along with suitable code in the Marlin to support these display changes. There are also versions for both R16 and R17 display models.  
## Change Log
 * Changed the colour scheme from the default Blue, to Orange, to make obvious the change to this Fork.
 * Converted most String objects to User Images, which temperatures and various information is displayed. These use to flash each time the number was updated. This no longer happens, it is now smooth.
 * User Images (updated above) now update once a second, rather than once every 5 seconds, providing users with a faster responding experience.
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
 * Added the ability to update the display software from the main printers SD Card. YES! Note this is not a fast process. The average update will take an hour, maybe a little more. It updates over the serial link between the Atmel MCU and the Display, at 200kbaud. See notes below for more information.
 
 ## Updating your stock R16 or R17 - MUST READ
 Out of the factory, the stock R16 and R17 displays use the SD card on the back of the display, to hold the application the display runs. When the display is powered up, the display will load the files from the card, and store them in RAM (memory) on the display, and execute from RAM. The displays Flash memory is largely unused. This has worked OK, however BCN3D has used up all the RAM so very few updates can happen. 
 So what changed in this release is the display has been changed to instead of run the application from RAM, it will store the application in Flash, keeping RAM for variables and normal runtime operations. This then means there is a ton of headroom for updates.
 To enable this to work, you CANNOT simply update the display with this new program without first upgrading the displays boot application. It is very simple however to do. Yes, you can go back to stock firmware and display application if you want. The stock application will still however be run from Flash and RAM rather than just from RAM, but you can run all the stock code again if you want.
 Firstly we need to update the Boot Application.
 Second we update the Display Application, which is the Sigma program. You can do this by changing out the SD card files, or you can do it by loading the update from the SD card in the front of the printer. The choice is yours. Changing the SD card files in the back of the display is fast/instant, but is fiddly to access, especially on an R17. Loading from the SD card in the front of the printer is slow, but easier to access, as the SD card is just the card for your gcode.
 
 WORK IN PROGRESS
