Solar Inverter polling script for the Ginlong GCI-3k inverter. 

Not tested on Windows - only on Linux!!

I am just using a generic no-name bluetooth adaptor off of ebay. Try using the "official" software first to test: http://www.ginlong.com/en/download

It'll probably work with other Ginlong based inverters - try it out and let me know!

I took inspiration from (and based this on): http://www.solarfreaks.com/cms2000-inverter-rs232-serial-port-hack-cms-2000-rs232-t271-240.html#p3410

You will need curl & appconfig support in Perl: 
sudo apt install libwww-curl-perl libappconfig-perl

I originally cut out all of the pvoutput and rrd related stuff since I use this script with Cacti, I also removed the config items I was not using since I didn't see a reason I needed to keep them. Then I added a new config item called "flip" - some of the data within the response needs to be flipped (be12 becomes 12be). I also had to split the response up differently to handle the current and last month energy readings.

I now use this script with check_mk: http://mathias-kettner.com/check_mk.html

I've now re-added some basic pvouput funcitonality - in the config.ini file you should add in your API key and System ID, flip the pvouput option to 1, and set up a cron job to run every minute (or however many times you want to send updates per hour - the maximum is 60 per hour according to the pvoutput API documentation (http://pvoutput.org/help.html#api-ratelimit)

In the inverter Bluetooth settings, make sure it is configured as #1 - or just modify the script.

Note: I found that I had to get rfcomm to continually try and reconnect using this crontab line (as root):

*/10 05-20 * * * rfcomm connect 0 xx:xx:xx:xx:xx:xx  1
