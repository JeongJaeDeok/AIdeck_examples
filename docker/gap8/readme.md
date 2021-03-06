## Docker Gap8

Prebuild the docker image while in the same directory as the docker file.

*Note: Depending on your operating system and docker installation you might need to perform the following commands with `sudo`. For more information see the [docker documentation](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user)*.

```
docker build --tag gapsdk:3.6 .
```

Open up the container to install the auto tiler
```
docker run --rm -it gapsdk:3.6 /bin/bash
```

Then in the container write:
```
cd /gap_sdk
source configs/ai_deck.sh
make autotiler
```
This will install the autotiler, which requires you to registrer your email and get a special URL token to download and install the autotiler.

In a seperate terminal on your local machine, commit the changes to the image by first looking up the container ID status:
```
docker ps
```

Copy and past the container ID and replace the <container id> on the line here below, then run it in the seperate terminal
```
docker commit <container id> gapsdk:3.6
```

This will save the install of the autotiler on your image.

_Note: if you would you would like to also build docker images for SDK version 3.4 and 3.5, just replace the line that checkouts the right version of the sdk to either **git checkout release-v3.4** or **git checkout release-v3.5**. Don't forget to change the version name to **gapsdk:3.4** or **gapsdk:3.5**_

### Running an Example
On your host navigate to the `make`-file you want to execute. For example

```
cd <path/to/AIdeck_examples/Repository>/GAP8/image_processing_examples/image_manipulations
```

The following docker commands will build and run the program on RAM (this will disappear after restart of the aideck). Make sure to replace the /dev/ttyUSB0 with the actual path of your debugger (which can be found by dmesg on Ubuntu). Also make sure that the right .cfg file is selected that fits your debuggger.

```
docker run --rm -it -v $PWD:/module/data/ --device /dev/ttyUSB0 --privileged -P gapsdk:3.6 /bin/bash -c 'export GAPY_OPENOCD_CABLE=interface/ftdi/olimex-arm-usb-tiny-h.cfg; source /gap_sdk/configs/ai_deck.sh; cd /module/data/;  make clean all run'
```

The following docker commands will build and flash the program on the GAP8 (this will NOT disappear after restart of the aideck).

```
docker run --rm -it -v $PWD:/module/data/ --device /dev/ttyUSB0 --privileged -P gapsdk:3.6 /bin/bash -c 'export GAPY_OPENOCD_CABLE=interface/ftdi/olimex-arm-usb-tiny-h.cfg; source /gap_sdk/configs/ai_deck.sh; cd /module/data/;  make clean all image flash'
```
