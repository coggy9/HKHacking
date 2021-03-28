

  

## Invoke

  

The Harman Kardon Invoke, codename Podium, is a Cortana powered smart speaker that released on October 22, 2017. As of March 7th 2021, Harman Kardon has released an update that removes all smart features of the speaker. This includes Cortana support and Wi-Fi connectivity.

  

This page focuses on the last Cortana enabled firmware, labeled "Barracuda_libre-11.1842.0".

  

## Hardware

The Invoke has two boards. A main amp board powering the speakers, and a daughterboard with the actual compute power. The compute daughterboard is a [LS9ADAC11DBT](https://fccid.io/2ADBM-LS9ADAC11DBT/User-Manual/User-manual-3586873) by Libre Technologies.

## Software
The Invoke runs a custom version of Linux, with some Android bits, such as adbd and logcat, included.
A partial dump of the system can be found on the [releases](https://github.com/coggy9/HKHacking/releases) page.

  

## Initial Setup

During initial setup, the Invoke has it's own Wi-Fi AP, which the device performing setup connects to in order to send the SSID and password of the Wi-Fi network the Invoke will connect to. While this AP is active, the Invoke has a static IP of 192.168.43.1 .

  

Known pages accessible through AP:

https://192.168.43.1/getlogcat.asp - Sends a logcat

https://192.168.43.1/goform/HandleCommand?CMD= - Performs a command on the device.

Known commands: RECONNECT_ACK , STOP_SAC

https://192.168.43.1/goform/HandleSACConfiguration - SSID and passsword info gets sent here with a simple POST with the following info: `SSID=ssidgoeshere&Passphrase=passwordgoeshere&Security=WPA-PSK`

https://192.168.43.1/cgi-bin - Execute CGI scripts, if any exist on device. All common ones return a not found error.

  

nmap results while in setup mode:

  

PORT | STATE | SERVICE
------ | ------|----------
22/tcp| filtered| ssh
53/tcp| open| domain
7777/tcp|open| cbt
8080/tcp|open| http-proxy
9998/tcp|filtered| distinct32
9999/tcp|filtered| abyss

  
  
  
  

## Normal Operation

During normal operation, the Invoke does not respond to any of the URLs accessible when in setup mode.

## Flashing Mode

To enter "flashing mode", hold reset while plugging in the Invoke, and press the mic mute button 4 times within 5 seconds. Once the ring turns yellow, you can let go of the reset button.


## Reflash Image

With Harman releasing [tools](https://github.com/coggy9/HKHacking/releases/tag/HarmanFlash "Harman.Kardon.INVOKE.Flashing.zip") to reflash the Invoke to some users, we can get a closer look at how the system works.

The tools require the user to run commands in a telnet/serial terminal while the Invoke is in flashing mode. The terminal is a U-Boot terminal. 
  

## Exploits

Currently, there is only one exploit known to work with the Invoke, [CVE-2001-0228](https://www.cvedetails.com/cve/CVE-2001-0228/) , which allows retrieval of some files from the filesystem while in initial setup mode. HTTPS must be used, and files past a certain size will timeout and fail to be sent.

  

[CVE-2002-1951](https://www.exploit-db.com/exploits/21707) has not produced any results yet.


[CVE-2002-1603](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2002-1603) retrieves the code for ASP pages, but there is nothing useful in the ASP files.
  

## GPL Code

Harman-Kardon gave me a partial release of the GPL licensed source after a year of asking for it. It is available [here](https://archive.org/details/HK-Invoke-source-disclosure).

The actual kernel code can be found [here](https://archive.org/details/invoke-kernel).
