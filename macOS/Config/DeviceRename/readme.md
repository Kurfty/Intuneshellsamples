# Script to rename a Mac device based on model type and serial number

This script renames a Mac device by looking at the model type and at the serial number
This is ideal for devices that are enrolled without user affinity. The script can be further customized to include the user name as part of the device rename.

## DeviceRename.sh

The script consists of five steps:
1) determine the model type and, based on the retrieved type, set a 4 characters variable $ModelCode
    e.g. MacBook Air ==> $ModelCode = MABA
2) collect the serial number and keep the first 10 characters
    e.g. Serial Number = C02BA222DC79 ==> $SerialNum = C02BA222DC
3) check MDM enrollment type and set $OwnerPrefix variable
    e.g. enrolled via Apple Business Manager ==> $OwnerPrefix = CO, otherwise ==> $OwnerPrefix = BYO
4) fetch country code, based on current IP address, set variable $Country
    e.g. 209.142.68.29 ==> $Country = US
5) build the final name by combining $ModelCode and $serial
    e.g. ABM device ==> $NewName = CO-MBA-C02BA222DC-US, BYOD ==> BYO-MBA-C02BA222DC-US

```bash
# Define variables
appname="DeviceRename"
logandmetadir="/Library/Logs/Microsoft/IntuneScripts/$appname"
log="$logandmetadir/$appname.log"
CorporatePrefix="CO"   # May be adjusted, e.g. your company shortname, or set to empty string
PersonalPrefix="BYO"   # May be adjusted, or set to empty string
ABMOnly="false"        # Change to "true" to change device names for ABM enrolled devices only
EnforceBYOD="false"    # Change to "true" if you want to run the script repeatedly and enforce device names for BYOD devices
```

## Script Settings

- Run script as signed-in user : No
- Hide script notifications on devices : Not configured
- Script frequency : Not configured
- Mac number of times to retry if script fails : 3

### Log file example

>**Note:** The log file will output to **/Library/Logs/Microsoft/IntuneScripts/DeviceRename/DeviceRename.log** by default. Exit status is either 0 or 1. To gather this log with Intune remotely take a look at  [Troubleshoot macOS shell script policies using log collection](https://docs.microsoft.com/en-us/mem/intune/apps/macos-shell-scripts#troubleshoot-macos-shell-script-policies-using-log-collection)

```
##############################################################
# Mon Jan 25 16:18:28 GMT 2021 | Starting DeviceRename
############################################################

 Mon Jan 25 16:18:28 GMT 2021 | Checking if renaming is necessary
 Mon Jan 25 16:18:29 GMT 2021 | Serial detected as ABCDEF000017
 Mon Jan 25 16:18:29 GMT 2021 | Current computername detected as TestVM
 Mon Jan 25 16:18:29 GMT 2021 | Old Name: TestVM
 Mon Jan 25 16:18:30 GMT 2021 | Retrieved model name: MacBook Pro
 Mon Jan 25 16:18:30 GMT 2021 | This device is enrolled by ABM
 Mon Jan 25 16:18:30 GMT 2021 | Generating four characters code based on retrieved model name MacBook Pro
 Mon Jan 25 16:18:30 GMT 2021 | ModelCode variable set to MBP
 Mon Jan 25 16:18:30 GMT 2021 | Retrieved serial number: ABCDEF000017
 Mon Jan 25 16:18:30 GMT 2021 | Building the new name...
 Mon Jan 25 16:18:30 GMT 2021 | Generated Name: CO-MBP-ABCDEF000017-US
 Mon Jan 25 16:18:30 GMT 2021 | Computername changed from TestVM to CO-MBP-ABCDEF000017-US
 Mon Jan 25 16:18:30 GMT 2021 | HostName changed from TestVM to CO-MBP-ABCDEF000017-US
 Mon Jan 25 16:18:30 GMT 2021 | LocalHostName changed from TestVM to CO-MBP-ABCDEF000017-US

```
