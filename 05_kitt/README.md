### Install instructions

Install the following dependencies
```
sudo apt-get install python-pyaudio python3-pyaudio python3-pip sox
pip3 install pyaudio
sudo apt-get install libatlas-base-dev
```

Configure audio output and input  

The following command allows to determine that the playback device is card 0, device 0, or hw:0,0 (hw:0,1 is HDMI audio out).
```
$ aplay -l
 **** List of PLAYBACK Hardware Devices ****
 card 0: ALSA [bcm2835 ALSA], device 0: bcm2835 ALSA [bcm2835 ALSA]
   Subdevices: 8/8
   Subdevice #0: subdevice #0
   Subdevice #1: subdevice #1
   Subdevice #2: subdevice #2
   Subdevice #3: subdevice #3
   Subdevice #4: subdevice #4
   Subdevice #5: subdevice #5
   Subdevice #6: subdevice #6
   Subdevice #7: subdevice #7
 card 0: ALSA [bcm2835 ALSA], device 1: bcm2835 ALSA [bcm2835 IEC958/HDMI]
   Subdevices: 1/1
   Subdevice #0: subdevice #0
```   

The following command allows to determine that the recording device is card 1, device 0, or hw1:0.
```
$ arecord -l
**** List of CAPTURE Hardware Devices ****
card 1: Camera [Vimicro USB2.0 UVC Camera], device 0: USB Audio [USB Audio]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
```

The following file allows to setup defaults values for the audio input and output
```
vi ~/.asoundrc

pcm.!default {
  type asym
   playback.pcm {
     type plug
     slave.pcm "hw:0,0"
   }
   capture.pcm {
     type plug
     slave.pcm "hw:1,0"
   }
}
```

Run a voice record test
```
rec t.wav
```

Install swig 3.0.10 or above
```
wget http://downloads.sourceforge.net/swig/swig-3.0.10.tar.gz
sudo apt-get install libpcre3 libpcre3-dev
./configure --prefix=/usr                      \
            --without-clisp                    \
            --without-maximum-compile-warnings &&
make
PY3=1 make check-python-examples
PY3=1 make check-python-test-suite
make install &&
install -v -m755 -d /usr/share/doc/swig-3.0.10 &&
cp -v -R Doc/* /usr/share/doc/swig-3.0.10
```

Download snowboy repository and compile it
```
git clone https://github.com/Kitt-AI/snowboy.git
cd swig/Python
make
```

Run the examples

Bluetooth speaker configuration

```
root@raspberrypi:/tmp/snowboy/examples/Python# bluetoothctl
[NEW] Controller B8:27:EB:80:28:C8 raspberrypi [default]
[bluetooth]# power on
Changing power on succeeded
[bluetooth]# agent on
Agent registered
[bluetooth]# scan on
Discovery started
[CHG] Controller B8:27:EB:80:28:C8 Discovering: yes
[NEW] Device 00:06:8D:00:7B:13 S8
[bluetooth]# pair 00:06:8D:00:7B:13
Attempting to pair with 00:06:8D:00:7B:13
[CHG] Device 00:06:8D:00:7B:13 Connected: yes
[CHG] Device 00:06:8D:00:7B:13 UUIDs:
        00001108-0000-1000-8000-00805f9b34fb
        0000110b-0000-1000-8000-00805f9b34fb
        0000110e-0000-1000-8000-00805f9b34fb
        0000111e-0000-1000-8000-00805f9b34fb
[CHG] Device 00:06:8D:00:7B:13 Paired: yes
Pairing successful
[bluetooth]# trust 00:06:8D:00:7B:13
[CHG] Device 00:06:8D:00:7B:13 Trusted: yes
Changing 00:06:8D:00:7B:13 trust succeeded
[bluetooth]# paired-devices
Device 00:06:8D:00:7B:13 S8
[bluetooth]# connect 00:06:8D:00:7B:13
Attempting to connect to 00:06:8D:00:7B:13
Failed to connect: org.bluez.Error.Failed
```

```
sudo cat /var/log/syslog | grep -i protocol 
a2dp-source profile connect failed for AA:BB:CC:11:22:33: Protocol not available
```

```
sudo aptitude install bluetooth pulseaudio-module-bluetooth
```

```
vi /etc/pulse/default.pa

#load-module module-bluetooth-discover
```

```
vi /usr/bin/start-pulseaudio-x11

if [ x”$SESSION_MANAGER” != x ] ; then
  /usr/bin/pactl load-module module-x11-xsmp “display=$DISPLAY session_manager=$SESSION_MANAGER” > /dev/null
fi
/usr/bin/pactl load-module module-bluetooth-discover
```

Reboot your Pi
```
pactl load-module module-bluetooth-discover
```

### References
https://github.com/Kitt-AI/snowboy  
http://docs.kitt.ai/snowboy/  
http://www.linuxfromscratch.org/blfs/view/7.10/general/swig.html  
https://www.raspberrypi.org/forums/viewtopic.php?t=53299

