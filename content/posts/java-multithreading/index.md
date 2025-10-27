---
title: "Java Multithreading - Lập trình đa luồng trong Java"
date: 2024-02-15T10:00:00+07:00
draft: false
description: "Hướng dẫn chi tiết về Multithreading trong Java: Thread, Runnable, Synchronization, ExecutorService"
tags: ["Java", "Multithreading", "Concurrency", "Thread"]
categories: ["Java"]
author: "Trung Tín"
showAuthor: true
showDate: true
showDateUpdated: true
showReadingTime: true
showTableOfContents: true
showPagination: true
showTaxonomies: true
showWordCount: true
---

{{< lead >}}
⚡ Master **Java Multithreading** - unlock sức mạnh xử lý đa luồng, tối ưu CPU utilization và build ứng dụng high-performance!
{{< /lead >}}

## Giới thiệu về Multithreading

{{< alert "circle-info" >}}
**💡 Power of Concurrency:** Multithreading cho phép Java chạy **nhiều tasks đồng thời** trong một process - tăng performance gấp 10-100 lần!
{{< /alert >}}

**Multithreading** là khả năng thực thi nhiều threads (luồng) đồng thời trong một chương trình. Mỗi thread là một lightweight process có thể chạy độc lập.

### Lợi ích của Multithreading

1. **Better CPU Utilization**: Sử dụng tối đa CPU
2. **Responsiveness**: Ứng dụng phản hồi nhanh hơn
3. **Parallelism**: Thực hiện nhiều tác vụ cùng lúc
4. **Better Performance**: Giảm thời gian xử lý

### Process vs Thread

| Process | Thread |
|---------|--------|
| Heavyweight | Lightweight |
| Có memory space riêng | Chia sẻ memory với threads khác |
| Tốn resource hơn | Ít tốn resource |
| Communication khó | Communication dễ |

## Tạo Thread trong Java

### Cách 1: Extends Thread class

```java
// Tạo class extends Thread
class MyThread extends Thread {
    private String threadName;
    
    public MyThread(String name) {
        this.threadName = name;
    }
    
    @Override
    public void run() {
        // Code sẽ chạy trong thread này
        for (int i = 1; i <= 5; i++) {
            System.out.println(threadName + " - Count: " + i);
            
            try {
                Thread.sleep(500); // Sleep 500ms
            } catch (InterruptedException e) {
                System.out.println(threadName + " interrupted");
            }
        }
        
        System.out.println(threadName + " finished!");
    }
}

// Sử dụng
public class ThreadExample {
    public static void main(String[] args) {
        // Tạo threads
        MyThread thread1 = new MyThread("Thread-1");
        MyThread thread2 = new MyThread("Thread-2");
        
        // Start threads
        thread1.start(); // Gọi start(), không phải run()!
        thread2.start();
        
        System.out.println("Main thread finished!");
    }
}
```

### Cách 2: Implements Runnable interface (Recommended)

```java
// Implement Runnable
class MyRunnable implements Runnable {
    private String taskName;
    
    public MyRunnable(String name) {
        this.taskName = name;
    }
    
    @Override
    public void run() {
        for (int i = 1; i <= 5; i++) {
            System.out.println(taskName + " - Count: " + i);
            
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                System.out.println(taskName + " interrupted");
            }
        }
        
        System.out.println(taskName + " completed!");
    }
}

// Sử dụng
public class RunnableExample {
    public static void main(String[] args) {
        // Tạo Runnable objects
        Runnable task1 = new MyRunnable("Task-1");
        Runnable task2 = new MyRunnable("Task-2");
        
        // Tạo threads với Runnable
        Thread thread1 = new Thread(task1);
        Thread thread2 = new Thread(task2);
        
        // Start threads
        thread1.start();
        thread2.start();
    }
}
```

### Cách 3: Lambda Expression (Java 8+)

```java
public class LambdaThreadExample {
    public static void main(String[] args) {
        // Lambda expression với Runnable
        Thread thread1 = new Thread(() -> {
            for (int i = 1; i <= 5; i++) {
                System.out.println("Lambda Thread - " + i);
                try {
                    Thread.sleep(500);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });
        
        thread1.start();
        
        // Inline lambda
        new Thread(() -> {
            System.out.println("Inline lambda thread running!");
        }).start();
    }
}
```

## Thread Lifecycle (Vòng đời của Thread)

{{< mermaid >}}
stateDiagram-v2
    [*] --> NEW: Create Thread
    NEW --> RUNNABLE: start()
    RUNNABLE --> RUNNING: Scheduler picks
    RUNNING --> BLOCKED: Wait for lock
    RUNNING --> WAITING: wait()
    RUNNING --> TIMED_WAITING: sleep()/wait(timeout)
    RUNNING --> TERMINATED: Complete
    BLOCKED --> RUNNABLE: Lock acquired
    WAITING --> RUNNABLE: notify()/notifyAll()
    TIMED_WAITING --> RUNNABLE: Timeout/notify()
    TERMINATED --> [*]
    
    style NEW fill:#ff6b6b
    style RUNNING fill:#51cf66
    style TERMINATED fill:#868e96
{{< /mermaid >}}

**7 trạng thái của Thread:**
1. {{< badge >}}NEW{{< /badge >}} Thread được tạo nhưng chưa start
2. {{< badge >}}RUNNABLE{{< /badge >}} Thread ready để chạy
3. {{< badge >}}RUNNING{{< /badge >}} Thread đang executing
4. {{< badge >}}BLOCKED{{< /badge >}} Thread đang chờ monitor lock
5. {{< badge >}}WAITING{{< /badge >}} Thread đang chờ thread khác
6. {{< badge >}}TIMED_WAITING{{< /badge >}} Thread chờ trong khoảng thời gian
7. {{< badge >}}TERMINATED{{< /badge >}} Thread hoàn thành

## Thread Methods

```java
public class ThreadMethodsExample {
    public static void main(String[] args) throws InterruptedException {
        Thread thread = new Thread(() -> {
            for (int i = 1; i <= 5; i++) {
                System.out.println("Count: " + i);
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    System.out.println("Thread interrupted!");
                    return;
                }
            }
        });
        
        // Get thread name
        System.out.println("Thread name: " + thread.getName());
        
        // Set thread name
        thread.setName("MyWorkerThread");
        
        // Get thread priority (1-10, default 5)
        System.out.println("Priority: " + thread.getPriority());
        
        // Set priority
        thread.setPriority(Thread.MAX_PRIORITY); // 10
        
        // Check if alive
        System.out.println("Is alive? " + thread.isAlive()); // false
        
        // Start thread
        thread.start();
        
        System.out.println("Is alive? " + thread.isAlive()); // true
        
        // Join - chờ thread complete
        thread.join(); // Main thread chờ
        
        System.out.println("Thread finished!");
        System.out.println("Is alive? " + thread.isAlive()); // false
        
        // Get current thread
        Thread currentThread = Thread.currentThread();
        System.out.println("Current thread: " + currentThread.getName());
        
        // Sleep current thread
        Thread.sleep(2000); // Sleep 2 seconds
    }
}
```

## Synchronization (Đồng bộ hóa)

{{< badge >}}
🔒 Thread Safety
{{< /badge >}}

Khi nhiều threads truy cập shared resource, cần synchronization để tránh race condition.

{{< alert "triangle-exclamation" >}}
**⚠️ Race Condition Warning:** Khi nhiều threads modify shared data cùng lúc → kết quả KHÔNG DỰ ĐOÁN được! Luôn cần synchronization!
{{< /alert >}}

### Problem: Race Condition

```java
class Counter {
    private int count = 0;
    
    public void increment() {
        count++; // Không thread-safe!
    }
    
    public int getCount() {
        return count;
    }
}

public class RaceConditionExample {
    public static void main(String[] args) throws InterruptedException {
        Counter counter = new Counter();
        
        // Tạo 1000 threads, mỗi thread increment 1000 lần
        Thread[] threads = new Thread[1000];
        for (int i = 0; i < 1000; i++) {
            threads[i] = new Thread(() -> {
                for (int j = 0; j < 1000; j++) {
                    counter.increment();
                }
            });
            threads[i].start();
        }
        
        // Chờ tất cả threads
        for (Thread thread : threads) {
            thread.join();
        }
        
        // Expected: 1,000,000
        // Actual: < 1,000,000 (do race condition)
        System.out.println("Count: " + counter.getCount());
    }
}
```

### Solution 1: Synchronized Method

```java
class Counter {
    private int count = 0;
    
    // Synchronized method
    public synchronized void increment() {
        count++; // Thread-safe!
    }
    
    public synchronized int getCount() {
        return count;
    }
}

// Bây giờ kết quả đúng: 1,000,000
```

### Solution 2: Synchronized Block

```java
class Counter {
    private int count = 0;
    private Object lock = new Object();
    
    public void increment() {
        // Synchronized block
        synchronized(lock) {
            count++;
        }
    }
    
    public int getCount() {
        synchronized(lock) {
            return count;
        }
    }
}
```

### Solution 3: Atomic Classes

```java
import java.util.concurrent.atomic.AtomicInteger;

class Counter {
    private AtomicInteger count = new AtomicInteger(0);
    
    public void increment() {
        count.incrementAndGet(); // Thread-safe atomic operation
    }
    
    public int getCount() {
        return count.get();
    }
}
```

## Producer-Consumer Problem

```java
import java.util.LinkedList;
import java.util.Queue;

class SharedBuffer {
    private Queue<Integer> buffer = new LinkedList<>();
    private int capacity = 5;
    
    // Producer adds items
    public synchronized void produce(int value) throws InterruptedException {
        // Nếu buffer đầy, chờ
        while (buffer.size() == capacity) {
            System.out.println("Buffer full, producer waiting...");
            wait(); // Release lock và chờ
        }
        
        buffer.add(value);
        System.out.println("Produced: " + value);
        
        // Notify consumer
        notify();
    }
    
    // Consumer takes items
    public synchronized int consume() throws InterruptedException {
        // Nếu buffer rỗng, chờ
        while (buffer.isEmpty()) {
            System.out.println("Buffer empty, consumer waiting...");
            wait();
        }
        
        int value = buffer.poll();
        System.out.println("Consumed: " + value);
        
        // Notify producer
        notify();
        
        return value;
    }
}

public class ProducerConsumerExample {
    public static void main(String[] args) {
        SharedBuffer buffer = new SharedBuffer();
        
        // Producer thread
        Thread producer = new Thread(() -> {
            try {
                for (int i = 1; i <= 10; i++) {
                    buffer.produce(i);
                    Thread.sleep(100);
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        
        // Consumer thread
        Thread consumer = new Thread(() -> {
            try {
                for (int i = 1; i <= 10; i++) {
                    buffer.consume();
                    Thread.sleep(200);
                }
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });
        
        producer.start();
        consumer.start();
    }
}
```

## ExecutorService - Thread Pool

```java
import java.util.concurrent.*;

public class ExecutorServiceExample {
    public static void main(String[] args) {
        // Tạo thread pool với 3 threads
        ExecutorService executor = Executors.newFixedThreadPool(3);
        
        // Submit tasks
        for (int i = 1; i <= 10; i++) {
            int taskId = i;
            
            executor.submit(() -> {
                System.out.println("Task " + taskId + " - Thread: " + 
                    Thread.currentThread().getName());
                
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                
                System.out.println("Task " + taskId + " completed!");
            });
        }
        
        // Shutdown executor
        executor.shutdown();
        
        try {
            // Chờ tất cả tasks complete (max 1 phút)
            if (!executor.awaitTermination(60, TimeUnit.SECONDS)) {
                executor.shutdownNow();
            }
        } catch (InterruptedException e) {
            executor.shutdownNow();
        }
        
        System.out.println("All tasks completed!");
    }
}
```

### Callable và Future

```java
import java.util.concurrent.*;

public class CallableFutureExample {
    public static void main(String[] args) throws Exception {
        ExecutorService executor = Executors.newFixedThreadPool(2);
        
        // Callable trả về giá trị
        Callable<Integer> task1 = () -> {
            System.out.println("Task 1 starting...");
            Thread.sleep(2000);
            return 100;
        };
        
        Callable<Integer> task2 = () -> {
            System.out.println("Task 2 starting...");
            Thread.sleep(3000);
            return 200;
        };
        
        // Submit và nhận Future
        Future<Integer> future1 = executor.submit(task1);
        Future<Integer> future2 = executor.submit(task2);
        
        System.out.println("Tasks submitted, main thread continues...");
        
        // Get results (blocking)
        Integer result1 = future1.get(); // Chờ task1 complete
        Integer result2 = future2.get(); // Chờ task2 complete
        
        System.out.println("Result 1: " + result1);
        System.out.println("Result 2: " + result2);
        System.out.println("Sum: " + (result1 + result2));
        
        executor.shutdown();
    }
}
```

## Deadlock - Khóa chết

```java
public class DeadlockExample {
    private static Object lock1 = new Object();
    private static Object lock2 = new Object();
    
    public static void main(String[] args) {
        // Thread 1: lock1 → lock2
        Thread thread1 = new Thread(() -> {
            synchronized (lock1) {
                System.out.println("Thread 1: Locked lock1");
                
                try { Thread.sleep(100); } catch (InterruptedException e) {}
                
                synchronized (lock2) {
                    System.out.println("Thread 1: Locked lock2");
                }
            }
        });
        
        // Thread 2: lock2 → lock1
        Thread thread2 = new Thread(() -> {
            synchronized (lock2) {
                System.out.println("Thread 2: Locked lock2");
                
                try { Thread.sleep(100); } catch (InterruptedException e) {}
                
                synchronized (lock1) {
                    System.out.println("Thread 2: Locked lock1");
                }
            }
        });
        
        thread1.start();
        thread2.start();
        
        // DEADLOCK! Cả 2 threads đều chờ nhau
    }
}
```

### Tránh Deadlock

1. **Lock ordering**: Luôn lock theo thứ tự
2. **Timeout**: Sử dụng tryLock() với timeout
3. **Deadlock detection**: Kiểm tra và xử lý

## Best Practices

1. **Prefer Runnable over Thread**: Linh hoạt hơn
2. **Use ExecutorService**: Quản lý thread pool hiệu quả
3. **Minimize Synchronization**: Chỉ sync khi cần
4. **Use Concurrent Collections**: ConcurrentHashMap, CopyOnWriteArrayList
5. **Avoid Deadlock**: Lock ordering, use timeout
6. **Handle InterruptedException**: Đừng ignore!

## Kết luận

{{< timeline >}}

{{< timelineItem icon="play" header="Thread Creation" badge="Foundation" >}}
3 cách tạo Thread: {{< badge >}}extends Thread{{< /badge >}} {{< badge >}}implements Runnable{{< /badge >}} {{< badge >}}Lambda{{< /badge >}}. Prefer Runnable!
{{< /timelineItem >}}

{{< timelineItem icon="lock" header="Synchronization" badge="Safety" >}}
Tránh race conditions với {{< badge >}}synchronized{{< /badge >}} {{< badge >}}AtomicClasses{{< /badge >}} {{< badge >}}wait/notify{{< /badge >}}
{{< /timelineItem >}}

{{< timelineItem icon="server" header="Thread Pool" badge="Performance" >}}
{{< badge >}}ExecutorService{{< /badge >}} {{< badge >}}Callable{{< /badge >}} {{< badge >}}Future{{< /badge >}} - Professional thread management!
{{< /timelineItem >}}

{{< /timeline >}}

{{< alert "check" >}}
**✅ Best Practice:** 
- Luôn dùng **ExecutorService** thay vì tạo threads manually
- Minimize synchronization scope
- Avoid deadlocks với proper lock ordering
{{< /alert >}}

## Tài liệu tham khảo

{{< button href="https://docs.oracle.com/javase/tutorial/essential/concurrency/" target="_blank" >}}
📚 Oracle Concurrency Tutorial
{{< /button >}}

{{< button href="https://jcip.net/" target="_blank" >}}
📖 Java Concurrency in Practice
{{< /button >}}

{{< button href="https://docs.oracle.com/javase/8/docs/api/java/lang/Thread.html" target="_blank" >}}
📘 Thread API Documentation
{{< /button >}}

{{< button href="https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ExecutorService.html" target="_blank" >}}
⚙️ ExecutorService Guide
{{< /button >}}

