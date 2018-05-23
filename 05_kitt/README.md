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
exit
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

Reboot your Pi and run the following commands as a non root user. You might need to remove the device and pair it again after modules loading
```
/usr/bin/pulseaudio --kill
/usr/bin/pulseaudio --start
pactl load-module module-bluetooth-discover
pactl list modules | grep blue
sudo systemctl restart bluetooth
hciconfig hci0 up
```

```
root@raspberrypi:/home/pi# bluetoothctl
[NEW] Controller B8:27:EB:80:28:C8 raspberrypi [default]
[NEW] Device 00:06:8D:00:7B:13 S8
[bluetooth]# connect 00:06:8D:00:7B:13
Attempting to connect to 00:06:8D:00:7B:13
[CHG] Device 00:06:8D:00:7B:13 Connected: yes
Connection successful
[bluetooth]# info 00:06:8D:00:7B:13
Device 00:06:8D:00:7B:13
        Name: S8
        Alias: S8
        Class: 0x240404
        Icon: audio-card
        Paired: yes
        Trusted: yes
        Blocked: no
        Connected: yes
        LegacyPairing: no
        UUID: Headset                   (00001108-0000-1000-8000-00805f9b34fb)
        UUID: Audio Sink                (0000110b-0000-1000-8000-00805f9b34fb)
        UUID: A/V Remote Control Target (0000110c-0000-1000-8000-00805f9b34fb)
        UUID: A/V Remote Control        (0000110e-0000-1000-8000-00805f9b34fb)
        UUID: Handsfree                 (0000111e-0000-1000-8000-00805f9b34fb)
```

```
cat /var/log/syslog* | grep -i pulse
```

```
snowboy@raspberrypi:~/songs $ aplay bohemian.mp3
ALSA lib pulse.c:243:(pulse_connect) PulseAudio: Unable to connect: Access denied
aplay: main:722: audio open error: Connection refused
snowboy@raspberrypi:~/songs $ pulseaudio -k
E: [pulseaudio] main.c: Failed to kill daemon: No such process
snowboy@raspberrypi:~/songs $ pulseaudio --start
snowboy@raspberrypi:~/songs $ aplay bohemian.mp3
Playing raw data 'bohemian.mp3' : Unsigned 8 bit, Rate 8000 Hz, Mono
```

```
sudo usermod -a -G audio snowboy
```

```
jackd -d dummy
```

```
vi /usr/share/alsa/alsa.conf

pcm.rear cards.pcm.rear
pcm.center_lfe cards.pcm.center_lfe
pcm.side cards.pcm.side
pcm.hdmi cards.pcm.hdmi
pcm.modem cards.pcm.modem
pcm.phoneline cards.pcm.phoneline
```

### References
https://github.com/Kitt-AI/snowboy  
http://docs.kitt.ai/snowboy/  
http://www.linuxfromscratch.org/blfs/view/7.10/general/swig.html  
https://www.raspberrypi.org/forums/viewtopic.php?t=53299
https://raspberrypi.stackexchange.com/questions/67617/bluetoothctl-fails-to-connect-to-any-device-failed-to-connect-org-bluez-erro  
https://askubuntu.com/questions/689281/pulseaudio-can-not-load-bluetooth-module-15-10-16-04-16-10/689297#689297  
https://donjajo.com/bluetooth-fix-a2dp-source-profile-connect-failed-xx-protocol-not-available-linux/#.WwEPq2G1vIU  
https://bbs.archlinux.org/viewtopic.php?id=172261  
https://github.com/davidedg/NAS-mod-config/blob/master/bt-sound/bt-sound-Bluez5_PulseAudio5.txt  
https://github.com/ev3dev/ev3dev/issues/198  
https://github.com/Kitt-AI/snowboy/issues/9

