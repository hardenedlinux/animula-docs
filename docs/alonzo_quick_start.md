<center>
<img src="../img/_MG_4241_transparent.png" alt="Animula Alonzo" width="800" />
</center>

In this tutorial, we'll teach you the process to make the Alonzo board work. It's for newbies of Animula.

## Build from source code

```bash
sudo apt install getttext texinfo guile-3.0 guile-3.0-dev make automake git autoconf libtool

git clone https://github.com/hardenedlinux/laco.git
./configure --prefix=/usr
make -j5
sudo make install
```

## Build from Docker

Before continue, please install Docker, we recommend the [official installation of Docker](https://docs.docker.com/get-docker/).

When you have a workable docker system, please copy and run this command:
```bash
docker pull registry.gitlab.com/hardenedlinux/laco:latest
```

Please be patient and make sure you have sufficient free disk space. Fortunately, this image contains everything you need to work with Animula.

*NOTE: The image is more than 500MB.*

*NOTE: You may run the above command occasionally to upgrade the Laco compiler painlessly.*

### Create a workspace

Edit the source code in your host operating system, and compile the source code inside the docker environment (the guest system). So that you don't have to make your system dirty, and you are able to use your favorite editor/IDE to write code or debug.

Before getting into the docker environment, we need to prepare a workspace. The basic idea is to create a directory for your Animula projects. And this directory will be mapped into the docker environment, so you can control the files in both host and guest environments.

For example, in GNU/Linux, we can create the workspace like this:
```bash
mkdir ~/animula-workspace
```
OK, we've finished preparing the environment, now we can use it.

## Build with docker

Laco doesn't provide a prepared Docker image, we strongly recommend you to build it by yourself.

```bash
sudo docker build -t laco:latest .
```

### Get into the Laco environment

Please copy and run this command to login:
```bash
cd animula-workspace
docker run -it --rm \
                 -v $PWD:/workspace \
                 -w /workspace \
                 -u "animula:animula" \
                 laco:latest \
                 bash
```
Now you should see something similar like this:

```bash
animula@9e43fbe7cf38:/workspace$
```
Welcome to your new workspace, we can start to write code now.

*NOTE: The /workspace inside docker was mapped to ~/animula-workspace on your host system.*

*NOTE: Forget about the meaning of this command line, you'll learn more when you follow our steps.*

### Write the first Scheme program

Traditionally, when you start to learn a new programming environment, the first program is always **hello world** which is a trivial program to print a "hello world" string. However, in embedded software programming, the "hello world" is different, we call it **blink** which is also a trivial program to make LED blinking.

Open your favorite editor or IDE, and create a new file named **program.scm**. Just copy and paste the code below:

```scheme
(define (main x)
  (gpio-toggle! 'dev_led0)
  (usleep 100000)
  (gpio-toggle! 'dev_led0)
  (usleep 100000)
  (if (= x 1)
    #t
    (main (- x 1))))
(main 10)
```
The code is self-explain, you may try to modify it to verify your ideas. Don't worry, the Alonzo board will never be bricked.

*NOTE: If you bricked it, congrats! You have the chance to learn how to flash the bootloader of the Alonzo board.*

### Compile it

Now we can compile the program with the Laco compiler:
```bash
laco program.scm
```
If the compilation works successfully, you may not see anything output when it is finished. Now you should find there's a new file named **program.lef**. That's the generated bytecode file for the Animula virtual machine.

Copy the **program.lef** to **TF card** and then insert the TF card to Alonzo board.

<center>
<img src="../img/8G-tf-card.jpg" title="TF card" alt="TF card" width="450"/>
</center>

Animula virtual machine will scan the *TF card* and find the file named **program.lef**. When it's found then run it automatically. If you use other names then it doesn't work at all.

### Blinking!

Everything is OK! Please connect the Alonzo to your PC or power bank with a USB Type-C cable, and the board will power on, then you should see the LED blinking periodically.
