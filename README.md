# Performance Main Concepts

## Throughput
## Latency
## Capacity
## Utilization
## Efficiency
## Scalability
## Degradation

<br/>

# JVM overview

## Classloading
## Bytecode
### Anatomy of class file

- Magic number: 0xCAFEBABE
- Version of class file format: The minor and major versions of the class file
- Constant pool: The pool of constants for the class
- Access flags: Whether the class is abstract, static, and so on
- This class: The name of the current class
- Superclass: The name of the superclass
- Interfaces: Any interfaces in the class
- Fields: Any fields in the class
- Methods: Any methods in the class
- Attributes: Any attributes of the class (e.g., name of the source file, etc.)

## HotSpot
In April 1999 Sun introduced one of the biggest changes to Java in terms of performance. The HotSpot virtual machine is a key feature of Java that has evolved to enable performance that is comparable to **Ahead-of-Time (AOT)** compilation such as C and C++.

![](https://www.researchgate.net/profile/Hanspeter_Moessenboeck/publication/220169951/figure/fig1/AS:339987729010691@1458070799501/Architecture-of-the-Java-HotSpot-TM-VM.png)

## Just-in-Time compilation

HotSpot achieves this by compiling units of your program from interpreted bytecode into native code. The units of compilation in the HotSpot VM are the method and the loop. This is known as Just-in-Time (JIT) compilation.

## JVM Memory Management

Java looked to help resolve the problem by introducing automatically managed heap memory using a process known as garbage collection (GC). Simply put, garbage collection is a **nondeterministic** process that triggers to recover and reuse no-longer-needed memory when the JVM requires more memory for allocation.

GC comes at a cost: when it runs, it often stops the world, which means while GC is in progress the application pauses. Usually these pause times are designed to be incredibly small, but as an application is put under pressure they can increase


