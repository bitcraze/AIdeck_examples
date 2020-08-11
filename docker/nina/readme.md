# Docker Nina

Build the docker image while in the same directory as the docker file

```docker build --tag espidf:3.3.1 .```


First check with dmesg (for ubuntu) on which port the programmer is on, and write it down (should look like /dev/ttyUSB0 or another number)

Then use the following line of code in the terminal (make sure to change the --device if your programmer is on antoher port) and build the firmware (with the menuconfig) and flash the Nina module

```
sudo docker run --rm -it -v $PWD:/module/ --device /dev/ttyUSB0 --privileged -P espidf:3.3.1 /bin/bash -c "make menuconfig; make clean; make all; /openocd-esp32/bin/openocd -f interface/ftdi/olimex-arm-usb-tiny-h.cfg -f board/esp-wroom-32.cfg -c 'program_esp32 build/partitions_singleapp.bin 0x8000 verify' -c 'program_esp32 build/bootloader/bootloader.bin 0x1000 verify' -c 'program_esp32 build/ai-deck-jpeg-streamer-demo.bin 0x10000 verify reset exit'"
```


