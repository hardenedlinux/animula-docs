# Animula - A Functional Programming Virtual Machine for Embedded Systems

<center>
<img src="img/animula.png" alt="Animula logo"/>
</center>

[LambdaChip renamed to Animula](https://www.nalaginrut.com/archives/2022/10/09/lambdachip%20renamed%20to%20animula)

## What is Animula?

Animula is built on two powerful ideas: *functional programming* and *embedded systems*. This unique combination brings clarity, elegance, and expressive power to embedded development.

**Animula** means *"a little soul"* in Latin. The name reflects the philosophy behind the project—a minimal yet expressive engine that breathes life into tiny machines.

Animula is a highly optimized virtual machine with a strong emphasis on functional programming. It’s designed specifically for compact embedded systems. With as little as **10KB of memory**, you can run a high-level language like **Scheme**—bringing the power of abstraction and composability into resource-constrained environments.

Embedded systems typically operate under severe resource constraints—think 50KB of RAM and CPUs running below 80MHz. Despite these limitations, Animula aims to deliver practical performance and expressive power within such environments.

In other words, with Animula, you don’t write embedded software in C or C++. Instead, you use **Scheme**—a clean, expressive, and multi-paradigm programming language. Scheme is a dialect of Lisp, known for its elegant syntax and its prominent role in both functional programming research and computer science education.

## What is Functional Programming?

Functional programming is a paradigm that helps reduce software complexity using techniques grounded in mathematics. But don’t worry—you don’t need deep theoretical knowledge to benefit from it. Most of its power comes from simple, composable patterns that developers can use intuitively.

It's widely taught in university Computer Science programs—often as an introduction to programming itself. Beyond academia, functional programming is used to teach problem-solving, algebra, geometry, and even classical mechanics, as explored in *Structure and Interpretation of Classical Mechanics*.

<center>
<img src="img/fp.png" alt="The features of functional programming" title="The features of functional programming" width="650"/>
</center>

## What Are Embedded Systems?

Generally speaking, an embedded system is a computer that doesn’t look like a computer. It's the intelligence inside everyday devices—like your coffee machine or a laser printer.

In the era of the Internet of Things (IoT), embedded systems are everywhere. They power cameras, sensors, thermometers, and more. Embedded software development forms the foundation of the IoT ecosystem.

But IoT has also transformed embedded development itself. Devices are no longer isolated—they're connected. This connectivity demands more advanced processing, which has led to the integration of cloud computing and machine learning. Modern embedded systems now often play a role in data collection, analysis, and decision-making at scale.

<center>
<img src="img/embedded_system.jpg" alt="The application of embedded systems" title="The application of embedded systems" width="650"/>
</center>


## Why Scheme?

<center>
<img src="img/scheme_is_better.jpg" alt="The benefit of Scheme" title="The benefit of Scheme" width="800"/>
</center>

This idea—that a high-level, expressive language like Scheme can be practical for embedded systems—is inspired in part by Erann Gat’s paper *“Point of View: Lisp as an Alternative to Java.”* The vision is simple but bold: expressive languages can thrive even in constrained environments.

> *(Illustration inspired by [LambdaNative](http://www.lambdanative.org/), another excellent project that enables Scheme development for Android.)*

## Is Animula Open Source?

**Animula is not only open-source—it’s fully free software.**

Unlike some open-source models that come with limitations, Animula is licensed to protect and promote **your four essential freedoms**:

- 🟢 **Freedom to run** the software for any purpose
- 📖 **Freedom to study** how it works and adapt it to your needs
- 🔁 **Freedom to redistribute** copies to help others
- 🛠️ **Freedom to improve or modify** the software and share your changes

These freedoms go beyond what the term *“libre-and-open-source”* typically guarantees. They represent a commitment to user autonomy and developer empowerment.

### Licensing Details

- **Laco Compiler**: Licensed under GPLv3.
- **Animula Virtual Machine (Firmware)**: Licensed under LGPLv3.
- **Animula Alonzo Board (Hardware Design)**: Licensed under Creative Commons BY-SA 4.0 (Free Hardware Design)

---

# How It Works

Animula implements a carefully optimized instruction set to execute **LEF** (Lightweight Executable Format) bytecode efficiently on embedded hardware.

Its compiler, called **Laco**—short for *Lambda Compiler*—is at the heart of this system. Unlike an interpreter, Laco compiles Scheme code into compact and efficient bytecode optimized for resource-limited devices.

# Documents

- Basics
  - [Quick Start](docs/quick_start.md)
- Supporting hardware:
  - [Alonzo board spec](docs/alonzo_spec.md)
    - [Quick start](docs/alonzo_quick_start.md)
    - [Peripheral API guide](docs/alonzo_api.md)
    - [Bootloader & firmware upgrade guide](docs/alonzo_firmware_upgrade.md)
