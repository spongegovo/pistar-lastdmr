# pistar-lastdmr
## A script to monitor DMR (only) traffic on Pi-STAR

This script allows one to monitor DMR traffic on a pi-star node, either via SSH or, on an HDMI-connected console.
No web browser or other GUI client is required.  If you optionally install the package "figlet", the script can display the Callsign of the remote sender as a large banner.

With "figlet" installed, for the large font display of the callsign...

![Image](https://raw.githubusercontent.com/kencormack/pistar-lastdmr/master/with-figlet.jpg)

Without "figlet" installed, or when the "--nobig" option is given...

![Image](https://raw.githubusercontent.com/kencormack/pistar-lastdmr/master/without-figlet.jpg)

## Installation

The script is designed to run with the bash shell.  Just install, enable execute permission, and run it...
```
Put the pi-star filesystem in read-write mode...
  $ rpi-rw

Pull down the script...
  $ git clone https://github.com/kencormack/pistar-lastdmr.git

Change to the "pistar-lastdmr" directory that was just created...
  $ cd pistar-lastdmr

Set execute permissions on the script
  $ chmod +x pistar-lastdmr

Copy the script to /usr/local/sbin
  $ sudo cp pistar-lastdmr /usr/local/sbin/pistar-lastdmr

If you wish to add the ability to display call-signs in a large
font, you'll need to install "figlet", as follows...
  $ sudo apt update
  $ sudo apt install -y figlet

Finally, return the filesystem to read-only mode...
  $ rpi-ro

You are now ready to monitor DMR traffic from the commandline.
  $ pistar-lastdmr
```

## Updating

To update, 'cd' into the directory into which you originally installed pistar-lastdmr, and run the following command:
```
$ rpi-rw
$ git pull
$ sudo cp pistar-lastdmr /usr/local/sbin/pistar-lastdmr
$ rpi-ro
```
That should do it.

**If, for any reason, git detects that your local copy has changed, and gives the following message...**
```
error: Your local changes to the following files would be overwritten by merge:
        <filename>
Please commit your changes or stash them before you merge.
```
... copy your local changed file to an alternate location, and run the following command to reset git's pointers:
```
$ git reset --hard origin/master
```
... and then re-try the "git pull".  **This will overwrite your local changes with the update from github.**

## City, State, Country lookups for Callsigns

Upon first run of pistar-lastdmr version 0.96 or later, this script will download the master user.csv
file from https://database.radioid.net/static/user.csv

If the user.csv file is already present on your hotspot, but is older than 7 days, the script will update
the file to it's latest version.

pistar-lastdmr uses that file to display the "City", "State", and "Country" data related to the callsign.

## Call-Signs in a large font

The script will detect whether figlet is installed, and if so, will call upon it to display the contact's
callsign, in large letters.  If figlet is NOT installed, the large letter display is omitted, but all other
information is still displayed.

## Commandline options

If figlet is installed, but you prefer to supress the automatic large font display of a contact's callsign,
to conserve screen space, the script can be run with the following commandline option, to suppress the
large font display of the callsign...
```
  $ pistar-lastdmr --nobig
```

To exit the program, press Ctrl-C

## Daily Log Rotation

When the script is launched, it searches for the most recent MMDVM log in the /var/log/pi-star directory.
It then watches only that log, to perform it's monitoring.  When Pi-star's logs are rotated, and a new log
is created, this script detects the change, and re-launches itself to monitor the new log.

## User-Custom Talkgroup List

In order to display the name of a Talkgroup, the script consults the pi-star file "/usr/local/etc/TGList_BM.txt".  This file is updated automatically by pi-star, but does NOT include talkgroup names that include certain characters (such as those found in German and other
languages.)  To compensate, this script allows the user to build their own custom file containing any missing talkgroup names the
user wishes to display.

Create the file "/usr/local/etc/MY_LIST.txt".  Records in that file must appear in the same format as pi-star's TGList_BM.txt file.

Examples should look like the following:
```
2627;0;BADEN-WUERTTEMBERG;TG2627
2629;0;SACHSEN/THUERINGEN;TG2629
26232;0;DREILAENDERECK-MITTE-DEUTSCHLAND;TG26232
26274;0;BW-BOEBLINGEN;TG26274
26283;0;REGION-MUENCHEN;TG26283
26287;0;ALLGAEU-BODENSEE;TG26287
26298;0;THUERINGEN;TG26298
```

## Special Thanks

I want to offer a big "Thank You" to Wolfgang Wendefeuer, for the testing, input, discussion, ideas, and sample TG entries he provided.