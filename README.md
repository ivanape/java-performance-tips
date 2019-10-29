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

GC comes at a cost: when it runs, it often stops the world, which means while GC is in progress the application pauses. Usually these pause times are designed to be incredibly small, but as an application is put under pressure they can increase.


# Garbage Collection

The overall GC "Mark and Sweep" algorithm can then be expressed as:

- Loop through the allocated list, clearing the mark bit.
- Starting from the GC roots, find the live objects.
- Set a mark bit on each object reached.
- Loop through the allocated list, and for each object whose mark bit hasn’t been set:
  - Reclaim the memory in the heap and place it back on the free list.
  - Remove the object from the allocated list.

![](https://miro.medium.com/max/1741/1*_xkq7jGtAKf7SP1R1a8vcA.png)
![](https://miro.medium.com/max/1732/1*eZTk9FfqQVMpNWmmdgS8VA.png)

After the sweep phase, all the memory locations are rearranged to provide a more compact memory allocation. The downside of this approach is an increased GC pause duration as it needs to copy all objects to a new place and to update all references to such objects.

![](https://miro.medium.com/max/1729/1*8b-ANSuneRBXkO1JNtH6LQ.png)

## References
- https://www.cubrid.org/blog/understanding-java-garbage-collection
- https://blogs.oracle.com/jonthecollector/the-unspoken-phases-of-cms


# General Topics

## JVM
- OpenJDK
- Oracle
- Zulu
- IcedTea
- Zing
- J9
- Avian
- Android

## Monitoring and Tooling for the JVM

- Java Management Extensions (JMX)
- Java agents
- The JVM Tool Interface (JVMTI)
- The Serviceability Agent (SA)

