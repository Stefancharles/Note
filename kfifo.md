## kfifo概述
* kfifo是内核里面的一个First In First Out数据结构，它采用环形循环队列的数据结构来实现；它提供一个无边界的字节流服务，最重要的一点是，它使用并行无锁编程技术，即当它用于只有一个入队线程和一个出队线程的场情时，两个线程可以并发操作，而不需要任何加锁行为，就可以保证kfifo的线程安全。 

* 当只有一个读线程和一个写线程并发操作时，可以确保是线程安全的，不用添加额外的锁来使用这些功能。kfifo的这一特性，提高了kernel的并发效率。所以kfifo适用于一个线程存数据，一个线程取数据的应用场景。

## 数据结构
```c
struct kfifo {
    unsigned char *buffer;    /* the buffer holding the data */
    unsigned int size;    /* the size of the allocated buffer */
    unsigned int in;    /* data is added at offset (in % size) */
    unsigned int out;    /* data is extracted from off. (out % size) */
    spinlock_t *lock;    /* protects concurrent modifications */
};
```
这是kfifo的数据结构，kfifo主要提供了两个操作，__kfifo_put(入队操作)和__kfifo_get(出队操作)。 它的各个数据成员如下：

>* buffer: 用于存放数据的缓存

>* size: buffer空间的大小，在初化时，将它向上扩展成2的幂

>* lock: 如果使用不能保证任何时间最多只有一个读线程和写线程，需要使用该lock实施同步。

>* in, out: 和buffer一起构成一个循环队列。 in指向buffer中队头，而且out指向buffer中的队尾。