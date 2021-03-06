### Release Notes:
```
0.1   - public release
0.1.1 - added internal timer to poll GPIO status
      - added attribut interval
      - added attribut pollGPIOs
      - improved logging
      - added esp command "status"
      - added statusRequest
      - commands are case insensitive, now
      - updated command reference
      - delete unknown readings
0.1.2 - renamed attribut interval to Interval
      - presence check
      - added statusRequest cmd
      - added forgotten longpulse command
0.1.3 - added internal VERSION
      - moved internal URLCMD to $hash->{helper}
      - added pin mapping for Wemos D1 mini, NodeMCU, ... 
      - within set commands
      - added state mapping (on->1 off->0) within all set commands
      - added set command "clearReadings" (GPIO readings will be wiped out)
      - added get command "pinMap" (displays pin mapping)
      - show usage if there are too few arguments
      - command reference adopted
0.2.0 - chanched module design to bridge/device version
0.2.1 - own tcp port (default 8383) for communication from esp to fhem
      - added basic authentication for incoming requests
      - added attribut readingPrefixGPIO
      - added attribut readingSuffixGPIOState
0.2.2 - fixed statusRequest/presentCheck
      - minor fixes: copy/paste errors...
      - approved logging to better fit dev guide lines
      - handle renaming of devices
      - commands are case sensitive again, sorry :(
0.2.3 - added pwmfade command #https://forum.fhem.de/index.php/topic,55728.msg480966.html#msg480966
      - added raw command to send own commands to esp. 
        usage: 'raw <newCommand> <param1> <param2> <...>'
0.2.4  - code cleanup
       - fixed "use TcpServerUtils"
       - removed controls_ESPEasy.txt from dev version
0.2.5  - fixed: keys on reference is experimental for perl versions >= 5.20?
0.3.0  - process json data if available (ESPEasy Version R126 required)
0.3.1  - added uniqIDs attribut
       - added get user/pass commands
0.3.2  - fixed auth bug
0.3.3  - state will contain readingvals
       - default room for bridge is ESPEasy, too.
       - Log outdated ESPEasy (without json) once.
       - JSON decoding error handling
0.3.4  - code cleanup
0.3.5  - dispatch ESP paramater to device internals if changed
       - added attribute setState (disable value mapping to state)
0.4.0  - command reference updated
0.4 RC1 - code cleanup
0.4.1  - improved removing of illegal chars in device + reading names
       - removed uniqID helper from bridge if undef device (IOwrite)
       - use peer IP instead of configured IP (could be modified by NAT/PAT)
       - added http response: 400 Bad Request
       - added http response: 401 Unauthorized
       - fixed oledcmd cmd usage string
       - improved presence detection (incoming requests)
0.4.2  - more unique dispatch separator
       - moved on|off translation for device type "SWITCH" from ESPEasy Software to this module.
       - new attribute readingSwitchText
0.4.3  - bug fix: Use of uninitialized value $ident:: in concatenation (.) or string at 34_ESPEasy.pm line 867. Forum: topic,55728.msg488459.html
0.4.4  - modified behavior of attribute setState (# of characters in state, 0 = disabled)
       - fixed: Use of uninitialized value in string ne at ./FHEM/34_ESPEasy.pm line 9xx.
       - code and command reference cleanup
       - misc logging modifications
0.4.5  - timestamp of reading state will not be changed if state == opened,present or absent
       - added internal INTERVAL
       - Attr Interval can be set 0 to disable presence check and polling
       - removed deprecated code for old ESPEasy Versions without json support
       - reworked dispatching values
       - reworked presence detection (no more polling, check readings age)
       - added attribut adjustValue (see command ref for details)
       - added internal ESP_CONFIG -> EspIP:version,sleep,unit
       - added internal UNIQIDS to devices
0.4.7  - command reference updated
0.4.8  - logging adopted
0.4.9  - fixed check of empty device name, value name and value in received data
0.5.0  - eval JSON decoding in http response
       - removed Authorization String from debug log
       - combined internals logging
       - check for temporary bridge device in deleteFn and do no IOWrite
         see: https://forum.fhem.de/index.php/topic,55728.msg497366.html#msg497366
       - added check that fhem.pl is new enough (11000/2016-03-05)
         see: https://forum.fhem.de/index.php/topic,55728.msg497094.html#msg497094
0.5.1  - optimized logging
0.5.2  - fixed: PERL WARNING: Use of uninitialized value in substitution (s///) at ./FHEM/34_ESPEasy.pm line 569.
0.5.3  - adopted deletion of keys in hash->helper if a device will be deleted
       - fixed get <bridge> user/pass: no value was returned
       - attr adjustValue: reading can be a regexp
       - code cleanup
0.5.4  - improved closing tcp sessions
0.6.0  - increase verbose level for startup/shutdown log messages
       - moved attribute handling to NotifyFn
       - new attribute parseCmdResponse
       - disabled parsing httpRequests by default, enable with attr parseCmdResponse
       - more relaxed checking of existenz of 'esp name' and 'device name'.
       - dispatch error msgs to device, show last WARNING in internals
       - new attribute allowedIPs
       - added default attributes
0.6.1  - changed default behavior allowedIPs feature to 'allow'
0.6.2  - added attribut deniedIPs
0.6.3  - close open tcp session immediately if ESP is configured 
         to go to deep sleep (EPSEasy bug?)
0.6.4  - fixed faulty presence detection after FHEM restart
0.6.5  - added space between reading und value in state (Forum #55728.msg515626)
       - attribute uniqIDs is deprecated and will be removed soon
       - added function declarations (Forum #55728.msg511921)
       - new attribute combineDevices (Used to gather all ESP devices of a single ESP into 1 FHEM device even if different ESP devices names are used)
0.6.6  - minor fixes
0.6.7  - added attr rgbGPIOs, reading rgb, command rgb
       - added attr colorpicker
0.6.8  - removed attr uniqIDs
       - presence detection simplified
0.6.9  - fixed "argument is missing" for 'get user|pass' cmds in FHEMWEB
0.7.0  - removed internal event-on-change-reading from setState
0.7.1  - Command reference update
       - Removed change log from module
0.7.2  - Fixed gpio 0 bug
0.7.3  - Fixed attr rgbGPIOs: empty select within FHEMWEB
       - Ignore received value if adjustValueFn returns undef
0.7.4  - Added irsend command
         see: https://forum.fhem.de/index.php/topic,55728.msg530220.html#msg530220
0.7.5  - Added a command queue due to not overrun esp's ip stack (limit concurrent sessions)
       - Added attr maxQueueSize (default 250)
       - Added attr maxHttpSessions (default 5)
       - Added attr resendFailedCmd (default enabled)
       - new bridge commands: get queueSizes, set clearQueue
       - status command returns "?" in cause of of unknown gpio state
       - removed useless predefined subs, shutdown restart is required after module update
0.7.6  - revised some inexact regexps
0.7.7  - cmd rgb and attribute rgbGPIOs marked as being experimental
0.7.8  - Queue logging -> verbose 5
       - Queue error logging -> verbose 2
       - experimental commands rgb, hsv, ct, ...
       - changed resendFailedCmd default to be disabled
       - changed maxHttpSessions default to 3
0.7.9  - Added attr mapLightCmds: enable cmds like rgb, ct... and map them to an ESPEasy command
0.8.0  - Added attrs colorpickerCTww + colorpickerCTcw to select colorpickers ww-cw range
0.8.1  - commandref: lights command corrected
       - added commands: reboot, reset, ease. Use with care!
0.8.2  - preparation for svn.fhem.de check in

1.0.0  - Release 1.0 has been published on svn.fhem.de. Use FHEM's update command to get the current version
```
