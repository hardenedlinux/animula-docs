# Animula Virtual Machine Introduction

**Animula Virtual Machine (VM)** is firmware designed specifically for **compact embedded systems**. It can be thought of as an application running on a **Real-time Operating System (RTOS)**, making it easy to port to other platforms that the RTOS already supports.

By default, Animula supports [Zephyr RTOS](https://www.zephyrproject.org/). Animula VM can also run on **GNU/Linux**, typically using the [animula-linux project](https://github.com/hardenedlinux/animula-linux) for testing.

## The Idea Behind Animula

Animula was inspired by [PICOBIT: A Compact Scheme System for Microcontrollers](http://www.iro.umontreal.ca/~feeley/papers/StAmourFeeleyIFL09.pdf), although its design and implementation have been significantly reworked. The architecture of Animula (including the [Laco compiler](articles/docs/2)) was largely influenced by projects such as [GNU Guile](https://www.gnu.org/software/guile/) and [GCC](https://gcc.gnu.org/), both of which are designed to support multiple languages on a common compiler infrastructure.

Animula aims to serve as a platform for supporting **functional programming languages** that are highly optimized for embedded systems.

### Why Scheme?

The first language supported by Animula is **Scheme**, a well-researched language that has been widely used in computer science education and academic research for over three decades. There are several reasons why we chose Scheme as the first language on Animula:

- **Ease of Learning**: Many universities use Scheme to teach the basics of computer science ([MIT Press list](https://mitpress.mit.edu/sites/default/files/sicp/adopt-list.html)).
- **Extensive Resources**: There are numerous tutorials and articles available online.
- **Deep Knowledge**: Scheme allows you to dive deep into computer science concepts, should you choose to explore further.
- **Cross-Language Adaptability**: The knowledge you gain from learning Scheme can be easily transferred to other programming languages.
- **Powerful Expressiveness**: Scheme’s syntax and structure allow you to solve problems efficiently and elegantly.

Lastly, the [Laco compiler](articles/docs/2) is written in Scheme, so if you’re interested in compiler theory, working with Laco will provide additional learning opportunities.

## Animula as a Process Virtual Machine

Animula implements a **[process virtual machine](https://en.wikipedia.org/wiki/Virtual_machine#Process_virtual_machines)** running on an RTOS.

A **process virtual machine** abstracts the real CPU to provide a **platform-independent programming environment**. Its primary purpose is to hide the details of the underlying hardware or operating system, allowing a program to execute in the same way on any platform.

A process virtual machine interprets a series of sequential **bytecode** instructions.

For example, let's consider a simple "Hello World" program named **my.scm**:

```scheme
;; cat my.scm
(display "hello world\n")
```
In Animula, the executable bytecode program in its assembly form may look like this:
```scheme
;; laco -t sasm my.scm
(lef
  (memory
   (gen-intern-symbol-table) ;
   (main-entry) ;
   ) ; Memory end

  (program
    (label ____principio) ; Proc `____principio' begin
     (push-string-object "hello world\n") ;
     (prim-call 6 #t) ; Call primitive `display'
    (label-end ____principio); Proc `____principio' end
  (halt)
  ) ; Program end

 (clean
  ) ; Clean end

) ; End LEF
```
And its binary form may look like this:
```bash
# laco my.scm
# hexdump -C my.lef
00000000  4c 45 46 00 00 01 00 00  00 08 00 00 00 11 00 00  |LEF.............|
00000010  00 00 00 00 00 00 00 00  00 00 e2 08 68 65 6c 6c  |............hell|
00000020  6f 20 77 6f 72 6c 64 0a  00 c6 ff                 |o world....|
0000002b
```
In Animula VM, there's a big while-loop to execute each instruction one by one, till **(halt)**.

## Benefits of Animula VM

Animula VM is specifically designed for **embedded systems**, with a focus on optimizing for compact environments. For example, the size of the bytecode for the simple "Hello World" program mentioned earlier is only **43 bytes**.

### Optimized Performance
Compared to other interpreters running on embedded systems, Animula stands out by providing an **optimized compiler** and **virtual machine (VM)** that are tailored for resource-constrained environments.

### Key Features:
- **Tail-Call Optimization (TCO)**: Animula VM implements **[proper tail-call optimization (TCO)](https://en.wikipedia.org/wiki/Tail_call)**, as specified by the Scheme standard, ensuring efficient recursive function calls.
- **Closure Optimization**: Animula has optimized support for **[closures](https://en.wikipedia.org/wiki/Closure_(computer_programming))**, making it ideal for functional programming.
- **Peripherals Control**: Animula provides **dedicated primitive APIs** to interact with and control peripherals, making it a versatile tool for embedded applications.
- **Efficient Garbage Collection (GC)**: The VM features an efficient **[Garbage Collector (GC)](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science))**, essential for memory management in resource-limited environments.

Animula offers a compact, optimized, and efficient solution for running functional programming on embedded systems.
