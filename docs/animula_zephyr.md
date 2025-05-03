# Compile Animula firmware for ZephyrRTOS

**NOTE: This is only for advanced developers. You should have a GNU/Linux environment.**

Before continue, please install Docker, we recommend the [official installation of Docker](https://docs.docker.com/get-docker/).

First, clone the repository:
```bash
git clone --recursive https://github.com/hardenedlinux/animula-zephyr.git
```

You need to pull the docker environment for animula-zephyr:
```bash
docker pull registry.gitlab.com/hardenedlinux/animula-zephyr:latest
```
Please be patient since the image is huge.
```bash
cd animula-zephyr
docker run -it --rm \
                 -v $PWD:/workspace \
                 -w /workspace \
                 -u "animula:animula" \
                 registry.gitlab.com/animula/animula-zephyr:latest \
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

## Flash the firmware

Please rename the firmware to **firmware.bin**:
```bash
cp build/zephyr/zephyr.bin firmware.bin
```
*NOTE: Now you should press ctrl+d to exit the docker environment!*

Please read [bootloader & firmware upgrade guide](alonzo_firmware_upgrade.md) to learn how to flash the firmware.
