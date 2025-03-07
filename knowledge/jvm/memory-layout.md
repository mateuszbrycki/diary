# Memory Layout

## Object Layout

1. mark header + reference (reference to the object's class)
    - 32 bit JVM: 4 bytes mark + 4 bytes class ref
    - 64 bit JVM: 8 bytes mark + 8 bytes class ref
    - 64 bit JVM with CompressedOops: 8 bytes mark + 4 bytes compresses class ref
Why should I care?
int size = 4 bytes
java.lang.Integer size = 16 bytes

content_copy
```
bool size = 1 byte
java.lang.Boolean size = 16 bytes
```

The small int won't be allocated on the heap, it's on the stack. The big int will be allocated on heap so it will be scanned by GCs.

## Memory Layout
### Heap
    1. Young - your objects
        1. Eden
        1. Survariour 1
        1. Survariour 2
    1. Old - old objects
        1. Tenured

Garbage Collector is responsible for both allocating and cleaning the memory.
It has to know where an object has been allocated to know how to delete it later. 
Each of JVM's GCs has its own allocator.

`MemAllocator` is a JVM internal facility used for allocating, and initializing newly allocated objects.

`mem_allocate_inside_tlab` - thread local allocation buffer, enabled by default.
Thread local allocation buffers are used to eliminate the need of synchronization between threads allocating memmory.
Each of the threads has its own memory space (a small buffor). Objects are allocated within these buffors. Once a buffor is full, it takes another one from the pool. In case of no buffors available, the GC starts its work.

If an object is bigger than a buffor, it's allocated **outside of the TLAB**. In this part of the memory, there is an oldschool synchronized pointer that is moved after an allocation.

JVMs tunes the size of the buffors based on:
1. the number of threds,
1. how often TLABs need refill,
1. how often the allocation outside of a TLAB is performed.

TLAB allocation has better performance than `malloc` - Java is great in allocating memory.

**Epsilon GC** - a GC that does nothing.

#### OOPs
fat pointers - `class oopDesc {}`

An object in Java:
1. header:
  1. markword
    1. identity hascode
    1. object age (how many GC cycles it survived) - matters in Young, not updated in Old
    1. lock (used by `synchronized` keywordk)
  1. pointer to the objects class
1. body

Every Java object has overhead of 64 bit markWord and 64 bit class pointer.

Compressed OOPS - JVM recognizes how much memory a runtime has. If more than 32GB, 64-bit pointers are used. Below, 32-bit pointers are used.

Object layout should not bother you.

False sharing - [Wikipedia](https://en.wikipedia.org/wiki/False_sharing)

`-XX:+UseCompresseOops:`

#### Project Lilliput
1. The header should be even more compressable. 13-bits will be sufficient.
1. insted of pointers, we could use class IDs in a lookup table.
1. Instead of paddings, place pointers there.

[JEP 450: Compact Object Headers](https://openjdk.org/jeps/450)

#### Project Valhalla
Valhalla will influence how objects are allocated in the memory.

We will have objects that are copied, not passed by reference. Objects that have no identity. We will be able to simplify memory - in an array of strings, we won't need to mention that each of the objects is string. Only value will be stored - no header, no pointer.

`value class` - similar to a `record` but with more constraints. No inheritence, no synchronization.

Stack allocation - is an urban legend. Until Valhalla, Java doesn't allocate on the stack.
Escape analysis - [scalar replacement](https://shipilev.net/jvm/anatomy-quarks/18-scalar-replacement/).

JITWatch - to check where scalar replacement happens.

#### Object Allocation Tracking
1. `jcmd`
1. `jfr`
    - understands Java only
     - JDK mission controll
    - memory-tracking.jfr
    `jfr view --verbose allocation-by-class memory-tracking.jfr` - JDK 22+
        - can be enabled or disabled during application startup
        - JFR recording can be enabled from machines code
1. async profiler
    - supports more than Java
    - will report JVM as well
1. JVM logs
    - no good tools for parsing

If you spot a performance issue, understand:
1. what the user does,
1. what the application does,
1. what the JVM does,
1. what the OS does.

### Off-Heap

Known as:
1. native memory
1. unmanaged memory
1. direct buffers

It's not supervised by GC.

Any big data, streaming, databases in Java store data off-heap.
1. Large heaps are problematic for GCs.
1. The majority of these heaps are old generation - they are composed of long-living objects.
1. Calls to native functions.
1. I/O heavy workloads have serious overhead on CPU cycles and memory
    - disks/networking,
    - Java copies data that is saved/read from files to off-heap so that GC won't move it (the pointer to that table/buffer is passed to the OS as Java outsorces), chence `malloc` in `readBuffer`,
    - direct buffers allocate off-heap so when I/O is involved, no copying is required,
    - region pinning - [JEP 423: Region Pinning for G1](https://openjdk.org/jeps/423).

#### Foreign Function & Memory API
[JEP 454: Foreign Function & Memory API](https://openjdk.org/jeps/454)
- Off-heap API
  - `MemoryLayout`
  - `MemorySegment`
    - thread confinement - by default only the thread-creator has the access

#### Off-heap memory tracking
1. native memory tracking with JFR events, since Java 22, Tracking Java Native Memory With JDK Flight Recorder
    - the `Other` section
1. async profiler, with `malloc` set as event type,
1. use `jemalloc` with heap profiling,
1. use OS specific tooling,
1. Massif from Valgrind, but it's extremly slow, beyond being usable.

**When you find problems with native memory, first try to use other allocator, like *jemalloc* or *tcmalloc*.
Some errors are caused by `gclib` allocator and memory fragmentation.**

### Non-heap (aka CHeap)
#### Metaspace

In the Metaspace lands the code loaded from JARs.
It grows not only on application startup but also in the application's runtime. Any library genereting the code on the fly (like bytebuddy or aspects), causes the metaspace occupancy increase.

In order to disable GC from scanning Metaspace consider the `classUnloading` flag.

G1 metaspace collection:
- every GC handles Metaspace differently,
- Metaspace is collected during concurrent mark and full GC phases.

JFR events - `MetaspaceSummary`

#### Code Cache
The code optimized by JVM is stored in code cache.

3 regions:
1. JVM Internal (no-method) code,
1. profiled code,
1. non-profiled code.

Code cache is likely to grow as Metaspace grows - the more code you loaded, the more is to be optimized.

### Object Monitors
https://bugs.openjdk.org/browse/JDK-8305994

### Stack
It's not anything like interesting unless you're not using virtual threads.

It gets bigger depending on the:
1. number of threads,
1. depth of method calls.

`-Xss` thread stack size

Virtual threads when **mounted**, use platform thread stack. 
When unmounted, their stacks are allocated on heap as instances of `jdk.internal.vm.Continuation` class.
Empty `jdk.internal.vm.Continuation` requires 48 bytes.

[JEP 491: Synchronize Virtual Threads without Pinning](https://openjdk.org/jeps/491)


## Materials
1. [WJUG #167 - Garbage Collector w pigułce - Jakub Kubryński](https://www.youtube.com/watch?v=LCr3XyHdaZk)
1. [Java memory model](https://medium.com/@alxkm/java-memory-model-3b973e84dc8c)