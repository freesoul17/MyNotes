# 一、File类
- java.io.File类：文件和文件目录路径的抽象表示形式，与平台无关
- File 能新建、删除、重命名文件和目录，但**File不能访问文件内容本身**。如果需要访问文件内容本身，则需要使用输入/输出流。
  想要在Java程序中表示一个真实存在的文件或目录，那么必须有一个File对象，但是Java程序中的一个File对象，可能没有一个真实存在的文件或目录。
- File对象可以作为参数传递给流的构造器

## 1.1 File类的实例化
* File(String filePath):以filePath为路径创建File对象，可以是绝对路径或者相对路径
 * File(String parentPath,String childPath):以parentPath为父路径，childPath为子路径创建File对象。
* File(File parentFile,String childPath):根据一个父File对象和子文件路径创建File对象

**路径分隔符**

```java
windows:\\
unix:/
Java程序支持跨平台运行，因此路径分隔符要慎用。
为了解决这个隐患，File类提供了一个常量：
public static final String separator;
根据操作系统，动态的提供分隔符。
File file1= new File("d:\\info.txt");
File file2= new File("d:"+ File.separator+ "info.txt");
File file3= new File("d:/Work");
```
##  File类常用方法

| 方法                        | 描述                                                         |
| --------------------------- | ------------------------------------------------------------ |
| String getAbsolutePath()    | 获取绝对路径                                                 |
| String getPath()            | 获取路径                                                     |
| String getName()            | 获取名称                                                     |
| String getParent()          | 获取上层文件目录路径。若无，返回null                         |
| long length()               | 获取文件长度（即：字节数）。不能获取目录的长度。             |
| long lastModified()         | 获取最后一次的修改时间，毫秒值                               |
| public String[] list()      | 获取指定目录下的所有文件或者文件目录的名称数组，该文件必须存在 |
| public File[] listFiles()   | 获取指定目录下的所有文件或者文件目录的File数组，该文件必须存在 |
| boolean renameTo(File dest) | 把文件重命名为指定的文件路径<br/>比如：file1.renameTo(file2)为例：要想保证返回true,需要file1在硬盘中是存在的，且file2不能在硬盘中存在。 |
| boolean isDirectory()       | 判断是否是文件目录                                           |
| boolean isFile()            | 判断是否是文件                                               |
| boolean exists()            | 判断是否存在                                                 |
| boolean canRead()           | 判断是否可读                                                 |
| boolean canWrite()          | 判断是否可写                                                 |
| boolean isHidden()          | 判断是否隐藏                                                 |
| boolean createNewFile()     | 创建文件。若文件存在，则不创建，返回false                    |
| boolean mkdir()             | 创建文件目录。如果此文件目录存在，就不创建了。如果此文件目录的上层目录不存在，也不创建。 |
| boolean mkdirs()            | 创建文件目录。如果此文件目录存在，就不创建了。如果上层文件目录不存在，一并创建 |
| boolean delete()            | 删除文件或者文件夹。Java中的删除不走回收站。                 |

# 二、IO流
- IO是Input/Output的缩写，I/O技术是非常实用的技术，用于处理设备之间的数据传输。如读/写文件，网络通讯等。
- Java程序中，对于数据的输入/输出操作以“流(stream)”的方式进行。
- java.io包下提供了各种“流”类和接口，用以获取不同种类的数据，并通过标准的方法输入或输出数据。
- 输入input：读取外部数据（磁盘、光盘等存储设备的数据）到程序（内存）中。
- 输出output：将程序（内存）数据输出到磁盘、光盘等存储设备中。

## 2.1流的体系结构
  流|抽象基类 | 节点流（或文件流）|   缓冲流（处理流的一种）
 -|-|-|-
字节输入流|InputStream  |   FileInputStream(read(byte[] buffer))    |    BufferedInputStream (read(byte[] buffer))
字节输出流|OutputStream  |  FileOutputStream(write(byte[] buffer,0,len) | BufferedOutputStream (write(byte[] buffer,0,len) / flush()
字符输入流|Reader        |  FileReader(read(char[] cbuf))          |       BufferedReader (read(char[] cbuf) / readLine())
字符输出流|Writer        |  FileWriter(write(char[] cbuf,0,len)      |     BufferedWriter (write(char[] cbuf,0,len) / flush()
## 2.2节点流（文件流）

1. **对于文本文件(.txt,.java,.c,.cpp)，使用字符流处理** 
 2. **对于非文本文件(.jpg,.mp3,.mp4,.avi,.doc,.ppt,...)，使用字节流处理**

### 2.2.1 FileReader读入数据

将硬盘中的数据读入内存

1. File类实例化
2. FileReader流的实例化
3. 读入的操作
4. 关闭资源

**单个字符读入**

```java
public void test() {
    FileReader fr = null;
    try {
        //1.File类实例化
        File file = new File("hello.txt");
        //2.FileReader流的实例化
        fr = new FileReader(file);
        //read():返回读入的一个字符。如果达到文件末尾，返回-1.
        int data;
        //3.读入的操作
        while ((data = fr.read()) != -1) {
            System.out.print((char) data);
        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        if (fr != null) {
            try {
                //4.关闭资源
                fr.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

**利用数组读入多个字符**

```java
public void test2(){
    FileReader fr = null;
    try {
        //1.File类的实例化
        File file = new File("hello.txt");

        //2.FileReader流的实例化
        fr = new FileReader(file);

        //3.读入的操作
        //read(char[] cbuf):返回每次读入cbuf数组中的字符的个数。如果达到文件末尾，返回-1
        char[] cbuf = new char[1024];
        int len;
        while((len = fr.read(cbuf)) != -1){
            String str = new String(cbuf,0,len);
            System.out.print(str);
        }
    } catch (IOException e) {
        e.printStackTrace();
    }finally {
        if(fr != null){
            //4.资源的关闭
            try {
                fr.close();
            } catch (IOException e) {
                e.printStackTrace();
            }

        }
    }
}
```

**read()的理解：**

1. **返回读入的一个字符。如果达到文件末尾，返回-1**
2. 异常的处理：为了保证流资源一定可以执行关闭操作。需要使用try-catch-finally处理
3. 读入的文件一定要存在，否则就会报FileNotFoundException。

### 2.2.2FileWriter写出数据的操作
从内存中写出数据到硬盘的文件里。

1. 提供File类的对象，指明写出到的文件
2. 提供FileWriter的对象，用于数据的写出
3. 写出的操作
4. 流资源的关闭

```java
public void test3() {
    FileWriter fw = null;
    try {
        //1.提供File类的对象，指明写出到的文件
        File file = new File("hello1.txt");
        //2.提供FileWriter的对象，用于数据的写出
        fw = new FileWriter(file, false);
        //3.写出的操作
        fw.write("I have a dream!\n");
        fw.write("you need to have a dream!");
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        //4.流资源的关闭
        if (fw != null) {
            try {
                fw.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

**说明：**
1. 输出操作，对应的File可以不存在的。并不会报异常
2. File对应的硬盘中的文件如果不存在，在输出的过程中，会自动创建此文件。
3. File对应的硬盘中的文件如果存在：
   - 如果流使用的构造器是：FileWriter(file,false) / FileWriter(file):对原有文件的覆盖
   - 如果流使用的构造器是：FileWriter(file,true):不会对原有文件覆盖，而是在原有文件基础上追加内容

### 2.2.3使用FileReader和FileWriter实现文本文件的复制
```java
    @Test
    public void test4() {
        FileReader fr = null;
        FileWriter fw = null;
        try {
            //1.创建File类的对象，指明读入和写出的文件
            File srcFile = new File("hello1.txt");
            File srcFile2 = new File("hello2..txt");
            //2.创建输入流和输出流的对象
            fr = new FileReader(srcFile);
            fw = new FileWriter(srcFile2);
            //3.数据的读入和写出操作
            char[] cbuf = new char[1024];
            int len;//记录每次读入到cbuf数组中的字符的个数
            while((len = fr.read(cbuf)) != -1){
                //每次写出len个字符
                fw.write(cbuf,0,len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4.关闭流资源
            try {
                if(fw != null)
                    fw.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                if(fr != null)
                    fr.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```
### 2.2.4 使用FileInputStream和FileOutputStream复制非文本文件

```java
public void test() {
    FileInputStream fis = null;
    FileOutputStream fos = null;
    try {
        //1.造文件
        File srcFile = new File("爱情与友情.jpg");
        File destFile = new File("爱情与友情2.jpg");
        //2.造流
        fis = new FileInputStream(srcFile);
        fos = new FileOutputStream(destFile);
        //3.复制的过程
        byte[] buffer = new byte[1024];
        int len;
        //4.读数据
        while ((len = fis.read(buffer)) != -1) {
            fos.write(buffer, 0, len);
        }
        System.out.println("复制成功");
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        if (fos != null) {
            //5.关闭资源
            try {
                fos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        if (fis != null) {
            try {
                fis.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

## 2.3 缓冲流

为了提高数据读写的速度，Java API提供了带缓冲功能的流类，在使用这些流类时，会创建一个内部缓冲区数组，缺省使用8192个字节(8Kb)的缓冲区。

**缓冲流要“套接”在相应的节点流之上**，根据数据操作单位可以把缓冲流分为：

- BufferedInputStream和BufferedOutputStream
- BufferedReader和BufferedWriter

当读取数据时，数据按块读入缓冲区，其后的读操作则直接访问缓冲区

当使用BufferedInputStream读取字节文件时，BufferedInputStream会一次性从文件中读取8192个(8Kb)，存在缓冲区中，直到缓冲区装满了，才重新从文件中读取下一个8192个字节数组。

向流中写入字节时，不会直接写到文件，先写到缓冲区中直到缓冲区写满，BufferedOutputStream才会把缓冲区中的数据一次性写到文件里。**使用方法flush()可以强制将缓冲区的内容全部写入输出流**

关闭流的顺序和打开流的顺序相反。**只要关闭最外层流即可**，关闭最外层流也会相应关闭内层节点流

**flush()方法的使用：手动将buffer中内容写入文件**

如果是带缓冲区的流对象的close()方法，不但会关闭流，还会在关闭流之前刷新缓冲区，关闭后不能再写出。

### 2.3.1  使用BufferedInputStream和BufferedOutputStream实现非文本文件的复制

```java
@Test
public void BufferedStreamTest() {
    BufferedInputStream bis = null;
    BufferedOutputStream bos = null;
    try {
        //1.造文件
        File srcFile = new File("爱情与友情.jpg");
        File destFile = new File("爱情与友情3.jpg");
        //2.造流
        //2.1 造节点流
        FileInputStream fis = new FileInputStream((srcFile));
        FileOutputStream fos = new FileOutputStream(destFile);
        //2.2 造缓冲流
        bis = new BufferedInputStream(fis);
        bos = new BufferedOutputStream(fos);

        //3.复制的细节：读取、写入
        byte[] buffer = new byte[10];
        int len;
        while ((len = bis.read(buffer)) != -1) {
            bos.write(buffer, 0, len);
        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        //4.资源关闭
        //要求：先关闭外层的流，再关闭内层的流
        //说明：关闭外层流的同时，内层流也会自动的进行关闭。关于内层流的关闭，我们可以省略.
        if (bos != null) {
            try {
                bos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        if (bis != null) {
            try {
                bis.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

### 2.3.2 使用BufferedInputStream和BufferedOutputStream实现文本文件的复制

```java
@Test
public void testFileInputOutputStream() {
    FileInputStream fis = null;
    FileOutputStream fos = null;
    try {
        //1.造文件
        File srcFile = new File("爱情与友情.jpg");
        File destFile = new File("爱情与友情2.jpg");
        //2.造流
        fis = new FileInputStream(srcFile);
        fos = new FileOutputStream(destFile);
        //3.复制的过程
        byte[] buffer = new byte[5];
        int len;
        //4.读数据
        while ((len = fis.read(buffer)) != -1) {
            fos.write(buffer, 0, len);
        }
        System.out.println("复制成功");
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        if (fos != null) {
            //5.关闭资源
            try {
                fos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
        if (fis != null) {
            try {
                fis.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

## 2.4 转换流

- 转换流提供了在字节流和字符流之间的转换
- Java API提供了两个转换流：
  - InputStreamReader：将InputStream转换为Reader
    - 实现将字节的输入流按指定字符集转换为字符的输入流。
    - 需要和InputStream“套接”。
    - 构造器
      - public InputStreamReader(InputStream in)
      - public InputSreamReader(InputStream in,String charsetName)
  - OutputStreamWriter：将Writer转换为OutputStream
    - 实现将字符的输出流按指定字符集转换为字节的输出流。
    - 需要和OutputStream“套接”。
    - 构造器
      - public OutputStreamWriter(OutputStream out)
      - public OutputSreamWriter(OutputStream out,String charsetName)
  - 字节流中的数据都是字符时，转成字符流操作更高效。
  - 很多时候我们使用转换流来处理文件乱码问题。实现编码和解码的功能。

```java
File file1 = new File("dbcp.txt");
File file2 = new File("dbcp_gbk.txt");
FileInputStream fis = new FileInputStream(file1);
FileOutputStream fos = new FileOutputStream(file2);
//将字符输入流转换为字符输出流
InputStreamReader isr = new InputStreamReader(fis,"utf-8");
//将字符输出流转换为字符输入流
OutputStreamWriter osw = new OutputStreamWriter(fos,"gbk");
```

## 2.5 标准的输入、输出流

- System.in:标准的输入流，默认从键盘输入
- System.out:标准的输出流，默认从控制台输出
- System类的setIn(InputStream is) / setOut(PrintStream ps)方式重新指定输入和输出的流。

## 2.6 数据流
为了方便地操作Java语言的基本数据类型和String的数据，可以使用数据流。

数据流有两个类：(用于读取和写出基本数据类型、String类的数据）

**DataInputStream和DataOutputStream**

分别“套接”在InputStream和OutputStream子类的流上

- DataInputStream中的方法
```java
boolean readBoolean()	byte readByte()
char readChar()	float readFloat()
double readDouble()	short readShort()
long readLong()	int readInt()
String readUTF()	void readFully(byte[s] b)
```
- DataOutputStream中的方法
```java
将上述的方法的read改为相应的write即可。
```
## 2.7 对象流
- **ObjectInputStream和OjbectOutputSteam**
- 用于存储和读取基本数据类型数据或对象的处理流。它的强大之处就是可以**把Java中的对象写入到数据源中，也能把对象从数据源中还原回来。**
- 序列化：用ObjectOutputStream类保存基本类型数据或对象的机制
- 反序列化：用ObjectInputStream类读取基本类型数据或对象的机制
- **ObjectOutputStream和ObjectInputStream不能序列化static和transient修饰的成员变量**
- 对象序列化机制允许把内存中的Java对象转换成平台无关的二进制流，从而允许把这种二进制流持久地保存在磁盘上，或通过网络将这种二进制流传输到另一个网络节点。当其它程序获取了这种二进制流，就可以恢复成原来的Java对象
- 序列化的好处在于可将任何实现了Serializable接口的对象转化为字节数据，使其在保存和传输时可被还原
- 序列化是RMI（Remote Method Invoke –远程方法调用）过程的参数和返回值都必须实现的机制，而RMI 是JavaEE的基础。因此序列化机制是JavaEE平台的基础
- 如果需要让某个对象支持序列化机制，则必须让对象所属的类及其属性是可序列化的，为了让某个类是可序列化的，该类必须实现Serializable/Externalizable两个接口之一。否则，会抛出NotSerializableException异常

```java
import org.junit.Test;
import java.io.*;
public class ObjectTest {
    /**
     * 序列化过程：将内存中的java对象保存到磁盘中或通过网络传输出去
     * 使用ObjectOutputStream实现
     */
    @Test
    public void test(){
        ObjectOutputStream oos = null;
        try {
            //创造流
            oos = new ObjectOutputStream(new FileOutputStream("object.dat"));
            //制造对象
            oos.writeObject(new String("秦始皇陵欢迎你"));

            //刷新操作
            oos.flush();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(oos != null){
                //3.关闭流
                try {
                    oos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    /**
     * 反序列化：将磁盘文件中的对象还原为内存中的一个java对象
     * 使用ObjectInputStream来实现
     */
    @Test
    public void test2(){
        ObjectInputStream ois = null;
        try {
            ois = new ObjectInputStream(new FileInputStream("object.dat"));
            Object obj = ois.readObject();
            String str = (String) obj;
            System.out.println(str);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } finally {
            if(ois != null){
                try {
                    ois.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

#### 2.7.1 自定义类实现序列化与反序列化操作
若某个类实现了Serializable接口，该类的对象就是可序列化的：
**序列化**

1. 创建一个ObjectOutputStream
2. 调用ObjectOutputStream对象的writeObject(对象) 方法输出可序列化对象
3. 注意写出一次，操作flush()一次
4. 关闭流

**反序列化**

1. 创建一个ObjectInputStream对象调用readObject() 方法读取流中的对象
2. 强调：如果某个类的属性不是基本数据类型或String 类型，而是另一个引用类型，那么这个引用类型必须是可序列化的，否则拥有该类型的Field 的类也不能序列化

```java
import java.io.Serializable;


//1.需要实现接口：Serializable

public class Person implements Serializable {
    //2.需要提供serialVersionUID
    public static final long serialVersionUID = 475463454532L;
    private String name;
    private int age;
}
```

### 2.7.2 erialVersionUID的理解

- 凡是实现Serializable接口的类都有一个表示序列化版本标识符的静态变量：private static final long serialVersionUID;
- serialVersionUID用来表明类的不同版本间的兼容性。简言之，其目的是以序列化对象进行版本控制，有关各版本反序列化时是否兼容。
  如果类没有显示定义这个静态常量，它的值是Java运行时环境根据类的内部细节自动生成的。若类的实例变量做了修改,serialVersionUID可能发生变化。故建议，显式声明。
- 简单来说，Java的序列化机制是通过在运行时判断类的serialVersionUID来验证版本一致性的。在进行反序列化时，JVM会把传来的字节流中的serialVersionUID与本地相应实体类的serialVersionUID进行比较，如果相同就认为是一致的，可以进行反序列化，否则就会出现序列化版本不一致的异常。(InvalidCastException)

