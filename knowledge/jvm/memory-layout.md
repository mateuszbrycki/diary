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

### Off-heap - objects required by JVM to work
    1. metaspace (old perm-gen)
        1. runtime constant pool - literals, fields, methods etc.
    1. code cache - used by JIT

XSS - stack overflow it to small - thread stack size

## Materials
1. [WJUG #167 - Garbage Collector w pigułce - Jakub Kubryński](https://www.youtube.com/watch?v=LCr3XyHdaZk)
1. [Java memory model](https://medium.com/@alxkm/java-memory-model-3b973e84dc8c)