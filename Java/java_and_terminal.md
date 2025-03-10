# 编译运行

```java
//使用utf-8编码格式编译
javac -encoding utf-8 Main.java
//因为现在IDEA一般默认编码是UTF-8，而windows的终端默认编码是GBK。如果不指明编码方式，在编译时会发生转码错误

//运行
java Main
```

# 检测死锁

### jps和jstack检测死锁

![image-20250309154329532](image/image-20250309154329532.png)

![](image/屏幕截图 2025-03-09 154238.png)

先点击IDEA的按钮运行程序，然后在本地终端中输入jps会检测进程，进程前面的数字就是进程的PID

在输入jstack PID进行检测，最后会返回Found 1 deadlock表示发现死锁，然后**蓝色**的字体会告诉发生在代码的第几行

### jconsole检测死锁

![image-20250309155406195](image/image-20250309155406195.png)

本地终端输入jconsole会弹出一个可视化检测窗口，选择本地进程Main，点击连接

<img src="image/屏幕截图 2025-03-09 154909.png" style="zoom: 80%;" />

这里可以看到很多东西，内存，线程，类，CPU占用

<img src="image/屏幕截图 2025-03-09 154951.png" style="zoom: 80%;" />

我们点击上方的线程，会到这个窗口，点击下方的检测死锁

<img src="image/屏幕截图 2025-03-09 155020.png" style="zoom: 80%;" />

如果有死锁就会检测到，然后点击对应线程就会看到详细内容