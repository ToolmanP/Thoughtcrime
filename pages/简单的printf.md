- > 前言这不是教材也不是lecture note，可能会有些错误地方，毕竟是出于兴趣写的。所以请见谅。当然我会尽量写的稍微正确一点，我会把所有的reference写在最后
- > 本文的阅读时间：
-
- 学编程的应该都知道有这么个语言叫做C语言，如果你学个那么一开始的每个人的起手式应该是printf一串字符串了。
- 下面是一个最简单的C语言程序， 简单来说你要有一个程序的**入口**起点叫做main，你引入了一个头文件叫做stdio.h，然后在里面调用了一个C语言库函数叫做printf，然后就能愉快的打印了你想要的东西了。Easy Huh?
-
- ```C
  #include <stdio.h>
  
  int main(){
    printf("Hello world!\n");
    return 0;
  }
  ```
- 然后所有的教程都会告诉你，你需要把这个文件或者叫做**源文件**喂给一个叫做**编译器**的东西，然后ta负责把你的源文件进行加工组装，然后生成一个能够在你的系统上运行的终端程序了。
- 比如最简单的，如果你用linux/macos 你只需要使用GNU C Compiler (GNU C 编译器) 然后运行这个指令你就会在当前目录下面找到，在windows上你可以尝试使用msvc或者mingw来进行编译。
- ```console
  gcc test.c -o test
  ```
- 在此之后，你就可以在命令行里面输入
- ```console
  ./test
  ```
- 然后你就有了
- ```console
  Hello world!
  ```
- #### 好耶！
- ---
- ### 一些超出常规但是非常合理的思考
- 你的确打出了Hello world，然后你就接下来继续看你的**谭浩强**了，但是不知道聪明的你有没有想过...
- > **为什么这个文件能被正确的生成出来？**
- >**为什么这个文件能够被正确的执行？系统是怎么执行这个玩意的？**
- > **为什么这个Hello world输入到了执行他的终端里，为什么没有跑到外边或者跑到屏幕上去？**
-
- 当你学的更多的时候，你可能会想 ...
- > **这个printf在哪里呢？为什么我点进那个stdio.h里面就一个extern 然后就没了？**
- > **好好好，这个Hello World对应一个字符串数据，那么这个数据是怎么传出来的？**
- ---
- 好好好，你可能已经被我搞晕了，我将在以下文章里面阐述清楚这些问题。
- 现在我们可能就觉得这个Hello World没有那么的简单了，那么有了这些问题后，不妨让我们回到最初的美好，看看上古时期的计算机是怎么运行程序的。
- ---
- ## 万物起源——冯诺依曼架构
- 说到计算机那么我们就不得不提到计算机的最经典也是沿用至今的架构 —— 冯诺依曼架构。
- 什么是冯诺依曼架构呢？
- 以下是最简单的一个sample
-
- 好现在我们知道了我们的CPU呢需要一个PC指针，计算机知道该从他指向的那个地方从内存里面把我们需要的二进制指令拿出来，算出来，把计算结果写回去然后再更新这个指针，循环往复，直到计算机跑到了一个halt的指令或者被内存控制器告知这里到内存外面了，然后会因为报错而停下来。
- 现在我们有了内存，CPU，还有硬盘上的程序，然后我们知道了那么他是怎么进到内存里的？进到内存的那里？PC是怎么指到那个地方的？
- ## Unix下的显微镜 —— 二进制的工具集
-
- ## 小跑System Loader 101
-
- ## 给内存上一个VR —— 虚拟内存
-
- ## 最后的最后 —— System IO
-