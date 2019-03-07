---
author: C.G
title: 2019-3-6-java输入输出
grammar_cjkRuby: true
---
**学习任务：**
 1. 输入输出流
 2. 文本输入与输出
 3. 读写二进制数据
 4. 对象输入输出流与序列化
 5. 操作文件
 6. 内存映射文件
 7. 正则表达式
 

# **一、输入输出流**

**读写字节**：基于抽象类InputStream、OutputStream，基于byte;
**读写Unicode**：基于抽象类Reader、Writer,基于Char(Unicode码元）；

1.读写字节
InputStream类中有一抽象方法：
public abstract int read() throws IOException
此方法从输入流中读取一个字节，并返回读到的字节，0-255，遇到源结尾返回-1，具体的输入流必须覆盖这个方法；
相似，OutputStream有相似的机制，write(int b),

``` oxygene
public abstract void write(int b)
                    throws IOException
Writes the specified byte to this output stream. The general contract for write is that one byte is written to the output stream. The byte to be written is the eight low-order bits of the argument b. The 24 high-order bits of b are ignored.
Subclasses of OutputStream must provide an implementation for this method.

Parameters:
b - the byte.
Throws:
IOException - if an I/O error occurs. In particular, an IOException may be thrown if the output stream has been closed.
```
两个方法执行时都会阻塞（线程理解：[Java线程](https://www.cnblogs.com/GooPolaris/p/8079490.html)），直至字节确实被读入或写出，
![enter description here](./images/1551855690063.png)
![完整的流家族](./images/1551855942815.png)
![Read&Writer](./images/1551856028279.png)

# **文本输入与输出**

1.可以使用PrintWriter,可以打印字符串和数字
![Construcors](./images/1551857442210.png)
2.读入文本输入，Scanner类
![enter description here](./images/1551857687250.png)
3.以文本形式存储对象
![enter description here](./images/1551857839815.png)

# **读写二进制数据**

DataInput/DataOutput接口
![enter description here](./images/1551858160406.png)

# **随机访问文件**

RandomAccessFile类可以在文件中的任何位置查找或写入数据，磁盘文件都是随机访问的，但是网络套接字的通信的输入输出流却不是。
![enter description here](./images/1551858860749.png)

# **ZIP文档**

![enter description here](./images/1551859706832.png)

# **对象输入输出流与序列化**

**1.保存和加载序列化对象**

为了保存对象数据：

``` haxe
ObjectOutputStream oout = new ObjectOutputStream(new FileOutputStream("Employee.dat"));
oout.writeObject(new Employee());
```
为了读回对象数据：

``` haxe
ObjectInputStream in = new ObjectInputStream(new FileInputStream("Employe.dat"));
Employee e = (Employee) in.readObject();
```
但是在此之前，Employee类必须实现Serializable接口：

``` nimrod
class Employee implements Serializable{...}
```

![两个经理共用一个秘书时](./images/1551861227730.png)


**

## 操作文件

**

**1.path**

![Path常用API](./images/1551863605905.png)

**2.读写文件**

Files类可以使普通文件操作更加简单，比较适合中等长度文件，如果文件太大，或者是二进制文件，还是用前面的。

``` mipsasm
//输入
byte[] bytes = Files.readAllBytes(path);
String content = new String(bytes, charset);
List<String> lines = Files.readAllLines(path,charset);
//输出
Files.write(path, content.getBytes(charset));
Files.write(path, content.getBytes(charset),StandardOpenOption.APPEND);
Files.write(path,lines);
```

**创建文件和目录**

![enter description here](./images/1551927489121.png)
![enter description here](./images/1551927559656.png)

**复制、移动和删除文件**

![enter description here](./images/1551927871189.png)
![enter description here](./images/1551927883983.png)

**获取文件信息**

![enter description here](./images/1551927958027.png)
![enter description here](./images/1551927973019.png)



**内存映射文件**

[enter description here](https://www.cnblogs.com/ixenos/p/5863921.html)
![enter description here](./images/1551932554933.png)

**

# 正则表达式

**
[http://www.runoob.com/java/java-regular-expressions.html](http://www.runoob.com/java/java-regular-expressions.html)
