## FDM Commit script

Cisco Firepower Device Manager allows the automatic download of geolocation, intrusion rule and VDB improvements, but 
does not automatically deploy them.  This is a script to automatically commit intrusion rule, VDB and geolocation 
updates to your FDM.  It checks to make certain that there are no other pending changes to the configuration.  If there 
are additional changes or any errors, the script sends notification with a Webex Teams message.

The script was tested with FTD 6.6, but should also work with some previous versions.  If you are able to test on 
previous versions, I will gladly credit you as a contributor.

I set this script to run once daily after the FDM update task has completed via a cron job on a Pi.  It is still early
days, so I am working on refining error handling to account for common variations of these updates.

## Requirements
* requests
* urllib3
* webexteamssdk

## Environment Variables Required
* USER - FDM admin username
* PASS - FDM admin password
* EMAIL - email address of Webex Teams user to notify with errors
* WEBEX_TEAMS_ACCESS_TOKEN - Bearer token for Webex Teams
* FDMADDRESS - IP address or FQDN of FDM management

## Using the Script

* Bring the repository down to your environment.  
```
git clone https://github.com/brbuxton/fdm-commit
```
* If using virtual environment, instantiate and activate it. 
```
_Linux/Mac_ 
python3 -m venv venv
source bin activate
```
```
_Windows_ 
virtualenv env
\env\Scripts\activate.bat
```
* Install requirements
```
pip install requirements.txt
```
* Setup the pre-requisite environment variables.  All five of the variable should be set in order for the script to 
work error free.  The specific method to set a variable depends on you OS and use case.  ___NOTE: For testing purposes, 
a Webex Team token can be generated from https://developer.webex.com/docs/api/getting-started___
```
_Linux/Mac_
USER=admin
export USER
```
```
_Windows 10_
From the desktop, type "environment variable" into the search bar usually in the lower left of the screen.
```
* Run the script.  The script is going to access the FDMADDRESS with the USER/PASS to establish a token.  Then it will
use that token to pull down any pending changes and evaluate them.  If the changes are _exclusively_ intrusion file, VDB
or geolocation updates the script deploy/commits the pending changes.  If there are any other pending changes (i.e. 
ACP/ACL rule updates), the script will use the WEBEX_TEAMS_ACCESS_TOKEN to send a 1-to-1 message to EMAIL informing you
of the issues with the deployment.

[![published](https://static.production.devnetcloud.com/codeexchange/assets/images/devnet-published.svg)](https://developer.cisco.com/codeexchange/github/repo/brbuxton/fdm-commit)