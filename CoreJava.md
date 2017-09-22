# Core Java Volume I-Fundamentals

## chapter 5 继承

### 5.1 类、超类和子类

#### super和this

- super：不是一个对象的引用，不能将super的值赋给另一个变量，只是一个关键字

  this：当前对象的引用

- 用途

  super：

  - 调用超类方法
  - 调用超类构造器

  this：

  - 引用隐式参数
  - 调用该类其他构造方法

#### 子类构造器

Manager是子类，Employee是父类

```java
public Manager(String name, double salary, int year, int month, int day) {
  super(name, salary, year, month, day);
  bonus = 0;
}
```

- `super(...)`是调用超类构造器的简写形式


- 调用超类构造器的语句必须是子类构造器的第一条语句

```java
//Manager和Employee中都有Salary方法，实现方式不同
Manager manager = new Manager();
Employee employee = manager;
//用employee调用方法
employee.getSalary();
```

- 调用能够确定执行哪个getSalary方法。尽管将employee声明为Employee类型，但是实际上employee既可以引用Employee类型的对象，也可以引用Manager类型的对象。
- 虚拟机知道employee的实际引用类型，因此能够正确调用相应的方法

#### 多态和动态绑定

- 多态：一个对象变量可以指定多种实际类型的现象

  > 在java程序设计语言中，对象变量是多态的。一个Employee变量既可以引用Employee类对象，也可以引用一个Employee类的任何一个子类的对象

  注意：

  - 不能将超类的引用赋给子类变量

  - 子类数组的引用可以装换成超类数组的引用，不需要强制类型转换

    ```java
    Manager[] managers = new Manager[10];
    Employee[] staff = managers; //ok
    ```

  - 所有数组牢记创建他们的元素类型

    ```java
    Manager[] managers = new Manager[10];
    Employee[] staff = managers; 
    staff[0] =  new Employee(...);
    //managers[0]和staff[0]引用的应该是同一个对象
    //setBonus()为Manager派生的方法
    managers[0].setBonus(1000); //运行错误
    ```

    >  `managers[0].setBonus(1000)` 
    >
    >  会导致调用一个不存在的实例域，进而搅乱相邻存储空间的内容
    >
    >  ​
    >
    >  为避免此类错误，所有数组都要牢记创建他们的元素类型，并负责监督仅将类型兼容的引用存储到数组中

动态绑定：在运行时能够自动地选择调用哪个方法的现象

#### 理解方法调用

- 方法签名：方法名 + 参数列表

- 方法覆盖：子类中定义了和父类签名相同的方法

  - 覆盖方法时，要保证返回类型的兼容性

    （子类返回类型是父类返回类型的子类或相同类）

- 方法调用步骤：

  调用：x.f(args)

  1. 编译器查看对象的声明类型和方法名。编译器会列举与该方法名f相同的方法，以及超类中public权限且方法名对应的方法------>获取可能被调用的候选方法

  2. 编译器查看调用方法时提供的参数类型。找到有且仅有一个方法与之匹配，就选择这个方法，否则报错

  3. 静态绑定：private方法、static方法、final方法或者构造器，这些都是编译器将可以准确知道应该调用哪个方法

     与之对应，调用的方法依赖于隐式参数的实际类型，并且在运行时实现动态绑定

  4. 当程序运行，并且采用动态绑定调用方法，虚拟机一定调用与x所引用对象的实际类型最适合的那个类的方法。

  - 每次调用方法都要进行搜索，时间开销非常大。虚拟机会预先为每个类创建了一个方法表，其中列出了所有方法的签名和实际调用的方法，真正调用方法的时候，检索这张表就可以了

#### 阻止继承：final类和方法

final类：不允许扩展的类，该类所有方法自动为final方法，域不为final

final方法：子类不能覆盖的方法

final域（成员变量）：构造对象后就不允许改变它们的值了

​					基本类型来说是其值不可变，而对于对象变量来说其引用不可再变 

> final域初始化:
>
> 一是其定义处，也就是说在final变量定义时直接给其赋值
>
> 二是在构造函数中 
>
> 这两个地方只能选其一

内联：如果一个方法没有被覆盖并且很短，编译器能对它进行优化处理

eg：内联调用e.getName()将被替换为访问e.name域，这是由于CPU在处理调用方法的指令时，使用的分支转移会扰乱预取指令的策略，这是不受欢迎的

#### 强制类型转换

强制类型转换的唯一原因：在暂时忽视对象的实际类型后，使用对象的全部功能

- 只有在继承层次内进行类型转换
- 在将超类转子类之前，应该使用instanceof进行检查

>父类Employee，子类Manager拓展了setBonus()方法
>
>如果鉴于某种原因发现需要通过Employee对象调用setBonus()方法，那么应该检查超类设计是否合理。重新设计一下超类，并添加setBonus()方法才是正确的选择

注：一般情况下，应该尽量少用instanceof运算符

#### 抽象类

抽象类不能被实例化

### 5.2 Object：所有类的超类

在java中，只有基本类型（数值、字符、布尔）不是对象

所有数组类型都扩展了Object类

```java
Employee[] staff = new Employee[10];
Object obj = staff; //ok
obj = new int[10]; //ok
```

#### equals 方法

判断两个对象是否equals

- 方法一：`a.equals(b)`
- 方法二：`Object.eqluas(a, b)`

方法二可以避免a/b为null的情况

>  在子类中定义equals方法时，应首先调用超类的equals
>
>  如果检测失败，对象就不可能相等
>
>  检测成功，再比较子类中的实例域

#### hashCode 方法

hashCode方法定义在Object类中：默认散列码值为对象的储存地址

- 如果重新定义equals方法，就必须重新定义equals方法

- 最好使用`Object.hashCode(aaa)`，避免空指针，如果参数是null，返回0

- 需要组合多个散列值，可以使用Objects.hash()并提供多个参数

- equals和hashCode的定义必须一致

  > 如果x.equals(y)返回true，那么x.hashCode()和y.hashCode()必须返回相同值

  > 如果Employee.equals比较ID，那么hashCode就需要散列ID，而不是姓名等

#### toString 方法

Object类默认toString方法，返回对象所属的类名和散列码

> 数组调用toString方法，数组继承的是Object中的toString方法
>
> 如果希望按照数组元素的toString来获取返回值，可以使用Arrays.toString(arr)
>
> 多维数组可以使用Arrays.deepToString()

### 5.3 范型数组列表

在java中，能够在运行时确定数组大小(c和c++必须在编译时就指定数组大小)

```java
int actualSize = ...;
Employee staff = new Employee[actualSize];
```

- 如果清楚或能够估计出数组可能存在的元素数量，可以在填充数组之前调用`staff.ensureCapacity(100);`

- 可以使用ArraysList构造器 `ArrayList<Employee> staff = new ArrayList<>(100)`

  - 一旦可以确认数组列表大小不在发生吧变化，可以使用timeToSize方法

    >  这个方法可以将存储区域大小调整为当前元素数量所需要的储存空间数目
    >
    >  垃圾回收器将回收多余的储存空间
    >
    >  一旦整理了数组列表，添加新元素就需要花时间再次移动存储块

**泛型对比Object声明类型的优势**

可以让编译器提前检测出错误

``` java
// staff是一个Employee的ArrayList
staff.set(i, "abc");
//如果是泛型就能提前检测错误
```



@SuppressWarnings("unchecked") 标记能够接受未检查的转换

```
@SuppressWarnings("unchecked") 
ArrayList<Employee> result = emploueeDB.find(query);//find()返回原始类型的ArrayList
//@SuppressWarnings("unchecked") 消除类型类型转换的警告
```

### 5.4 对象包装器与自动装箱

- 对象包装器是不可变的

  即一旦构造了包装器，就不允许更改包装在其中的值了（如果对包装器赋予新值，其实是改变了变量的指向）

  - 如果想编写可以修改数值参数的方法，需要使用`org.omg.CORBA`包中定义的持有者（holder）类型，包括IntHolder、BooleanHolder等类型，每个持有者都有公有域值

    ```java
            IntHolder holder = new IntHolder(1);
            System.out.println(holder.value);
    		//可以通过修改holder.value = 123来修改值
    ```

- 包装器类是final类，继承于Number类

> ArrayList\<Integer>的效率远低于int[]
>
> 因此，应用它构造小型集合，原因是此时操作的方便性要比执行效率更重要

- 自动装箱、拆箱：

  ```java
  ArrayList<Integer> list = new ArrayList<>();
  list.add(3);//自动装箱，相当于list.add(Integer.valueOf(3));

  list.get(1);//自动拆箱，翻译成list.get(i).intValue();
  ```

  ```
  Integer n = 3;
  n++;//自动插入执行一条拆箱指令，然后自增，再装箱
  ```

  - 如果在一个条件表达式中混合使用Integer和Double，Interger会自动拆箱，提升为double，再装箱为Double：

    ```java
    Integer n = 1;
    Double x = 2.0;
    System.out.println(true ? n : x);//prints 1.0
    ```

### 5.6 枚举类



## chapter 6 接口、lambda表达式与内部类

### 6.3 lambda 表达式

lambda表达式是一个可传递的代码块，可以在以后执行一次或多次

将一个代码块传递到某个对象