PyCozmo
=======

`PyCozmo` is a pure-Python [Anki Cozmo](https://anki.com/en-us/cozmo.html) communication library. It allows controlling
a Cozmo robot directly, without having to go through a mobile device, running the Cozmo app.

The library is loosely based on the [Anki Cozmo Python SDK](https://github.com/anki/cozmo-python-sdk) and the
[cozmoclad](https://pypi.org/project/cozmoclad/) ("C-Like Abstract Data") library.

This project is a tool for exploring the hardware and software of Anki Cozmo. It is unstable and heavily under
development.


Usage
-----

Basic mode:
```python
import time
import pycozmo

def pycozmo_program(cli):
    cli.set_head_angle(angle=0.6)
    time.sleep(1)

pycozmo.run_program(pycozmo_program)
```

Advanced mode:
```python
import pycozmo

cli = pycozmo.Client()
cli.start()
cli.connect()
cli.wait_for_robot()

cli.drive_wheels(lwheel_speed=50.0, rwheel_speed=50.0, duration=2.0)

cli.disconnect()
cli.stop()
```


Documentation
-------------

- [Cozmo protocol](docs/protocol.md) description
- [Cozmo function](docs/functions.md) description
- [Capturing Cozmo communication](docs/capturing.md)
- API documentation: http://pycozmo.readthedocs.io/


Examples
--------

- [rc.py](examples/rc.py) - turns Cozmo into an RC tank that can be driven with an XBox 360 Wireless controller or 
    Logitech Gamepad F310
- [extremes.py](examples/extremes.py) - demonstrates Cozmo lift and head control
- [backpack_lights.py](examples/backpack_lights.py) - demonstrates Cozmo backpack LED control
- [cube_lights.py](examples/cube_lights.py) - demonstrates cube connection and LED control
- [cube_light_animation.py](examples/cube_light_animation.py) - demonstrates cube LED animation control
- [charger_lights.py](examples/charger_lights.py) - demonstrates Cozmo charging platform LED control
- [display_image.py](examples/display_image.py) - demonstrates visualization of image files on Cozmo's display
- [display_lines.py](examples/display_lines.py) - demonstrates 2D graphics, using
    [PIL.ImageDraw](https://pillow.readthedocs.io/en/stable/reference/ImageDraw.html) on Cozmo's display
- [audio.py](examples/audio.py) - demonstrates 22 kHz, 8-bit, mono WAVE file playback through Cozmo's speaker 
- [events.py](examples/events.py) - demonstrates event handling
- [camera.py](examples/camera.py) - demonstrates capturing a camera image 


Tools
-----

- [pycozmo_dump.py](tools/pycozmo_dump.py) - a command-line application that can read and annotate Cozmo communication
    from [pcap files](https://en.wikipedia.org/wiki/Pcap) or capture it live using
    [pypcap](https://github.com/pynetwork/pypcap).
- [pycozmo_replay.py](tools/pycozmo_replay.py) - a basic command-line application that can replay .pcap files back to
    Cozmo.


Robot Support
-------------

Sensors:
- [x] Camera
- [x] Cliff sensor
- [x] Accelerometers
- [x] Gyro
- [x] Battery voltage
- [x] Cube battery voltage
- [x] Cube accelerometers

Actuators:
- [x] Wheel motors
- [x] Head motor
- [x] Lift motor
- [x] Backpack LEDs
- [x] IR LED
- [x] OLED display
- [ ] Speaker - work progress
- [x] Cube LEDs
- [x] Platform LEDs

Communication:
- [x] Wi-Fi AP
- [x] Bluetooth LE

Storage:
- [x] NVRAM
- [x] Firmware update

Other:
- [ ] Animations from FlatBuffers .bin files - work in progress
- [ ] Procedural faces - work in progress


Connecting to Cozmo over Wi-Fi
------------------------------

A Wi-Fi connection needs to be established with Cozmo before using PyCozmo applications.

1. Wake up Cozmo by placing it on the charging platform
2. Make Cozmo display it's Wi-Fi PSK by rising and lowering its lift
3. Scan for Cozmo's Wi-Fi SSID (depends on the OS)
4. Connect using Cozmo's Wi-Fi PSK (depends on the OS)


PyCozmo vs. the Cozmo SDK
-------------------------

A Cozmo SDK application (aka "game") acts as a client to the Cozmo app (aka "engine") that runs on a mobile device.
The low-level communication happens over USB and is handled by the `cozmoclad` library.

In contrast, an application using PyCozmo basically replaces the Cozmo app and acts as the "engine". PyCozmo handles
the low-level UDP communication with Cozmo.
   
```
+------------------+                     +------------------+                     +------------------+
|     SDK app      |      Cozmo SDK      |    Cozmo app     |       PyCozmo       |      Cozmo       |
|      "game"      |      cozmoclad      |     "engine"     |                     |     "robot"      |
|                  | ------------------> |   Wi-Fi client   | ------------------> |     Wi-Fi AP     |
|                  |         USB         |    UDP client    |      UDP/Wi-Fi      |    UDP Server    |
+------------------+                     +------------------+                     +------------------+
```


Limitations
-----------

- some high-level Cozmo SDK features are implemented in the Cozmo app and have no equivalent in PyCozmo, today:
    - personality engine
    - behaviors
    - motion detection
    - face detection
    - facial expression estimation
    - text-to-speech
    - songs
- there is no Wi-Fi control. The library assumes a Wi-Fi connection to Cozmo, established in advance.
- frame retransmission is not implemented
- transmission of multiple packets in a single frame is not implemented


Requirements
------------

- Python 3.5.4
- [Pillow](https://github.com/python-pillow/Pillow) 6.0.0 - Python image library
- [FlatBuffers](https://github.com/google/flatbuffers) - serialization library
- [dpkt](https://github.com/kbandla/dpkt) - TCP/IP packet parsing library 


Installation
------------

Using pip:

```
pip install pycozmo
```

From source:

```
git clone https://github.com/zayfod/pycozmo.git
cd pycozmo
python setup.py install
```

From source, for development:

```
git clone git@github.com:zayfod/pycozmo.git
cd pycozmo
python setup.py develop
```

 
Support
-------

Bug reports and patches should be sent via GitHub:

https://github.com/zayfod/pycozmo

Anki Robot Discord server, channel #cozmo:

https://discord.gg/ew92haS
