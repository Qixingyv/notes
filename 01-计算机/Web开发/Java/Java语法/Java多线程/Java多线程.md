# 1 Java多线程

## 1.1 线程和多线程的概念

线程：一个顺序的单一的程序执行流程就是一个线程。代码一句一句的有先后顺序的执行。

多线程：多个单一顺序执行的流程并发运行。造成"感官上同时运行"的效果。

### 1.1.1 什么是并发及并发的用途

多个线程实际运行是走走停停的。线程调度程序会将CPU运行时间划分为若干个时间片段并

尽可能均匀的分配给每个线程，拿到时间片的线程被CPU执行这段时间。当超时后线程调度

程序会再次分配一个时间片段给一个线程使得CPU执行它。如此反复。由于CPU执行时间在

纳秒级别，我们感觉不到切换线程运行的过程。所以微观上走走停停，宏观上感觉一起运行

的现象成为并发运行!

**用途:**

- 当出现多个代码片段执行顺序有冲突时，希望它们各干各的时就应当放在不同线程上"同时"运行
- 一个线程可以运行，但是多个线程可以更快时，可以使用多线程运行

### 1.1.2 Java线程简介

java中所有的代码都是靠线程执行的，thread方法也不例外。JVM启动后会创建一条线程来执行thread

方法，该线程的名字叫做"thread",所以通常称它为"主线程"。我们自己定义的线程在不指定名字的情况下系统会分配一个名字，格式为"thread-x"(x是一个数)。

### 1.1.3 线程的生命周期图

![线程生命周期](C:\WorkSpace\Notes\Java笔记\Java多线程\assets\线程生命周期.png)

### 1.1.4 线程优先级

线程start后会纳入到线程调度器中统一管理,线程只能被动的被分配时间片并发运行,而无法主动索取时间片.线程调度器尽可能均匀的将时间片分配给每个线程.

线程有10个优先级,使用整数1-10表示

- 1为最小优先级,10为最高优先级.5为默认值
- 调整线程的优先级可以最大程度的干涉获取时间片的几率.优先级越高的线程获取时间片的次数越多,反之则越少.

```java
public class PriorityDemo {
    public static void thread(String[] args) {
        
        Thread max = new Thread(){
            public void run()
            {
                for(int i=0;i<10000;i++)
                {
                    System.out.println("max");
                }
            }
        };
        
        Thread min = new Thread(){
            public void run()
            {
                for(int i=0;i<10000;i++)
                {
                    System.out.println("min");
                }
            }
        };
        
        Thread norm = new Thread(){
            public void run()
            {
                for(int i=0;i<10000;i++)
                {
                    System.out.println("nor");
                }
            }
        };
        
        min.setPriority(Thread.MIN_PRIORITY);
        max.setPriority(Thread.MAX_PRIORITY);
        min.start();
        norm.start();
        max.start();
    }
}
```



## 1.2 线程的使用

### 1.2.1 创建线程有两种方式

#### 方式一:继承Thread并重写run方法

定义一个线程类，重写run方法，在其中定义线程要执行的任务(希望和其他线程并发执行的任务)。

注:启动该线程要调用该线程的start方法，而不是run方法！！！

```java
package thread;

public class ThreadDemo1 {
    public static void thread(String[] args) {
        //创建两个线程
        Thread t1 = new MyThread1();
        /*
            启动线程,注意:不要调用run方法！！
            线程调用完start方法后会纳入到系统的线程调度器程序中被统一管理。
            线程调度器会分配时间片段给线程，使得CPU执行该线程这段时间，用完后
            线程调度器会再分配一个时间片段给一个线程，如此反复，使得多个线程
            都有机会执行一会，做到走走停停，并发运行。
            线程第一次被分配到时间后会执行它的run方法开始工作。
         */
        t1.start();
    }
}
/**
 * 第一种创建线程的优点:
 * 结构简单，利于匿名内部类形式创建。
 *
 * 缺点:
 * 1:由于java是单继承的，这会导致继承了Thread就无法再继承其他类去复用方法
 * 2:定义线程的同时重写了run方法，这等于将线程的任务定义在了这个线程中导致
 *   线程只能干这件事。重(chong)用性很低。
 */
class MyThread1 extends Thread{
    public void run(){
        for (int i=0;i<1000;i++){
            System.out.println("hello姐~");
        }
    }
}
```

#### 方式二:实现Runnable接口单独定义线程任务

```java
package thread;

/**
 * 第二种创建线程的方式
 * 实现Runnable接口单独定义线程任务
 */
public class ThreadDemo2 {
    public static void thread(String[] args) {
        //实例化任务
        Runnable r1 = new MyRunnable1();
        //创建线程并指派任务
        Thread t1 = new Thread(r1);

        t1.start();
    }
}
class MyRunnable1 implements Runnable{
    public void run() {
        for (int i=0;i<1000;i++){
            System.out.println("你是谁啊?");
        }
    }
}
```

#### 匿名内部类形式的线程创建

```java
package thread;

/**
 * 使用匿名内部类完成线程的两种创建
 */
public class ThreadDemo3 {
    public static void thread(String[] args) {
        Thread t1 = new Thread(){
            public void run(){
                for(int i=0;i<1000;i++){
                    System.out.println("你是谁啊?");
                }
            }
        };
//        Runnable r2 = new Runnable() {
//            public void run() {
//                for(int i=0;i<1000;i++){
//                    System.out.println("我是查水表的!");
//                }
//            }
//        };
        //Runnable可以使用lambda表达式创建
        Runnable r2 = ()->{
                for(int i=0;i<1000;i++){
                    System.out.println("我是查水表的!");
                }
        };


        Thread t2 = new Thread(r2);

        t1.start();
        t2.start();
    }
}

```

### 1.2.2 线程分类

#### 主线程

Java中的代码都是靠线程运行的，执行thread方法的线程称为"主线程"

#### 守护线程

守护线程也称为:后台线程

- 守护线程是通过普通线程调用setDaemon方法设置而来的,因此创建上与普通线程无异
- 守护线程与普通线程的区别在于结束的时机不同
- 进程结束:当一个进程中的所有普通线程都结束时,进程就会结束,此时会杀掉所有正在运行的守护线程
- 设置守护线程必须在线程启动前进行
- 通常当我们不关心某个线程的任务什么时候停下来,它可以一直运行,但是程序主要的工作都结束时它应当跟着结束时,这样的任务就适合放在守护线程上执行.比如GC就是在守护线程上运行的.


```java
public class DaemonThreadDemo {
    public static void thread(String[] args) {
        Thread rose = new Thread(){
            public void run()
            {
                for(int i=0;i<5;i++)
                {
                    System.out.println("rose:let me go!");
                    try
                    {
                        Thread.sleep(1000);
                    } 
                    catch (InterruptedException e){}
                }
                System.out.println("rose:啊啊啊啊啊啊AAAAAAAaaaaa....");
                System.out.println("噗通");
            }
        };

        Thread jack = new Thread(){
            public void run()
            {
                while(true)
                {
                    System.out.println("jack:you jump!i jump!");
                    try 
                    {
                        Thread.sleep(1000);
                    } 
                    catch (InterruptedException e) {}
                }
            }
        };
        rose.start();
        jack.setDaemon(true);
        jack.start();
    }
}
```

### 1.2.3 线程API

#### 获取线程相关信息

```java
Thread thread = Thread.currentThread();//获取当前线程，静态方法

String name = thread.getName();//获取线程的名字
System.out.println("名字:"+name);

long id = thread.getId();//获取该线程的唯一标识
System.out.println("id:"+id);

int priority = thread.getPriority();//获取该线程的优先级
System.out.println("优先级:"+priority);

boolean isAlive = thread.isAlive();//该线程是否活着
System.out.println("是否活着:"+isAlive);

boolean isDaemon = thread.isDaemon();//是否为守护线程
System.out.println("是否为守护线程:"+isDaemon);

boolean isInterrupted = thread.isInterrupted();//是否被中断了
System.out.println("是否被中断了:"+isInterrupted);
```

#### sleep阻塞

线程提供了一个静态方法:

- static void sleep(long ms)
- 使运行该方法的线程进入阻塞状态指定的毫秒,超时后线程会自动回到RUNNABLE状态等待再次获取时间片并发运行.

```JAVA
package thread;

public class SleepDemo {
    public static void main(String[] args) {
        System.out.println("程序开始了!");
        try 
        {
            Thread.sleep(5000);//主线程阻塞5秒钟
        } 
        catch (InterruptedException e) 
        {
            e.printStackTrace();
        }
        System.out.println("程序结束了!");
    }
}
```

**中断线程睡眠的方法**:interrupt()

```java
/*
	当一个线程调用sleep方法处于睡眠阻塞的状态时,
	该线程的interrupt()方法被调用,
	sleep方法会抛出InterruptedException异常从而打断睡眠状态.
*/
public class SleepDemo2 {
    public static void main(String[] args) {
        
        Thread lin = new Thread(){
            public void run()
            {
                System.out.println("林:刚美完容，睡一会吧~");
                try 
                {
                    Thread.sleep(9999999);
                } 
                catch (InterruptedException e) 
                {
                    System.out.println("林:干嘛呢!干嘛呢!干嘛呢!都破了像了!");
                }
                System.out.println("林:醒了");
            }
        };

        Thread huang = new Thread(){
            public void run()
            {
                System.out.println("黄:大锤80!小锤40!开始砸墙!");
                for(int i=0;i<5;i++){
                    System.out.println("黄:80!");
                    try 
                    {
                        Thread.sleep(1000);
                    } 
                    catch (InterruptedException e) {}
                }
                System.out.println("咣当!");
                System.out.println("黄:大哥，搞定!");
                lin.interrupt();//中断lin的睡眠阻塞
            }
        };
        lin.start();
        huang.start();
    }
}
```



## 1.2 并发安全问题

当多个线程并发操作同一临界资源，由于线程切换的时机不确定，导致操作顺序出现混乱，严重时可能导致系统瘫痪

临界资源:同时只能被单一线程访问操作过程的资源

### 1.2.1 synchronized关键字

为了解决多线程抢夺一个资源的情况，我们可以将该资源加上synchronized关键字，使其在同一时间只能被一个线程使用，使用方式如下

- 在方法上修饰,此时该方法变为一个同步方法
- 同步块,可以更准确的锁定需要排队的代码片段

#### 同步方法

当一个方法使用synchronized修饰后,这个方法称为"同步方法",即:多个线程不能同时 在方法内部执行.只能有先后顺序的一个一个进行. 将并发操作同一临界资源的过程改为同步执行就可以有效的解决并发安全问题.

```java
package thread;

public class SyncDemo {
    public static void main(String[] args) {
        
        Table table = new Table();
        Thread t1 = new Thread(){
            public void run()
            {
                while(true)
                {
                    int bean = table.getBean();
                    Thread.yield();
                    System.out.println(getName()+":"+bean);
                }
            }
        };
        
        Thread t2 = new Thread(){
            public void run()
            {
                while(true)
                {
                    int bean = table.getBean();
                    /*
                        static void yield()
                        线程提供的这个静态方法作用是让执行该方法的线程
                        主动放弃本次时间片。
                        这里使用它的目的是模拟执行到这里CPU没有时间了，发生
                        线程切换，来看并发安全问题的产生。
                     */
                    Thread.yield();
                    System.out.println(getName()+":"+bean);
                }
            }
        };
        t1.start();
        t2.start();
    }
}

class Table{
    private int beans = 20;//桌子上有20个豆子

    /**
     * 当一个方法使用synchronized修饰后，这个方法称为同步方法，多个线程不能
     * 同时执行该方法。
     * 将多个线程并发操作临界资源的过程改为同步操作就可以有效的解决多线程并发
     * 安全问题。
     * 相当于让多个线程从原来的抢着操作改为排队操作。
     */
    public synchronized int getBean(){
        if(beans==0)
        {
            throw new RuntimeException("没有豆子了!");
        }
        Thread.yield();
        return beans--;
    }
}
```

#### 同步块

有效的缩小同步范围可以在保证并发安全的前提下尽可能的提高并发效率

同步块可以更准确的控制需要多个线程排队执行的代码片段

```java
synchronized(同步监视器对象)
{
   需要多线程同步执行的代码片段
}
```

#### 同步监视器对象

- 使用同步块需要指定同步监视器对象，即:上锁的对象
- 这个对象可以是java中任何引用类型的实例，只要保证多个线程需要排队
- 设置该对象时，需要分析到底哪个对象是临界资源，即哪个资源被多个线程使用
- 在方法上使用synchronized，那么同步监视器对象就是this

```java
package thread;

public class SyncDemo2 {
    public static void main(String[] args) 
    {
        Shop shop = new Shop();
        Thread t1 = new Thread()
        {
            public void run()
            {
                shop.buy();
            }
        };
        
        Thread t2 = new Thread(){
            public void run()
            {
                shop.buy();
            }
        };
        
        t1.start();
        t2.start();
    }
}

class Shop{
    public void buy()
    {
        
        Thread t = Thread.currentThread();//获取运行该方法的线程
        try 
        {
            System.out.println(t.getName()+":正在挑衣服...");
            Thread.sleep(5000);
            /*
                使用同步块需要指定同步监视器对象，即:上锁的对象
                这个对象可以是java中任何引用类型的实例，只要保证多个需要排队
                执行该同步块中代码的线程看到的该对象是"同一个"即可
             */
            synchronized (this) 
            {
                System.out.println(t.getName() + ":正在试衣服...");
                Thread.sleep(5000);
            }

            System.out.println(t.getName()+":结账离开");
        } 
        catch (InterruptedException e) 
        {
            e.printStackTrace();
        }

    }
}
```

#### 在类对象中使用synchronized

- 当在静态方法上使用synchronized后,该方法是一个同步方法.由于静态方法所属类,所以一定具有同步效果.

- 静态方法使用的同步监视器对象为当前类的类对象(Class的实例).

```java
package thread;

public class SyncDemo3 {
    public static void main(String[] args) {
        
        Thread t1 = new Thread(){
            public void run()
            {
                Boo.dosome();
            }
        };
        
        Thread t2 = new Thread(){
            public void run()
            {
                Boo.dosome();
            }
        };
        
        t1.start();
        t2.start();
    }
}
class Boo{
    
    public synchronized static void dosome()
    {
            Thread t = Thread.currentThread();
            try 
            {
                System.out.println(t.getName() + ":正在执行dosome方法...");
                Thread.sleep(5000);
                System.out.println(t.getName() + ":执行dosome方法完毕!");
            } 
        	catch (InterruptedException e) 
        	{
                e.printStackTrace();
            }
    }
}
```

**静态方法中使用同步块时,指定的锁对象通常也是当前类的类对象**

```java
class Boo{
    
    public static void dosome()
    {
        synchronized (Boo.class) 
        {
            Thread t = Thread.currentThread();
            
            try 
            {
                System.out.println(t.getName() + ":正在执行dosome方法...");
                Thread.sleep(5000);
                System.out.println(t.getName() + ":执行dosome方法完毕!");
            } 
            catch (InterruptedException e)
            {
                e.printStackTrace();
            }
        }
    }
}
```



## 1.3 锁

### 1.3.1 互斥锁

- 多个线程不能同时访问这些资源

- 当多个线程执行不同的代码片段,但是这些代码片段之间不能同时运行时就要设置为互斥的.

- 使用synchronized锁定多个代码片段,并且指定的同步监视器是同一个时,这些代码片段之间就是互斥的.

```java
public class SyncDemo4 {
    
    public static void main(String[] args) 
    {
        Foo foo = new Foo();
        
        Thread t1 = new Thread(){
            public void run()
            {
                foo.methodA();
            }
        };
        
        Thread t2 = new Thread(){
            public void run()
            {
                foo.methodB();
            }
        };
        
        t1.start();
        t2.start();
    }
}


class Foo{
    
    public synchronized void methodA()
    {
        Thread t = Thread.currentThread();
        try 
        {
            System.out.println(t.getName()+":正在执行A方法...");
            Thread.sleep(5000);
            System.out.println(t.getName()+":执行A方法完毕!");
        } 
        catch (InterruptedException e) 
        {
            e.printStackTrace();
        }
    }
    
    public synchronized void methodB()
    {
        Thread t = Thread.currentThread();
        try 
        {
            System.out.println(t.getName()+":正在执行B方法...");
            Thread.sleep(5000);
            System.out.println(t.getName()+":执行B方法完毕!");
        } 
        catch (InterruptedException e) 
        {
            e.printStackTrace();
        }
    }
    
}
```

### 1.3.2 死锁

两个线程各自持有一个锁对象的同时等待对方先释放锁对象，此时会出现僵持状态。这个现象就是死锁。

```java
public class DeadLockDemo {
    //定义两个锁对象，"筷子"和"勺"
    public static Object chopsticks = new Object();
    public static Object spoon = new Object();

    public static void main(String[] args)
    {
        
        Thread np = new Thread(){
            public void run()
            {
                System.out.println("北方人开始吃饭.");
                System.out.println("北方人去拿筷子...");

                synchronized (chopsticks){
                    System.out.println("北方人拿起了筷子开始吃饭...");
                    try 
                    {
                        Thread.sleep(5000);
                    } 
                    catch (InterruptedException e)
                    {
                    }
                    System.out.println("北方人吃完了饭，去拿勺...");
                    
                    synchronized (spoon)
                    {
                        System.out.println("北方人拿起了勺子开始喝汤...");
                        try 
                        {
                            Thread.sleep(5000);
                        }
                        catch (InterruptedException e){}
                        System.out.println("北方人喝完了汤");
                    }
                    System.out.println("北方人放下了勺");
                }
                System.out.println("北方人放下了筷子，吃饭完毕!");
            }
        };


        Thread sp = new Thread(){
            public void run()
            {
                System.out.println("南方人开始吃饭.");
                System.out.println("南方人去拿勺...");
                
                synchronized (spoon)
                {
                    System.out.println("南方人拿起了勺开始喝汤...");
                    try 
                    {
                        Thread.sleep(5000);
                    } 
                    catch (InterruptedException e) {}
                    System.out.println("南方人喝完了汤，去拿筷子...");
                    
                    synchronized (chopsticks)
                    {
                        System.out.println("南方人拿起了筷子开始吃饭...");
                        try 
                        {
                            Thread.sleep(5000);
                        } 
                        catch (InterruptedException e) {}
                        System.out.println("南方人吃完了饭");
                    }
                    System.out.println("南方人放下了筷子");
                }
                System.out.println("南方人放下了勺，吃饭完毕!");
            }
        };

        np.start();
        sp.start();
    }
}
```

#### 解决死锁

- 尽量避免在持有一个锁的同时去等待持有另一个锁(避免synchronized嵌套)

 * 当无法避免synchronized嵌套时，就必须保证多个线程锁对象的持有顺序必须一致。
   - 即:A线程在持有锁1的过程中去持有锁2时,B线程也要以这样的持有顺序进行。

```java
package thread;

public class DeadLockDemo2 {
    //筷子
    private static Object chopsticks = new Object();
    //勺
    private static Object spoon = new Object();

    public static void main(String[] args)
    {
        
        //北方人
        Thread np = new Thread(){
            public void run()
            {
                try 
                {
                    System.out.println("北方人:开始吃饭");
                    System.out.println("北方人去拿筷子...");
                    
                    synchronized (chopsticks) 
                    {
                        System.out.println("北方人拿起了筷子，开始吃饭...");
                        Thread.sleep(5000);
                    }
                    
                    System.out.println("北方人吃完了饭，放下了筷子");
                    System.out.println("北方人去拿勺子...");
                    
                    synchronized (spoon)
                    {
                        System.out.println("北方人拿起了勺子，开始喝汤...");
                        Thread.sleep(5000);
                    }
                    
                    System.out.println("北方人喝完了汤,北方人放下了勺子");
                    System.out.println("吃饭完毕。");
                } 
                catch (InterruptedException e)
                {
                    e.printStackTrace();
                }
            }
        };
        
        //南方人
        Thread sp = new Thread(){
            public void run()
            {
                try 
                {
                    System.out.println("南方人:开始吃饭");
                    System.out.println("南方人去拿勺...");
                    
                    synchronized (spoon) 
                    {
                        System.out.println("南方人拿起了勺，开始喝汤...");
                        Thread.sleep(5000);
                    }
                    
                    System.out.println("南方人喝完了汤，放下勺子...");
                    System.out.println("南方人去拿筷子...");
                    
                    synchronized (chopsticks)
                    {
                        System.out.println("南方人拿起了筷子，开始吃饭...");
                        Thread.sleep(5000);
                    }
                    
                    System.out.println("南方人吃完了饭,南方人放下了筷子");
                    System.out.println("吃饭完毕。");
                } 
                catch (InterruptedException e) 
                {
                    e.printStackTrace();
                }
            }
        };

        np.start();
        sp.start();
    }
}
```

