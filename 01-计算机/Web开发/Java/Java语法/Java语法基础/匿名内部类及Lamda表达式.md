# 1 内部类

内部类就是定义在一个类里面的类。里面的类可以理解成寄生类，外部类可以理解成宿主类。

使用场景：当一个事物的内部，还有一个部分需要一个完整的结构进行描述时。

作用：

1. 内部类通常可以方便访问外部类的成员，包括私有的成员
2. 内部类提供了更好的封装性，内部类本身就可以用private ，protectecd等修饰，封装性可以做更多控制

## 1.1 静态内部类

- 有static修饰，属于外部类本身
- 它的特点和使用与普通类是完全一样的，类有的成分它都有，只是位置在别人里面而已

```java
//外部类
public class Outer{
    //静态成员内部类
    public static class Inner{
        
    }
}

/*
	创建对象:
		外部类名.内部类名 对象变量 = new 外部类名.内部类构造器;
*/
Outer.Inner in = new Outer.Inner();
```

有关访问的问题：

- 静态内部类中是否可以直接访问外部类的静态成员
- 静态内部类中不可以直接访问外部类的实例成员，外部类实例成员必须使用外部类对象访问

## 1.2 成员内部类

- 无static修饰，属于外部类的对象。

- JDK16之前，成员内部类中不能定义静态成员，JDK 16开始也可以定义静态成员了。

```java
//外部类
public class Outer{
    //内部类
    public class Inner{
        
    }
}

/*
	创建对象：
		外部类名.内部类名 对象名 = new  外部类构造器.new 内部类构造器();
*/
Outer.Inner in = new Outer().new Inner();
```

有关访问的问题：

- 成员内部类中可以直接访问外部类的静态成员
- 成员内部类的实例方法可以直接访问外部类的实例成员，因为必须现有外部类才会有内部类，格式：`外部类型.this`

## 1.3 局部内部类

- 局部内部类放在方法、代码块、构造器等执行体中。

- 局部内部类的类文件名为： 外部类$N内部类.class。

# 2 匿名内部类

- 本质是一个没有名字的局部内部类
- 作用是方便创建对象，简化代码书写

```java
public class Test{
    public static void main(String[] args)
    {
        /*
        	1. 创建一个Animal的子类，并实现shout方法
        	2. 使用该子类创建一个对象，将对象引用给cat变量
        */
        Animal cat = new Animal(){
            public void shout(){
                System.out.println("我是一只猫，喵喵喵");
            }
        };
        
        cat.shout();
    }
}

abstract class Animal{
    public abstract void shout();
}
```

# 3 Lambda表达式

- 作用是简化匿名内部类的书写

- Lambda表达式时`JDK8`之后的一种新语法形式

- Lambda表达式只能简化函数式接口的匿名内部类的书写

  > 函数式接口：接口中只有一个方法，并且该方法没有具体实现。大多数函数时接口有@FunctionalInterface注解标注

## 3.1 快速入门

举个例子：使用`java.swing.Timer`类来进行计时，每过5秒打印一句`"Lambda is working!"`

```java
//版本1
public class TimerTest1 {

    public static void main(String[] args) {
        
        /*
        	- 构造Timer对象时，第一个参数为计时时间，第二个参数为需要执行的代码
        	- 由于Java不能将函数当作参数传递，所以需要传递一个对象并执行相应的方法
        	- 为了确保相应的对象有对应的方法，需要使用接口来进行规范
        	- 下面Linsterner类实现的ActionLinstener就是相应接口
        */
        Timer t = new Timer(3000, new Linsterner());
        t.start();

        //提供窗口来结束程序
        JOptionPane.showMessageDialog(null,"Quit Program?");
        System.exit(0);
    }
}

/*
	该类负责指定计时器到时间后执行什么代码
*/
class Linsterner implements ActionListener{

    @Override
    public void actionPerformed(ActionEvent e) {
        System.out.println("Lambda is working");
    }
}
```

可以看到，代码很复杂，需要新建一个类，下面用匿名内部类进行实现

```java
//版本2
public class TimerTest1 {

    public static void main(String[] args) {
        
        /*
        	使用匿名内部类来进行执行器的代码
        */
        Timer t = new Timer(3000, new ActionListener(){
            @Override
            public void actionPerformed(ActionEvent e) {
                System.out.println("Lambda is working");
            }
        });
        t.start();

        //提供窗口来结束程序
        JOptionPane.showMessageDialog(null,"Quit Program?");
        System.exit(0);
    }
}
```

可以看到，相对于版本1，省略了一个类的书写。然而，`ActionLinstener`是一个函数式接口，我们可以使用Lambda继续优化

```java
//版本3
public class TimerTest1 {

    public static void main(String[] args) {
        
        /*
        	使用Lambda表达式进行优化
        */
        Timer t = new Timer(3000, e->System.out.println("Lambda is working") );
        t.start();

        //提供窗口来结束程序
        JOptionPane.showMessageDialog(null,"Quit Program?");
        System.exit(0);
    }
}
```

可以看到，使用Lambda表达式后将版本1简化成了一句话

## 3.2 省略规则

- 参数类型可以省略不写。
- 如果只有一个参数，参数类型可以省略，同时()也可以省略。
- 如果Lambda表达式的方法体代码只有一行代码。可以省略大括号不写,同时要省略分号！
- 如果Lambda表达式的方法体代码只有一行代码。可以省略大括号不写。此时，如果这行代码是return语句，必须省略return不写，同时也必须省略";"不写



