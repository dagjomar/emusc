# EmuSC

## About
libEmuSC is a software synthesizer library that aims to emulate the Roland Sound Canvas SC-55 lineup to recreate the original sounds of these '90s era synthesizers. Emulation is done by extracting relevant information from the original control and PCM ROMs and reimplement the synth's behavior in modern C++.

This project is in no way endorsed by or affiliated with Roland Corp.

## Status
We are in the early stages of development, but EmuSC is already able to display a relatively correct GUI compared to the original synth. MIDI input and audio output works, albeit with some quirks, for Linux, macOS and Windows. 

For current status on the quality of the synth audio emulation, see Status section in [libEmuSC](../libemusc/README.md).

## Requirements
EmuSC depends on C++11 and libQT5 in addition to platform dependent APIs for MIDI and audio. In addition you will need the original control and PCM ROMs.

### Linux
ALSA sequencer is needed for MIDI input. Both ALSA and PulseAudio are supported for audio output, but note that PulseAudio has a lot of latency in its default configuration so it is recommended to use ALSA if possible.

### Windows
There is rudimentary support for all modern versions of Windows for both MIDI input and audio output. Windows has however no default MIDI sequencer, so you will either need to have a hardware MIDI port with appropriate device driver, or you will have to use a "virtual loopback MIDI cable" program. There is unfortunately no free software alternative for the latter alternative today, but for example [LoopBe1](https://www.nerds.de/en/loopbe1.html) and [loopMIDI](https://www.nerds.de/en/loopbe1.html) are freely available for non-commercial use.

### macOS
There is rudimentary support for macOS 10.6 and newer for both MIDI input and audio output. Only default audio output device is currently supported.

## Building
CMake is used to generate the necessary files for building EmuSC. Depending on which operating system, audio system and build environment you are using the build instructions may vary. Independent of platform, a A C++11 compiler with support for std::threads, libEmuSC and libQt5 (Core, Widgets and GUI) are absolute requirements.

### Linux
Note the following dependencies for Linux:
* ALSA (libasound2-dev on debian based distributions) is needed for MIDI input and ALSA audio.
* PulseAudio (libpulse-dev for debian based distributions) is needed for PulseAudio support.

### Windows
For building EmuSC on MSYS2/MinGW-w64 you need to do the following steps:

1. Install MSYS2 build environment: https://www.msys2.org and follow the [instructions for building and installing libEmuSC](../libemusc/README.md)
2. Start the **MSYS2 UCRT64** console and install extra libraries needed by EmuSC
```
pacman -S mingw-w64-ucrt-x86_64-qt5-base mingw-w64-ucrt-x86_64-qt5-multimedia
```
3. Enter the correct build direcotory
```
cd emusc/emusc
```
4. Run cmake (note that you have to specify the build generator) & make
```
cmake . -G "MSYS Makefiles"
```
5. And finally build the application by running
```
make
```
The `emusc.exe` binary is located in `emusc/src/` if no build directory was specified. Note that you will need to deploy Qt and include a number of DLL-files if you want to run EmuSC outside MSYS. A simple script for automating the deployment of Qt and copying all the necessary DLL-files is found in the emusc/utils directory. To use this script you must have the Qt tools package installed:
```
pacman -S mingw-w64-ucrt-x86_64-qt5-tools
```
And then run the create bundle script:
```
cd emusc/utils
./msys_create_dll_bundle.sh ../src
```
The `emusc/BUILD_WIN32` directory shall now contain all files needed to run EmuSC on any Windows computer.

Note that if you have a running in a Linux environment you can also cross-compile a Windows binary by using the MinGW toolchain.

### macOS
For some reason Apple decided to not follow the C standard in their MIDI implementation. Due to this, Clang is needed for compiling MIDI support on macOS.

If you are using homebrew, install qt@5.
```
brew install qt@5
```
Then run cmake and specify the correct path to qt.
Find the correct path depending on your system and how you installed QT. Here are some examples, to help you find the one on your own system.
```
cmake . -DCMAKE_PREFIX_PATH=/opt/homebrew/opt/qt@5
cmake . -DCMAKE_PREFIX_PATH=/usr/local/Cellar/qt@5/5.15.6
```
On macOS the default build is not a binary file, but a bundle. To install the application copy src/emusc.app to your application folder. To run EmuSC directly from the terminal:
```
open src/emusc.app
```

## Contribute
Interested in contributing to this project? All contributions are welcome! Download / fork the source code and have a look. Create an issue if you have any questions (or input / suggestions) and we will do our best to help you out getting the hang of how it all works!

All EmuSC source code is released under the GPL 3+ license.
