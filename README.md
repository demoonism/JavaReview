# JavaReview

#Java Memory Model
ref:
https://www.youtube.com/watch?v=Z4hMFBvCDV4
https://zhuanlan.zhihu.com/p/258393139

A set of requirements that abstract away the memory management regardless of the underlying os. It defines a main memory (shared) and working memory (per thread).
A thread can not work with variables in the main memory, it has to be copy/pasted into the working memory first. Thread cannot see the working memory of another
thread.
This specification guarantees atomicity, visibility and ordering. i.e. guarantees visibility of fields (aka. happens before) among re-ordering of instrutions (for optimization)
JMM is not a physical thing, it enforces the JVM memory structure.

Atomicity:

Single steps are always atomic:
ex. i = 1;
j = i + 1 is not, two steps, read i, then assign to j. simillar idea to i++ or i = i + 1.
Therefore, must provide ways to do this. (ex. synchronized, reentrantlock etc.)


Visbility:
Once variable is changed, must be visible to other threads.
Therefore, provided volatile (synchronized, and lock can also do this, must sync var to main memory before unlock)

orderness:
must be able to ensure order. (both volatile and syncrhonized/locks can do this.)

lock(锁定)，作用于主内存中的变量，把变量标识为线程独占的状态。
read(读取)，作用于主内存的变量，把变量的值从主内存传输到线程的工作内存中，以便下一步的load操作使用。
load(加载)，作用于工作内存的变量，把read操作主存的变量放入到工作内存的变量副本中。
use(使用)，作用于工作内存的变量，把工作内存中的变量传输到执行引擎，每当虚拟机遇到一个需要使用到变量的值的字节码指令时将会执行这个操作。
assign(赋值)，作用于工作内存的变量，它把一个从执行引擎中接受到的值赋值给工作内存的变量副本中，每当虚拟机遇到一个给变量赋值的字节码指令时将会执行这个操作。
store(存储)，作用于工作内存的变量，它把一个从工作内存中一个变量的值传送到主内存中，以便后续的write使用。
write(写入)：作用于主内存中的变量，它把store操作从工作内存中得到的变量的值放入主内存的变量中。
unlock(解锁)：作用于主内存的变量，它把一个处于锁定状态的变量释放出来，释放后的变量才可以被其他线程锁定。

Volatile：
1. guarantee visibility
2. prevent re-order of instructions
once changed in working memory, sync back to main memory, invalidate other thread's copy, force them to read from main memory.
Add memory barrior, anything happens before a write of operation of a volatile must be visibile to to other threads.

