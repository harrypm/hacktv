# HackTV - Analogue TV transmitter software suite for the HackRF

https://sanslogic.co.uk/hacktv


> [!WARNING]
> - The hackrf is not designed to be connected directly to AV equipment and could
be damaged by, or cause damage to, your receiver. 
> - Please ensure no DC voltages or control signals are sent back into the hackrf, and that the RF power levels
out of the hackrf are not too high for your receiver.


WHAT'S IT DO

Generates a PAL, NTSC, SECAM, D/D2-MAC video signal from a video file, stream
or test pattern. Also supports older 819, 405, 240 and 30 line standards, as
well as the NASA Apollo video standards, both colour and mono.

Input is any file type or URL supported by ffmpeg.

Output can be to a file, HackRF, fl2k-supported VGA adaptors or any SDR
supported by SoapySDR.

It also supports:

+ Teletext (625-line only)
+ NICAM stereo audio
+ Videocrypt I/II/S hardware support
+ Partial Nagravision Syster hardware support
+ Analogue Copy Protection system, similar to Macrovision


WHAT'S IT NOT DO (yet)

+ An optional notch filter for the colour subcarrier would be nice


WHAT IT WON'T DO

+ DVB or other pure digital signals
+ Bring back Firefly :(


# REQUIREMENTS

Depends on libhackrf and various ffmpeg libraries.

## For Fedora (with rpmfusion)

    yum install hackrf-devel osmo-fl2k-devel SoapySDR-devel ffmpeg-devel

## For Debian and related
    apt-get update
    apt-get install libhackrf-dev libavutil-dev libavdevice-dev libswresample-dev libswscale-dev libavformat-dev libavcodec-dev

On Debian (sid)

    apt-get install hacktv

## Build The Software

   cd src
   make
   make install


# Usage Examples

## Generate a file containing a PAL baseband signal from a video

    hacktv -o baseband.bin -m pal example.mkv

## Transmit a test pattern on UHF channel 31 (PAL System I), 47dB TX gain

   hacktv -f 551250000 -m i -g 47 test

## Transmit a test pattern with teletext

   hacktv -f 551250000 -m i -g 47 --teletext demo.tti test

## Download and transmit teletext pages from the Teefax service

   svn checkout http://teastop.plus.com/svn/teletext/ teefax
   hacktv -f 551250000 -m i -g 47 --teletext teefax test

## Transmit two channels simultaneously on UHF channel 68 and 69 (PAL I)
 
   hacktv -s 20000000 --offset -6.75e6 --level 0.5 --filter -o - test | hacktv -s 20000000 -f 854e6 --offset 1.25e6 --level 0.5 --passthru /dev/stdin -g 47 --filter test

## Grab and transmit the local display (X11)

   hacktv -f 551.25e6 -m i -g 47 -ffmt x11grab --fopts framerate=25 ffmpeg::0

## LINKS

https://github.com/captainjack64/hacktv - Fork of hacktv with support for additional scrambling systems
https://github.com/steeviebops/jhacktv-gui - A cross platform GUI for hacktv written in Java
https://github.com/inaxeon/hacktv-hackrf - The HackDAC high quality baseband genaration hat for Composite & Audio.

-Philip Heron <phil@sanslogic.co.uk>


