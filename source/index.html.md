---
title: Testing API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - lua
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---


# Versions

<aside class="warning">
This is test documentation for Markdown styling ONLY. Please do not reference.
</aside>

Code Version | Document Version | Document Date
------------ | ---------------- | -------------
1.3          | 1.0              | 2 July 2018


#The Application Tools

The following tools provided easy access to control and maintain the ConnectedLife system:

* [Check System files](#check-system-files)
* [Check System Manifest File ](#check-system-manifest-file)
* [Factory Reset](#factory-reset)
* [Server Control](#server-control)
* [Set timezone](#set-timezone)
* [Zigbee tools](#zigbee)
* [Zwave tools](#zwave)

They are in the folder `/opt/sensorhub/tools`.


#Check System files
File: **checkSystemFiles.lua**

`checkSystemFiles.lua [options]`

This tool allows you to check the system files, and make sure each system file has the following attributes:

1. The file exists
2. The file is the correct version.
3. The file has the correct permissions.
4. The file has the correct ownership.

This tool will look at a meta file to get a list of files to check.

The default manifest will be loaded at `/var/lib/sensorhub/resetData/manifest.conf`.

See [Check System Manifest File](#check-system-manifest-file) below about the format of the manifest file.

##Options

```lua
-m, --manifest=FILENAME          manifest config file,
                                 defaults to the filename /var/lib/sensorhub/resetData/manifest.conf
-w, --write                      Write and copy the files over
-v, --verbose                    a combined short and long option
    --version                    display version information, then exit
-h, --help                       display this help, then exit
```

```python
Here is some sample python code!
```

```javascript
Here is some sample javascript code!
```

The following options are available


##Example

```lua 
$ ./checkSystemFiles.lua --write
```

```python
Here is some sample python code!
```

```javascript
Here is some sample javascript code!
```
Check files and write changes


#Check System Manifest File

The manifest file contains all of the files that need to be checked.

Any line starting with `#` will be treated as a comment line and will be ignored in processing this file.

##File Section All
Section for all default values applied to any parameter that is not specified in the filename sections.

`[__all__]`

##File Section 'Name'
Each file checked by the Check System Files tool is names as a section.

`[name]`

Where `name` is the name of the file section.

##Section Options
Each section can contain a _name_=_value_ pair. The section options is specified as the _name_ part.
So to set the source path option you need to add in the line:

`source=/my/path`

### source=
Absolute source path to a source file with the default global folder and file appended.

### destination=
Absolute destination path to a destination with the default folder and file appended.

### sourceFolder=
Absolute path with no folder appended

### destinationFolder=
Absolute path with no folder appended

### folder=
Relative folder to append to both source and destination paths

### file=
Filename to append to the source and destination paths

### destinationFile=
Absolute path to destination file

### sourceFile=
Absolute path to the source file

##General rules for folder/file
```lua
sourceFile = <sourceFile> | <sourceFolder><file> | <source><folder><file>
destinationFile = <destinationFile> | <destinationFolder><file> | <destination><folder><file>
```

```python
Here is some sample python code!
```

```javascript
Here is some sample javascript code!
```
The following rules will be applied to generating the source and destination file
based on the values assigned in the file section or in the global section.

###chmod=
```lua
([ugoa])[+-=][rwx]

values within a () are optional
values within a [] are a single char in that range

The ([ugoa]) is optional, defaults to 'a'
so '+x' is the same as 'a+x'
```

```python
Here is some sample python code!
```

```javascript
Here is some sample javascript code!
```

Changing file rights using the chmod option, making sure the files have the correct bits set.

Simple version of chmod values, can be in the following format:


###chown=
Change the file ownership to `<username>:<groupname>` or `<userId>:<groupId>`

###operation=
Decide on the type of operation to perform on the file, they can be one of the following values:

    new       - only copy if does not exist.
    update    - copy if does not exist or if different to the source.
    delete    - remove the file if it exists as the source.
    install   - not enabled at the moment, since we are now using yocto build,
            but copy over when doing a forced copy.

###Example
```lua
[__all__]
source = /var/lib/sensorhub/resetData
destination = /etc

# The files that need checking...
[avahi-http.service]
title = avahi http.service
# source file will be at /var/lib/sensorhub/resetData/avahi/services/http.service
# destination file will be at /etc/avahi/services/http.service
file = http.service
folder = avahi/services
# only copy the file if it does not exist
operation = new
```

```python
Here is some sample python code!
```

```javascript
Here is some sample javascript code!
```


#Factory Reset
File: **factoryReset.sh**

`factoryReset.sh`

This tool allows you to start, stop, enable, disable and restart all of the ConnectLife servers.
The servers available are the following:

Bash script to reset all files to the factory default. This script does the following tasks:

* Stop all services
* Stop the redis server
* Delete all redis data
* Start redis server
* System Check all files
* Restart web server lighttpd
* Start up watchdog service

#Server Control
File: **serverContorl.lua**

This tool allows you to start, stop, enable, disable and restart all of the ConnectedLife servers.
The servers available are the following:

Name               | Systemctl name             |  Application name              |   Description
-------------------| ---------------------------| -------------------------------| -------------------
action             | sensorhub-action           | sensorHubAction.lua            |  Process Actions
data               | sensorhub-data             | sensorHubData.lua              |  Send event data
watchdog           | sensorhub-watchdog         | sensorHubWatchdog.lua          |  General watchdog/maintenance server
bluetooth          | sensorhub-bluetooth        | sensorHubBluetooth.lua         |  Process Bluetooth devices
bluetoothScanner   | sensorhub-bluetooth-scanner| sensorHubBluetoothScanner.lua  |  Bluetooth reader
network            | sensorhub-network          | sensorHubNetwork.lua           |  Process network devices
zwave              | sensorhub-zwave            | sensorHubZwave.lua             |  Process Zwave devices

##Usage
`serverControl.lua command [ServerName]`

Where **command** is the same command used in `systemctl <command>` and so can be any of the following:

* start
* stop
* enable
* disable
* restart

**ServerName** can be empty, meaning all of the servers in the table above or a given server name listed in the table above.

##Options
```lua
 -w, --wait                   wait for all process to finish before exiting on the stop command,
                              this is the default action
     --waitTimeout=[SECONDS]  number of seconds to wait, default is 30 seconds
 -v, --verbose                a combined short and long option
     --version                display version information, then exit
 -h, --help                   display this help, then exit
```

```python
Here is some sample python code!
```

```javascript
Here is some sample javascript code!
```

##Example
`$ ./serverControl.lua restart`

Restart all servers

`$ ./serverControl.lua stop data`

Stop only the data server



#Set Timezone
File: **setTimezone.lua**

This tool sets the timezone based on the zone name, specified as `TimeZonename` sush as Asia/Singapore.

`setTimezone.lua [TimeZoneName]`

##Options

```lua
 -r, --refresh      set the current timezone again, incase the TZ file needs refreshing
 -f, --force        on the '--default' option, if the timezone is already set then force the change
     --version      display version information, then exit
 -h, --help         display this help, then exit
```

```python
Here is some sample python code!
```

```javascript
Here is some sample javascript code!
```

##Example

Sets the timezone for Singapore

`
$ ./setTimezone.lua Asia/Singapore
`


#Zigbee
File: **ZigbeeTools.lua**

**To Do**

Still in development

#Zwave
File: **ZwaveTools.lua**

##Usage

```lua
zwaveTools.lua <command> [<command options>]
```

Where a commands can be one of the following:

```lua
pair                                     - adds a new node to the zwave network.
update <nodeid>                          - update connected zwave devices using the provided 'node id'.
list                                     - list all nodes on the zwave network.
listen                                   - listen for events on the zwave network.
unpair <nodeid>                          - delete the node from the zwave network, you must provide a 'node id' to delete.
reset                                    - reset the zip network. You will need to power off the gateway after completing this task.
switch <nodeid> <on|off|read|toggle>     - turn on/off a switch at <node id>, read switch value or toggle on/off ( default).
discover                                 - search the network for the Zwave Gateway.
send <raw command>                       - send to the zwave controller a raw command, the zipgateway will be stopped
```

```python
Here is some sample python code!
```

```javascript
Here is some sample javascript code!
```

##Options

```lua
 -h, --help                      display this help, then exit.
     --debug                     display extra information for debugging.
     --logfile=FILENAME          write the log to FILENAME.
     --timeout=SECONDS           Number of seconds to wait for the nodes to appear, defaults to 60 seconds.
```

```python
Here is some sample python code!
```

```javascript
Here is some sample javascript code!
```

##Example

Request a zwave to pair

`
$ ./zwaveTools.lua pair
`


