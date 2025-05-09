## Getting Started: Build and Run Animula on Linux

In this tutorial, you'll learn how to:

- Build the **Laco compiler**
- Write and compile a simple **Scheme program**
- Run it using the **Animula Virtual Machine**—all on your Ubuntu Linux PC

This is a great way to get hands-on experience with Animula's toolchain before deploying it to embedded hardware.

## Why Animula on Linux?

Running Animula on Linux is a great way to get started with the toolchain. It allows you to experiment with the Laco compiler and the Animula Virtual Machine without needing to set up an embedded environment right away. Animula Linux is important for CI/CD of Animula project, and It provides a convenient platform for developers to test their code before deploying it to embedded systems.

# Install Laco compiler

## Deps

```bash
sudo apt install guile-3.0-dev guile-3.0 build-essential
```

## Build Laco compiler

Laco compiler is a Scheme compiler that generates Lightweight Executable Format (LEF) bytecode. It is the first step in the Animula toolchain.

```bash
git clone https://github.com/hardenedlinux/laco
cd laco
./configure --prefix=/usr
make -j5
sudo make install
```

## Build Animula Virtual Machine for Linux

```bash
git clone --recursive https://github.com/hardenedlinux/animula-linux
cd animula-linux
make
```

Now there should be a file called `animula-vm` in the current directory. This is the Animula Virtual Machine for Linux, you may run LEF bytecode on it.


## Hello World

Any language or toolchains tutorial starts from a simple "Hello, World!" program.

```scheme
(display "hello world\n")
```
Let's compile it with Laco compiler:

```bash
laco hello.scm -o hello.lef
```

Now let's run it:

```bash
./animula-vm hello.lef
# hello world
```

## What's next?

Laco compiler implemented R7RS-small Scheme standard, so it's very limited compared to other Scheme implementations. But it is enough for embedded systems.
There are missing features, it's still a developing project for educational purpose and research, it's not ready for product. You may find some bugs, please report them to us.
