# Performance Main Concepts

## Throughput
A throughput measurement is based on the amount of work that can be accomplished in a certain period of time. This measurement is frequently referred to as transactions per second (TPS), requests per second (RPS), or operations per second (OPS).

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

<br/>

# JVM Memory Management

Java looked to help resolve the problem by introducing automatically managed heap memory using a process known as garbage collection (GC). Simply put, garbage collection is a **nondeterministic** process that triggers to recover and reuse no-longer-needed memory when the JVM requires more memory for allocation.

GC comes at a cost: when it runs, it often stops the world, which means while GC is in progress the application pauses. Usually these pause times are designed to be incredibly small, but as an application is put under pressure they can increase.

## Garbage Collection

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

- GC algorithms generally divide the heap into old and young generations.
- GC algorithms generally employ a stop-the-world approach to clearing objects from the young generation, which is usually a very quick operation.
- Minimizing the effect of performing GC in the old generation is a trade off between pause times and CPU usage.

### OpenJDK Collectors

- https://docs.oracle.com/en/java/javase/12/gctuning/available-collectors.html#GUID-F215A508-9E58-40B4-90A5-74E29BF3BD3C

### Basic GC Tuning
- Sizing the Heap
- Sizing the Generations
- Sizing Metaspace
- Controlling Parallelism
- Adaptive Sizing

## Allocation and Lifetime
There are two primary drivers of the garbage collection behavior of a Java application:

- Allocation rate
- Object lifetime

The allocation rate is the amount of memory used by newly created objects over some time period (usually measured in MB/s). 

## OOPs & KlassOOPs
- https://www.infoq.com/articles/Introduction-to-HotSpot/

## GC Tools

### jstack

```
jstat -gcutil process_id 1000
```

### Xlog
- http://openjdk.java.net/jeps/158

Xlog configuration examples:

```
-Xlog:gc*,gc+ref=debug,gc+heap=debug,gc+age=trace:file=gc-%p-%t.log:tags,uptime,time,level:filecount=10,filesize=50m

-Xlog:safepoint*:file=safepoints-%p-%t.log:tags,uptime,time,level:filecount=10,filesize=50m
```

### GCViewer
- https://github.com/chewiebug/GCViewer/wiki/Changelog
- https://gceasy.io/

## GC Tips
- GC logs are the key piece of data required to diagnose GC issues; they should be collected routinely (even on production servers).
- A better GC logfile is obtained with the PrintGCDetails flag.
- Programs to parse and understand GC logs are readily available; they are quite helpful in summarizing the data in the GC log.
- `jstat` can provide good visibility into GC for a live program.

## GC Causes Distilled

- https://dzone.com/articles/java-gc-causes-distilled
- https://es.slideshare.net/KubaKubryski/xxuseg1gc

<br/>

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

## Java Monitoring Tools
To gain insight into the JVM itself, Java monitoring tools are required. A number of tools come with the JDK:

### jcmd
Prints basic class, thread, and VM information for a Java process. This is suitable for use in scripts; it is executed like this:

```
% jcmd process_id command optional_arguments
```
Supplying the command help will list all possible commands, and supplying help <command> will give the syntax for a particular command.

### jconsole
Provides a graphical view of JVM activities, including thread usage, class usage, and GC activities.

### jmap
Provides heap dumps and other information about JVM memory usage. Suitable for scripting, though the heap dumps must be used in a postprocessing tool.

### jinfo
Provides visibility into the system properties of the JVM, and allows some system properties to be set dynamically. Suitable for scripting.

### jstack
Dumps the stacks of a Java process. Suitable for scripting.

### jstat
Provides information about GC and class-loading activities. Suitable for scripting.

### jvisualvm
A GUI tool to monitor a JVM, profile a running application, and analyze JVM heap dumps (which is a postprocessing activity, though jvisualvm can also take the heap dump from a live program).

#### References
- https://blog.frankel.ch/openjdk-11-tools-trade/

## Profiling Tools
Profilers are the most important tool in a performance analyst’s toolbox. Profiling happens in one of two modes: sampling mode or instrumented mode.

### Sampling Profilers
Sampling mode is the basic mode of profiling and carries the least amount of overhead.

### Instrumented Profilers
Instrumented profilers are much more intrusive than sampling profilers, but they can also give more beneficial information about what’s happening inside a program.

Instrumented profilers yield more information about an application, but can possibly have a greater effect on the application than a sampling profiler. Instrumented profilers should be set up to instrument small sections of the code—a few classes or packages. That limits their impact on the application’s performance.

### Native Profilers
Native profiling tools are those that profile the JVM itself. This allows visibility into what the JVM is doing, and if an application includes its own native libraries, it allows visibility into that code as well. Any native profiling tool can be used to profile the C code of the JVM (and of any native libraries), but some native-based tools can profile both the Java and C/C++ code of an application.

## Java Flight Recorder

The list of events that follows displays two bullet points for each event type. Events can collect basic information that can be collected with other tools like jconsole and jcmd; that kind of information is described in the first bullet. The second bullet describes information the event provides that is difficult to obtain outside of JFR.

Classloading
- Number of classes loaded and unloaded
- Which classloader loaded the class; time required to load an individual class

Thread statistics
- Number of threads created and destroyed; thread dumps
- Which threads are blocked on locks (and the specific lock they are blocked on)

Throwables
- Throwable classes used by the application
- How many exceptions and errors are thrown and the stack trace of their creation

TLAB allocation
- The number of allocations in the heap and size of thread-local allocation buffers (TLABs)
- The specific objects allocated in the heap and the stack trace where they are allocated

File and socket I/O
- Time spent performing I/O
- Time spent per read/write call, the specific file or socket taking a long time to read or write

Monitor blocked
- Threads waiting for a monitor
- Specific threads blocked on specific monitors and the length of time they are blocked

Code cache
- Size of code cache and how much it contains
- Methods removed from the code cache; code cache configuration

Code compilation
- Which methods are compiled, OSR compilation, and length of time to compile
- Nothing specific to JFR, but unifies information from several sources

Garbage collection
- Times for GC, including individual phases; sizes of generations
- Nothing specific to JFR, but unifies the information from several tools

Profiling
- Instrumenting and sampling profiles
- Not as much as you’d get from a true profiler, but the JFR profile provides a good high-order overview


