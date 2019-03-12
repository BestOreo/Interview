# Intern QA Record

* [JAVA](#JAVA)
  * [标题2](#标题2)
  * [标题3](#标题3)
        * [标题3.1](#标题3.1)
        * [标题3.2](#标题3.2)
  * [标题4](#标题4)
  
  
### JAVA

##### 1. HashSet, HashTable, HashMap

- Hashmap和Hashtable最主要的区别在于Hashtable是线程安全，而Hashmap非线程安全

- Hashmap可以用null做key而hashtable不行。**HashMap以null作为key时，总是存储在table数组的第一个节点上**

- HashMap的初始容量为16，Hashtable初始容量为11，两者的填充因子默认都是0.75
  HashMap扩容时是当前容量翻倍即: capacity\*2 , Hashtable扩容时是容量翻倍+1即:capacity\*2+1

- 两者计算hash的方法不同

  Hashtable计算hash是直接使用key的hashcode对table数组的长度直接进行取模

  ```java
  int hash = key.hashCode();
  int index = (hash & 0x7FFFFFFF) % tab.length;
  ```

  HashMap计算hash对key的hashcode进行了二次hash，以获得更好的散列值，然后对table数组长度取摸

  ```java
  static int hash(int h) {
          // This function ensures that hashCodes that differ only by
          // constant multiples at each bit position have a bounded
          // number of collisions (approximately 8 at default load factor).
          h ^= (h >>> 20) ^ (h >>> 12);
          return h ^ (h >>> 7) ^ (h >>> 4);
      }
  
   static int indexFor(int h, int length) {
          return h & (length-1);
      }
  ```

  

- HashMap和Hashtable的底层实现都是数组+链表结构实现
- HashSet不是key-value结构，仅仅是存储不重复的元素，相当于简化版的HashMap，只是包含HashMap中的key而已

##### 2. JAVA线程和goroutine的区别

- [OS](https://www.baidu.com/s?wd=OS&tn=24004469_oem_dg&rsv_dl=gh_pl_sl_csd)的线程由OS内核调度，每隔几毫秒，一个硬件时钟中断发到[CPU](https://www.baidu.com/s?wd=CPU&tn=24004469_oem_dg&rsv_dl=gh_pl_sl_csd)，CPU调用一个调度器内核函数。这个函数暂停当前正在运行的线程，把他的寄存器信息保存到内存中，查看线程列表并决定接下来运行哪一个线程，再从内存中恢复线程的注册表信息，最后继续执行选中的线程。这种线程切换需要一个完整的[上下文切换](https://www.baidu.com/s?wd=%E4%B8%8A%E4%B8%8B%E6%96%87%E5%88%87%E6%8D%A2&tn=24004469_oem_dg&rsv_dl=gh_pl_sl_csd)：即保存一个线程的状态到内存，再恢复另外一个线程的状态，最后更新调度器的数据结构。某种意义上，这种操作还是很慢的。

  Go运行的时候包含一个自己的调度器，这个调度器使用一个称为一个M:N调度技术，m个goroutine到n个os线程（可以用GOMAXPROCS来控制n的数量），Go的调度器不是由硬件时钟来定期触发的，而是由特定的go语言结构来触发的，他不需要切换到内核语境，所以调度一个goroutine比调度一个线程的成本低很多。

- goroutine没有一个特定的标识。

  在大部分支持多线程的操作系统和编程语言中，线程有一个独特的标识，通常是一个整数或者指针，这个特性可以让我们构建一个线程的局部存储，本质是一个全局的map，以线程的标识作为键，这样每个线程可以独立使用这个map存储和获取值，不受其他线程干扰。

  goroutine中没有可供程序员访问的标识，原因是一种纯函数的理念，不希望滥用线程局部存储导致一个不健康的超距作用，即函数的行为不仅取决于它的参数，还取决于运行它的线程标识。

### C++

