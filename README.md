# Performance Main Concepts

## Throughput
## Latency
## Capacity
## Utilization
## Efficiency
## Scalability
## Degradation

# JVM overview

## Classloading
## Executing Bytecode
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

