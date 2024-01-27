


# Windows installation

## WAMP

One of easiest way to install PHP and use for windows

link : 
 - https://www.wampserver.com/
 - https://wampserver.aviatechno.net/  =>all files and needed addons
 - https://github.com/abbodi1406/vcredist  => repor where we can find VisualCppRedist AIO

Before installing WAMP we need to make sure that all necessay vc redistribution package are installed, use "VisualCppRedist AIO" it will automatically install all needed packages 

Adding new php version :   
 1 - Download binaries on php.net  
 2 - Extract all files in a new folder : C:/wamp/bin/php/php5.4.13/  
 3 - Copy the wampserver.conf from another php folder (like php/php5.2.8/) to the new folder  
 4 - Rename php.ini-development file to phpForApache.ini  
 5 - Done ! Restart WampServer (>Right Mouseclick on trayicon >Exit)  

