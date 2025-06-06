# What is Laco compiler?

The complete name of Laco compiler is Lambda Combination compiler. We also call it Lambda Compiler for short. This name implies the compiler was related to a bunch of [Lambda calculus](https://en.wikipedia.org/wiki/Lambda_calculus) combinations.

These Lambda calculus combinations base on [CPS (Continuation Passing Style)](https://en.wikipedia.org/wiki/Continuation-passing_style). There're several notable use cases for CPS, co-routine, exception handling, and the [IR (Intermediate Representation)](https://en.wikipedia.org/wiki/Intermediate_representation) in a compiler architecture. CPS was a well-researched technic for decades, it's widely used for functional programming featured compilers. In Laco, the CPS plays a significant role in optimizations. The compiler optimization in Laco takes advantage of some different CPS transformation algorithms, and they're implemented with Lambda calculus combinations. That's why the compiler is named Laco.

## The architecture of Laco compiler

The architecture of Laco compiler consists of three main parts, the front-end, middle-end and the back-end.
The different front-end implies different programming language that implemented on Laco. The middle-end is mainly for compiler optimization. The back-end is used for generating Animula specific bytecode that can run on the Animula Virtual Machine running on the embedded system.

**Laco compiler is flexible enough to add different language front-end, except for Scheme, we plan to add Lua and Javascript.**

<center>
<img src="../img/laco_arch_intro.png" alt="Laco architecture" width="800" />
</center>

## How to compile with Laco compiler

It's very simple:
```bash
laco [option] filename -o output_name
```
If you don't specify the output filename with **-o** option, then the output file will be named as **filename.lef** automatically.

## What does a CPS look like?

For example, we have a simple Scheme program named **a.scm**:
```scheme
(define (add x y) (+ x y))

(add 1 2)
```

We may output the optmized CPS result with Laco:
```bash
laco -t opt a.scm
```

Then we get the output:
``` scheme
(module
  (define add
    (lambda (#{#kont-93}# x94 y95)
      ((local #{#kont-93}# 0)
       (+ (local x94 1) (local y95 2)))))
  ((global add) return 1 2))
```
The option **-t opt** will output the optimized CPS result, it's human readable and it eliminated the useless intermediate code, if you want to see the raw CPS (it's verbose and redundant), then you may try:
```bash
laco -t cps a.scm
```

*Please notice that the output result was beautified for human readability, the actual CPS forms in the internal compiler are complicated and unreadable for non-expertise.*
