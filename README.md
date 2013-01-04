# SoCo
SoCo (Sonos Controller) is a simple Python class that allows you to programmatically control [Sonos speakers](http://www.sonos.com/system/). It was created at [Music Hack Day Sydney](http://sydney.musichackday.org/2012/), so there is still much room for improvement, however the basic control functionality is there and works well. I've tested it with both a Play:3 and a Play:5.

For more background on this project, please see my [related blog post](http://www.hirahim.com/blog/2012/04/29/dissecting-the-sonos-controller/).

## Set up
SoCo depends on the [Requests](http://docs.python-requests.org/) HTTP library. The easiest way to install it is through pip:

`pip install requests`

## Basic Usage

Discovery does IP addresses only for now, returning a list IP addresses of players.

```python
#!/usr/bin/env python
from soco import SoCo
from soco import SonosDiscovery

if __name__ == '__main__':
    sonos_devices = SonosDiscovery()

    for ip in sonos_devices.get_speaker_ips():
        device = SoCo(ip)
        zone_name = device.get_speaker_info()['zone_name']
        print "IP of %s is %s" % (zone_name, ip)

```

```python
#!/usr/bin/env python
from soco import SoCo

if __name__ == '__main__':
    sonos = SoCo('10.0.0.102') # Pass in the IP of your Sonos speaker

    # Pass in a URI to a media file to have it streamed through the Sonos speaker
    sonos.play_uri('http://archive.org/download/TenD2005-07-16.flac16/TenD2005-07-16t10Wonderboy_64kb.mp3')

    track = sonos.get_current_track_info()

    print track['title']

    sonos.pause()

    # Play a stopped or paused track
    sonos.play()
```

## Example Applications
To show off what can be made with SoCo, a basic commandline application and a simple web application are included in the `examples` folder.

![Screenshot of web app](https://github.com/rahims/SoCo/raw/master/examples/webapp/screenshot.png)

## Features
SoCo currently supports the following basic controls:

* Play
* Pause
* Stop
* Next track
* Previous track
* Get information about the currently playing track
    * Track title
    * Artist
    * Album
    * Album Art (if available)
    * Track length
    * Duration played (for example, 30 seconds into a 3 minute song)
    * Playlist position (for example, item 5 in the playlist)
    * Track URI
* Mute (or unmute) the speaker
* Get or set the speaker volume
* Get or set the speaker's bass EQ
* Get or set the speaker's treble EQ
* Toggle the speaker's loudness compensation
* Turn on (or off) the white status light on the unit
* Switch the speaker's source to line-in (doesn't work on the Play:3 since it doesn't have a line-in)
* Get the speaker's information
    * Zone Name
    * Zone Icon
    * UID (usually something like RINCON_XXXXXXXXXXXXXXXXX)
    * Serial Number
    * Software version
    * Hardware version
    * MAC Address
* Find all the Sonos speakers in a network. Code contributed by Thomas Bartvig.
* Put all Sonos speakers in a network into "party mode". Code contributed by Thomas Bartvig.
* Manage the Sonos queue (get the items in it, add to it, clear it, play a specific song from it)

## To-Do
Want to contribute to SoCo? Here's what needs to be done:

* playlist management
* better error checking

## Contributors
* Thomas Bartvig [thomas.bartvig@gmail.com](mailto:thomas.bartvig@gmail.com)
* Dave O'Connor doc@andvari.net
* [nixscripter](https://github.com/nixscripter)
* [labero](https://github.com/labero)

## License
Copyright (C) 2012 Rahim Sonawalla ([rsonawalla@gmail.com](mailto:rsonawalla@gmail.com) / [@rahims](http://twitter.com/rahims)).

Released under the [MIT license](http://www.opensource.org/licenses/mit-license.php).
