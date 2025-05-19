# Compile Animula firmware for ZephyrRTOS

**NOTE: This is only for advanced developers who wants to develop & debug Animula VM on ZephyrRTOS. You should have a GNU/Linux environment.**

Again, if you're new to Animula, or you just want to enjoy functional programming on embedded system, please stop here. We always welcome you back here when you are ready to be a hardcore hacker.

## How to compile Animula firmware for ZephyrRTOS

### Build with Doccker (recommended)

We provide a Docker image for working with the Animula firmware for ZephyrRTOS. However, it's limited to the version of zephyrRTOS we are currently testing. If you want to use higher version of zephyrRTOS, please jump to the next section.

If you're new to Docker, please read [the official installation of Docker](https://docs.docker.com/get-docker/).

First, clone the repository:

```bash
git clone --recursive https://github.com/hardenedlinux/animula-zephyr.git
```

You need to pull the docker environment for animula-zephyr:

```bash
docker build -t hardenedlinux/animula-zephyr:latest .
```

Please be patient since the image is huge.
```bash
cd animula-zephyr
docker run -it --rm \
                 -v $PWD:/workspace \
                 -w /workspace \
                 -u "animula:animula" \
                 hardenedlinux/animula-zephyr:latest \
                 bash
```
You should see something similar like this:

```bash
animula@75c89755d8cc:/workspace$
```
Now just run make:
```bash
make
```

### Build under GNU/Linux with ZephyrRTOS source

We assume you have a working Debian/Ubuntu GNU/Linux environment.

#### Prepare the environment

```bash
mkdir zephyrRTOS
cd zephyrRTOS

# prepare the virtual environment
sudo apt install python3-venv
python3 -m venv .venv
source .venv/bin/activate

pip install west
west init zephyr
west update

# export a Zephyr CMake package. This allows CMake to automatically load boilerplate code required for building Zephyr applications.
west zephyr-export

# export a Zephyr CMake package. This allows CMake to automatically load boilerplate code required for building Zephyr applications.
west packages pip --install

```

**west tool** will start to download the zephyrRTOS source code. This may take a while depending on your internet connection.

#### Download ZephyrRTOS SDK

You need SDK for cross-compiling toolchains.

```bash
wget -c https://github.com/zephyrproject-rtos/sdk-ng/releases/download/v0.17.0/zephyr-sdk-0.17.0_linux-x86_64.tar.xz
tar xvf zephyr-sdk-0.17.0_linux-x86_64.tar.xz
mv zephyr-sdk-0.17.0 sdk
```

#### Download ZephyrRTOS source

```bash

#### Set the environment variables

Put the following lines in your **~/.bashrc** or **~/.profile** file:

```bash
export ZEPHYR_TOOLCHAIN_VARIANT=zephyr
export ZEPHYR_BASE=/your_project_dir/zephyrRTOS/zephyr
export ZEPHYR_SDK_INSTALL_DIR=/your_project_dir/zephyrRTOS/sdk
#export ZEPHYR_TOOLCHAIN_VARIANT=gnuarmemb
export GNUARMEMB_TOOLCHAIN_PATH=/your_project_dir/zephyrRTOS/sdk/arm-zephyr-eabi/
export Zephyr_DIR=$ZEPHYR_BASE
```

Don't forget to source the file:
```bash
source ~/.bashrc
```

#### Build the firmware

```bash
make
```

## Flash the firmware

Please rename the firmware to **firmware.bin**:
```bash
cp build/zephyr/zephyr.bin firmware.bin
```
*NOTE: Now you should press ctrl+d to exit the docker environment!*

Please read [bootloader & firmware upgrade guide](alonzo_firmware_upgrade.md) to learn how to flash the firmware.
