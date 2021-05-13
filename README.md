# pistar-lastdmr
## A script to monitor DMR (only) traffic on Pi-STAR

This script allows one to monitor DMR traffic on a pi-star node, either via SSH or, on an HDMI-connected console.
No web browser or other GUI client is required.

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
$ git pull
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