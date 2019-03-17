# 1.修饰数据
在编写程序时，我们经常需要说明一个数据是不可变的，我们成为常量。在java中，用final关键字修饰的变量，只能进行一次赋值操作，并且在生存期内不可以改变它的值。更重要的是，final会告诉编译器，这个数据是不会修改的，那么编译器就可能会在编译时期就对该数据进行替换甚至执行计算，这样可以对我们的程序起到一点优化。不过在针对基本类型和引用类型时，final关键字的效果存在细微差别。我们来看下面的例子：

``` javascript
class Value {
    int v;
    public Value(int v) {
        this.v = v;
    }
}

public class FinalTest {
    
    final int f1 = 1;
    final int f2;
    public FinalTest() {
        f2 = 2;
    }

    public static void main(String[] args) {
        final int value1 = 1;
        // value1 = 4;
        final double value2;
        value2 = 2.0;
        final Value value3 = new Value(1);
        value3.v = 4;
    }
}
```
上面的例子中，我们先来看一下main方法中的几个final修饰的数据，在给value1赋初始值之后，我们无法再对value1的值进行修改，final关键字起到了常量的作用。从value2我们可以看到，final修饰的变量可以不在声明时赋值，即可以先声明，后赋值。value3时一个引用变量，这里我们可以看到final修饰引用变量时，只是限定了引用变量的引用不可改变，即不能将value3再次引用另一个Value对象，但是引用的对象的值是可以改变的

最后我们需要注意的一点是，同时使用static和final修饰的成员在内存中只占据一段不能改变的存储空间。

# 2.修饰方法参数

前面我们可以看到，如果变量是我们自己创建的，那么使用final修饰表示我们只会给它赋值一次且不会改变变量的值。那么如果变量是作为参数传入的，我们怎么保证它的值不会改变呢？这就用到了final的第二种用法，即在我们编写方法时，可以在参数前面添加final关键字，它表示在整个方法中，我们不会（实际上是不能）改变参数的值：

``` java
public class FinalTest {

    /* ... */

    public void finalFunc(final int i, final Value value) {
        // i = 5; 不能改变i的值
        // v = new Value(); 不能改变v的值
        value.v = 5; // 可以改变引用对象的值
    }
}
```

# 3.修饰方法

第三种方式，即用final关键字修饰方法，它表示该方法不能被覆盖。这种使用方式主要是从设计的角度考虑，即明确告诉其他可能会继承该类的程序员，不希望他们去覆盖这个方法。这种方式我们很容易理解，然而，关于private和final关键字还有一点联系，这就是类中所有的private方法都隐式地指定为是final的，由于无法在类外使用private方法，所以也就无法覆盖它。

# 4.修饰类
了解了final关键字的其他用法，我们很容易可以想到使用final关键字修饰类的作用，那就是用final修饰的类是无法被继承的。