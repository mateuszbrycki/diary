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
1. Heap
    1. Young - your objects
        1. Eden
        1. Survariour 1
        1. Survariour 2
    1. Old - old objects
        1. Tenured
1. Off-heap - objects required by JVM to work
    1. metaspace (old perm-gen)
        1. runtime constant pool - literals, fields, methods etc.
    1. code cache - used by JIT

XSS - stack overflow it to small - thread stack size

## Materials
1. [WJUG #167 - Garbage Collector w pigułce - Jakub Kubryński](https://www.youtube.com/watch?v=LCr3XyHdaZk)
1. [Java memory model](https://medium.com/@alxkm/java-memory-model-3b973e84dc8c)