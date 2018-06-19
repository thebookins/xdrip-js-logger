# NOTE: This is not the current maintained version of Logger. If you are looking to use Logger, it is maintained in the following repository:

https://github.com/xdrip-js/Logger

# xdrip-js-logger - the xdrip-js One-Shot Mode Logger.

Logger connects to the g5 transmitter, waits for the first bg, logs a json entry record, then exits. Doing it one-shot at a time seems to make xdrip-js more reliable in some cases. xdrip-get-entries.sh is a wrapper shell script that is called from cron every minute. It calls the one shot mode logger handling the preparation and sending of the blood glucose data to Nightscout and to OpenAPS.

# Warning! 

Logger and xdrip-get-entries.sh currently use the raw g5 blood glucose data and a simple offset for calibration. Only those who closely monitor and check blood glucose regularly comparing to the raw ("unfiltered") g5 glucose data should use this program at this time.

[![Join the chat at https://gitter.im/thebookins/xdrip-js](https://badges.gitter.im/thebookins/xdrip-js.svg)](https://gitter.im/thebookins/xdrip-js?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

*Please note this project is neither created nor backed by Dexcom, Inc. This software is not intended for use in therapy.*
## Prerequisites
Update node version. Please see wiki page for instructions https://github.com/thebookins/xdrip-js/wiki

## Installation
```
cd ~/src
git clone https://github.com/efidoman/xdrip-js-logger.git
cd xdrip-js-logger
sudo npm install
sudo apt-get install bluez-tools
cd ~/src/xdrip-js-logger
chmod 755 xdrip-get-entries.sh post-ns.sh post-xdripAPS.sh
```

Add cron job entry (replace "40SNU6" with your g5 transmitter id in both places below) ...
```
* * * * * cd /root/src/xdrip-js-logger && ps aux | grep -v grep | grep -q '40SNU6' || ./xdrip-get-entries.sh 40SNU6 | tee -a /var/log/openaps/xdrip-js-loop.log
```

## Logger usage - running stand-alone
`sudo node logger <######>` where `<######>` is the 6-character serial number of the transmitter.

To see verbose output, use `sudo DEBUG=* node logger <######>`, or replace the `*` with a comma separated list of the modules you would like to debug. E.g. `sudo DEBUG=smp,transmitter,bluetooth-manager node logger <######>`.

You can also run
```
cd /root/src/xdrip-js-logger
./xdrip-get-entries 40SNU6
# replace 40SNU6 with your transmitter id
```



